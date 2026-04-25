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

source_url: https://github.com/prometheus/docs/blob/main/docs/introduction/comparison.md
revision: 8bdb919e820ad27adc12fc66daf38531c3d9a801
status: ready

title: Comparação com alternativas
sort_rank: 4
---

## Prometheus vs. Graphite

### Escopo

[Graphite](http://graphite.readthedocs.org/en/latest/) concentra-se em ser um
banco de dados passivo de séries temporais com uma linguagem de consulta e
recursos de geração de gráficos.
Quaisquer outras preocupações são tratadas por componentes externos.

O Prometheus é um sistema completo de monitoramento e análise de tendências que
inclui coleta, armazenamento, consulta, geração de gráficos e alertas integrados
e ativos com base em dados de séries temporais.
Ele tem conhecimento sobre como o mundo deveria ser (quais endpoints deveriam
existir, quais padrões de séries temporais indicam problemas, etc.) e tenta
ativamente encontrar falhas.

### Modelo de dados

O Graphite armazena amostras numéricas para séries temporais nomeadas, de forma
muito semelhante ao Prometheus.
No entanto, o modelo de metadados do Prometheus é mais rico: enquanto os nomes
das métricas do Graphite consistem em componentes separados por pontos que
codificam implicitamente as dimensões, o Prometheus codifica as dimensões
explicitamente como pares chave-valor, chamados rótulos, anexados a um nome de
métrica.
Isso permite filtrar, agrupar e corresponder facilmente por esses rótulos por
meio da linguagem de consulta.

Além disso, especialmente quando o Graphite é usado em combinação com o
[StatsD](https://github.com/etsy/statsd/), é comum armazenar apenas dados
agregados de todas as instâncias monitoradas, em vez de preservar a instância
como uma dimensão e poder detalhar instâncias problemáticas individuais.

Por exemplo, armazenar o número de requisições HTTP para servidores de API com o
código de resposta `500` e o método `POST` para o endpoint `/tracks` seria
normalmente codificado assim em Graphite/StatsD:

```
stats.api-server.tracks.post.500 -> 93
```

No Prometheus, os mesmos dados poderiam ser codificados desta forma
(considerando três instâncias do servidor de API):

```
api_server_http_requests_total{method="POST",handler="/tracks",status="500",instance="<sample1>"} -> 34
api_server_http_requests_total{method="POST",handler="/tracks",status="500",instance="<sample2>"} -> 28
api_server_http_requests_total{method="POST",handler="/tracks",status="500",instance="<sample3>"} -> 31
```

### Armazenamento

O Graphite armazena dados de séries temporais em disco local no formato
[Whisper](http://graphite.readthedocs.org/en/latest/whisper.html), um banco de
dados no estilo RRD que espera que as amostras cheguem em intervalos regulares.
Cada série temporal é armazenada em um arquivo separado e novas amostras
sobrescrevem as antigas após um determinado período.

O Prometheus também cria um arquivo local por série temporal, mas permite
armazenar amostras em intervalos arbitrários à medida que coletas ou avaliações
de regras ocorrem.
Como novas amostras são simplesmente anexadas, os dados antigos podem ser
mantidos por tempo indeterminado.
O Prometheus também funciona bem para muitos conjuntos de séries temporais de
curta duração que mudam com frequência.

### Resumo

O Prometheus oferece um modelo de dados e uma linguagem de consulta mais ricos,
além de ser mais fácil de executar e integrar ao seu ambiente.
Se você deseja uma solução em cluster que possa armazenar dados históricos a
longo prazo, o Graphite pode ser uma escolha melhor.

## Prometheus vs. InfluxDB

O [InfluxDB](https://influxdata.com/) é um banco de dados de séries temporais de
código aberto, com uma opção comercial para escalonamento e clustering.
O projeto InfluxDB foi lançado quase um ano após o início do desenvolvimento do
Prometheus, portanto, não pudemos considerá-lo como uma alternativa na época.
Ainda assim, existem diferenças significativas entre o Prometheus e o InfluxDB,
e ambos os sistemas são voltados para casos de uso ligeiramente diferentes.

### Escopo

Para uma comparação justa, também devemos considerar o
[Kapacitor](https://github.com/influxdata/kapacitor) juntamente com o InfluxDB,
pois em conjunto, eles abordam o mesmo espaço de problemas que o Prometheus e o
Alertmanager.

As mesmas diferenças de escopo do caso do [Graphite](#prometheus-vs-graphite) se
aplicam aqui ao próprio InfluxDB.
Além disso, o InfluxDB oferece consultas contínuas, que são equivalentes às
regras de gravação do Prometheus.

O escopo do Kapacitor é uma combinação das regras de gravação do Prometheus,
regras de alerta e a funcionalidade de notificação do Alertmanager.
O Prometheus oferece
[uma linguagem de consulta mais poderosa para geração de gráficos e alertas](https://www.robustperception.io/translating-between-monitoring-languages/).
O Alertmanager do Prometheus oferece ainda funcionalidades de agrupamento,
desduplicação e silenciamento.

### Modelo de dados / armazenamento

Assim como o Prometheus, o modelo de dados do InfluxDB possui pares de
chave-valor como rótulos, chamados de tags.
Além disso, o InfluxDB possui um segundo nível de rótulos chamados campos, cujo
uso é mais limitado.
O InfluxDB suporta timestamps com resolução de até nanossegundos e tipos de
dados float64, int64, bool e string.
Em contraste, o Prometheus suporta o tipo de dados float64 com suporte limitado
para strings e timestamps com resolução de milissegundos.

O InfluxDB usa uma variante de uma
[árvore de mesclagem estruturada em log para armazenamento com um log de gravação antecipada](https://docs.influxdata.com/influxdb/v1.7/concepts/storage_engine/),
fragmentada por tempo.
Isso é muito mais adequado para registro de eventos do que a abordagem do
Prometheus de arquivo somente de acréscimo por série temporal.

O artigo
[Logs and Metrics and Graphs, Oh My!](https://grafana.com/blog/2016/01/05/logs-and-metrics-and-graphs-oh-my/)
descreve as diferenças entre o registro de eventos e o registro de métricas.

### Arquitetura

Os servidores Prometheus são executados independentemente uns dos outros e
dependem apenas de seu armazenamento local para suas funcionalidades principais:
coleta de dados, processamento de regras e alertas.
A versão de código aberto do InfluxDB é semelhante.

A oferta comercial do InfluxDB é, por definição, um cluster de armazenamento
distribuído, com armazenamento e consultas sendo gerenciados por vários nós
simultaneamente.

Isso significa que o InfluxDB comercial será mais fácil de escalar
horizontalmente, mas também significa que você terá que gerenciar a complexidade
de um sistema de armazenamento distribuído desde o início.
O Prometheus será mais simples de executar, mas em algum momento você precisará
fragmentar os servidores explicitamente de acordo com limites de escalabilidade,
como produtos, serviços, data centers ou aspectos semelhantes.
Servidores independentes (que podem ser executados de forma redundante em
paralelo) também podem oferecer melhor confiabilidade e isolamento de falhas.

A versão de código aberto do Kapacitor não possui opções
distribuídas/redundantes integradas para regras, alertas ou notificações.
A versão de código aberto do Kapacitor pode ser escalada por meio de
fragmentação manual pela pessoa usuária, semelhante ao próprio Prometheus.
O InfluxDB oferece o
[Enterprise Kapacitor](https://docs.influxdata.com/enterprise_kapacitor), que
suporta um sistema de alertas de alta disponibilidade/redundante.

Por outro lado, o Prometheus e o Alertmanager oferecem uma opção redundante
totalmente de código aberto, executando réplicas redundantes do Prometheus e
usando o modo de
[Alta Disponibilidade](https://github.com/prometheus/alertmanager#high-availability)
do Alertmanager.

### Resumo

Existem muitas semelhanças entre os sistemas.
Ambos possuem rótulos (chamados tags no InfluxDB) para suportar métricas
multidimensionais de forma eficiente.
Ambos usam basicamente os mesmos algoritmos de compressão de dados.
Ambos possuem extensas integrações, inclusive entre si.
Ambos possuem recursos que permitem estendê-los ainda mais, como analisar dados
em ferramentas estatísticas ou executar ações automatizadas.

Onde o InfluxDB é melhor:

- Se você estiver fazendo registro de eventos.
- A opção comercial oferece clustering para o InfluxDB, que também é melhor para
  armazenamento de dados a longo prazo.
- Visão eventualmente consistente dos dados entre as réplicas.

Onde o Prometheus é melhor:

- Se você trabalha principalmente com métricas.
- Linguagem de consulta, alertas e funcionalidades de notificação mais
  poderosas.
- Maior disponibilidade e tempo de atividade para geração de gráficos e alertas.

O InfluxDB é mantido por uma única empresa comercial seguindo o modelo
open-core, oferecendo recursos premium como clustering, hospedagem e suporte de
código fechado.
O Prometheus é um [projeto totalmente open source e independente](/community/),
mantido por diversas empresas e pessoas, algumas das quais também oferecem
serviços e suporte comerciais.

## Prometheus vs. OpenTSDB

O [OpenTSDB](http://opentsdb.net/) é um banco de dados distribuído de séries
temporais baseado em [Hadoop](http://hadoop.apache.org/) e
[HBase](http://hbase.apache.org/).

### Escopo

As mesmas diferenças de escopo do
[Graphite](http://docs/introduction/comparison/#prometheus-vs-graphite) se
aplicam aqui.

### Modelo de dados

O modelo de dados do OpenTSDB é quase idêntico ao do Prometheus: as séries
temporais são identificadas por um conjunto de pares chave-valor arbitrários (as
tags do OpenTSDB são rótulos do Prometheus).
Todos os dados de uma métrica são
[armazenados juntos](http://opentsdb.net/docs/build/html/user_guide/writing/index.html#time-series-cardinality),
limitando a cardinalidade das métricas.
Existem algumas diferenças menores: o Prometheus permite caracteres arbitrários
em valores de rótulos, enquanto o OpenTSDB é mais restritivo.
O OpenTSDB também não possui uma linguagem de consulta completa, permitindo
apenas agregações e operações matemáticas simples por meio de sua API.

### Armazenamento

O armazenamento do [OpenTSDB](http://opentsdb.net/) é implementado sobre o
[Hadoop](http://hadoop.apache.org/) e o [HBase](http://hbase.apache.org/).
Isso significa que é fácil escalar o OpenTSDB horizontalmente, mas você precisa
aceitar a complexidade geral de executar um cluster Hadoop/HBase desde o início.

O Prometheus será mais simples de executar inicialmente, mas exigirá
particionamento explícito quando a capacidade de um único nó for excedida.

### Resumo

O Prometheus oferece uma linguagem de consulta muito mais rica, pode lidar com
métricas de cardinalidade mais alta e faz parte de um sistema de monitoramento
completo.
Se você já usa o Hadoop e prioriza o armazenamento de longo prazo em detrimento
desses benefícios, o OpenTSDB é uma boa escolha.

## Prometheus vs. Nagios

O [Nagios](https://www.nagios.org/) é um sistema de monitoramento que surgiu na
década de 1990 como NetSaint.

### Escopo

O Nagios se concentra principalmente em alertas baseados nos códigos de saída de
scripts.
Esses códigos são chamados de "verificações".
É possível silenciar alertas individuais, mas não há agrupamento, roteamento ou
deduplicação.

Existem diversos plugins disponíveis.
Por exemplo, é possível redirecionar os poucos kilobytes dos plugins perfData
[para um banco de dados de séries temporais, como o Graphite](https://github.com/shawn-sterling/graphios),
ou usar o NRPE para
[executar verificações em máquinas remotas](https://exchange.nagios.org/directory/Addons/Monitoring-Agents/NRPE--2D-Nagios-Remote-Plugin-Executor/details).

### Modelo de dados

O Nagios é baseado em hosts.
Cada host pode ter um ou mais serviços e cada serviço pode executar uma
verificação.

Não há noção de rótulos ou linguagem de consulta.

### Armazenamento

O Nagios não possui armazenamento próprio, além do estado atual da verificação.
Existem plugins que podem armazenar dados, como
[para visualização](https://docs.pnp4nagios.org/).

### Arquitetura

Os servidores Nagios são independentes.
Toda a configuração das verificações é feita por meio de arquivo.

### Resumo

O Nagios é adequado para monitoramento básico de sistemas pequenos e/ou
estáticos onde a sondagem de caixa preta é suficiente.

Se você deseja realizar monitoramento de caixa branca ou possui um ambiente
dinâmico ou baseado em nuvem, o Prometheus é uma boa opção.

## Prometheus vs. Sensu

O [Sensu](https://sensu.io) é um pipeline de monitoramento e observabilidade de
código aberto com uma distribuição comercial que oferece recursos adicionais
para escalabilidade.
Ele pode reusar plugins existentes do Nagios.

### Escopo

O Sensu é um pipeline de observabilidade que se concentra no processamento e
alerta de dados de observabilidade como um fluxo de
[eventos](https://docs.sensu.io/sensu-go/latest/observability-pipeline/observe-events/events/).
Ele fornece uma estrutura extensível para
[filtragem](https://docs.sensu.io/sensu-go/latest/observability-pipeline/observe-filter/),
agregação,
[transformação](https://docs.sensu.io/sensu-go/latest/observability-pipeline/observe-transform/)
e
[processamento](https://docs.sensu.io/sensu-go/latest/observability-pipeline/observe-process/)
de eventos – incluindo o envio de alertas para outros sistemas e o armazenamento
de eventos em sistemas de terceiros.
Os recursos de processamento de eventos do Sensu são semelhantes em escopo às
regras de alerta do Prometheus e ao Alertmanager.

### Modelo de dados

Os
[eventos](https://docs.sensu.io/sensu-go/latest/observability-pipeline/observe-events/events/)
do Sensu representam a saúde do serviço e/ou
[métricas](https://docs.sensu.io/sensu-go/latest/observability-pipeline/observe-events/events/#metric-attributes)
em um formato de dados estruturado, identificado por um nome de
[entidade](https://docs.sensu.io/sensu-go/latest/observability-pipeline/observe-entities/entities/)
(por exemplo, servidor, instância de computação em nuvem, contêiner ou serviço),
um nome de evento e
[metadados](https://docs.sensu.io/sensu-go/latest/observability-pipeline/observe-events/events/#metadata-attributes)
opcionais, chamados de "rótulos" ou "anotações".
A carga útil de eventos do Sensu pode incluir uma ou mais
[`points`](https://docs.sensu.io/sensu-go/latest/observability-pipeline/observe-events/events/#points-attributes)
de métricas, representados como um objeto JSON contendo um `name`, `tags` (pares
chave/valor), `timestamp` e `value` (sempre um número de ponto flutuante).

### Armazenamento

O Sensu armazena informações sobre o status de eventos atuais e recentes, bem
como dados de inventário em tempo real, em um banco de dados embutido (etcd) ou
em um SGBD relacional externo (PostgreSQL).

### Arquitetura

Todos os componentes de uma implementação do Sensu podem ser agrupados em
clusters para alta disponibilidade e melhoria na taxa de transferência de
processamento de eventos.

### Resumo

O Sensu e o Prometheus têm algumas funcionalidades em comum, mas adotam
abordagens muito diferentes para o monitoramento.
Ambos oferecem mecanismos de descoberta extensíveis para ambientes dinâmicos
baseados em nuvem e plataformas de computação efêmeras, embora os mecanismos
subjacentes sejam bastante distintos.
Ambos oferecem suporte à coleta de métricas multidimensionais por meio de
rótulos e anotações.
Ambos possuem amplas integrações, e o Sensu oferece suporte nativo à coleta de
métricas de todos os exportadores do Prometheus.
Ambos são capazes de encaminhar dados de observabilidade para plataformas de
dados de terceiros (por exemplo, repositórios de eventos ou TSDBs).
A principal diferença entre Sensu e Prometheus reside em seus casos de uso.

Onde o Sensu se destaca:

- Se você estiver coletando e processando dados de observabilidade híbrida
  (incluindo métricas _e/ou_ eventos).
- Se você estiver consolidando várias ferramentas de monitoramento e precisar de
  suporte para métricas _e_ plugins no estilo Nagios ou scripts de verificação.
- Plataforma de processamento de eventos mais poderosa.

Onde o Prometheus se destaca:

- Se você estiver coletando e avaliando principalmente métricas.
- Se você estiver monitorando infraestrutura Kubernetes homogênea (se 100% das
  cargas de trabalho que você está monitorando estiverem em K8s, o Prometheus
  oferece melhor integração com K8s).
- Linguagem de consulta mais poderosa e suporte integrado para análise de dados
  históricos.

O Sensu é mantido por uma única empresa comercial que segue o modelo de negócios
open-core, oferecendo recursos premium como correlação e agregação de eventos de
código fechado, federação e suporte.
O Prometheus é um projeto totalmente open source e independente, mantido por
diversas empresas e pessoas, algumas das quais também oferecem serviços e
suporte comerciais.
