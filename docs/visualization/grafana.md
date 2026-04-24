---
# SPDX-FileCopyrightText: 2014-2026 The Prometheus Authors.
# SPDX-FileCopyrightText: 2026 The Linux Foundation. All rights reserved.
# The Linux Foundation has registered trademarks and uses trademarks.
# For a list of trademarks of The Linux Foundation, please see the Trademark
# Usage page.
# https://www.linuxfoundation.org/trademark-usage
#
# SPDX-License-Identifier: Apache-2.0
# Documentation licensed under the Apache License, Version 2.0.
# The original work was translated from English into Brazilian Portuguese.
# https://github.com/docsdevbr/prometheus-doc-pt-br/blob/-/LICENSES/Apache-2.0.txt

source_url: https://github.com/prometheus/docs/blob/main/docs/visualization/grafana.md
revision: fb6b195a8f00929cf066b27629cbd5bc3012d90d
status: ready

title: Suporte do Grafana para Prometheus
nav_title: Grafana
sort_rank: 2
---

O Grafana (http://grafana.com/) é uma plataforma de análise e visualização de
código aberto usada para monitorar e analisar métricas de diversas fontes de
dados.
Ele permite que as pessoas usuárias criem, explorem e compartilhem dashboards
interativos, com suporte para integrações com bancos de dados como Prometheus,
InfluxDB, Elasticsearch e outros.
O Grafana é amplamente utilizado para observabilidade, fornecendo alertas,
extensibilidade por meio de plugins e um editor de consultas flexível para
visualização de dados em tempo real.

Observação: A fonte de dados do Grafana para Prometheus está incluída desde a
versão 2.5.0 (28/10/2015).

A seguir, um exemplo de painel do Grafana que consulta o Prometheus para obter
dados:

[![Captura de tela do Grafana](/assets/docs/grafana_prometheus.png)](/assets/docs/grafana_prometheus.png)

## Instalação

Para instalar o Grafana, consulte a
[documentação oficial do Grafana](https://grafana.com/grafana/download/).

## Utilização

Por padrão, o Grafana estará escutando em
[http://localhost:3000](http://localhost:3000).
O login padrão é "admin" / "admin".

### Criando uma fonte de dados do Prometheus

Para criar uma fonte de dados do Prometheus no Grafana:

1. Clique no ícone de engrenagem na barra lateral para abrir o menu
   Configuration.
2. Clique em "Data Sources".
3. Clique em "Add data source".
4. Selecione "Prometheus" como o tipo.
5. Defina a URL do servidor Prometheus apropriada (por exemplo,
   `http://localhost:9090/`).
6. Ajuste outras configurações da fonte de dados conforme desejado (por exemplo,
   escolhendo o método de acesso correto).
7. Clique em "Salvar e testar" para salvar a nova fonte de dados.

A seguir, um exemplo de configuração de fonte de dados:

[![Configuração da fonte de dados](/assets/docs/grafana_configuring_datasource.png)](/assets/docs/grafana_configuring_datasource.png)

### Criando um gráfico do Prometheus

Siga o procedimento padrão para adicionar um novo gráfico ao Grafana.
Em seguida:

1. Clique no título do gráfico e depois em "Edit".
2. Na guia "Metrics", selecione sua fonte de dados do Prometheus (canto
   inferior direito).
3. Insira qualquer expressão do Prometheus no campo "Query", usando o campo
   "Metric" para buscar métricas por meio do recurso de autocompletar.
4. Para formatar os nomes da legenda das séries temporais, use a opção "Legend
   format".
   Por exemplo, para exibir apenas os rótulos `method` e `status` de um
   resultado de consulta retornado, separados por um hífen, você pode usar a
   string de formato de legenda `{{method}} - {{status}}`.
5. Ajuste outras configurações do gráfico até obter um gráfico funcional.

A seguir, um exemplo de configuração de gráfico do Prometheus:

[![Criação de gráfico do Prometheus](/assets/docs/grafana_qps_graph.png)](/assets/docs/grafana_qps_graph.png)

No Grafana 7.2 e versões posteriores, a variável `$__rate_interval` é
[recomendada](https://grafana.com/docs/grafana/latest/datasources/prometheus/#using-__rate_interval)
para uso nas funções `rate` e `increase`.

### Importando dashboards pré-construídos do Grafana.com

O Grafana.com mantém
[uma coleção de dashboards compartilhados](https://grafana.com/dashboards) que
podem ser baixados e usados com instâncias independentes do Grafana.
Use a opção "Filter" do Grafana.com para navegar pelos dashboards da fonte de
dados "Prometheus".

Atualmente, você precisa editar manualmente os arquivos JSON baixados e corrigir
as entradas `datasource:` para refletir o nome da fonte de dados do Grafana que
você escolheu para o seu servidor Prometheus.
Use a opção "Dashboards" → "Home" → "Importar" para importar o arquivo de
dashboard editado para a sua instalação do Grafana.
