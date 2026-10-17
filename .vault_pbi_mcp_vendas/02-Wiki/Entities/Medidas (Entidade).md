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

## Medidas existentes (28, todas com `description`)

| Pasta | Medidas |
|---|---|
| `0. Aux` | `txtAtualizacao`, `Dummy` |
| `1. Vendas` | Receita, Receita Bruta, Desconto, Desconto %, Custo, Margem Bruta, Margem Bruta %, Quantidade Vendida, Pedidos, Pedidos Cancelados, Taxa de Cancelamento %, Ticket Médio, Clientes Ativos |
| `2. Meta` | Meta, Atingimento %, Gap Meta |
| `3. Tempo` | Receita Ano Anterior, Receita Var. Anual %, Receita Mês Anterior, Receita Var. Mensal %, Receita Acumulada Ano, Meta Acumulada Ano, Atingimento Acumulado Ano % |
| `4. HTML` | HTML KPIs Visão Geral, HTML Narrativa Visão Geral, HTML Insight Visão Geral |

Regras: medidas de venda filtram `fato_vendas[Status] = "Faturado"`; Receita = Σ Quantidade × PrecoUnitario × (1 − DescontoPct); variações retornam BLANK sem base ou sem valor atual. As três medidas `HTML *` geram strings com tokens do [[DESING - Contrato Visual]] para o custom visual HTML Content:

| Medida | Conteúdo |
|---|---|
| `HTML KPIs Visão Geral` | 4 cards em uma linha: Receita, Atingimento da meta, Pedidos, Ticket médio; comparação vs. ano anterior; período declarado uma vez |
| `HTML Narrativa Visão Geral` | Resumo executivo completo: insight → contexto (região, vendedor, categoria e canal líderes; margem; cancelamento) → pontos de atenção (menor atingimento, maior queda anual, cancelamentos) → próximos passos derivados dos números, com "Responsável e prazo: não definidos". **Sem visual no relatório; mantida no modelo por decisão do usuário (2026-10-17), não remover como órfã.** |
| `HTML Insight Visão Geral` | Só cabeçalho com período + frase de insight (receita, % da meta com gap/folga, variação anual) |

Guardrails aplicados: frases somem quando falta base (sem meta, sem ano anterior, meta batida); sem receita → "Sem dados para os filtros atuais"; rankings via `TOPN` sobre `VALUES` no contexto dos slicers.

## Convenções

- `displayFolder` prefixado por número (`0. Aux`, `1. ...`) para ordenar o painel.
- Preencher `description` — alimenta [[documentacao (Entidade)|documentacao]].
- Agregação implícita está desligada nas tabelas (`summarizeBy: none`); toda métrica é medida explícita.

Criadas via [[MCP (Conceito)|MCP]] em 2026-10-12 sobre [[fato_vendas (Entidade)|fato_vendas]] e [[metas_mensais (Entidade)|metas_mensais]].
