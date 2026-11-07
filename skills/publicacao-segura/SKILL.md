---
name: publicacao-segura
description: Publicar em servidor com backup verificado, destino confirmado, preservacao de configuracao e verificacao pos-publicacao com reversao automatica.
---

# Publicacao segura e verificavel

## Quando usar

Qualquer envio de codigo para um ambiente que nao e a maquina de desenvolvimento.

## Regra zero: confirmar o destino

Antes de tocar em qualquer diretorio, leia a configuracao do servidor web e descubra **qual
diretorio aquele nome de dominio realmente serve**. Nunca deduza pelo nome da pasta.

Um pacote corresponde a exatamente um destino. O mesmo pacote enviado para dois diretorios
diferentes e sinal de erro — pare e confirme.

Componentes distintos vivem em diretorios distintos: site institucional, aplicacao, painel
administrativo, API e processos em segundo plano nunca compartilham destino.

## Sequencia obrigatoria

```
1. construir            -> artefato fora da pasta de codigo-fonte
2. validar artefato     -> existe, tem tamanho > 0, contem o arquivo de entrada esperado
3. enviar               -> transferir e CONFERIR que chegou
4. backup do destino    -> copia completa verificada; se falhar, ABORTAR
5. limpar destino       -> preservando configuracao, variaveis de ambiente, cofre e logs
6. extrair
7. iniciar servico
8. verificar            -> resposta HTTP, servico ativo, conteudo correto
9. reverter se falhar   -> restaurar o backup automaticamente
```

Pontos onde a publicacao costuma destruir o ambiente:

- **Backup encadeado com `;` em vez de `&&`.** Se a copia falha por permissao, o roteiro continua
  e apaga o destino sem copia de volta. Use `&&` ou interrupcao imediata em erro, e aborte quando
  o backup nao existir.
- **Limpeza com caminho vindo de variavel.** Variavel vazia transforma a limpeza em "apague o
  diretorio atual". Sempre valide que a variavel nao esta vazia e que o diretorio existe antes.
- **Arquivos que precisam sobreviver.** Antes de limpar, leia a definicao do servico e liste todo
  arquivo de ambiente que ele carrega. Cada um precisa de exclusao explicita na limpeza. Diretorio
  de cofre, arquivo de configuracao e logs tambem.
- **Pasta de dados do usuario dentro da raiz publicada** (uploads, capas, anexos): publicacao deve
  ser aditiva ou excluir essas pastas da limpeza.
- **Verificacao colocada entre a extracao e a partida do servico.** Se ela falhar, o servico fica
  parado — ou seja, a verificacao virou a causa da indisponibilidade. Verifique depois de iniciar.

## Configuracao nunca vai no pacote

O arquivo de configuracao gerado pela construcao local carrega valores de desenvolvimento. Publicar
esse arquivo sobrescreve a configuracao real do ambiente — e o sintoma aparece dias depois.

Solucao definitiva: **excluir o arquivo de configuracao do pacote** e reaplicar no servidor apenas
os campos estaticos conhecidos. Nunca confiar em "o servidor ja tem o arquivo certo" sem uma
guarda no roteiro.

## Verificacao pos-publicacao

| O que | Como | Esperado |
|---|---|---|
| Conteudo | listar o destino | arquivos do tipo esperado, nada de codigo-fonte |
| Resposta | requisicao ao dominio | 200 |
| Servico | estado no gerenciador de servicos | ativo |
| Versao | comparar hash do arquivo principal local e servido | identico |

Resposta 200 e processo ativo **nao** provam que a versao certa subiu. Compare o hash.

Servico que le segredos de um cofre externo pode demorar para responder na primeira partida: a
verificacao precisa de novas tentativas por algumas dezenas de segundos, senao a reversao
automatica desfaz uma publicacao correta.

## Limpeza

Ao terminar: remover pacotes temporarios do servidor, remover diretorios de publicacao aninhados
e apagar backups antigos **depois** de confirmar estabilidade. Guarde o backup anterior a mudanca
enquanto ela nao for aceita — e o unico ponto de retorno ao estado anterior.

## Checklist

- [ ] Destino confirmado na configuracao do servidor web
- [ ] Artefato construido fora da pasta de codigo-fonte e validado
- [ ] Backup criado e verificado antes de qualquer operacao destrutiva
- [ ] Limpeza com caminho validado e exclusoes explicitas
- [ ] Configuracao do ambiente preservada
- [ ] Resposta HTTP, estado do servico e hash conferidos
- [ ] Reversao testada ou disponivel
- [ ] Temporarios removidos

## Pronto quando

O dominio responde, o servico esta ativo, o hash servido e igual ao construido e existe um backup
do estado anterior.
