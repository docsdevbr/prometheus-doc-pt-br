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

source_url: https://github.com/prometheus/docs/blob/main/docs/introduction/faq.md
revision: 8bdb919e820ad27adc12fc66daf38531c3d9a801
status: ready

title: Perguntas frequentes
nav_title: FAQ
sort_rank: 5
---

## Geral

### O que é o Prometheus?

O Prometheus é um conjunto de ferramentas de código aberto para monitoramento e
alertas de sistemas com um ecossistema ativo.
É o único sistema diretamente suportado pelo
[Kubernetes](https://kubernetes.io/) e o padrão de fato em todo o
[ecossistema nativo da nuvem](https://landscape.cncf.io/).
Consulte a [visão geral](/docs/introduction/overview/).

### Como o Prometheus se compara a outros sistemas de monitoramento?

Consulte a página de [comparação](/docs/introduction/comparison/).

### Quais são as dependências do Prometheus?

O servidor principal do Prometheus é executado de forma independente como um
único binário monolítico e não possui dependências externas.

#### Isso é nativo da nuvem?

Sim.

O modelo operacional nativo da nuvem é flexível, rompendo com as antigas
fronteiras de serviço para permitir implantações mais flexíveis e escaláveis.

A
[descoberta de serviços](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)
do Prometheus se integra com a maioria das ferramentas e nuvens.
Seu modelo de dados dimensionais e sua escalabilidade para dezenas de milhões de
séries ativas permitem o monitoramento de grandes implantações nativas da nuvem.
Sempre há compromissos a serem feitas ao executar serviços, e o Prometheus
prioriza, acima de tudo, o envio confiável de alertas para pessoas.

### É possível tornar o Prometheus altamente disponível?

Sim, execute servidores Prometheus idênticos em duas ou mais máquinas separadas.
Alertas idênticos serão desduplicados pelo
[Alertmanager](https://github.com/prometheus/alertmanager).

O Alertmanager oferece suporte à
[alta disponibilidade](https://github.com/prometheus/alertmanager#high-availability)
interconectando várias instâncias do Alertmanager para criar um cluster.
As instâncias de um cluster se comunicam usando um protocolo de comunicação
gerenciado pela biblioteca
[Memberlist da HashiCorp](https://github.com/hashicorp/memberlist).

### Me disseram que o Prometheus "não é escalável".

Isso geralmente é mais uma afirmação de marketing do que qualquer outra coisa.

Uma única instância do Prometheus pode ter um desempenho melhor do que alguns
sistemas que se posicionam como soluções de armazenamento de longo prazo para o
Prometheus.
Você pode executar o Prometheus de forma confiável com dezenas de milhões de
séries ativas.

Se precisar de mais do que isso, existem várias opções.
O artigo
[Escalando e federando o Prometheus](https://www.robustperception.io/scaling-and-federating-prometheus/)
no blog da Robust Perception é um bom ponto de partida, assim como os sistemas
de armazenamento de longo prazo listados em nossa
[página de integrações](https://prometheus.io/docs/operating/integrations/#remote-endpoints-and-storage).

### Em que linguagem o Prometheus foi escrito?

A maioria dos componentes do Prometheus são escritos em Go.
Alguns também são escritos em Java, Python e Ruby.

### Quão estáveis são os recursos, formatos de armazenamento e APIs do Prometheus?

Todos os repositórios da organização Prometheus no GitHub que atingiram a versão
1.0.0 seguem amplamente o [versionamento semântico](http://semver.org/).
Alterações que quebram a compatibilidade são indicadas por incrementos na versão
principal.
Exceções são possíveis para componentes experimentais, que são claramente
marcados como tal nos anúncios.

Mesmo os repositórios que ainda não atingiram a versão 1.0.0 são, em geral,
bastante estáveis.
Nosso objetivo é um processo de lançamento adequado e um lançamento eventual da
versão 1.0.0 para cada repositório.
Em qualquer caso, alterações que quebram a compatibilidade serão apontadas nas
notas de lançamento (marcadas por `[CHANGE]`) ou comunicadas claramente para
componentes que ainda não possuem lançamentos formais.

### Por que usar o método pull em vez do push?

O método pull via HTTP oferece diversas vantagens:

- Você pode iniciar instâncias de monitoramento adicionais conforme necessário,
  por exemplo, em seu notebook durante o desenvolvimento de alterações.
- Você pode verificar com mais facilidade e confiabilidade se um alvo está
  inativo.
- Você pode acessar manualmente um alvo e inspecionar seu status com um
  navegador web.

Em geral, acreditamos que o método pull é ligeiramente melhor do que o push, mas
isso não deve ser considerado um fator decisivo na escolha de um sistema de
monitoramento.

Para casos em que o push é necessário, oferecemos o
[Pushgateway](/docs/instrumenting/pushing/).

### Como alimentar o Prometheus com logs?

Resposta curta: Não faça isso!
Use algo como o [Grafana Loki](https://grafana.com/oss/loki/) ou o
[OpenSearch](https://opensearch.org/).

Resposta longa: O Prometheus é um sistema para coletar e processar métricas, não
um sistema de logging de eventos.
A postagem do blog do Grafana,
[Logs and Metrics and Graphs, Oh My!](https://grafana.com/blog/2016/01/05/logs-and-metrics-and-graphs-oh-my/),
fornece mais detalhes sobre as diferenças entre logs e métricas.

Se você deseja extrair métricas do Prometheus a partir de logs de aplicações, o
Grafana Loki foi projetado exatamente para isso.
Consulte a documentação de
[consultas de métricas](https://grafana.com/docs/loki/latest/logql/metric_queries/)
do Loki.

### Quem escreveu o Prometheus?

O Prometheus foi inicialmente criado de forma privada por
[Matt T. Proud](http://www.matttproud.com) e
[Julius Volz](http://juliusv.com).
A maioria do seu desenvolvimento inicial foi patrocinada pela
[SoundCloud](https://soundcloud.com).

Atualmente, ele é mantido e expandido por uma ampla gama de
[empresas](https://prometheus.devstats.cncf.io/d/5/companies-table?orgId=1) e
[pessoas](https://prometheus.io/governance).

### Sob qual licença o Prometheus é distribuído?

O Prometheus é distribuído sob a licença
[Apache 2.0](https://github.com/prometheus/prometheus/blob/main/LICENSE).

### Qual é o plural de Prometheus?

Após [extensa pesquisa](https://youtu.be/B_CDeYrqxjQ), determinou-se que o
plural correto de 'Prometheus' é 'Prometheis'.

Se você não se lembra disso, "instâncias do Prometheus" é uma boa solução
alternativa.

### Posso recarregar a configuração do Prometheus?

Sim, enviar um sinal `SIGHUP` para o processo do Prometheus ou uma requisição
HTTP POST para o endpoint `/-/reload` recarregará e aplicará o arquivo de
configuração.
Os diversos componentes tentam lidar com falhas de forma adequada.

### Posso enviar alertas?

Sim, com o [Alertmanager](https://github.com/prometheus/alertmanager).

Oferecemos suporte ao envio de alertas por
[e-mail, diversas integrações nativas](https://prometheus.io/docs/alerting/latest/configuration/)
e um
[sistema de webhook ao qual qualquer pessoa pode adicionar integrações](https://prometheus.io/docs/operating/integrations/#alertmanager-webhook-receiver).

### Posso criar dashboards?

Sim, recomendamos o [Grafana](/docs/visualization/grafana/) para uso em
produção.

Também existem [modelos de console](/docs/visualization/consoles/).

### Posso alterar o fuso horário? Por que tudo está em UTC?

Para evitar qualquer tipo de confusão com fusos horários, especialmente quando
se trata do chamado horário de verão, decidimos usar exclusivamente o tempo Unix
internamente e UTC para fins de exibição em todos os componentes do Prometheus.
Uma seleção cuidadosa de fuso horário pode ser introduzida na interface da
pessoa usuária.
Contribuições são bem-vindas.
Consulte [issue #500](https://github.com/prometheus/prometheus/issues/500) para
o estado atual deste projeto.

## Instrumentação

### Quais linguagens possuem bibliotecas de instrumentação?

Existem diversas bibliotecas de cliente para instrumentar seus serviços com
métricas do Prometheus.
Consulte a documentação das
[bibliotecas de cliente](/docs/instrumenting/clientlibs/) para obter detalhes.

Se você estiver interessado em contribuir com uma biblioteca de cliente para uma
nova linguagem, consulte os
[formatos de exposição](/docs/instrumenting/exposition_formats/).

### Posso monitorar máquinas?

Sim, o [Node Exporter](https://github.com/prometheus/node_exporter) expõe um
extenso conjunto de métricas em nível de máquina no Linux e outros sistemas
Unix, como uso de CPU, memória, utilização de disco, preenchimento do sistema de
arquivos e largura de banda da rede.

### Posso monitorar dispositivos de rede?

Sim, o [SNMP Exporter](https://github.com/prometheus/snmp_exporter) permite o
monitoramento de dispositivos que suportam SNMP.
Para redes industriais, também existe um
[Modbus Exporter](https://github.com/RichiH/modbus_exporter).

### Posso monitorar jobs em lote?

Sim, usando o [Pushgateway](/docs/instrumenting/pushing/).
Consulte também as
[melhores práticas](/docs/practices/instrumentation/#batch-jobs) para
monitoramento de jobs em lote.

### Quais aplicações o Prometheus pode monitorar imediatamente?

Consulte
[a lista de exportadores e integrações](/docs/instrumenting/exporters/).

### Posso monitorar aplicações JVM via JMX?

Sim, para aplicações que você não pode instrumentar diretamente com o cliente
Java, você pode usar o
[Exportador JMX](https://github.com/prometheus/jmx_exporter) seja de forma
independente ou como um Agente Java.

### Qual o impacto da instrumentação no desempenho?

O desempenho pode variar entre diferentes bibliotecas de cliente e linguagens.
Para Java,
[benchmarks](https://github.com/prometheus/client_java/blob/main/benchmarks/README.md)
indicam que incrementar um contador/medidor com o cliente Java levará 12-17ns,
dependendo da contenção.
Isso é insignificante para todos os códigos, exceto os mais críticos em termos
de latência.

## Implementação

### Por que todos os valores de amostra são floats de 64 bits?

Nos limitamos a floats de 64 bits para simplificar o projeto.
O
[formato de ponto flutuante binário de dupla precisão IEEE 754](http://en.wikipedia.org/wiki/Double-precision_floating-point_format)
suporta precisão inteira para valores de até 2<sup>53</sup>.
O suporte a inteiros nativos de 64 bits seria útil (apenas) se você precisasse
de precisão inteira acima de 2<sup>53</sup>, mas abaixo de 2<sup>63</sup>.
Em princípio, o suporte a diferentes tipos de valores de amostra (incluindo
algum tipo de inteiro grande, suportando até mais de 64 bits) poderia ser
implementado, mas não é uma prioridade no momento.
Um contador, mesmo que incrementado um milhão de vezes por segundo, só
apresentará problemas de precisão após mais de 285 anos.
