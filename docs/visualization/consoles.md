---
# Copyright (c) 2014-2026 The Prometheus Authors.
# Copyright (c) 2026 The Linux Foundation. All rights reserved.
# The Linux Foundation has registered trademarks and uses trademarks.
# For a list of trademarks of The Linux Foundation, please see the Trademark
# Usage page.
# https://www.linuxfoundation.org/trademark-usage
#
# SPDX-License-Identifier: Apache-2.0
# Documentation licensed under the Apache License, Version 2.0.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/prometheus-doc-pt-br/blob/-/LICENSES/Apache-2.0.txt

source_url: https://github.com/prometheus/docs/blob/main/docs/visualization/consoles.md
revision: 8afdb70ce0dcb6d88a00187310049227409da96d
status: ready

title: Templates de console
sort_rank: 4
---

ATENÇÃO: A partir do Prometheus 3.0, os templates e bibliotecas de console não
são mais fornecidos com o Prometheus.
Se você deseja usar templates de console, deve fornecer seus próprios templates
e bibliotecas especificando os parâmetros de linha de comando
`--web.console.templates` e `--web.console.libraries`.
Esta página de documentação é mantida para referência histórica e para
demonstrar os recursos dos templates de console.
Observe que quaisquer bibliotecas de console referenciadas da versão 2.x do
Prometheus não são mais mantidas e podem conter vulnerabilidades de segurança
conhecidas (CVEs).

