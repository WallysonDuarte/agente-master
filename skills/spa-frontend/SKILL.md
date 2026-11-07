---
name: spa-frontend
description: Construir interface de aplicacao com rotas protegidas, formularios validados, estados de carregamento e contrato alinhado ao backend.
---

# SPA frontend com estado previsivel

## Quando usar

Tela nova, formulario, listagem, autenticacao no cliente, integracao de uma rota de API na
interface.

## Fundamentos

- **Componentes isolados e tipados.** Entrada e saida explicitas; nada de estado global implicito.
- **Carregamento tardio por rota.** Primeiro acesso nao baixa o que o usuario ainda nao abriu.
- **Estado reativo.** Atualize por sinal/observavel e nao force redesenho manual; quando o
  framework exigir, dispare a deteccao de mudanca depois de callbacks assincronos.
- **Nada de dialogo nativo do navegador** em fluxo de producao: use componente proprio, acessivel
  e automatizavel.

## Contrato com o backend — auditoria obrigatoria

A maior fonte de bug silencioso e divergencia de nome de campo. Antes de declarar a tela pronta:

1. Compare o corpo **enviado** pelo formulario com o documento/linha efetivamente **gravada**.
2. Campo que o formulario envia e a API descarta e defeito, nao detalhe.
3. Campo opcional vazio deve ir ausente, nao como texto vazio — validador de e-mail recusa texto
   vazio.
4. Interceptador que desembrulha a resposta muda o formato para todos os consumidores: se existe,
   ninguem deve ler `resposta.data` de novo.

## Rotas protegidas

- Guarda **assincrona**: tenta renovar a sessao antes de redirecionar para o login. Guarda que
  decide de forma sincrona pela expiracao lida no cliente torna a renovacao de token codigo morto.
- Uma unica renovacao em voo compartilhada entre guardas paralelas.
- Resposta 401 encerra a sessao; resposta 403 mostra mensagem de acesso negado e **nao** desloga —
  tratar 403 como 401 transforma "sem permissao" em "nao consigo entrar".
- Pagina de login com sessao valida redireciona para dentro do app, mas somente se a sessao
  satisfaz o predicado real de autorizacao, nao apenas "existe usuario".

## Formularios

- Rotulo associado a cada campo (`label` com referencia ao `id`): sem isso o campo nao tem nome
  acessivel e nao e alcancavel por teste automatizado.
- Mascara de documento, telefone e CEP aplicada na exibicao **e** o valor normalizado no envio.
- Mensagem de erro ao lado do campo, nao apenas um aviso flutuante.
- Campo de senha com alternancia mostrar/ocultar.
- Botao desabilitado sempre acompanhado do motivo visivel. Botao morto sem explicacao e defeito.
- Validacao no cliente antes de gastar tentativa em endpoint com limite de tentativas.

## Estados de tela

Toda tela que carrega dados precisa de cinco estados desenhados: carregando, vazio, com dados,
sem resultado para o filtro, e erro. Erro mostra o que falhou e oferece nova tentativa.

## Armadilhas frequentes

- Imagem servida por endpoint que devolve 404 quando nao existe foto: o navegador desenha o icone
  de imagem quebrada. Trate o evento de erro e caia no marcador padrao — e aplique em **todas** as
  telas que renderizam a mesma fonte de imagem.
- Busca com atraso de digitacao precisa de contador de sequencia: sem isso, resposta antiga
  sobrescreve a nova.
- Abrir nova aba depois de `await` e bloqueado como popup: abra a aba de forma sincrona no clique e
  navegue depois.
- Arquivo de configuracao de ambiente editado localmente e publicado por engano aponta a aplicacao
  para o endereco de desenvolvimento. O arquivo versionado costuma estar certo — o pacote e que esta
  errado.
- Pagina principal deve ser servida sem cache; apenas os arquivos com hash no nome podem ser
  imutaveis. Sem isso o usuario continua vendo a versao antiga depois de publicar.

## Checklist

- [ ] Compila sem erro e sem aviso novo
- [ ] Contrato conferido campo a campo contra o que foi gravado
- [ ] Guarda assincrona com renovacao de sessao; 403 nao desloga
- [ ] Rotulo acessivel em todos os campos
- [ ] Cinco estados de tela desenhados
- [ ] Responsivo em 375 px, 768 px e 1440 px, sem rolagem horizontal
- [ ] Nenhum erro no console (aviso e tolerado, erro nao)
- [ ] Textos no idioma do usuario, com acentuacao correta

## Pronto quando

A tela faz o fluxo completo no ambiente publicado, os dados chegam gravados corretamente, e
nenhum campo enviado e descartado em silencio.
