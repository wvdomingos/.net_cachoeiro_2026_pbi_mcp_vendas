---
title: "Repositório GitHub"
tags:
  - synthesis
  - ferramental
  - git
  - github
date: 2026-10-12
status: active
fontes:
  - "[[README]]"
  - "[[CLAUDE]]"
---

# Repositório GitHub

Síntese de `README.md` e da seção de versionamento de `CLAUDE.md`. Projeto versionado desde 2026-10-12.

## Remoto

- URL: `https://github.com/wvdomingos/.net_cachoeiro_2026_pbi_mcp_vendas.git`
- Visibilidade: **público** — repositório de demonstração para a comunidade (.NET Cachoeiro 2026).
- Branch: `main`, rastreando `origin/main`.
- Licença: **MIT** (`LICENSE`, copyright 2026 Wander Vilhalva Domingos).

## O que é versionado

PBIP completo (TMDL + PBIR + temas + imagens), `dados/Base de Dados.xlsx` (dados **sintéticos**), `assets/`, slides da talk em `docs/apresentacao/` (PDF), `CLAUDE.md`, `DESING.md`, `README.md`, `LICENSE` e este vault (menos estado local do Obsidian).

## `.gitignore` da raiz

| Padrão | Motivo |
|---|---|
| `**/.pbi/localSettings.json`, `**/.pbi/cache.abf` | artefatos locais do Power BI Desktop |
| `*.pbix`, `*.pbit` | binários; a fonte de verdade é o [[PBIP (Conceito)\|PBIP]] |
| `.vault_pbi_mcp_vendas/.obsidian/workspace*.json`, `.trash/` | estado local do Obsidian |
| `.claude/settings.local.json` | configuração local do Claude Code |
| `~$*`, `*.tmp`, `.vscode/`, `.idea/`, `.vs/`, `Thumbs.db`, `desktop.ini`, `.DS_Store` | lixo de Office, editores e SO |

## README (o que a comunidade vê)

Três mensagens da demo:
1. **Power BI como código** — TMDL/PBIR versionáveis e diffáveis ([[Modelo Semantico]], [[Relatorio PBIR]]).
2. **IA operando o modelo vivo** — `powerbi-modeling-mcp` ([[MCP (Conceito)]], [[Ferramental MCP e Skills]]).
3. **Contexto como ativo** — `CLAUDE.md`, `DESING.md` ([[DESING - Contrato Visual]]) e este vault.

Passos para rodar: clonar → ajustar `pCaminhoExcel` em `expressions.tmdl` ([[Fonte de Dados]]) → abrir o `.pbip` → Atualizar.

> [!WARNING]
> `pCaminhoExcel` guarda caminho absoluto da máquina de origem; quem clona precisa trocar.
