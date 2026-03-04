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

source_url: https://github.com/prometheus/docs/blob/main/docs/introduction/release-cycle.md
revision: cda26d7c1ffc73b5326f00f2721a22da0a494c86
status: ready

title: Suporte de longo prazo
sort_rank: 10
---

O Prometheus LTS consiste em versões selecionadas do Prometheus que recebem
correções de falhas por um período prolongado.

A cada 6 semanas, um novo ciclo de versões secundárias do Prometheus se inicia.
Após essas 6 semanas, as versões secundárias geralmente não recebem mais
correções de falhas.
Se uma pessoa usuária for afetada por uma falha em uma versão secundária, ela
geralmente precisa atualizar para a versão mais recente do Prometheus.

A atualização do Prometheus deve ser simples graças às nossas
[garantias de estabilidade da API][stab].
No entanto, existe o risco de que novos recursos e melhorias também possam
causar regressões, exigindo outra atualização.

O Prometheus LTS recebe apenas correções de falhas, segurança e documentação,
mas durante um período de um ano.
A cadeia de ferramentas de compilação também será mantida atualizada.
Isso permite que empresas que dependem do Prometheus limitem os riscos de
atualização, mantendo ao mesmo tempo um servidor Prometheus mantido pela
comunidade.

## Lista de versões LTS

<table class="table table-bordered downloads">
    <thead>
        <tr>
            <th>Versão</th>
            <th>Data</th>
            <th>Fim do suporte</th>
        </tr>
    </thead>
    <tbody>
        <tr class="danger">
            <td>Prometheus 2.37</td><td>14/07/2022</td><td>31/07/2023</td>
        </tr>
        <tr class="danger">
            <td>Prometheus 2.45</td><td>23/06/2023</td><td>31/07/2024</td>
        </tr>
        <tr class="danger">
            <td>Prometheus 2.53</td><td>16/06/2024</td><td>31/07/2025</td>
        </tr>
        <tr class="success">
            <td>Prometheus 3.5</td><td>14/07/2025</td><td>31/07/2026</td>
        </tr>
    </tbody>
</table>

## Limitações do suporte LTS

Alguns recursos estão excluídos do suporte LTS:

- Itens listados como instáveis em nossas
  [garantias de estabilidade da API][stab].
- [Recursos experimentais][fflag].
- Suporte ao OpenBSD.

[stab]: https://prometheus.io/docs/prometheus/latest/stability/

[fflag]: https://prometheus.io/docs/prometheus/latest/feature_flags/
