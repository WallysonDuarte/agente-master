---
name: entrega-sem-pendencia
description: Fechar a atividade com build limpo, ausencia de placeholder, registro atualizado e evidencia do funcionamento no ambiente real.
---

# Criterio de entrega sem pendencia

## Quando usar

Sempre, no fechamento de qualquer atividade — antes de responder "concluido".

## O que significa pronto

Uma entrega esta pronta quando um usuario consegue completar o fluxo central no ambiente real.
Enquanto o modulo que justifica a atividade for marcador, tela vazia, rota morta ou aviso de
"em breve", a entrega e **parcial**, nunca concluida.

"Em breve" e aceitavel apenas em modulo genuinamente secundario.

## Checklist de fechamento

### Codigo
- [ ] Compila com zero erro
- [ ] Nenhum `TODO`, stub, marcador ou funcao vazia no caminho entregue
- [ ] Nenhum valor de configuracao fixado no codigo que deveria vir do ambiente
- [ ] Nenhum registro de depuracao com dado sensivel

### Verificacao
- [ ] Caminho feliz exercitado no ambiente real, nao apenas na maquina local
- [ ] Pelo menos dois erros exercitados (entrada invalida, recurso ausente)
- [ ] Nenhum erro no console do navegador ou no log do servico durante o teste

### Versionamento
- [ ] `git status` limpo em todos os repositorios tocados
- [ ] Commit descrevendo o que mudou, no idioma do projeto, sem assinatura automatica
- [ ] `git push` executado — commit sem envio e trabalho nao entregue
- [ ] Se usou arvore de trabalho separada, a mesclagem de volta ao ramo principal foi feita e
      confirmada

### Registro
- [ ] Decisoes e descobertas reutilizaveis anotadas onde a proxima pessoa vai procurar
- [ ] Nenhum dado sensivel anotado em arquivo de nota
- [ ] Arquivos temporarios criados durante a atividade removidos

## Como relatar

```
Resultado: CONCLUIDO | PARCIAL | BLOQUEADO
O que foi feito: <descricao objetiva>
Como verifiquei: <comando/URL/evidencia>
O que ficou de fora: <nada | item + motivo>
Decisoes: <o que foi decidido e por que>
```

Relate o resultado com fidelidade. Teste que falhou e relatado com a saida. Passo que nao foi
executado e declarado. Quando esta pronto e verificado, afirme sem hesitacao.

## Antipadroes

- Terminar com "proximos passos pendentes" sem ter avisado **antes** que o escopo nao caberia.
- Declarar concluido com base em compilacao bem-sucedida, sem exercitar o fluxo.
- Reduzir o escopo por conta propria em vez de entregar tudo e sinalizar o problema.
- Afirmar que algo nao existe sem ter executado uma busca.

## Pronto quando

Todos os itens do checklist estao marcados e existe uma evidencia concreta — comando, resposta
HTTP, captura de tela — de que o fluxo central funciona no ambiente real.
