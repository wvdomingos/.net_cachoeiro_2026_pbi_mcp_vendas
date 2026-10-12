# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## O que é este repositório

Projeto de demonstração (talk .NET Cachoeiro 2026) de um dashboard de **vendas** em Power BI mantido no
formato **PBIP / Developer Mode** — modelo em TMDL e relatório em PBIR, tudo texto plano e editável fora
do Power BI Desktop.

```
dados/Base de Dados.xlsx                                  # fonte de dados (5 planilhas)
dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.pbip        # ponto de entrada (abre no Power BI Desktop)
dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.SemanticModel/   # modelo semântico (TMDL)
dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.Report/          # relatório (PBIR, um JSON por visual)
DESING.md                                                 # contrato visual (paleta, tipografia, layout)
assets/                                                   # capa_1.jpg, capa_1_fundo.png, capa_2.jpg
docs/                                                     # vazio
.vault_pbi_mcp_vendas/                                    # vault Obsidian, só .obsidian/ (sem notas)
```

Não há build, lint ou testes. O ciclo é: editar TMDL/PBIR → abrir o `.pbip` no Power BI Desktop →
atualizar. Repositório git em `main`, remoto `origin` =
`https://github.com/wvdomingos/.net_cachoeiro_2026_pbi_mcp_vendas.git` (público). O `.gitignore`
da raiz exclui `.pbi/localSettings.json`, `.pbi/cache.abf` e `.obsidian/workspace.json`.

## Estado atual

Star schema montado e carregado. As 5 abas do Excel viraram tabelas via Power Query, e os 6
relacionamentos estão ativos, `*:1`, `OneDirection`, sem nenhuma linha órfã.

| Tabela | Grupo | Linhas | Período | Origem |
|---|---|---|---|---|
| `fato_vendas` | Fato | 3321 | 2025-01-01 → 2026-08-31 | aba `fato_vendas` |
| `metas_mensais` | Fato | 288 | 2025-01-01 → 2026-12-01 | aba `metas_mensais` |
| `dim_clientes` | Dimensão | 120 | — | aba `dim_clientes` |
| `dim_produtos` | Dimensão | 36 | — | aba `dim_produtos` |
| `dim_vendedores` | Dimensão | 12 | — | aba `dim_vendedores` |
| `dim_calendario` | Dimensão | 1095 | 2025-01-01 → 2027-12-31 | gerada em M |

```
fato_vendas[Data]          → dim_calendario[Data]
fato_vendas[ClienteID]     → dim_clientes[ClienteID]
fato_vendas[ProdutoID]     → dim_produtos[ProdutoID]
fato_vendas[VendedorID]    → dim_vendedores[VendedorID]
metas_mensais[DataMes]     → dim_calendario[Data]
metas_mensais[VendedorID]  → dim_vendedores[VendedorID]
```

**O que falta:** as medidas (só existem `txtAtualizacao` e `Dummy`, ambas em `0. Aux`) e o conteúdo
do relatório. Hoje são 3 páginas (`Capa`, `Subcapa`, `Overview`), cada uma com um único `cardVisual`
mostrando `txtAtualizacao`. Página ativa: `Overview`.

Dois limites conhecidos do desenho atual, nenhum deles um defeito a corrigir às cegas:

- `metas_mensais` tem grão mês × vendedor e liga ao calendário pelo **dia 1** de cada mês. Total por
  mês, trimestre e ano funciona; filtrar um dia específico, ou um intervalo que não inclua o dia 1,
  devolve meta vazia. Foi a alternativa escolhida no lugar de `TREATAS`. Se algum visual precisar de
  meta em grão diário, é aqui que quebra.
- `dim_calendario` começa em 2025-01-01 e vai até o fim do ano seguinte ao atual — `endDate` é
  calculado de `DateTime.LocalNow()` com `yearsAhead = 1`, então **o limite superior anda sozinho a
  cada virada de ano** e a contagem de linhas acima envelhece junto (em 2026: 1095 linhas até
  2027-12-31). Só existe dado de 2025-01 a 2026-12, então eixos de data e slicers mostram período
  vazio à direita se não forem filtrados.

