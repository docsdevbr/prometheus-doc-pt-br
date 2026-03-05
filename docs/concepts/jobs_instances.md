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

source_url: https://github.com/prometheus/docs/blob/main/docs/concepts/jobs_instances.md
revision: 8bdb919e820ad27adc12fc66daf38531c3d9a801
status: ready

title: Jobs e instâncias
sort_rank: 3
---

Nos termos do Prometheus, um endpoint que você pode coletar dados é chamado de
_instância_, correspondendo geralmente a um único processo.
Uma coleção de instâncias com o mesmo propósito, um processo replicado para
escalabilidade ou confiabilidade, por exemplo, é chamada de _job_.

Por exemplo, um job de servidor de API com quatro instâncias replicadas:

- job: `api-server`
  - instance 1: `1.2.3.4:5670`
  - instance 2: `1.2.3.4:5671`
  - instance 3: `5.6.7.8:5670`
  - instance 4: `5.6.7.8:5671`

## Rótulos e séries temporais gerados automaticamente

Quando o Prometheus coleta dados de um alvo, ele anexa automaticamente alguns
rótulos à série temporal coletada, que servem para identificar o alvo coletado:

- `job`: O nome do job configurado ao qual o alvo pertence.
- `instance`: A parte `<host>:<porta>` da URL do alvo que foi coletada.

Se algum desses rótulos já estiver presente nos dados coletados, o comportamento
depende da opção de configuração `honor_labels`.
Consulte a
[documentação de configuração de coleta de dados](/docs/prometheus/latest/configuration/configuration/#scrape_config)
para obter mais informações.

Para cada coleta de dados de instância, o Prometheus armazena uma
[amostra](/docs/introduction/glossary#sample) na seguinte série temporal:

- `up{job="<job-name>", instance="<instance-id>"}`: `1` se a instância estiver
  íntegra, ou seja, acessível, ou `0` se a coleta de dados falhar.
- `scrape_duration_seconds{job="<job-name>", instance="<instance-id>"}`:
  duração da coleta de dados.
- `scrape_samples_post_metric_relabeling{job="<job-name>", instance="<instance-id>"}`:
  o número de amostras restantes após a aplicação da reetiquetagem de métricas.
- `scrape_samples_scraped{job="<job-name>", instance="<instance-id>"}`:
  o número de amostras que o alvo expôs.
- `scrape_series_added{job="<job-name>", instance="<instance-id>"}`:
  o número aproximado de novas séries nesta coleta. *Novo na versão 2.10*

A série temporal `up` é útil para monitorar a disponibilidade da instância.

Com o recurso
[`extra-scrape-metrics`](/docs/prometheus/latest/feature_flags/#extra-scrape-metrics),
várias métricas adicionais ficam disponíveis:

- `scrape_timeout_seconds{job="<job-name>", instance="<instance-id>"}`: O
  `scrape_timeout` configurado para um alvo.
- `scrape_sample_limit{job="<job-name>", instance="<instance-id>"}`: O
  `sample_limit` configurado para um alvo.
  Retorna zero se nenhum limite estiver configurado.
- `scrape_body_size_bytes{job="<job-name>", instance="<instance-id>"}`: O
  tamanho descompactado da resposta de coleta mais recente, se bem-sucedida.
  Coleções que falham porque o `body_size_limit` foi excedido retornam -1,
  outras falhas de coleta retornam 0.
