---
name: api-backend
description: Projetar e implementar API REST com separacao de camadas, validacao, autenticacao e tratamento de erro consistente.
---

# API backend em camadas

## Quando usar

Ao criar ou estender uma API: novo recurso, novo endpoint, mudanca de contrato, autenticacao,
tarefa em segundo plano.

## Arquitetura

Quatro camadas, dependencia em uma unica direcao:

```
Apresentacao (controllers/rotas)
      v
Aplicacao (servicos, DTOs, casos de uso)
      v
Dominio (entidades, regras, interfaces)
      v
Infraestrutura (repositorios, integracoes externas)
```

Regras duras:

- Dominio nao referencia Infraestrutura. Se precisar, inverta a dependencia com interface.
- Controller nao tem regra de negocio: recebe, delega, devolve.
- Entidade de dominio nunca sai pela API. Sempre um DTO proprio de resposta.
- Nenhum servico faz entrada/saida diretamente: tudo via interface injetada.

## Procedimento

1. **Contrato primeiro.** Defina rota, metodo, corpo de entrada, corpo de saida e todos os
   codigos de status possiveis. Mudanca de contrato e sempre aditiva: campo novo e opcional,
   campo antigo continua sendo enviado.
2. **DTO de entrada com validacao declarativa.** Tamanho maximo, obrigatoriedade, faixa de
   valores. Campo que so aceita inteiro recebe tipo inteiro — o proprio binding rejeita fracao.
3. **Caso de uso na camada de aplicacao.** Uma responsabilidade por classe. Toda regra que pode
   recusar a operacao (limite, saldo, estoque, duplicidade) roda **antes** de qualquer efeito
   colateral ou mudanca de status.
4. **Repositorio na infraestrutura.** Consulta paginada no banco, nunca carregar tudo em memoria
   para paginar depois. Projecao: traga apenas as colunas usadas.
5. **Erros com significado.** Validacao de entrada e erro do cliente (400/422), nao falha do
   servidor. Falha interna nao devolve mensagem tecnica crua nem identificador interno.
6. **Operacao em dois passos precisa de reivindicacao atomica.** Antes do efeito, mude o status
   com uma atualizacao condicional (`... WHERE status = <esperado>`) e confirme que uma linha foi
   afetada. Sem isso, dois cliques criam dois registros.

## Autenticacao e autorizacao

- Token de acesso curto + token de renovacao longo e rotativo.
- Autorizacao por recurso: compare o dono atual na **tabela de vinculo canonica**, nao numa coluna
  copiada no registro filho. Transferencia de responsavel que atualiza so o vinculo deixa o registro
  antigo negando acesso ao novo responsavel.
- Toda rota administrativa nova precisa de verificacao explicita de permissao: copiar o atributo de
  um endpoint vizinho nao e prova de que o gate funciona. Teste com um token real do fluxo que a
  interface usa.
- Limite de tentativas de login por identidade e por origem, bloqueando **na** tentativa que
  atinge o limite — nao na seguinte.

## Tarefas em segundo plano

Fila em processo precisa de tres garantias para sobreviver a reinicio e entrega dupla:

1. Reivindicacao atomica do item (`pendente -> executando` numa unica operacao condicional).
2. Passagem de recuperacao na inicializacao: reenfileirar o que ficou `pendente` e devolver o que
   ficou `executando` com sinal de vida antigo, com numero maximo de tentativas.
3. Teto de itens simultaneos por cliente, aplicado no momento de enfileirar.

Todo trabalho atribuido a um executor precisa de caminho de devolucao. Se o executor morre e nada
devolve o item, ele fica invisivel para sempre — e o efeito e cumulativo.

## Checklist

- [ ] Contrato documentado, aditivo, com todos os status de resposta
- [ ] DTO de entrada validado; tipos corretos (inteiro e inteiro, data e data)
- [ ] Regras que recusam a operacao rodam antes de qualquer efeito
- [ ] Nenhuma entidade de dominio exposta na resposta
- [ ] Consulta paginada e projetada no banco
- [ ] Erro de validacao devolve 4xx com mensagem util no idioma do usuario
- [ ] Nenhum segredo em arquivo versionado
- [ ] Teste cobrindo o caminho feliz e pelo menos dois erros

## Antipadroes

- `catch` vazio ou que apenas registra e segue, escondendo a causa real.
- Data e hora do momento local em vez de tempo universal para dado persistido.
- Campo nulavel de subdocumento que o codigo legado grava como lista vazia: o desserializador
  estrito quebra. Registre um conversor tolerante quando o historico do banco exigir.
- Endpoint "interno" protegido apenas por checagem de origem local — atras de um proxy reverso,
  o proxio tambem e local. Recuse requisicoes que tragam cabecalho de encaminhamento.

## Pronto quando

Compila sem erro, o contrato responde o que esta documentado para sucesso e para falha, a regra de
negocio recusa o que deve recusar antes de gravar, e existe teste automatizado cobrindo isso.