## Arquitetura do modelo semântico

Tabelas em `dashboards/.net_cachoeiro_2026_pbi_mcp_vendas.SemanticModel/definition/tables/`, todas
`mode: import`. Elas se dividem em quatro categorias com propósitos bem distintos:

**1. Tabelas vindas do Excel (grupos `Fato` e `Dimensão`)** — `fato_vendas`, `metas_mensais`,
`dim_clientes`, `dim_produtos`, `dim_vendedores`. Todas seguem o mesmo M, mudando só o `Item=`:

```m
let
    Fonte = Excel.Workbook(File.Contents(pCaminhoExcel), null, true),
    Planilha = Fonte{[Item="fato_vendas",Kind="Sheet"]}[Data],
    CabecalhoPromovido = Table.PromoteHeaders(Planilha, [PromoteAllScalars=true]),
    TipoAlterado = Table.TransformColumnTypes(CabecalhoPromovido,{...})
in
    TipoAlterado
```

O caminho do arquivo vem do parâmetro `pCaminhoExcel`, declarado em `definition/expressions.tmdl`
(grupo `Aux`) e referenciado por `ref expression` em `model.tmdl`. Valor atual:
`D:\Projetos\.net_cachoeiro_2026_pbi_mcp_vendas\dados\Base de Dados.xlsx` — caminho absoluto, então
**ao mover a pasta do projeto ou a base, altere só o parâmetro**; as 5 consultas o consomem.

**2. Dimensão gerada em M (grupo `Dimensão`)** — não depende de fonte externa:
- `dim_calendario` — gerador de calendário parametrizado no topo do próprio Power Query (~linha 1178
  do `.tmdl`: `startDate = #date(2025,1,1)`, `yearsAhead = 1`, `startOfWeek = Day.Monday`,
  `monthOfFiscalYearEnd = 3`, `closingMonthStartDay = 16`, `language = "pt-BR"`). Inclui feriados
  brasileiros fixos e móveis, dias úteis, estações, quinzena, bimestre, semestre, ano fiscal.
  Tem `dataCategory: Time` e ~14 displayFolders. **Ajustar o calendário = mexer nesses parâmetros**,
  não nas ~1160 linhas de definição de coluna acima deles.

**3. Tabelas auxiliares (grupo `Aux`, ambas `isHidden`)**:
- `Atualizacao` — timestamp da última atualização, via `DateTime.LocalNow()` com
  `DateTimeZone.SwitchZone(...,-3)` para horário de Brasília.
- `Medidas` — tabela vazia que serve só de **container de medidas**. Toda medida nova vai aqui.

**4. Camada de auto-documentação (tabelas calculadas DAX)** — este é o traço mais incomum do modelo:
- `Meta_Tables`, `Meta_Columns`, `Meta_Measures`, `Meta_Relationships` = `INFO.VIEW.TABLES()`,
  `INFO.VIEW.COLUMNS()`, `INFO.VIEW.MEASURES()`, `INFO.VIEW.RELATIONSHIPS()`.
- `documentacao` = `UNION` das medidas com as colunas visíveis (excluindo a tabela `Data Dictionary`),
  produzindo Type / Name / Description / Location / Expression.

Consequência prática: **a propriedade `description` de medidas e colunas não é decorativa** — ela
alimenta a tabela `documentacao`, que é exibida no relatório. Ao criar medidas ou colunas visíveis,
preencha `description`. Hoje as tabelas do Excel têm descrição em todas as colunas; as duas medidas
existentes e `dim_calendario` não têm nenhuma.

## Convenções

- Cultura `pt-BR` em todo o modelo; nomes de objetos em português.
- Dimensões em `snake_case` com prefixo `dim_` (`dim_calendario`, `dim_clientes`); fatos devem seguir
  `fato_` (as planilhas de origem já usam essa convenção).
- Toda tabela nova precisa de `queryGroup:` — `Aux` (ordem 0), `Fato` (1) ou `Dimensão` (2),
  declarados em `model.tmdl`.
- Medidas ficam na tabela `Medidas`, com `displayFolder` prefixado por número (`0. Aux`, e assim por
  diante) para controlar a ordenação no painel.
