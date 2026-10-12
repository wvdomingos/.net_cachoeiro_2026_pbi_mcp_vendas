---
title: "Paleta - Dashboard Power BI"
tags:
  - entity
  - design
  - paleta
date: 2026-10-12
status: active
fontes:
  - "[[DESING]]"
---

# Paleta - Dashboard Power BI

Fonte normativa de cores do [[DESING - Contrato Visual]]. Tokens fixos: copiar o HEX literalmente. Nome sem sufixo `(Entidade)` para resolver o wikilink usado em `DESING.md`.

| Token | HEX | Papel | Contraste (sobre `#11162D`) |
|---|---|---|---|
| `surface.page` | `#080B1F` | Fundo sólido do canvas | 1,09:1 vs card |
| `surface.card` | `#11162D` | Cards, gráficos e controles | — |
| `text.primary` | `#F5F7FF` | Títulos, valores, rótulos | 16,69:1 |
| `text.secondary` | `#BFC8E2` | Eixos, legenda, período | 10,69:1 |
| `accent.selection` / `series.1` | `#4E9BFF` | Seleção, contorno, série 1 | 6,34:1 |
| `accent.reference` | `#3CCBFF` | Ícones e referência | 9,50:1 |
| `series.2` | `#B28CFF` | Série 2 | 6,87:1 |
| `series.3` | `#F26CDB` | Série 3 | 6,75:1 |
| `series.4` / `state.favorable` | `#50D6B2` | Série 4; favorável rotulado | 9,86:1 |
| `series.5` / `state.attention` | `#FFC66D` | Série 5; atenção rotulada | 11,52:1 |
| `series.6` / `state.unfavorable` | `#FF7D92` | Série 6; desfavorável rotulado | 7,31:1 |

## Regras

- Metas: 4,5:1 para texto comum, 3:1 para gráficos e controles.
- Card/página só 1,09:1 — delimitar cards com espaço, título e agrupamento, ou contorno azul 2 px.
- Texto branco sobre preenchimento azul **não validado**; botões ficam escuros com contorno azul.
- Pares não comprovam diferenciação entre séries nem adequação a daltonismo.

## Onde está aplicada

Tema custom do [[Relatorio PBIR]] (`Custom3444442517655203.json`): `dataColors[0..6]` = `#4E9BFF`, `#3CCBFF`, `#B28CFF`, `#F26CDB`, `#50D6B2`, `#FFC66D`, `#FF7D92`, seguido de `#BFC8E2`.
