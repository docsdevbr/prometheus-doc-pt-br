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

source_url: https://github.com/prometheus/docs/blob/main/docs/introduction/glossary.md
revision: f2dfa4b406ac54709829fd1411578fed8d969eec
status: ready

title: Glossário
sort_rank: 9
---

## Adaptador de gravação remota

Nem todos os sistemas suportam diretamente a gravação remota.
Um adaptador de gravação remota fica entre o Prometheus e outro sistema,
convertendo as amostras na gravação remota em um formato que o outro sistema
possa entender.

## Adaptador de leitura remota

Nem todos os sistemas suportam diretamente a leitura remota.
Um adaptador de leitura remota fica entre o Prometheus e outro sistema,
convertendo as requisições e respostas de séries temporais entre eles.

## Alerta

Um alerta é o resultado de uma regra de alerta no Prometheus que está em
execução.
Os alertas são enviados do Prometheus para o Alertmanager.

## Alertmanager

O [Alertmanager](/docs/alerting/latest/overview/) recebe alertas, os agrega em
grupos, remove duplicatas, silencia, limita a taxa de envio e, em seguida, envia
notificações por e-mail, PagerDuty, Slack, etc.

## Alvo

Um alvo é a definição de um objeto a ser coletado.
Por exemplo, quais rótulos aplicar, qualquer autenticação necessária para
conectar ou outras informações que definem como a coleta ocorrerá.

## Amostra

Uma amostra é um valor único em um ponto específico no tempo em uma série
temporal.

No Prometheus, cada amostra consiste em um valor float64 e um timestamp com
precisão de milissegundos.

## Biblioteca cliente

Uma biblioteca cliente é uma biblioteca em alguma linguagem (por exemplo, Go,
Java, Python, Ruby) que facilita a instrumentação direta do seu código, a
escrita de coletores personalizados para extrair métricas de outros sistemas e
expor as métricas ao Prometheus.

## Coletor

Um coletor é parte de um exportador que representa um conjunto de métricas.
Pode ser uma única métrica se fizer parte da instrumentação direta, ou várias
métricas se estiver extraindo métricas de outro sistema.

## Endpoint

Uma fonte de métricas que podem ser coletadas, geralmente correspondente a um
único processo.

## Endpoint de gravação remota

Um endpoint de gravação remota é o sistema com o qual o Prometheus se comunica
ao realizar uma gravação remota.

## Endpoint de leitura remota

Um endpoint de leitura remota é o sistema com o qual o Prometheus se comunica ao
realizar uma leitura remota.

## Exportador

Um exportador é um binário executado com a aplicação da qual você deseja obter
métricas.
O exportador expõe métricas do Prometheus, geralmente convertendo métricas
expostas em um formato não compatível com o Prometheus para um formato suportado
pelo Prometheus.

## Gravação remota

A gravação remota é um recurso do Prometheus que permite o envio de amostras
ingeridas em tempo real para outros sistemas, como armazenamento de longo prazo.

## Instância

Uma instância é um rótulo que identifica exclusivamente um alvo em uma tarefa.

## Instrumentação direta

A instrumentação direta é a instrumentação adicionada diretamente no
código-fonte de um programa, usando uma
[biblioteca cliente](#biblioteca-cliente).

## Leitura remota

A leitura remota é um recurso do Prometheus que permite a leitura transparente
de séries temporais de outros sistemas (como armazenamento de longo prazo) como
parte de consultas.

## Mixin

Um mixin é um conjunto reutilizável e extensível de alertas do Prometheus,
regras de gravação e painéis do Grafana para um componente ou sistema
específico.
Os mixins são normalmente empacotados usando o [Jsonnet](https://jsonnet.org/) e
podem ser combinados para criar configurações de monitoramento abrangentes.
Eles permitem o monitoramento padronizado em componentes de infraestrutura
semelhantes.

## Notificação

Uma notificação representa um grupo de um ou mais alertas e é enviada pelo
Alertmanager para e-mail, PagerDuty, Slack, etc.

## Ponte

Uma ponte é um componente que recebe amostras de uma biblioteca cliente e as
expõe a um sistema de monitoramento que não seja o Prometheus.
Por exemplo, os clientes Python, Go e Java podem exportar métricas para o
Graphite.

## Promdash

O Promdash era um construtor de dashboards nativo do Prometheus.
Ele foi descontinuado e substituído pelo [Grafana](../visualization/grafana.md).

## Prometheus

Prometheus geralmente se refere ao binário principal do sistema Prometheus.
Também pode se referir a todo o sistema de monitoramento Prometheus.

## PromQL

[PromQL](/docs/prometheus/latest/querying/basics/) é a Linguagem de Consulta do
Prometheus.
Ela permite uma ampla gama de operações, incluindo agregação, segmentação e
análise, previsão e junções.

## Pushgateway

O [Pushgateway](../instrumenting/pushing.md) persiste o envio mais recente de
métricas de trabalhos em lote.
Isso permite que o Prometheus colete suas métricas após a conclusão dos
trabalhos.

## Regras de gravação

As regras de gravação pré-computam expressões frequentemente necessárias ou
computacionalmente custosas e salvam seus resultados como um novo conjunto de
séries temporais.

## Séries temporais

As séries temporais do Prometheus são fluxos de valores com timestamp
pertencentes à mesma métrica e ao mesmo conjunto de dimensões rotuladas.
O Prometheus armazena todos os dados como séries temporais.

## Silenciamento

Um silenciamento no Alertmanager impede que alertas com rótulos correspondentes
ao silenciamento sejam incluídos nas notificações.

## Tarefa

Um conjunto de alvos com o mesmo propósito, por exemplo, monitorar um grupo de
processos semelhantes replicados para escalabilidade ou confiabilidade, é
chamado de tarefa.
