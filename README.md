# Dashboard de Vendas — Power BI + MCP + Claude Code

Projeto de demonstração apresentado no **.NET Cachoeiro 2026**: um dashboard de vendas em Power BI
mantido inteiramente como **texto** (formato PBIP / Developer Mode) e desenvolvido com apoio de IA via
[Model Context Protocol](https://modelcontextprotocol.io/) e [Claude Code](https://claude.com/claude-code).

A ideia é mostrar, na prática, três coisas:

1. **Power BI como código** — modelo semântico em TMDL e relatório em PBIR, versionáveis no git,
   diffáveis e editáveis fora do Desktop.
2. **IA operando o modelo vivo** — o servidor `powerbi-modeling-mcp` da Microsoft conecta o agente ao
   Power BI Desktop aberto, permitindo criar medidas, relacionamentos e tabelas por conversa.
3. **Contexto como ativo** — `CLAUDE.md`, `DESING.md` e um vault Obsidian guardam as regras do projeto
   para que qualquer agente (ou pessoa) trabalhe com as mesmas convenções.

## Estrutura

```
dados/Base de Dados.xlsx                              # fonte: 5 planilhas (vendas, metas, 3 dimensões)
dashboards/
  .net_cachoeiro_2026_pbi_mcp_vendas.pbip             # abra este arquivo no Power BI Desktop
  .net_cachoeiro_2026_pbi_mcp_vendas.SemanticModel/   # modelo em TMDL (definition/tables/*.tmdl)
  .net_cachoeiro_2026_pbi_mcp_vendas.Report/          # relatório em PBIR (um JSON por visual)
assets/                                               # imagens de capa
CLAUDE.md                                             # guia operacional para o agente de IA
DESING.md                                             # contrato visual: paleta, tipografia, layout
.vault_pbi_mcp_vendas/                                # vault Obsidian (LLM Wiki) com sínteses do projeto
```

## O modelo

Star schema clássico, carregado por Power Query a partir do Excel:

| Tabela | Tipo | Linhas | Período |
|---|---|---|---|
| `fato_vendas` | Fato | 3321 | 2025-01 → 2026-08 |
| `metas_mensais` | Fato | 288 | 2025-01 → 2026-12 |
| `dim_clientes` | Dimensão | 120 | — |
| `dim_produtos` | Dimensão | 36 | — |
| `dim_vendedores` | Dimensão | 12 | — |
| `dim_calendario` | Dimensão | 1095 | gerada em M, com feriados brasileiros |

Detalhes que valem a visita no código:

- **Calendário parametrizado em M** — `dim_calendario.tmdl` tem os parâmetros (`startDate`,
  `yearsAhead`, `monthOfFiscalYearEnd`, `closingMonthStartDay`...) no topo do Power Query.
- **Auto-documentação em DAX** — tabelas calculadas com `INFO.VIEW.TABLES()`, `INFO.VIEW.COLUMNS()`,
  `INFO.VIEW.MEASURES()` e `INFO.VIEW.RELATIONSHIPS()` alimentam uma tabela `documentacao` exibida no
  próprio relatório. A propriedade `description` de cada objeto vira documentação viva.
- **Agregação implícita desligada** — colunas numéricas com `summarizeBy: none`; toda métrica é medida
  explícita na tabela `Medidas`.

## Como rodar

**Pré-requisitos**

- [Power BI Desktop](https://aka.ms/pbidesktop) com a opção *Power BI Project (.pbip)* habilitada em
  *Opções → Recursos de visualização*.
- Excel não é necessário; o Power Query lê o `.xlsx` direto.

**Passos**

1. Clone o repositório.
2. Ajuste o caminho da base: abra
   `dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.SemanticModel/definition/expressions.tmdl` e troque o
   valor do parâmetro `pCaminhoExcel` para o caminho absoluto do seu `dados/Base de Dados.xlsx`.
   É o único lugar que precisa mudar — as 5 consultas consomem esse parâmetro.
3. Abra `dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.pbip` no Power BI Desktop e clique em
   **Atualizar**.

## Como usar com IA (MCP)

O fluxo demonstrado na talk usa o [Claude Code](https://claude.com/claude-code) e o servidor
[`@microsoft/powerbi-modeling-mcp`](https://www.npmjs.com/package/@microsoft/powerbi-modeling-mcp):

```bash
claude mcp add powerbi-modeling-mcp --scope user -- npx -y @microsoft/powerbi-modeling-mcp@latest --start
```

Com o Desktop aberto no projeto, inicie o Claude Code na raiz do repositório. O `CLAUDE.md` já instrui
o agente a conectar na instância local, ler o modelo e aplicar as convenções do projeto. O
`DESING.md` define a paleta e o layout que qualquer visual gerado deve respeitar.

Sem o MCP, o agente edita os arquivos TMDL/PBIR diretamente e você atualiza no Desktop.

> **Regra de ouro:** nunca edite os arquivos TMDL com o projeto aberto no Desktop. Ao salvar, o Desktop
> grava o modelo em memória por cima do disco e descarta sua edição.

## Vault Obsidian

`.vault_pbi_mcp_vendas/` é um vault no padrão *LLM Wiki*: fontes entram em `01-Sources/Raw/`, viram
sínteses, entidades e conceitos em `02-Wiki/`, e tudo fica catalogado em `00-System/index.md`. Comece
por `MAPA.md` ou `Dashboard.md`. Abra a pasta como vault no Obsidian para navegar pelos links.

## Dados

A base é **sintética**, gerada para a demonstração. Clientes, produtos, vendedores e valores não
correspondem a nenhuma empresa real.

## Licença

[MIT](LICENSE).
