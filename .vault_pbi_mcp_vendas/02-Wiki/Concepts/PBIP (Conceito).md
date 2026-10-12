---
title: "PBIP"
tags:
  - concept
  - power-bi
  - developer-mode
date: 2026-10-12
status: active
fontes:
  - "[[CLAUDE]]"
---

# PBIP (Conceito)

**Power BI Project** — formato de projeto do [[Power BI (Entidade)|Power BI]] Developer Mode. Em vez de um `.pbix` binário, uma pasta com arquivos de texto: modelo em [[TMDL (Conceito)|TMDL]] (`*.SemanticModel/`) e relatório em [[PBIR (Conceito)|PBIR]] (`*.Report/`), ligados por `definition.pbir` e pelo `.pbip`.

## Neste projeto

```
dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.pbip
dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.SemanticModel/
dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.Report/
```

- Renomear/mover pastas exige atualizar `.pbip`, `definition.pbir` e `displayName` nos dois `.platform`.
- `.pbi/localSettings.json` e `.pbi/cache.abf` são artefatos locais (ignorados em `.gitignore`).
- Projeto não é repositório git.

Ver [[Modelo Semantico]], [[Relatorio PBIR]].
