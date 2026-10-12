---
title: "Relatório (PBIR)"
tags:
  - synthesis
  - relatorio
  - pbir
date: 2026-10-12
status: active
fontes:
  - "[[CLAUDE]]"
---

# Relatório (PBIR)

Síntese de `CLAUDE.md`, seção "Relatório (PBIR)". Relatório em formato [[PBIR (Conceito)|PBIR]] em `dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.Report/`.

## Estado

- 3 páginas: `Capa`, `Subcapa`, `Overview` (ativa). Cada uma com um único `cardVisual` mostrando `txtAtualizacao`.
- Páginas em **1920×1080**.
- Tema base `Fluent2-CY26SU09` + tema custom `StaticResources/RegisteredResources/Custom3444442517655203.json`.
- Imagens de `assets/` registradas em `RegisteredResources/` (sufixo numérico) e usadas como fundo em `page.json`.

> [!INFO]
> Os 7 primeiros `dataColors` do tema custom são exatamente `series.1`…`series.6` + `text.secondary` da [[Paleta - Dashboard Power BI]]. Mudar cor de série = mudar o tema, não o visual.

## Estrutura de arquivos

- Um visual = `definition/pages/<pageId>/visuals/<visualId>/visual.json`.
- Ordem e página ativa em `pages.json`.
- IDs hexadecimais opacos — localizar por `displayName` e `visualType`.
- `definition.pbir` aponta ao modelo por caminho relativo (`../.net_cachoeiro_2026_pbi_mcp_vendas.SemanticModel`). Renomear/mover pastas exige atualizar `.pbip`, `definition.pbir` e `displayName` nos dois `.platform`.

## Regras ao gerar PBIR

Ler [[DESING - Contrato Visual]] antes. Resumo:
- Sem cor, gradiente, fonte ou efeito fora do contrato; máximo 6 séries.
- Opacidade 100% em página e cards.
- Não inventar dados ou comparações.
- Nunca afirmar "acessibilidade garantida".

Modelo que o relatório consome: [[Modelo Semantico]].
