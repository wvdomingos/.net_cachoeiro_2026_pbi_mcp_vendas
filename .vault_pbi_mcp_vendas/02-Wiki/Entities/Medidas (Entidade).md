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

## Medidas existentes

| Medida | Expressão | Pasta |
|---|---|---|
| `txtAtualizacao` | `"Atualização: " & MAX(Atualizacao[Data e Hora])` | `0. Aux` |
| `Dummy` | `{0}` | `0. Aux` |

Nenhuma tem `description`.

## Convenções

- `displayFolder` prefixado por número (`0. Aux`, `1. ...`) para ordenar o painel.
- Preencher `description` — alimenta [[documentacao (Entidade)|documentacao]].
- Agregação implícita está desligada nas tabelas (`summarizeBy: none`); toda métrica é medida explícita.

**Pendência do projeto:** medidas de negócio sobre [[fato_vendas (Entidade)|fato_vendas]] e [[metas_mensais (Entidade)|metas_mensais]].
