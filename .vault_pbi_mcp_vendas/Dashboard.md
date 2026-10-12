---
title: "Dashboard"
tags:
  - dashboard
  - hub
date: 2026-10-12
status: active
---

# Dashboard — Vault PBI MCP Vendas

Hub de navegação do conhecimento do projeto **.net_cachoeiro_2026_pbi_mcp_vendas** (dashboard de vendas em Power BI, formato PBIP, modelo TMDL + relatório PBIR).

> [!INFO]
> Estrutura e fluxo de trabalho do vault: [[MAPA]]. Regras de ingestão: [[ingestion-protocol]].

## Sistema

- [[MAPA]] — mapa do vault
- [[index]] — catálogo de conteúdo
- [[log]] — log de operações
- [[ingestion-protocol]] — protocolo de ingestão

## Sínteses

### Modelo
- [[Modelo Semantico]] — star schema, 4 categorias de tabela, convenções, armadilhas

### Relatorio
- [[Relatorio PBIR]] — 3 páginas 1920×1080, tema custom, estrutura de arquivos

### Design
- [[DESING - Contrato Visual]] — atmosfera, tipografia, componentes, layout, guardrails para IA

### Dados
- [[Fonte de Dados]] — Excel com 5 planilhas, M de carga, `pCaminhoExcel`

### Ferramental
- [[Ferramental MCP e Skills]] — skills, MCP `powerbi-modeling-mcp`, fallbacks TMDL/ADOMD

## Entidades

- [[fato_vendas (Entidade)]]
- [[metas_mensais (Entidade)]]
- [[dim_calendario (Entidade)]]
- [[Medidas (Entidade)]]
- [[documentacao (Entidade)]]
- [[Power BI (Entidade)]]
- [[Paleta - Dashboard Power BI]]

## Conceitos

- [[MCP (Conceito)]]
- [[PBIP (Conceito)]]
- [[TMDL (Conceito)]]
- [[PBIR (Conceito)]]
- [[Star Schema (Conceito)]]

## Fontes arquivadas

- [[CLAUDE]] — guia operacional para Claude Code
- [[DESING]] — contrato visual

## Estatísticas

| Métrica | Valor |
|---|---|
| Total de Sínteses | 5 |
| Total de Entidades | 7 |
| Total de Conceitos | 5 |
| Fontes processadas | 2 |
| Último ingest | 2026-10-12 |