- `model.tmdl` mantém a lista `ref table ...`; adicionar um `.tmdl` novo em `tables/` **sem** o `ref`
  correspondente faz a tabela ser ignorada.
- `compatibilityLevel: 1606`; `__PBI_TimeIntelligenceEnabled = 0` (sem hierarquias de data automáticas).
- Colunas numéricas de fato e dimensão usam `summarizeBy: none`, de propósito: mata a agregação
  implícita e obriga medida explícita. Arrastar `Quantidade` para um visual não soma nada — é assim
  que deve ser. Não "conserte" isso trocando para `sum`.
- Descrições em TMDL são comentários `///` na linha acima do objeto, não uma propriedade indentada.
- Nomes de coluna de `dim_calendario` são em português (`MesAnoNome`, `MesAnoNumero`, `DiasParaHoje`,
  `AnoAtual`...), não os nomes em inglês do M que os gera.

## Relatório (PBIR)

- `definition.pbir` aponta para o modelo por caminho relativo
  (`../.net_cachoeiro_2026_pbi_mcp_vendas.SemanticModel`). Renomear ou mover qualquer uma das duas
  pastas quebra o vínculo — é preciso atualizar também o `.pbip`, `definition.pbir` e o `displayName`
  em ambos os `.platform`.
- Um visual = um arquivo `definition/pages/<pageId>/visuals/<visualId>/visual.json`; a ordem e a página
  ativa vivem em `pages.json`. IDs são hexadecimais opacos — localize páginas/visuais por `displayName`
  e `visualType`, não pelo nome do diretório.
- Páginas em **1920×1080**. Tema base `Fluent2-CY26SU09` + tema custom
  `StaticResources/RegisteredResources/Custom3444442517655203.json`, cujos 7 primeiros `dataColors`
  são exatamente a paleta `series.1`…`series.6` + `text.secondary` do `DESING.md`. Mudar cor de
  série = mudar o tema, não o visual.
- As imagens de `assets/` já estão registradas em `StaticResources/RegisteredResources/` (com sufixo
  numérico) e usadas como fundo de página em `page.json`.

## Contrato visual: `DESING.md`

`DESING.md` (grafia com typo, preservar) é o **contrato normativo** para qualquer trabalho no
relatório: tokens de cor com HEX fixo e papel (`surface.page #080B1F`, `surface.card #11162D`,
`text.primary #F5F7FF`, `accent.selection #4E9BFF`…), tipografia Segoe UI com tamanhos por elemento,
grid de 12 colunas com base de 8 px, anatomia de card KPI (título → valor → comparação → período →
estado) e uma seção explícita de guardrails para IA. Regras que mais importam ao gerar PBIR:

- Não criar cor, gradiente, fonte ou efeito fora do contrato. Máximo 6 séries, na ordem `series.1`…`6`.
- Opacidade 100% em página e cards; imagem decorativa só fora das áreas de leitura.
- Não inventar dados, metas, comparações ou recomendações; definição ausente vira pendência, não exemplo.
- Nunca afirmar "acessibilidade garantida" — os índices de contraste valem só para os pares listados.

Os wikilinks do arquivo (`[[Paleta - Dashboard Power BI]]`, `[[MCP (Conceito)]]`) apontam para notas
de um vault Obsidian que **não está neste diretório** — `.vault_pbi_mcp_vendas/` só tem a config
`.obsidian/`. Trate o `DESING.md` como fonte única; não tente resolver os links.

## Armadilhas conhecidas

- `SemanticModel/TMDLScripts/Script 1.tmdl` é um **scratchpad** do TMDL View no Desktop (um
  `createOrReplace` de `dim_calendario`), não faz parte da definição do modelo e pode estar defasado
  em relação a `definition/tables/dim_calendario.tmdl`. A fonte de verdade é `definition/`.
- `.pbi/localSettings.json` e `.pbi/cache.abf` são artefatos locais — não versionar nem editar.
  `.pbi/editorSettings.json` tem `autodetectRelationships: true`: ao carregar tabela nova no Desktop,
  confira se ele não criou relacionamento sozinho.
