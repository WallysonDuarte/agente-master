---
name: modelagem-dados
description: Modelar entidades, indices e consultas em banco documental e relacional, com migracao segura e integridade comprovada.
---

# Modelagem e consulta de dados

## Quando usar

Nova entidade, campo novo, consulta lenta, migracao, correcao de dado em producao.

## Antes de escrever qualquer consulta

1. **Leia o esquema real.** Nome de coluna no banco costuma divergir do nome da propriedade no
   codigo. Consulte o catalogo de colunas antes de escrever comando direto.
2. **Leia um documento real** antes de inserir manualmente num banco documental: nomes de campo de
   auditoria variam por projeto, e nome errado cria campo orfao que nenhum codigo le.
3. **Confirme o nome exato da colecao/tabela** no construtor do repositorio. Variante de plural ou
   de caixa cria destino silenciosamente vazio.
4. **Confirme a qual instancia o servico realmente se conecta** antes de afirmar que escreveu em
   producao. Base com o mesmo nome em outro servidor pode ser copia que ninguem le.

## Indices

- Indice composto na ordem das condicoes da consulta mais frequente.
- Indice unico em campo opcional precisa de filtro parcial. Em banco documental, condicao de
  existencia tambem casa com valor nulo — use condicao de tipo. Caso contrario dois registros sem
  o campo colidem.
- Expiracao automatica por tempo para dado efemero (sessao, codigo temporario, cache).
- Numero sequencial unico (documento fiscal, protocolo) **so** fica garantido por indice unico no
  banco. Travamento em memoria do processo nao protege multiplas instancias nem sobrevive a
  reinicio; o filtro parcial deve excluir os registros que legitimamente reaproveitam numero.

## Consultas

- Pagine no banco, nao em memoria. Carregar tudo e cortar depois derruba a aplicacao quando o
  volume cresce.
- Projete apenas as colunas usadas. Documento grande trafegado inteiro e o custo invisivel mais
  comum.
- Detecte consulta em cadeia dentro de laco (uma consulta por item) e troque por uma consulta com
  lista de identificadores.
- Para serie temporal, monte a grade de datas do periodo pedido e preencha com zero o que nao tem
  registro: agrupar apenas o que existe faz duas datas distantes aparecerem lado a lado.
- Busca por texto digitado pelo usuario precisa de normalizacao: remover acento, normalizar caixa e
  tratar separadores. Expressao crua montada com o texto do usuario, alem de fragil, e insegura.

## Migracao e correcao de dado

- Toda migracao e **idempotente**: inserir apenas o que falta (`... WHERE destino IS NULL`),
  atualizar apenas o que ainda nao tem o campo.
- Correcao de codigo cobre apenas gravacoes futuras. Dado antigo gravado antes da correcao continua
  errado — conte as linhas afetadas **antes** de declarar o defeito resolvido.
- Antes de alterar dado em massa: backup com os valores antigos (identificador + valor), executar,
  conferir a contagem depois.
- Exclusao pela interface normalmente e logica. Ao limpar dado de teste, confirme no banco e remova
  tambem o arquivo correspondente no armazenamento.
- Varredura que percorre muitas bases precisa liberar o pool de conexoes de cada base ao terminar:
  um pool por base, com conexoes ociosas vivas, esgota o limite do servidor e derruba o sistema.

## Checklist

- [ ] Esquema real conferido (nomes de coluna/colecao)
- [ ] Indices cobrindo as consultas frequentes; unico com filtro parcial onde o campo e opcional
- [ ] Paginacao e projecao no banco
- [ ] Nenhuma consulta dentro de laco
- [ ] Migracao idempotente, com backup e contagem antes/depois
- [ ] Dado legado conferido, nao apenas o caminho novo
- [ ] Conexao apontando para a instancia correta, comprovada

## Pronto quando

A consulta responde no tempo esperado com volume realista, o indice impede duplicidade, e a
contagem no banco prova que o dado antigo tambem foi corrigido.
