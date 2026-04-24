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

source_url: https://github.com/prometheus/docs/blob/main/docs/visualization/browser.md
revision: 8bdb919e820ad27adc12fc66daf38531c3d9a801
status: ready

title: Navegador de expressões
sort_rank: 1
---

O navegador de expressões está disponível em `/graph` no servidor Prometheus,
permitindo que você insira qualquer expressão e veja seu resultado em uma tabela
ou em um gráfico ao longo do tempo.

Isso é útil principalmente para consultas ad-hoc e depuração.
Para gráficos, use o [Grafana](/docs/visualization/grafana/) ou
[templates de console](/docs/visualization/consoles/).