- **Nunca edite TMDL com o projeto aberto no Desktop.** Ao salvar, o Desktop escreve o modelo em
  memória por cima dos arquivos e apaga a edição. Feche o Desktop, edite, reabra.
- Abrir dois PBIP na mesma sessão do Desktop pode falhar com
  `PFE_IMBI_DB_COMPLEVEL_DOWNGRADE` — banco tabular não faz downgrade de CompatibilityLevel. Se um
  modelo 1606 já está carregado e o outro PBIP declara 1600, o segundo não abre. A mensagem de topo
  cita o arquivo a corrigir; a causa real está na InnerException. Solução: fechar tudo e abrir só um,
  ou subir o `database.tmdl` do outro projeto para 1606 (upgrade sem volta).
- Ao ligar relacionamentos pela UI, confira a coluna de destino. O Desktop aceita
  `metas_mensais[VendedorID] → dim_vendedores[Vendedor]` porque o nome é único, mas nenhum valor casa
  e todas as metas caem na linha em branco. Depois de mexer em relacionamento, valide com
  `INFO.VIEW.RELATIONSHIPS()` e conte órfãs com `ISBLANK(RELATED(...))`.

## Ferramental: MCP e skills

### Skills a invocar

| Skill | Quando |
|---|---|
| `powerbi-modeling` | Qualquer trabalho no modelo semântico: tabelas, relacionamentos, medidas, RLS, calculation groups, performance. Define o workflow (conectar → analisar → agir). |
| `powerbi-expert` | Referência de DAX, M/Power Query, star schema, time intelligence e anti-patterns. Complementa a anterior. |
| `caveman` | Estilo de comunicação padrão deste projeto: respostas terse, sem filler. Substância técnica inteira preservada; só a gordura cai. Código, commits e PRs seguem escrita normal. |

Invoque `powerbi-modeling` e `powerbi-expert` juntas antes de mexer no modelo — a primeira dá o
processo, a segunda o repertório. Ao mexer no relatório, leia `DESING.md` antes.

### MCP `powerbi-modeling-mcp`

É o caminho preferencial para editar o modelo: opera no modelo **vivo** aberto no Desktop, sem
precisar reabrir arquivo. Fluxo do skill `powerbi-modeling`:

```
connection_operations(operation: "ListLocalInstances")   # descobre a instância do Desktop
connection_operations(operation: "Connect")
model_operations(operation: "Get")
table_operations / relationship_operations / measure_operations
```

**Antes de assumir que o MCP existe, verifique.** Ele já esteve configurado com escopo de projeto
apontando para outra pasta, o que o deixa invisível aqui. Se `ToolSearch` não achar
`connection_operations`, o servidor não está carregado nesta sessão. Para resolver:

```
claude mcp add powerbi-modeling-mcp --scope user -- npx -y @microsoft/powerbi-modeling-mcp@latest --start
```

Servidores MCP só carregam na inicialização — **reinicie a sessão** depois de adicionar.

### Fallbacks quando o MCP não está disponível

1. **Editar TMDL direto.** Os arquivos são texto; escreva em `definition/tables/*.tmdl` e adicione o
   `ref table` correspondente em `model.tmdl`. Depois o usuário atualiza no Desktop.
2. **Leitura do modelo vivo via ADOMD** (só consulta, não altera). A porta do `msmdsrv` é dinâmica —
   uma nova a cada abertura do Desktop — então descubra em tempo de execução:

```powershell
$pid = (Get-Process msmdsrv).Id
$port = (Get-NetTCPConnection -OwningProcess $pid -State Listen)[0].LocalPort
Add-Type -Path "C:\Program Files\Microsoft.NET\ADOMD.NET\160\Microsoft.AnalysisServices.AdomdClient.dll"
$cn = New-Object Microsoft.AnalysisServices.AdomdClient.AdomdConnection("Data Source=localhost:$port")
$cn.Open()
```

Com a conexão aberta, `EVALUATE INFO.VIEW.TABLES()` / `INFO.VIEW.RELATIONSHIPS()` /
`INFO.VIEW.MEASURES()` respondem o estado real do modelo — útil para conferir se o disco e a memória
divergiram.
