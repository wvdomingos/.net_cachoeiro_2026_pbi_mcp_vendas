---
title: "Índice do Vault"
tags:
  - system
  - index
date: 2026-10-12
status: active
---

# Índice do Vault

Catálogo geral de conteúdo. Atualizado a cada ingest.

## Sínteses

| Síntese | Categoria | Fonte | Data |
|---|---|---|---|
| [[Modelo Semantico]] | Modelo | [[CLAUDE]] | 2026-10-12 |
| [[Relatorio PBIR]] | Relatorio | [[CLAUDE]] | 2026-10-12 |
| [[DESING - Contrato Visual]] | Design | [[DESING]] | 2026-10-12 |
| [[Fonte de Dados]] | Dados | [[CLAUDE]] | 2026-10-12 |
| [[Ferramental MCP e Skills]] | Ferramental | [[CLAUDE]] | 2026-10-12 |

## Entidades

| Entidade | Descrição |
|---|---|
| [[fato_vendas (Entidade)]] | Fato principal, 3321 linhas, 2025-01 → 2026-08 |
| [[metas_mensais (Entidade)]] | Fato de metas, grão mês × vendedor, liga pelo dia 1 |
| [[dim_calendario (Entidade)]] | Calendário gerado em M, parâmetros no topo do M |
| [[Medidas (Entidade)]] | Container de medidas (só `txtAtualizacao` e `Dummy`) |
| [[documentacao (Entidade)]] | Auto-documentação via `INFO.VIEW.*()` |
| [[Power BI (Entidade)]] | Plataforma; Desktop em Developer Mode |
| [[Paleta - Dashboard Power BI]] | Tokens de cor normativos (HEX fixo) |

## Conceitos

| Conceito | Descrição |
|---|---|
| [[MCP (Conceito)]] | Model Context Protocol; servidor `powerbi-modeling-mcp` |
| [[PBIP (Conceito)]] | Power BI Project, formato texto do Developer Mode |
| [[TMDL (Conceito)]] | Linguagem de definição do modelo tabular |
| [[PBIR (Conceito)]] | Formato JSON do relatório |
| [[Star Schema (Conceito)]] | Modelagem fato × dimensão |

## Estatísticas

| Métrica | Valor |
|---|---|
| Total de Sínteses | 5 |
| Total de Entidades | 7 |
| Total de Conceitos | 5 |
| Fontes processadas | 2 |
| Último ingest | 2026-10-12 |
