---
title: DESING - Dashboard Power BI
tags: [power-bi, design-system, ia, acessibilidade]
date: 2026-10-12
status: pronto-para-aplicacao
---

# DESING - Dashboard [[Power BI]]

Fonte normativa: [[Paleta - Dashboard Power BI]]. Referência decorativa: [[capa_1_fundo.png]].
Contrato para orientar a IA na criação de dashboards consistentes e legíveis; não comprova acessibilidade do relatório final.

## Atmosfera visual

- Visual executivo escuro: navy quase preto, superfícies sólidas e destaques azuis, ciano e violeta.
- Prioridade: leitura de decisões e dados; sem decoração sobre valores, rótulos ou controles.
- Imagem de referência somente fora das áreas de leitura; não derivar novas cores dela.
- Página e cards: opacidade 100% (transparência 0%). Não aplicar gradiente nos componentes.
- Gradiente, sombra, sobreposição e transparência não garantem os contrastes deste contrato.
- Hierarquia: insight → contexto → ação. A recomendação vem depois da evidência.

## Paleta e papéis

Tokens fixos: copiar o HEX literalmente. Os índices são os pares calculados na fonte, sem recálculo neste documento.

| Token | HEX | Papel | Par exato e contraste |
|---|---|---|---|
| `surface.page` | `#080B1F` | Fundo sólido do canvas | `#080B1F` / `#080B1F`: 1,00:1 |
| `surface.card` | `#11162D` | Cards, gráficos e controles | `#11162D` / `#080B1F`: 1,09:1 |
| `text.primary` | `#F5F7FF` | Títulos, valores, rótulos | `#F5F7FF` / `#11162D`: 16,69:1 |
| `text.secondary` | `#BFC8E2` | Eixos, legenda, período | `#BFC8E2` / `#11162D`: 10,69:1 |
| `accent.selection` / `series.1` | `#4E9BFF` | Seleção, contorno, série 1 | `#4E9BFF` / `#11162D`: 6,34:1 |
| `accent.reference` | `#3CCBFF` | Ícones e referência | `#3CCBFF` / `#11162D`: 9,50:1 |
| `series.2` | `#B28CFF` | Série 2 | `#B28CFF` / `#11162D`: 6,87:1 |
| `series.3` | `#F26CDB` | Série 3 | `#F26CDB` / `#11162D`: 6,75:1 |
| `series.4` / `state.favorable` | `#50D6B2` | Série 4; favorável rotulado | `#50D6B2` / `#11162D`: 9,86:1 |
| `series.5` / `state.attention` | `#FFC66D` | Série 5; atenção rotulada | `#FFC66D` / `#11162D`: 11,52:1 |
| `series.6` / `state.unfavorable` | `#FF7D92` | Série 6; desfavorável rotulado | `#FF7D92` / `#11162D`: 7,31:1 |

- Textos correntes: `text.primary` e `text.secondary`; cores de série: dados e destaques curtos.
- Metas da fonte: 4,5:1 para texto comum e 3:1 para informação gráfica e controles.
- Card/página: apenas 1,09:1. Delimitar cards com espaço, título e agrupamento.
- Quando necessário para identificar controles ou estados, usar contorno azul de 2 px sobre o card.
- Texto branco sobre preenchimento azul não está validado aqui; botões ficam escuros com contorno azul.
- Os pares não comprovam diferenciação entre séries nem adequação a todas as formas de daltonismo.

## Tipografia

Padrão: Segoe UI; manter o mesmo estilo em toda a página.
Tamanhos em pontos configurados no Power BI; verificar leitura no tamanho real de exibição.

| Elemento | Tamanho | Peso | Cor |
|---|---|---|---|
| Título da página | 26 pt | Semibold | `text.primary` |
| Insight principal | 20 pt | Semibold | `text.primary` |
| Valor KPI | 32 pt | Semibold | `text.primary` |
| Título de card/gráfico | 16 pt | Semibold | `text.primary` |
| Comparação, filtros, botões | 14 pt | Regular | `text.primary` |
| Eixos, legenda, período | 12 pt | Regular | `text.secondary` |

- Não reduzir texto abaixo de 12 pt para acomodar conteúdo; reduzir densidade ou dividir a página.
- Títulos curtos, sem caixa alta integral; unidades explícitas e decimais coerentes com a métrica.
- Títulos/textos alinhados à esquerda; números comparáveis com alinhamento consistente.
- Tamanho de fonte não é prova de contraste ou acessibilidade.

## Componentes

### Cards KPI

- Fundo `surface.card`, opacidade 100%, padding 24 px e raio 8 px quando disponível.
- Ordem: título → valor → comparação → período → estado. Não omitir a base da comparação.
- Título descreve a métrica; valor inclui unidade; comparação declara meta ou período anterior.
- Período declara o intervalo aplicado; estado combina ícone, rótulo e cor.
- Usar somente métrica, valor, diferença, período e estado reais; não inserir números fictícios.
- Sem definição, indicar “Comparação não definida” ou “Estado não definido”.
- Crescimento não é automaticamente favorável: o sentido depende da regra de negócio.

