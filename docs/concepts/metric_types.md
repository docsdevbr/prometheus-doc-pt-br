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

source_url: https://github.com/prometheus/docs/blob/main/docs/concepts/metric_types.md
revision: 4933d0570cfd4cb44f1686cf63df5c0482594349
status: ready

title: Tipos de métricas
sort_rank: 2
---

As bibliotecas de instrumentação do Prometheus oferecem quatro tipos de métricas
principais.
Com exceção dos histogramas nativos, estes são atualmente diferenciados apenas
nas APIs das bibliotecas de instrumentação e nos protocolos de exposição.
O servidor Prometheus ainda não faz uso das informações de tipo e transforma
todos os tipos, exceto histogramas nativos, em séries temporais não tipadas de
valores de ponto flutuante.
Histogramas nativos, no entanto, são ingeridos como séries temporais de amostras
de histogramas compostos especiais.
No futuro, o Prometheus também poderá lidar com outros tipos de métricas como
[tipos compostos](/blog/2026/02/14/modernizing-prometheus-composite-samples/).
Há também trabalho em andamento para persistir as informações de tipo das
amostras simples de ponto flutuante.

## Counter

Um _counter_ (contador) é uma métrica cumulativa que representa um único
[counter monotonicamente crescente](https://en.wikipedia.org/wiki/Monotonic_function),
cujo valor só pode aumentar ou ser zerado na reinicialização.
Por exemplo, você pode usar um counter para representar o número de requisições
atendidas, tarefas concluídas ou erros.

Não use um counter para expor um valor que pode diminuir.
Por exemplo, não use um counter para o número de processos em execução; em vez
disso, use um gauge.

Documentação de uso da biblioteca de instrumentação para counters:

- [Go](http://godoc.org/github.com/prometheus/client_golang/prometheus#Counter)
- [Java](https://prometheus.github.io/client_java/getting-started/metric-types/#counter)
- [Python](https://prometheus.github.io/client_python/instrumenting/counter/)
- [Ruby](https://github.com/prometheus/client_ruby#counter)
- [.Net](https://github.com/prometheus-net/prometheus-net#counters)
- [Rust](https://docs.rs/prometheus-client/latest/prometheus_client/metrics/counter/index.html)

## Gauge

Um _gauge_ (medidor) é uma métrica que representa um único valor numérico que
pode subir e descer arbitrariamente.

Os gauges são normalmente usados para valores medidos, como temperaturas ou uso
atual de memória, mas também para "contagens" que podem subir e descer, como o
número de requisições simultâneas.

Documentação de uso da biblioteca de instrumentação para gauges:

- [Go](http://godoc.org/github.com/prometheus/client_golang/prometheus#Gauge)
- [Java](https://prometheus.github.io/client_java/getting-started/metric-types/#gauge)
- [Python](https://prometheus.github.io/client_python/instrumenting/gauge/)
- [Ruby](https://github.com/prometheus/client_ruby#gauge)
- [.Net](https://github.com/prometheus-net/prometheus-net#gauges)
- [Rust](https://docs.rs/prometheus-client/latest/prometheus_client/metrics/gauge/index.html)

## Histogram

Um _histogram_ (histograma) registra observações (geralmente coisas como
durações de requisições ou tamanhos de respostas) contando-as em buckets
(intervalos) configuráveis.
Ele também fornece a soma de todos os valores observados.
Como tal, um histograma é essencialmente um counter agrupado.
No entanto, um histograma também pode representar o estado atual de uma
distribuição, caso em que é chamado de _gauge histogram_.
Ao contrário dos histogramas comuns do tipo counter, gauge histograms são
raramente expostos diretamente por programas instrumentados e, portanto, não são
(ainda) usáveis em bibliotecas de instrumentação, mas são representados em
versões mais recentes do formato de exposição protobuf e no
[OpenMetrics](https://openmetrics.io/).
Eles também são criados regularmente por expressões PromQL.
Por exemplo, o resultado da aplicação da função `rate` a um counter histogram é
um gauge histogram, da mesma forma que o resultado da aplicação da função `rate`
a um counter é um gauge.

Histogramas existem em duas versões fundamentalmente diferentes: os mais
recentes _histogramas nativos_ e os mais antigos _histogramas clássicos_.

Um histograma nativo é exposto e ingerido como amostras compostas, onde cada
amostra representa a contagem e a soma das observações juntamente com um
conjunto dinâmico de buckets.

Um histograma clássico, no entanto, consiste em múltiplas séries temporais de
amostras simples de ponto flutuante.
Um histograma clássico com um nome de métrica base `<basename>` resulta na
seguinte série temporal:

- Counters cumulativos para os buckets de observação, expostos como
  `<basename>_bucket{le="<limite superior inclusivo>"}`.
- A **soma total** de todos os valores observados, exposta como
  `<basename>_sum`.
- A **contagem** de eventos observados, exposta como `<basename>_count`
  (idêntica a `<basename>_bucket{le="+Inf"}` acima).

Histogramas nativos são geralmente muito mais eficientes do que histogramas
clássicos, permitem uma resolução muito maior, não exigem configuração explícita
de limites de buckets durante a instrumentação e fornecem atomicidade quando
transferidos pela rede (por exemplo, via protocolo de gravação remota do
Prometheus, onde histogramas clássicos sofrem com possível transferência parcial
porque suas séries temporais constituintes são transferidas independentemente).
Seu esquema de agrupamento garante que eles sejam sempre agregáveis entre si,
mesmo que a resolução possa ter mudado, enquanto histogramas clássicos com
limites de buckets diferentes não são geralmente agregáveis.
Se a biblioteca de instrumentação que você está usando suporta histogramas
nativos (atualmente, este é o caso para Go e Java), você provavelmente deve
[preferir histogramas nativos em vez de histogramas clássicos](/docs/practices/histograms).

Se você tiver que usar histogramas clássicos por algum motivo, existe uma
maneira de obter pelo menos alguns dos benefícios dos histogramas nativos: você
pode configurar o Prometheus para ingerir histogramas clássicos em uma forma
especial de histogramas nativos, chamados de Histogramas Nativos com Limites de
Bucket Personalizados (NHCB).
Os NHCBs são armazenados como as mesmas amostras compostas que os histogramas
nativos comuns, proporcionando maior eficiência e transferências de rede
atômicas, semelhantes aos histogramas nativos regulares.
No entanto, os buckets dos NHCBs ainda têm o mesmo layout que em suas
contrapartes clássicas, configurados estaticamente durante a instrumentação, com
a mesma resolução e intervalo limitados e os mesmos problemas de agregabilidade
ao alterar os limites dos buckets.

Use a função
[`histogram_quantile()`](/docs/prometheus/latest/querying/functions/#histogram_quantile)
para calcular quantis a partir de histogramas ou mesmo agregações de
histogramas.
Ela funciona tanto para histogramas clássicos quanto nativos, usando uma sintaxe
ligeiramente diferente.
Os histogramas também são adequados para calcular um
[score Apdex](http://en.wikipedia.org/wiki/Apdex).

Você pode operar diretamente nos buckets de um histograma clássico, pois eles
são representados como séries individuais
(chamadas `<basename>_bucket{le="<limite superior inclusivo>"}`, conforme
descrito acima).
Lembre-se, no entanto, que esses buckets são
[cumulativos](https://en.wikipedia.org/wiki/Histogram#Cumulative_histogram), ou
seja, cada bucket conta todas as observações menores ou iguais ao limite
superior fornecido como rótulo.
Com histogramas nativos, você pode examinar observações dentro de limites
específicos com a função
[`histogram_fraction()`](/docs/prometheus/latest/querying/functions/#histogram_fraction)
(para calcular frações de observações) e os [operadores de recorte]() (para
filtrar a faixa de observações desejada).

Consulte [histograms e summaries](/docs/practices/histograms) para obter
detalhes sobre o uso de histogramas e as diferenças em relação aos
[summaries](#summary).

NOTA: A partir do Prometheus v3.0, os valores do rótulo `le` dos histogramas
clássicos são normalizados durante a ingestão para seguir o formato dos
[Números Canônicos do OpenMetrics](https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#considerations-canonical-numbers).

Documentação de uso da biblioteca de instrumentação para histogramas:

- [Go](http://godoc.org/github.com/prometheus/client_golang/prometheus#Histogram)
- [Java](https://prometheus.github.io/client_java/getting-started/metric-types/#histogram)
- [Python](https://prometheus.github.io/client_python/instrumenting/histogram/)
- [Ruby](https://github.com/prometheus/client_ruby#histogram)
- [.Net](https://github.com/prometheus-net/prometheus-net#histogram)
- [Rust](https://docs.rs/prometheus-client/latest/prometheus_client/metrics/histogram/index.html)

## Summary

Semelhante a um _histograma_, um _summary_ (resumo) amostra observações
(geralmente coisas como duração de requisições e tamanhos de respostas).
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
detalhadas sobre quantis-φ, uso de summaries e diferenças em relação a
[histogramas](#histogram).

NOTA: A partir do Prometheus v3.0, os valores do rótulo `quantile` são
normalizados durante a ingestão para seguir o formato dos
[Números Canônicos do OpenMetrics](https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#considerations-canonical-numbers).

Documentação de uso da biblioteca de instrumentação para summaries:

- [Go](http://godoc.org/github.com/prometheus/client_golang/prometheus#Summary)
- [Java](https://prometheus.github.io/client_java/getting-started/metric-types/#summary)
- [Python](https://prometheus.github.io/client_python/instrumenting/summary/)
- [Ruby](https://github.com/prometheus/client_ruby#summary)
- [.Net](https://github.com/prometheus-net/prometheus-net#summary)
