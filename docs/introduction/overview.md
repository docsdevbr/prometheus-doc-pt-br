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

source_url: https://github.com/prometheus/docs/blob/main/docs/introduction/overview.md
revision: 8bdb919e820ad27adc12fc66daf38531c3d9a801
status: ready

title: Visão geral
sort_rank: 1
---

# Visão geral

## O que é o Prometheus?

[Prometheus](https://github.com/prometheus) é um conjunto de ferramentas de
código aberto para monitoramento de sistemas e alertas, originalmente
desenvolvido na [SoundCloud](http://soundcloud.com).
Desde sua criação em 2012, muitas empresas e organizações adotaram o Prometheus,
e o projeto possui uma [comunidade](/community/) de pessoas desenvolvedoras e
usuárias muito ativa.
Atualmente, é um projeto de código aberto independente e mantido sem vínculo com
qualquer empresa.
Para enfatizar isso e esclarecer a estrutura de governança do projeto, o
Prometheus se juntou à [Cloud Native Computing Foundation](https://cncf.io/) em
2016 como o segundo projeto hospedado, depois do
[Kubernetes](http://kubernetes.io/).

O Prometheus coleta e armazena suas métricas como dados de séries temporais, ou
seja, as informações de métricas são armazenadas com o registro de data e hora
em que foram registradas, juntamente com pares de chave-valor opcionais chamados
rótulos.

Para obter visões gerais mais detalhadas do Prometheus, consulte os recursos
listados na seção [Mídias](/introduction/media.md).

### Recursos

Os principais recursos do Prometheus são:

- Um [modelo de dados](/concepts/data_model.md) multidimensional com dados de
  séries temporais identificados por nome de métrica e pares de chave/valor.
- PromQL, uma [linguagem de consulta flexível](/prometheus/querying/basics.md)
  para aproveitar essa dimensionalidade.
- Nenhuma dependência em armazenamento distribuído; nós de servidor único são
  autônomos.
- A coleta de séries temporais acontece por meio de um modelo pull via HTTP.
- O [envio de séries temporais](/instrumenting/pushing.md) é suportado por meio
  de um gateway intermediário.
- Os alvos são descobertos por meio da descoberta de serviço ou por configuração
  estática.
- Vários modos de suporte a gráficos e dashboards.

### O que são métricas?

Métricas são medições numéricas, em termos simples.
O termo série temporal se refere ao registro de mudanças ao longo do tempo.
O que as pessoas usuárias querem medir difere de aplicação para aplicação.
Para um servidor web, pode ser o tempo de resposta das requisições; para um
banco de dados, pode ser o número de conexões ativas ou consultas ativas, e
assim por diante.

As métricas desempenham um papel importante na compreensão do comportamento da
sua aplicação.
Suponha que você esteja executando uma aplicação web e descubra que ela está
lenta.
Para entender o que está acontecendo com a sua aplicação, você precisará de
algumas informações.
Por exemplo, quando o número de requisições é alto, a aplicação pode ficar
lenta.
Se você tiver a métrica de contagem de requisições, poderá determinar a causa e
aumentar o número de servidores para lidar com a carga.

### Componentes

O ecossistema Prometheus consiste em diversos componentes, muitos dos quais são
opcionais:

- O [servidor Prometheus](https://github.com/prometheus/prometheus) principal
  que coleta e armazena dados de séries temporais.
- [Bibliotecas de cliente](/instrumenting/clientlibs.md) para instrumentar o
  código da aplicação.
- Um [gateway de push](https://github.com/prometheus/pushgateway) para suportar
  tarefas de curta duração.
- [Exportadores](/instrumenting/exporters.md) de propósito especial para
  serviços como HAProxy, StatsD, Graphite, etc.
- Um [gerenciador de alertas](https://github.com/prometheus/alertmanager) para
  lidar com alertas.
- Diversas ferramentas de suporte.

A maioria dos componentes do Prometheus são escritos em
[Go](https://golang.org/), tornando-os fáceis de construir e implementar como
binários estáticos.

### Arquitetura

Este diagrama ilustra a arquitetura do Prometheus e alguns componentes do seu
ecossistema:

![Arquitetura do Prometheus](/assets/docs/architecture.png)

O Prometheus coleta métricas de trabalhos instrumentados, diretamente ou por
meio de um gateway intermediário de push para trabalhos de curta duração.
Ele armazena todas as amostras coletadas localmente e executa regras sobre esses
dados para agregar e registrar novas séries temporais a partir de dados
existentes ou gerar alertas.
O [Grafana](https://grafana.com/) ou outros consumidores de API podem ser usados
para visualizar os dados coletados.

## Quando ele é adequado?

O Prometheus funciona bem para registrar qualquer série temporal puramente
numérica.
Ele se encaixa tanto no monitoramento centrado em máquinas quanto no
monitoramento de arquiteturas altamente dinâmicas orientadas a serviços.
Em um mundo de microsserviços, seu suporte à coleta e consulta de dados
multidimensionais é um ponto forte em particular.

O Prometheus foi projetado para confiabilidade, para ser o sistema ao qual você
recorre durante uma queda de serviço, permitindo diagnosticar problemas
rapidamente.
Cada servidor Prometheus é independente, não dependendo de armazenamento em rede
ou outros serviços remotos.
Você pode confiar nele quando outras partes da sua infraestrutura estiverem com
problemas, e não precisa configurar uma infraestrutura extensa para usá-lo.

## Quando ele não é adequado?

O Prometheus prioriza a confiabilidade.
Você sempre pode visualizar as estatísticas disponíveis sobre o seu sistema,
mesmo em condições de falha.
Se você precisa de 100% de precisão, como para faturamento por requisição, o
Prometheus não é uma boa escolha, pois os dados coletados provavelmente não
serão detalhados e completos o suficiente.
Nesse caso, seria melhor usar outro sistema para coletar e analisar os dados
para faturamento e o Prometheus para o restante do seu monitoramento.
