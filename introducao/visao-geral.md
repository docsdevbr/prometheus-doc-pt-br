---
source_url: https://github.com/prometheus/docs/blob/main/content/docs/introduction/overview.md
revision: d837d88c8853b1ce4d0d52c9a054fb0321ba8847
status: ready
---

# Visão geral

## O que é o Prometheus?

[Prometheus](https://github.com/prometheus) é um conjunto de ferramentas de
monitoramento de sistemas e alerta de código aberto originalmente criado na
[SoundCloud](http://soundcloud.com).
Desde seu início em 2012, muitas empresas e organizações adotaram o Prometheus,
e o projeto tem uma [comunidade](https://prometheus.io/community/) de pessoas
desenvolvedoras e usuárias muito ativa.
Agora é um projeto de código aberto independente e mantido independentemente de
qualquer empresa.
Para enfatizar isso e esclarecer a estrutura de governança do projeto, o
Prometheus se juntou à [Cloud Native Computing Foundation](https://cncf.io/) em
2016 como o segundo projeto hospedado, depois
do [Kubernetes](http://kubernetes.io/).

O Prometheus coleta e armazena suas métricas como dados de séries temporais, ou
seja, as informações de métricas são armazenadas com o registro de data e hora
em que foram registradas, juntamente com pares de chave-valor opcionais chamados
rótulos.

Para visões gerais mais elaboradas do Prometheus, consulte os recursos listados
na seção de [mídia](/introduction/media.md).

### Recursos

Os principais recursos do Prometheus são:

* um [modelo de dados](/concepts/data_model.md) multidimensional com dados de
  séries temporais identificados por nome de métrica e pares de chave/valor;
* PromQL, uma [linguagem de consulta flexível](/prometheus/querying/basics.md)
  para aproveitar essa dimensionalidade;
* nenhuma dependência em armazenamento distribuído; nós de servidor único são
  autônomos;
* a coleta de séries temporais acontece por meio de um modelo _pull_ sobre
  HTTP;
* o [envio de séries temporais](/instrumenting/pushing.md) é suportado por meio
  de um _gateway_ intermediário;
* os alvos são descobertos por meio da descoberta de serviço ou por configuração
  estática;
* vários modos de suporte de gráficos e painéis.

### O que são métricas?

Métricas são medições numéricas em termos leigos.
O termo série temporal se refere ao registro de mudanças ao longo do tempo.
O que os usuários querem medir difere de aplicação para aplicação.
Para um servidor _web_, pode ser o tempo das requisições; para um banco de
dados, pode ser o número de conexões ativas ou consultas ativas, e assim por
diante.

Métricas desempenham um papel importante na compreensão do motivo pelo qual sua
aplicação está funcionando de uma determinada maneira.
Vamos supor que você esteja executando uma aplicação _web_ e descubra que ela
está lenta.
Para saber o que está acontecendo com sua aplicação, você precisará de algumas
informações.
Por exemplo, quando o número de requisições é alto, a aplicação pode ficar
lenta.
Se você tiver a métrica de contagem de requisições, poderá determinar a causa e
aumentar o número de servidores para lidar com a carga.

### Componentes

O ecossistema Prometheus consiste em vários componentes, muitos dos quais são
opcionais:

* o [servidor Prometheus](https://github.com/prometheus/prometheus) principal
  que coleta e armazena dados de séries temporais;
* [bibliotecas de cliente](/instrumenting/clientlibs.md) para instrumentar
  código de aplicação;
* um [_gateway_ de _push_](https://github.com/prometheus/pushgateway) para dar
  suporte a trabalhos de curta duração;
* [exportadores](/instrumenting/exporters.md) de propósito especial para
  serviços como HAProxy, StatsD, Graphite, etc.;
* um [gerenciador de alertas](https://github.com/prometheus/alertmanager) para
  lidar com alertas;
* várias ferramentas de suporte.

A maioria dos componentes do Prometheus é escrita em [Go](https://golang.org/),
tornando-os fáceis de construir e implementar como binários estáticos.

### Arquitetura

Este diagrama ilustra a arquitetura do Prometheus e alguns dos componentes de
seu ecossistema:

![Arquitetura do Prometheus](/assets/architecture.png)

O Prometheus extrai métricas de trabalhos instrumentados, diretamente ou por
meio de um _gateway_ de _push_ intermediário para trabalhos de curta duração.
Ele armazena todas as amostras extraídas localmente e executa regras sobre esses
dados para agregar e registrar novas séries temporais de dados existentes ou
gerar alertas.
O [Grafana](https://grafana.com/) ou outros consumidores de API podem ser usados
para visualizar os dados coletados.

## Quando ele é útil?

O Prometheus funciona bem para registrar qualquer série temporal puramente
numérica.
Ele se encaixa tanto no monitoramento centralizado em máquina quanto no
monitoramento de arquiteturas altamente dinâmicas orientadas a serviços.
Em um mundo de microsserviços, seu suporte à coleta e consulta de dados
multidimensionais é um ponto forte particular.

O Prometheus foi projetado para confiabilidade, para ser o sistema ao qual você
recorre durante uma queda de serviço para permitir que você diagnostique
problemas rapidamente.
Cada servidor Prometheus é autônomo, não depende de armazenamento de rede ou
outros serviços remotos.
Você pode confiar nele quando outras partes de sua infraestrutura estiverem
quebradas, e você não precisa configurar uma infraestrutura extensa para usá-lo.

## Quando ele não é útil?

O Prometheus valoriza a confiabilidade.
Você sempre pode visualizar quais estatísticas estão disponíveis sobre seu
sistema, mesmo em condições de falha.
Se você precisa de 100% de precisão, como para cobrança por requisição, o
Prometheus não é uma boa escolha, pois os dados coletados provavelmente não
serão detalhados e completos o suficiente.
Nesse caso, seria melhor usar outro sistema para coletar e analisar os dados
para faturamento, e o Prometheus para o restante do monitoramento.
