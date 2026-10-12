---
title: "Medidas"
tags:
  - entity
  - modelo
  - dax
date: 2026-10-12
status: active
fontes:
  - "[[CLAUDE]]"
---

# Medidas (Entidade)

Tabela vazia do [[Modelo Semantico]] que serve só de **container de medidas**. Grupo `Aux`, tabela **visível** (só a coluna `Coluna 1` é oculta). Toda medida nova vai aqui.

## Medidas existentes (26, todas com `description`)

| Pasta | Medidas |
|---|---|
| `0. Aux` | `txtAtualizacao`, `Dummy` |
| `1. Vendas` | Receita, Receita Bruta, Desconto, Desconto %, Custo, Margem Bruta, Margem Bruta %, Quantidade Vendida, Pedidos, Pedidos Cancelados, Taxa de Cancelamento %, Ticket Médio, Clientes Ativos |
| `2. Meta` | Meta, Atingimento %, Gap Meta |
| `3. Tempo` | Receita Ano Anterior, Receita Var. Anual %, Receita Mês Anterior, Receita Var. Mensal %, Receita Acumulada Ano, Meta Acumulada Ano, Atingimento Acumulado Ano % |
| `4. HTML` | HTML KPIs Visão Geral |

Regras: medidas de venda filtram `fato_vendas[Status] = "Faturado"`; Receita = Σ Quantidade × PrecoUnitario × (1 − DescontoPct); variações retornam BLANK sem base ou sem valor atual. `HTML KPIs Visão Geral` gera 4 cards (Receita, Atingimento, Pedidos, Ticket) com tokens do [[DESING - Contrato Visual]] para o custom visual HTML Content.

## Convenções

- `displayFolder` prefixado por número (`0. Aux`, `1. ...`) para ordenar o painel.
- Preencher `description` — alimenta [[documentacao (Entidade)|documentacao]].
- Agregação implícita está desligada nas tabelas (`summarizeBy: none`); toda métrica é medida explícita.

Criadas via [[MCP (Conceito)|MCP]] em 2026-10-12 sobre [[fato_vendas (Entidade)|fato_vendas]] e [[metas_mensais (Entidade)|metas_mensais]].