### Filtros e botões

- Filtros: título visível, seleção legível, indicação textual de filtros ativos; altura mínima 48 px.
- Fundo `surface.card`, texto `text.primary`; contorno azul para foco/seleção quando disponível.
- Botões: verbo e objeto no rótulo, altura mínima 48 px, padding horizontal 16 px e fundo escuro sólido.
- Ação principal: contorno azul; secundária: rótulo visível e agrupamento separado.
- Limpar filtros tem rótulo explícito; não esconder a ação exclusivamente em ícone.
- Desabilitado: “Indisponível” e motivo; não usar apenas redução de opacidade.
- Validar teclado e foco na publicação; não pressupor suporte por configuração visual.

### Gráficos, estados e rótulos

- Máximo de seis séries simultâneas na ordem `series.1` a `series.6`; não gerar sétima cor.
- Preservar a cor de cada entidade entre visuais e páginas.
- Título informativo, unidade, período, legenda textual e rótulos diretos quando não houver sobreposição.
- Em sobreposição, combinar cor com marcador ou estilo de linha; evitar traços significativos muito finos.
- Referência em ciano com rótulo explicando meta/base; não inventar referência.
- Favorável: confirmação + “Favorável”; atenção: aviso + “Atenção”; desfavorável: alerta + “Desfavorável”.
- Crítico exige “Crítico” e ícone de alerta, conforme regra de negócio explícita.
- Carregamento: “Carregando dados”; ausência: “Sem dados para os filtros atuais”.
- Erro: “Não foi possível carregar os dados”. Nunca substituir valor ausente por zero.
- Não usar seta como única explicação da tendência; não depender somente da cor.

## Layout

- Canvas 16:9; 1920 × 1080 é recomendação de projeto, não dimensão verificada da imagem.
- Base de espaçamento: 8 px. Escala: 8, 16, 24, 32 e 48 px.
- Para o canvas recomendado: margem externa 48 px, grid de 12 colunas e gutters de 24 px.
- Área útil horizontal 1824 px; coluna 130 px. Blocos de 3, 4 e 6 colunas: 438, 592 e 900 px.
- Cabeçalho: título, período e filtros essenciais; reservar até 120 px sem comprimir fontes.
- Insight: uma conclusão apoiada em dados; se faltar evidência, declarar a limitação.
- KPI: até quatro cards de 3 colunas; altura recomendada 160–200 px, ajustada ao conteúdo real.
- Contexto: gráficos em blocos de 6 colunas; detalhamento somente quando necessário à decisão.
- Ação: recomendação contextualizada; responsável/próximo passo somente quando conhecidos.
- Espaço entre blocos e padding interno: 24 px; não sobrepor filtros, textos e gráficos.
- Em outra resolução, preservar 16:9 e estrutura; revalidar leitura no destino sem escalonar texto cegamente.

## Guardrails para IA

- Este contrato e a paleta são normativos; não alterar HEX, ordem ou papéis sem pedido explícito.
- Não introduzir cores, gradientes, fontes, efeitos ou escolhas arbitrárias para “melhorar” o visual.
- Não inventar dados, metas, períodos, comparações, regras, responsáveis ou recomendações.
- Definição de negócio ausente: registrar pendência e pedir decisão, sem exemplos genéricos.
- Integração por [[MCP (Conceito)]] respeita os mesmos tokens e confirma configuração final dos visuais.
- Usar recursos disponíveis; propriedade inexistente exige registro da limitação, não alegação de aplicação.
- Não afirmar “acessibilidade garantida”; índices valem somente para os pares sólidos informados.
- Revalidar após mudança de fundo, transparência, sombra, sobreposição, tema, exportação ou publicação.

## Checklist de validação

- [ ] HEX e papéis correspondem à fonte; nenhuma cor adicional foi criada.
- [ ] Página/componentes sólidos com opacidade 100%; decoração fora das áreas de leitura.
- [ ] Fontes, alinhamentos, unidades e períodos legíveis no tamanho real de exibição.
- [ ] KPI com título, valor, comparação/base, período e estado, ou limitação explícita.
- [ ] Até seis séries; legenda, rótulos e sinais adicionais permitem leitura sem depender só de cor.
- [ ] Controles com rótulos e foco; teclado e ordem de navegação testados no destino.
- [ ] Ausência, carregamento e erro não se confundem com zero ou resultado favorável.
- [ ] Insight → contexto → ação preservado, com evidências reais e pendências registradas.
- [ ] Contraste reavaliado quando a renderização divergir dos pares; publicação/exportação inspecionadas.

Referências da fonte: [WCAG 1.4.3 — texto](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html), [WCAG 1.4.11 — elementos não textuais](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html) e [WCAG 1.4.1 — uso de cor](https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html).
