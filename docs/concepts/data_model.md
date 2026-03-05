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

source_url: https://github.com/prometheus/docs/blob/main/docs/concepts/data_model.md
revision: 8bdb919e820ad27adc12fc66daf38531c3d9a801
status: ready

title: Modelo de dados
sort_rank: 1
---

O Prometheus armazena fundamentalmente todos os dados como
[_séries temporais_](http://en.wikipedia.org/wiki/Time_series): fluxos de
valores com timestamp pertencentes à mesma métrica e ao mesmo conjunto de
dimensões rotuladas.
Além das séries temporais armazenadas, o Prometheus pode gerar séries temporais
derivadas temporárias como resultado de consultas.

## Nomes e rótulos de métricas

Cada série temporal é identificada exclusivamente pelo nome da métrica e por
pares de chave-valor opcionais chamados rótulos.

***Nomes de métricas:***

- Os nomes das métricas DEVEM especificar a característica geral de um sistema
  que está sendo medida (por exemplo, `http_requests_total` - o número total de
  requisições HTTP recebidas).
- Os nomes das métricas PODEM usar quaisquer caracteres UTF-8.
- Os nomes das métricas DEVEM corresponder à expressão regular
  `[a-zA-Z_:][a-zA-Z0-9_:]*` para melhor experiência e compatibilidade (consulte
  o alerta abaixo).
  Nomes de métricas fora desse conjunto exigirão o uso de aspas, por exemplo,
  quando usados em PromQL (consulte o [guia UTF-8](../guides/utf8.md#querying)).

NOTA: Dois pontos (':') são reservados para regras de gravação definidas pela
pessoa usuária.
Eles NÃO DEVEM ser usados por exportadores ou instrumentação direta.

***Rótulos de métricas:***

Os rótulos permitem capturar diferentes instâncias do mesmo nome de métrica.
Por exemplo: todas as requisições HTTP que usaram o método `POST` para o
manipulador `/api/tracks`.
Referimo-nos a isso como o "modelo de dados dimensional" do Prometheus.
A linguagem de consulta permite filtragem e agregação com base nessas dimensões.
A alteração do valor de qualquer rótulo, incluindo a adição ou remoção de
rótulos, criará uma nova série temporal.

- Os nomes dos rótulos PODEM usar quaisquer caracteres UTF-8.
- Os nomes dos rótulos que começam com `__` (dois sublinhados) DEVEM ser
  reservados para uso interno do Prometheus.
- Os nomes dos rótulos DEVEM corresponder à expressão regular
  `[a-zA-Z_][a-zA-Z0-9_]*` para melhor experiência e compatibilidade (consulte o
  alerta abaixo).
  Nomes de rótulos fora dessa expressão regular precisarão ser colocados entre
  aspas, por exemplo: Quando usado em PromQL (consulte o
  [guia UTF-8](../guides/utf8.md#querying)).
- Os valores dos rótulos PODEM conter quaisquer caracteres UTF-8.
- Rótulos com um valor vazio são considerados equivalentes a rótulos
  inexistentes.

ALERTA: O suporte a [UTF-8](../guides/utf8.md) para nomes de métricas e rótulos
foi adicionado recentemente no Prometheus v3.0.0.
Pode levar algum tempo para que o ecossistema mais amplo (projetos e
fornecedores compatíveis com PromQL, ferramentas, instrumentação de terceiros,
coletores, etc.) adote novos mecanismos de citação, validação flexível, etc.
Para melhor compatibilidade, recomenda-se usar o conjunto de caracteres
recomendado ("DEVE").

INFORMAÇÃO: Consulte também as
[melhores práticas para nomear métricas e rótulos](/docs/practices/naming/).

## Amostras

As amostras formam os dados reais da série temporal.
Cada amostra consiste em:

- Um valor float64 ou um
  [histograma nativo](https://prometheus.io/docs/specs/native_histograms/).
- Um timestamp com precisão de milissegundos.

## Notação

Dado um nome de métrica e um conjunto de rótulos, as séries temporais são
frequentemente identificadas usando esta notação:

    <nome da métrica>{<nome do rótulo>="<valor do rótulo>", ...}

Por exemplo, uma série temporal com o nome da métrica `api_http_requests_total`
e os rótulos `method="POST"` e `handler="/messages"` poderia ser escrita assim:

    api_http_requests_total{method="POST", handler="/messages"}

Esta é a mesma notação usada pelo [OpenTSDB](http://opentsdb.net/).

Nomes com caracteres UTF-8 fora do conjunto recomendado devem ser colocados
entre aspas, usando a seguinte notação:

    {"<nome da métrica>", <nome do rótulo>="<valor do rótulo>", ...}

Como os nomes das métricas são representados internamente como um par de rótulos
com um nome de rótulo especial (`__name__="<nome da métrica>"`), também é
possível usar a seguinte notação:

    {__name__="<nome da métrica>", <nome do rótulo>="<valor do rótulo>", ...}
