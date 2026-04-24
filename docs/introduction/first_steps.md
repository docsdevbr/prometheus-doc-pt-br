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

source_url: https://github.com/prometheus/docs/blob/main/docs/introduction/first_steps.md
revision: 8bdb919e820ad27adc12fc66daf38531c3d9a801
status: ready

title: Primeiros passos com o Prometheus
nav_title: Primeiros passos
sort_rank: 3
---

Bem-vindo ao Prometheus!
O Prometheus é uma plataforma de monitoramento que coleta métricas de alvos
monitorados, extraindo dados de endpoints HTTP desses alvos.
Este guia mostrará como instalar, configurar e monitorar nosso primeiro recurso
com o Prometheus.
Você fará o download, a instalação e a execução do Prometheus.
Você também fará o download e a instalação de um exportador, uma ferramenta que
expõe dados de séries temporais em hosts e serviços.
Nosso primeiro exportador será o próprio Prometheus, que fornece uma ampla
variedade de métricas em nível de host sobre uso de memória, coleta de lixo e
muito mais.

## Baixando o Prometheus

[Baixe a versão mais recente](/download) do Prometheus para sua plataforma e, em
seguida, extraia-a:

```language-bash
tar xvfz prometheus-*.tar.gz
cd prometheus-*
```

O servidor Prometheus é um único binário chamado `prometheus` (ou
`prometheus.exe` no Microsoft Windows).
Podemos executar o binário e ver a ajuda sobre suas opções passando o parâmetro
`--help`.

```language-bash
./prometheus --help
usage: prometheus [<flags>]

The Prometheus monitoring server

. . .
```

Antes de iniciar o Prometheus, vamos configurá-lo.

## Configurando o Prometheus

A configuração do Prometheus é feita em [YAML](https://yaml.org/).
O download do Prometheus inclui um arquivo de configuração de exemplo chamado
`prometheus.yml`, que é um bom ponto de partida.

Removemos a maioria dos comentários do arquivo de exemplo para torná-lo mais
conciso (comentários são as linhas precedidas por um `#`).

```language-yaml
global:
  scrape_interval:     15s
  evaluation_interval: 15s

rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']
```

Existem três blocos de configuração no arquivo de configuração de exemplo:
`global`, `rule_files` e `scrape_configs`.

O bloco `global` controla a configuração global do servidor Prometheus.
Temos duas opções presentes.
A primeira, `scrape_interval`, controla a frequência com que o Prometheus coleta
dados dos alvos.
Você pode sobrescrever esse valor para alvos individuais.
Neste caso, a configuração global é coletar dados a cada 15 segundos.
A opção `evaluation_interval` controla a frequência com que o Prometheus avalia
as regras.
O Prometheus usa regras para criar novas séries temporais e gerar alertas.

O bloco `rule_files` especifica a localização de quaisquer regras que desejamos
que o servidor Prometheus carregue.
Por enquanto, não temos regras.

O último bloco, `scrape_configs`, controla quais recursos o Prometheus monitora.
Como o Prometheus também expõe dados sobre si como um endpoint HTTP, ele pode
coletar dados e monitorar sua própria integridade.
Na configuração padrão, existe uma única tarefa, chamada `prometheus`, que
coleta os dados de séries temporais expostos pelo servidor Prometheus.
A tarefa contém um único alvo configurado estaticamente, o `localhost` na porta
`9090`.
O Prometheus espera que as métricas estejam disponíveis em alvos no caminho
`/metrics`.
Portanto, esta tarefa padrão coleta dados através da URL:
http://localhost:9090/metrics.

Os dados de séries temporais retornados detalharão o estado e o desempenho do
servidor Prometheus.

Para uma especificação completa das opções de configuração, consulte a
[documentação de configuração](/docs/operating/configuration).

## Iniciando o Prometheus

Para iniciar o Prometheus com o arquivo de configuração recém-criado, acesse o
diretório que contém o binário do Prometheus e execute:

```language-bash
./prometheus --config.file=prometheus.yml
```

O Prometheus deve iniciar.
Você também deve conseguir acessar uma página de status sobre ele em
http://localhost:9090.
Aguarde cerca de 30 segundos para que ele colete dados sobre si a partir de seu
próprio endpoint de métricas HTTP.

Você também pode verificar se o Prometheus está fornecendo métricas sobre si
acessando seu próprio endpoint de métricas: http://localhost:9090/metrics.

## Usando o navegador de expressões

Vamos analisar alguns dados que o Prometheus coletou sobre si.
Para usar o navegador de expressões integrado do Prometheus, acesse
http://localhost:9090/graph e escolha a visualização "Table" na guia "Graph".

Como você pode ver em http://localhost:9090/metrics, uma métrica que o
Prometheus exporta sobre si é chamada `promhttp_metric_handler_requests_total`
(o número total de requisições `/metrics` que o servidor Prometheus atendeu).
Digite o seguinte no console de expressões:

```
promhttp_metric_handler_requests_total
```

Isso deve retornar várias séries temporais diferentes (juntamente com o último
valor registrado para cada uma), todas com o nome da métrica
`promhttp_metric_handler_requests_total`, mas com rótulos diferentes.
Esses rótulos designam diferentes status de requisição.

Se estivéssemos interessados apenas em requisições que resultaram no código HTTP
`200`, poderíamos usar esta consulta para recuperar essa informação:

```
promhttp_metric_handler_requests_total{code="200"}
```

Para contar o número de séries temporais retornadas, você poderia escrever:

```
count(promhttp_metric_handler_requests_total)
```

Para mais informações sobre a linguagem de expressões, consulte a
[documentação da linguagem de expressões](/docs/querying/basics/).

## Usando a interface de gráficos

Para gerar gráficos de expressões, acesse http://localhost:9090/graph e use a
guia "Graph".

Por exemplo, insira a seguinte expressão para gerar um gráfico da taxa de
requisições HTTP por segundo que retornam o código de status 200 no Prometheus
coletado automaticamente:

```
rate(promhttp_metric_handler_requests_total{code="200"}[1m])
```

Você pode experimentar com os parâmetros de intervalo do gráfico e outras
configurações.

## Monitorando outros alvos

Coletar métricas apenas do Prometheus não representa adequadamente as
capacidades do Prometheus.
Para ter uma ideia melhor do que o Prometheus pode fazer, recomendamos explorar
a documentação sobre outros exportadores.
O guia
[Monitorando métricas de hosts Linux ou macOS usando um exportador de nós](/docs/guides/node-exporter)
é um bom ponto de partida.

## Resumo

Neste guia, você instalou o Prometheus, configurou uma instância do Prometheus
para monitorar recursos e aprendeu alguns conceitos básicos sobre como trabalhar
com dados de séries temporais no navegador de expressões do Prometheus.
Para continuar aprendendo sobre o Prometheus, confira a
[Visão geral](/docs/introduction/overview) para algumas ideias sobre o que
explorar a seguir.