Os templates de console permitem a criação de consoles arbitrários usando a
linguagem de templates do Go (http://golang.org/pkg/text/template/).
Eles são servidos pelo servidor Prometheus.

Os templates de console são a maneira mais poderosa de criar templates que podem
ser facilmente gerenciados no controle de versão.
Existe uma curva de aprendizado, então pessoas usuárias novas nesse estilo de
monitoramento devem experimentar o [Grafana](/docs/visualization/grafana/)
primeiro.

## Começando

O Prometheus vem com um conjunto de consoles de exemplo para você começar.
Eles podem ser encontrados em `/consoles/index.html.example` em uma instância do
Prometheus em execução e exibirão consoles do Node Exporter se o Prometheus
estiver coletando dados de Node Exporters com um rótulo `job="node"`.

Os consoles de exemplo têm 5 partes:

1. Uma barra de navegação na parte superior.
1. Um menu à esquerda.
1. Controles de tempo na parte inferior.
1. O conteúdo principal no centro, geralmente gráficos.
1. Uma tabela à direita.

A barra de navegação serve para links para outros sistemas, como outros sistemas
do Prometheus
<sup>[1](/docs/introduction/faq/#what-is-the-plural-of-prometheus)</sup>,
documentação e qualquer outra coisa que faça sentido para você.
O menu serve para navegação dentro do mesmo servidor Prometheus, o que é muito
útil para poder abrir rapidamente um console em outra aba para correlacionar
informações.
Ambos são configurados em `console_libraries/menu.lib`.

Os controles de tempo permitem alterar a duração e o intervalo dos gráficos.
As URLs do console podem ser compartilhadas e exibirão os mesmos gráficos para
outras pessoas usuárias.

O conteúdo principal geralmente são gráficos.
Há uma biblioteca de gráficos JavaScript configurável que lida com a requisição
de dados do Prometheus e a renderização deles via
[Rickshaw](https://shutterstock.github.io/rickshaw/).

Por fim, a tabela à direita pode ser usada para exibir estatísticas de forma
mais compacta do que os gráficos.

## Exemplo de console

Este é um console básico.
Ele mostra o número de tarefas, quantas delas estão em execução, o uso médio da
CPU e o uso médio de memória na tabela à direita.
O conteúdo principal apresenta um gráfico de consultas por segundo.

```
{{template "head" .}}

{{template "prom_right_table_head"}}
<tr>
  <th>MyJob</th>
  <th>{{ template "prom_query_drilldown" (args "sum(up{job='myjob'})") }}
      / {{ template "prom_query_drilldown" (args "count(up{job='myjob'})") }}
  </th>
</tr>
<tr>
  <td>CPU</td>
  <td>{{ template "prom_query_drilldown" (args
      "avg by(job)(rate(process_cpu_seconds_total{job='myjob'}[5m]))"
      "s/s" "humanizeNoSmallPrefix") }}
  </td>
</tr>
<tr>
  <td>Memory</td>
  <td>{{ template "prom_query_drilldown" (args
       "avg by(job)(process_resident_memory_bytes{job='myjob'})"
       "B" "humanize1024") }}
  </td>
</tr>
{{template "prom_right_table_tail"}}


{{template "prom_content_head" .}}
<h1>MyJob</h1>

<h3>Queries</h3>
<div id="queryGraph"></div>
<script>
new PromConsole.Graph({
  node: document.querySelector("#queryGraph"),
  expr: "sum(rate(http_query_count{job='myjob'}[5m]))",
  name: "Queries",
  yAxisFormatter: PromConsole.NumberFormatter.humanizeNoSmallPrefix,
  yHoverFormatter: PromConsole.NumberFormatter.humanizeNoSmallPrefix,
  yUnits: "/s",
  yTitle: "Queries"
})
</script>

{{template "prom_content_tail" .}}

{{template "tail"}}
```

Os templates `prom_right_table_head` e `prom_right_table_tail` contêm a tabela
do lado direito.
Isso é opcional.

`prom_query_drilldown` é um template que avaliará a expressão passada para ele,
formatará a expressão e criará um link para a expressão no
[navegador de expressões](/docs/visualization/browser/).
O primeiro argumento é a expressão.
O segundo argumento é a unidade a ser usada.
O terceiro argumento define como formatar a saída.
Apenas o primeiro argumento é obrigatório.

Formatos de saída válidos para o terceiro argumento de `prom_query_drilldown`:

- Não especificado: Saída de exibição padrão do Go.
- `humanize`: Exibe o resultado usando
  [prefixos de métricas](http://en.wikipedia.org/wiki/Metric_prefix).
- `humanizeNoSmallPrefix`: Para valores absolutos maiores que 1, exibe o
  resultado usando
  [prefixos métricos](http://en.wikipedia.org/wiki/Metric_prefix).
  Para valores absolutos menores que 1, exibe 3 dígitos significativos.
  Isso é útil para evitar unidades como miligramas por segundo que podem ser
  produzidas por `humanize`.
- `humanize1024`: Exibe o resultado humanizado usando uma base de 1024 em vez de
  1000.
  Isso geralmente é usado com `B` como segundo argumento para produzir unidades
  como `KiB` e `MiB`.
- `printf.3g`: Exibe 3 dígitos significativos.

Formatos personalizados podem ser definidos.
Consulte
[prom.lib](https://github.com/prometheus/prometheus/blob/release-2.55/console_libraries/prom.lib)
para exemplos.

## Biblioteca de gráficos

A biblioteca de gráficos é invocada como:

```
<div id="queryGraph"></div>
<script>
new PromConsole.Graph({
  node: document.querySelector("#queryGraph"),
  expr: "sum(rate(http_query_count{job='myjob'}[5m]))"
})
</script>
```

O template `head` carrega o Javascript e o CSS necessários.

Parâmetros da biblioteca de gráficos:

| Nome            | Descrição
|-----------------| -------------
| expr            | Obrigatório. Expressão a ser representada no gráfico. Pode ser uma lista.
| node            | Required. Obrigatório. Nó DOM onde renderizar.
| duration        | Optional. Opcional. Duração do gráfico. O padrão é 1 hora.
| endTime         | Opcional. Horário Unix em que o gráfico termina. O padrão é agora.
| width           | Opcional. Largura do gráfico, excluindo títulos. O padrão é detecção automática.
| height          | Opcional. Altura do gráfico, excluindo títulos e legendas. O padrão é 200 pixels.
| min             | Opcional. Valor mínimo do eixo x. O padrão é o menor valor de dados.
| max             | Opcional. Valor máximo do eixo y. O padrão é o maior valor de dados.
| renderer        | Opcional. Tipo de gráfico. As opções são `line` e `area` (gráfico empilhado). O padrão é `line`.
| name            | Opcional. Título dos gráficos na legenda e nos detalhes ao passar o mouse. Se for passada uma string, `[[ label ]]` será substituído pelo valor do rótulo. Se for passada uma função, ela receberá um mapa de rótulos e deverá retornar o nome como uma string. Pode ser uma lista.
| xTitle          | Opcional. Título do eixo x. O padrão é `Time`.
| yUnits          | Opcional. Unidades do eixo y. O padrão é vazio.
| yTitle          | Opcional. Título do eixo y. O padrão é vazio.
| yAxisFormatter  | Opcional. Formatador de números para o eixo y. O padrão é `PromConsole.NumberFormatter.humanize`.
| yHoverFormatter | Opcional. Formatador de números para os detalhes exibidos ao passar o mouse. O padrão é `PromConsole.NumberFormatter.humanizeExact`.
| colorScheme     | Opcional. Esquema de cores a ser usado nos gráficos. Pode ser uma lista de códigos de cores hexadecimais ou um dos [nomes de esquema de cores](https://github.com/shutterstock/rickshaw/blob/master/src/js/Rickshaw.Fixtures.Color.js) suportados pelo Rickshaw. O padrão é `'colorwheel'`.

Se `expr` e `name` forem listas, elas devem ter o mesmo comprimento.
O nome será aplicado aos gráficos para a expressão correspondente.

Opções válidas para `yAxisFormatter` e `yHoverFormatter`:

- `PromConsole.NumberFormatter.humanize`: Formata usando
  [prefixos métricos](http://en.wikipedia.org/wiki/Metric_prefix).
- `PromConsole.NumberFormatter.humanizeNoSmallPrefix`: Para valores absolutos
  maiores que 1, formata usando
  [prefixos métricos](http://en.wikipedia.org/wiki/Metric_prefix).
  Para valores absolutos menores que 1, formata com 3 dígitos significativos.
  Isso é útil para evitar unidades como milésimos de segundo que podem ser
  produzidas por `PromConsole.NumberFormatter.humanize`.
- `PromConsole.NumberFormatter.humanize1024`: Formata o resultado humanizado
  usando uma base de 1024 em vez de 1000.
