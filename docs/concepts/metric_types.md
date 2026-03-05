---
# Copyright (c) 2014-2026 The Prometheus Authors.
# Copyright (c) 2026 The Linux Foundation. All rights reserved.
# The Linux Foundation has registered trademarks and uses trademarks.
# For a list of trademarks of The Linux Foundation, please see the Trademark
# Usage page.
# https://www.linuxfoundation.org/trademark-usage
#
# Documentation licensed under the Creative Commons Attribution 4.0
# International License.
# The original work was translated from English into Brazilian Portuguese.
# https://creativecommons.org/licenses/by/4.0/

source_url: https://github.com/prometheus/docs/blob/main/docs/concepts/metric_types.md
revision: 8bdb919e820ad27adc12fc66daf38531c3d9a801
status: ready

title: Tipos de métricas
sort_rank: 2
---

As bibliotecas de cliente Prometheus oferecem quatro tipos de métricas
principais.
Elas são atualmente diferenciadas apenas nas bibliotecas de cliente (para
permitir APIs adaptadas ao uso dos tipos específicos) e no protocolo de
comunicação.
O servidor Prometheus ainda não utiliza as informações de tipo e transforma
todos os dados em séries temporais sem tipo.
Isso pode mudar no futuro.

## Counter

Um _counter_ (contador, em português) é uma métrica cumulativa que representa um
único [counter monotonicamente crescente](https://en.wikipedia.org/wiki/Monotonic_function),
cujo valor só pode aumentar ou ser zerado na reinicialização.
Por exemplo, você pode usar um counter para representar o número de requisições
atendidas, tarefas concluídas ou erros.

Não use um counter para expor um valor que pode diminuir.
Por exemplo, não use um counter para o número de processos em execução; em vez
disso, use um gauge.

Documentação de uso da biblioteca cliente para counters:

- [Go](http://godoc.org/github.com/prometheus/client_golang/prometheus#Counter)
- [Java](https://prometheus.github.io/client_java/getting-started/metric-types/#counter)
- [Python](https://prometheus.github.io/client_python/instrumenting/counter/)
- [Ruby](https://github.com/prometheus/client_ruby#counter)
- [.Net](https://github.com/prometheus-net/prometheus-net#counters)
- [Rust](https://docs.rs/prometheus-client/latest/prometheus_client/metrics/counter/index.html)

## Gauge

Um _gauge_ (medidor, em português) é uma métrica que representa um único valor
numérico que pode subir e descer arbitrariamente.

Os gauges são normalmente usados para valores medidos, como temperaturas ou uso
atual de memória, mas também para "contagens" que podem subir e descer, como o
número de requisições simultâneas.

Documentação de uso da biblioteca cliente para gauges:

- [Go](http://godoc.org/github.com/prometheus/client_golang/prometheus#Gauge)
- [Java](https://prometheus.github.io/client_java/getting-started/metric-types/#gauge)
- [Python](https://prometheus.github.io/client_python/instrumenting/gauge/)
- [Ruby](https://github.com/prometheus/client_ruby#gauge)
- [.Net](https://github.com/prometheus-net/prometheus-net#gauges)
- [Rust](https://docs.rs/prometheus-client/latest/prometheus_client/metrics/gauge/index.html)

## Histogram

Um _histogram_ (histograma, em português) amostra observações (geralmente coisas
como a duração das requisições ou tamanhos de resposta) e os conta em intervalos
configuráveis.
Também fornece a soma de todos os valores observados.

Um histogram com um nome de métrica base `<basename>` expõe várias séries
temporais durante uma coleta:

- Counters cumulativos para os intervalos de observação, expostos como
  `<basename>_bucket{le="<limite superior inclusivo>"}`.
- A **soma total** de todos os valores observados, exposta como
  `<basename>_sum`.
- A **contagem** de eventos observados, exposta como `<basename>_count`
  (idêntico a `<basename>_bucket{le="+Inf"}` acima).

Use a
[função `histogram_quantile()`](/docs/prometheus/latest/querying/functions/#histogram_quantile)
para calcular quantis a partir de histograms ou mesmo agregações de histograms.
Um histogram também é adequado para calcular uma
[pontuação Apdex](http://en.wikipedia.org/wiki/Apdex).
Ao operar com intervalos (buckets), lembre-se de que o histogram é
[cumulativo](https://en.wikipedia.org/wiki/Histogram#Cumulative_histogram).
Consulte [histograms e summaries](/docs/practices/histograms) para obter
detalhes sobre o uso de histograms e as diferenças em relação aos
[summaries](#summary).

NOTA: A partir do Prometheus v2.40, há suporte experimental para histograms
nativos.
Um histogram nativo requer apenas uma série temporal, que inclui um número
dinâmico de intervalos (buckets), além da soma e da contagem de observações.
Os histograms nativos permitem uma resolução muito maior a uma fração do custo.
A documentação detalhada será disponibilizada assim que os histograms nativos
estiverem mais próximos de se tornarem um recurso estável.

NOTA: A partir do Prometheus v3.0, os valores do rótulo `le` dos histograms
clássicos são normalizados durante a ingestão para seguir o formato dos
[Números Canônicos do OpenMetrics](https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#considerations-canonical-numbers).

Documentação de uso da biblioteca cliente para histograms:

- [Go](http://godoc.org/github.com/prometheus/client_golang/prometheus#Histogram)
- [Java](https://prometheus.github.io/client_java/getting-started/metric-types/#histogram)
- [Python](https://prometheus.github.io/client_python/instrumenting/histogram/)
- [Ruby](https://github.com/prometheus/client_ruby#histogram)
- [.Net](https://github.com/prometheus-net/prometheus-net#histogram)
- [Rust](https://docs.rs/prometheus-client/latest/prometheus_client/metrics/histogram/index.html)

## Summary

Semelhante a um _histogram_, um _summary_ (resumo, em português) amostra
observações (geralmente coisas como duração de requisições e tamanhos de
respostas).
Embora também forneça uma contagem total de observações e uma soma de todos os
valores observados, ele calcula quantis configuráveis em uma janela de tempo
deslizante.

Um summary com um nome de métrica base `<basename>` expõe várias séries
temporais durante uma coleta:

- **Quantis-φ** em fluxo contínuo (0 ≤ φ ≤ 1) de eventos observados, expostos
  como `<basename>{quantile="<φ>"}`.
- A **soma total** de todos os valores observados, exposta como
  `<basename>_sum`.
- A **contagem** de eventos observados, exposta como `<basename>_count`.

Consulte [histograms e summaries](/docs/practices/histograms) para explicações
detalhadas sobre quantis-φ, uso de summary e diferenças em relação a
[histograms](#histogram).

NOTA: A partir do Prometheus v3.0, os valores do rótulo `quantile` são
normalizados durante a ingestão para seguir o formato dos
[Números Canônicos do OpenMetrics](https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#considerations-canonical-numbers).

Documentação de uso da biblioteca cliente para summaries:

- [Go](http://godoc.org/github.com/prometheus/client_golang/prometheus#Summary)
- [Java](https://prometheus.github.io/client_java/getting-started/metric-types/#summary)
- [Python](https://prometheus.github.io/client_python/instrumenting/summary/)
- [Ruby](https://github.com/prometheus/client_ruby#summary)
- [.Net](https://github.com/prometheus-net/prometheus-net#summary)
