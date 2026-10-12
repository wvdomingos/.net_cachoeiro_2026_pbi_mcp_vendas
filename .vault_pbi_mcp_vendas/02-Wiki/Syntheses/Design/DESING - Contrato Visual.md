---
title: "DESING - Contrato Visual do Dashboard"
tags:
  - synthesis
  - design
  - acessibilidade
  - power-bi
date: 2026-10-12
status: active
fontes:
  - "[[DESING]]"
---

# DESING - Contrato Visual do Dashboard

Síntese de `DESING.md` (grafia com typo, preservada no projeto). Contrato normativo para orientar a IA na construção de páginas e visuais do relatório [[Power BI (Entidade)|Power BI]]. Paleta detalhada em [[Paleta - Dashboard Power BI]]. Integração via [[MCP (Conceito)]] respeita os mesmos tokens.

> [!WARNING]
> O contrato orienta; não comprova acessibilidade do relatório final. Índices de contraste valem só para os pares sólidos listados na paleta.

## Atmosfera

- Executivo escuro: navy quase preto, superfícies sólidas, destaques azul/ciano/violeta.
- Página e cards com opacidade 100%; sem gradiente, sombra ou transparência nos componentes.
- Imagem decorativa (`capa_1_fundo.png`) só fora das áreas de leitura; não derivar cores dela.
- Hierarquia de leitura: **insight → contexto → ação**.

## Tipografia (Segoe UI)

| Elemento | Tamanho | Peso | Cor |
|---|---|---|---|
| Título da página | 26 pt | Semibold | `text.primary` |
| Insight principal | 20 pt | Semibold | `text.primary` |
| Valor KPI | 32 pt | Semibold | `text.primary` |
| Título de card/gráfico | 16 pt | Semibold | `text.primary` |
| Comparação, filtros, botões | 14 pt | Regular | `text.primary` |
| Eixos, legenda, período | 12 pt | Regular | `text.secondary` |

Mínimo 12 pt. Alinhamento à esquerda; sem caixa alta integral; unidades explícitas.

## Componentes

- **Card KPI**: fundo `surface.card`, padding 24 px, raio 8 px. Ordem: título → valor → comparação → período → estado. Sem comparação definida: "Comparação não definida". Crescimento não é automaticamente favorável.
- **Filtros/botões**: altura mínima 48 px, fundo escuro sólido, contorno azul 2 px para foco/ação principal. "Limpar filtros" com rótulo textual. Desabilitado = "Indisponível" + motivo.
- **Gráficos**: máximo 6 séries na ordem `series.1`…`series.6`; cor por entidade preservada entre visuais; referência em ciano com rótulo; estados com ícone + texto ("Favorável", "Atenção", "Desfavorável", "Crítico"). Vazio: "Sem dados para os filtros atuais". Nunca substituir ausente por zero.

## Layout

- Canvas 16:9, 1920 × 1080 recomendado. Base 8 px; escala 8/16/24/32/48.
- Margem externa 48 px, grid 12 colunas, gutter 24 px → coluna 130 px; blocos 3/4/6 colunas = 438/592/900 px.
- Cabeçalho até 120 px; até 4 KPIs de 3 colunas (160–200 px de altura); gráficos em blocos de 6 colunas.

## Guardrails para IA

- HEX, ordem e papéis da paleta são fixos; nenhuma cor, fonte ou efeito novo.
- Não inventar dados, metas, períodos, comparações, responsáveis ou recomendações. Definição ausente → pendência registrada.
- Propriedade inexistente no Power BI → registrar limitação, não alegar aplicação.
- Revalidar contraste após mudança de fundo, tema, exportação ou publicação.

## Relação com o relatório atual

O tema custom do relatório ([[Relatorio PBIR]]) já carrega os 7 primeiros `dataColors` iguais à paleta. Páginas em 1920×1080 conforme o contrato.

Referências: WCAG 1.4.3 (texto), 1.4.11 (não textual), 1.4.1 (uso de cor).
