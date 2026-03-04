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

source_url: https://github.com/prometheus/docs/blob/main/docs/introduction/roadmap.md
revision: 8bdb919e820ad27adc12fc66daf38531c3d9a801
status: ready

title: Roadmap
sort_rank: 6
---

A seguir, apresentamos apenas uma seleção de alguns dos principais recursos que
planejamos implementar em breve.
Para obter uma visão geral mais completa dos recursos planejados e do trabalho
atual, consulte os rastreadores de issues dos vários repositórios, por exemplo,
o [servidor Prometheus](https://github.com/prometheus/prometheus/issues).

## Suporte a metadados de métricas no servidor

Atualmente, os tipos de métricas e outros metadados são usados apenas nas
bibliotecas do cliente e no formato de exposição, mas não são persistidos ou
utilizados no servidor Prometheus.
Planejamos utilizar esses metadados no futuro.
O primeiro passo é agregar esses dados na memória no Prometheus e
disponibilizá-los por meio de um endpoint de API experimental.

## Adoção do OpenMetrics

O grupo de trabalho OpenMetrics está desenvolvendo um novo padrão para exposição
de métricas.
Planejamos oferecer suporte a esse formato em nossas bibliotecas de cliente e no
próprio Prometheus.

## Avaliações retroativas de regras

Adicionar suporte para avaliações retroativas de regras usando preenchimento
retroativo.

## TLS e autenticação em endpoints de serviço HTTP

O TLS e a autenticação estão sendo implementados gradualmente no Prometheus,
Alertmanager e nos exportadores oficiais.
Adicionar esse suporte tornará mais fácil para as pessoas implantarem
componentes do Prometheus com segurança, sem a necessidade de um proxy reverso
para adicionar esses recursos externamente.

## Suporte ao ecossistema

O Prometheus possui uma variedade de bibliotecas de clientes e exportadores.
Sempre há mais linguagens que poderiam ser suportadas ou sistemas dos quais
seria útil exportar métricas.
Apoiaremos o ecossistema na criação e expansão desses recursos.
