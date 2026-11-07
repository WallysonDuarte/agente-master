<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=00ff41&height=220&section=header&text=MASTER&fontColor=ffffff&fontSize=72&fontAlignY=38&desc=Full%20Stack%20Developer&descSize=22&descAlignY=60" width="100%" />

<img src="./assets/portrait.png" width="200" alt="Master" />

# ⚡ Master · Full Stack Developer

**Implementa features de ponta a ponta — backend, frontend, banco e publicacao — e so considera a entrega concluida quando ela esta rodando e verificada.**

![Skills](https://img.shields.io/badge/skills-5-00ff41?style=for-the-badge)
![CLIs](https://img.shields.io/badge/CLIs-8-2980b9?style=for-the-badge)
![Idioma](https://img.shields.io/badge/pt--BR-nativo-1abc9c?style=for-the-badge)

</div>

---

## Por que usar este agente

- **Fecha o ciclo inteiro.** Especifica, implementa, compila, publica e verifica — sem devolver o problema pela metade.
- **Recusa entrega falsa.** Tem criterio explicito de pronto; `TODO` e tela vazia nao passam.
- **Seguro por padrao.** Segredo fora do repositorio, nenhuma porta nova, limpeza de diretorio sempre com guarda contra variavel vazia.
- **Economico em contexto.** Busca antes de ler, le por trecho, nao despeja arquivo inteiro no historico.
- **Verifica no ambiente real.** Resposta HTTP, servico ativo e fluxo principal exercitado — nao apenas "compilou".

---

## Skills

| Skill | Titulo | O que resolve |
|---|---|---|
| [`api-backend`](skills/api-backend/SKILL.md) | API backend em camadas | Projetar e implementar API REST com separacao de camadas, validacao, autenticacao e tratamento de erro consistente. |
| [`spa-frontend`](skills/spa-frontend/SKILL.md) | SPA frontend com estado previsivel | Construir interface de aplicacao com rotas protegidas, formularios validados, estados de carregamento e contrato alinhado ao backend. |
| [`publicacao-segura`](skills/publicacao-segura/SKILL.md) | Publicacao segura e verificavel | Publicar em servidor com backup verificado, destino confirmado, preservacao de configuracao e verificacao pos-publicacao com reversao automatica. |
| [`modelagem-dados`](skills/modelagem-dados/SKILL.md) | Modelagem e consulta de dados | Modelar entidades, indices e consultas em banco documental e relacional, com migracao segura e integridade comprovada. |
| [`entrega-sem-pendencia`](skills/entrega-sem-pendencia/SKILL.md) | Criterio de entrega sem pendencia | Fechar a atividade com build limpo, ausencia de placeholder, registro atualizado e evidencia do funcionamento no ambiente real. |

Cada skill e um arquivo markdown autocontido (`skills/<slug>/SKILL.md`): pode ser instalada
isolada, sem o resto do repositorio.

---

## Instalacao rapida

```bash
git clone https://github.com/WallysonDuarte/agente-master.git
cd agente-master

# Claude Code — skills nativas
cp -r skills/* ~/.claude/skills/

# Qualquer outra CLI — arquivo unico de instrucao
cat AGENT.md skills/*/SKILL.md > AGENTS.md
```

| CLI | Instalacao (Windows / PowerShell) | Onde colocar as skills | Rodar |
|---|---|---|---|
| **Claude Code** | `irm https://claude.ai/install.ps1 \| iex` | `~/.claude/skills/` | `claude -p "<pedido>"` |
| **Codex** | `irm https://chatgpt.com/codex/install.ps1 \| iex` | `AGENTS.md na raiz do projeto` | `codex exec --skip-git-repo-check "<pedido>"` |
| **Grok** | `irm https://x.ai/cli/install.ps1 \| iex` | `passado no prompt` | `grok -p "<pedido>"` |
| **Antigravity** | `irm https://antigravity.google/cli/install.ps1 \| iex` | `passado no prompt` | `agy -p "<pedido>"` |
| **Hermes** | `iex (irm https://hermes-agent.nousresearch.com/install.ps1)` | `passado no prompt` | `hermes` |
| **OpenClaw** | `& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard` | `~/.openclaw/skills/` | `openclaw agent --local -m "<pedido>"` |
| **OpenCode** | `irm https://opencode.ai/install \| iex` | `AGENTS.md na raiz do projeto` | `opencode` |
| **Kimi Code** | `irm https://code.kimi.com/kimi-code/install.ps1 \| iex` | `passado no prompt` | `kimi -p "<pedido>"` |

Passo a passo por CLI, incluindo macOS/Linux: **[install/README.md](install/README.md)**

---

## Estrutura

```
agente-master/
├── AGENT.md                 definicao canonica do agente
├── .agent/agent.json        metadados legiveis por maquina
├── skills/
│   ├── api-backend/SKILL.md
│   ├── spa-frontend/SKILL.md
│   ├── publicacao-segura/SKILL.md
│   ├── modelagem-dados/SKILL.md
│   └── entrega-sem-pendencia/SKILL.md
├── install/README.md        instalacao nas 8 CLIs
└── assets/portrait.png
```

---

## Ecossistema

Este agente faz parte de um time de 9 agentes especializados:

[![Master](https://img.shields.io/badge/Master-00ff41?style=flat-square)](https://github.com/WallysonDuarte/agente-master) [![Nova](https://img.shields.io/badge/Nova-9b59b6?style=flat-square)](https://github.com/WallysonDuarte/agente-nova) [![Atlas](https://img.shields.io/badge/Atlas-1abc9c?style=flat-square)](https://github.com/WallysonDuarte/agente-atlas) [![Pixel](https://img.shields.io/badge/Pixel-e91e63?style=flat-square)](https://github.com/WallysonDuarte/agente-pixel) [![Forge](https://img.shields.io/badge/Forge-e67e22?style=flat-square)](https://github.com/WallysonDuarte/agente-forge) [![Cipher](https://img.shields.io/badge/Cipher-c0392b?style=flat-square)](https://github.com/WallysonDuarte/agente-cipher) [![Luna](https://img.shields.io/badge/Luna-f39c12?style=flat-square)](https://github.com/WallysonDuarte/agente-luna) [![Lira](https://img.shields.io/badge/Lira-f0932b?style=flat-square)](https://github.com/WallysonDuarte/agente-lira) [![Rank](https://img.shields.io/badge/Rank-2980b9?style=flat-square)](https://github.com/WallysonDuarte/agente-rank)

[![Hub](https://img.shields.io/badge/ver%20o%20time%20completo-00ff41?style=for-the-badge)](https://github.com/WallysonDuarte/softwarehub-agentes)

---

<div align="center">

## Apoie este trabalho

Os 9 agentes sao mantidos de forma independente e publicados abertamente.
Se este material te poupou tempo, voce pode retribuir com um cafe:

<a href="https://buy.stripe.com/bJefZi3wJ4JH18WeuEgA800">
<img src="https://img.shields.io/badge/Apoie--me-R$%2010-00ff41?style=for-the-badge&logo=stripe&logoColor=white" alt="Apoie-me com R$ 10" />
</a>

## Links

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dududev/)
[![Do Zero ao Junior](https://img.shields.io/badge/Do%20Zero%20ao%20Junior-1abc9c?style=for-the-badge)](https://dozeroaojunior.com.br/)
[![Curitiba Software](https://img.shields.io/badge/Curitiba%20Software-2980b9?style=for-the-badge)](https://curitibasoftware.com.br/)
[![SoftwareHub](https://img.shields.io/badge/SoftwareHub-00ff41?style=for-the-badge)](https://softwarehub.com.br/)


</div>

<img src="https://capsule-render.vercel.app/api?type=waving&color=00ff41&height=100&section=footer" width="100%" />
