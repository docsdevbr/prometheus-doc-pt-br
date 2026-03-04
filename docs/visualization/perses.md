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

source_url: https://github.com/prometheus/docs/blob/main/docs/visualization/perses.md
revision: 19b2d910956d0aac4cb5b1358cbe17d33be649cf
status: ready

title: Suporte do Perses para Prometheus
nav_title: Perses
sort_rank: 3
---

[Perses](https://perses.dev) é uma plataforma de visualização e dashboards de
código aberto, projetada para observabilidade, com suporte nativo para
Prometheus como fonte de dados.
Ela permite que as pessoas usuárias criem, gerenciem e compartilhem dashboards
para monitorar métricas e visualizar dados.
O Perses visa fornecer uma alternativa simples, flexível e extensível a outras
ferramentas de dashboards, com foco na facilidade de uso, desenvolvimento
orientado pela comunidade, recursos GitOps e abordagem de dashboard como código.

Aqui está um exemplo de um painel do Perses consultando o Prometheus para obter
dados:

[![Captura de tela do Perses](/assets/docs/perses_prometheus.png)](/assets/docs/perses_prometheus.png)

## Instalação

Para instalar o Perses, consulte a
[documentação oficial do Perses](https://perses.dev/perses/docs/installation/in-a-container/).

## Uso

Por padrão, o Perses estará escutando na porta `8080`.
Você pode acessar a interface web em `http://localhost:8080`.
Não há login por padrão.

### Criando uma fonte de dados do Prometheus

Para saber como configurar uma fonte de dados no Perses, consulte a
[documentação do Perses](https://perses.dev/perses/docs/concepts/datasources).
Após configurar essa conexão com sua instância do Prometheus, você poderá
consultá-la a partir das visualizações Dashboard e Explore.

### Importando dashboards pré-construídos

O Perses oferece um conjunto de dashboards pré-construídos que você pode
importar para sua instância.
Esses dashboards são mantidos pela comunidade e podem ser encontrados no
[repositório de dashboards do Perses](https://github.com/perses/community-dashboards).
