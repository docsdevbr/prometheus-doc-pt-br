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

title: Pushing metrics
sort_rank: 3
---

Occasionally you will need to monitor components which cannot be scraped. The
[Prometheus Pushgateway](https://github.com/prometheus/pushgateway) allows you
to push time series from [short-lived service-level batch
jobs](/docs/practices/pushing/) to an intermediary job which Prometheus can
scrape. Combined with Prometheus's simple text-based exposition format, this
makes it easy to instrument even shell scripts without a client library.

 * For more information on using the Pushgateway and use from a Unix shell, see the project's
[README.md](https://github.com/prometheus/pushgateway/blob/master/README.md).

 * For use from Java see the
[Pushgateway documentation](https://prometheus.github.io/client_java/exporters/pushgateway/).

 * For use from Go see the [Push](https://godoc.org/github.com/prometheus/client_golang/prometheus/push#Pusher.Push) and [Add](https://godoc.org/github.com/prometheus/client_golang/prometheus/push#Pusher.Add) methods.

 * For use from Python see [Exporting to a Pushgateway](https://prometheus.github.io/client_python/exporting/pushgateway/).

 * For use from Ruby see the [Pushgateway documentation](https://github.com/prometheus/client_ruby#pushgateway).

* To find out about Pushgateway support of [client libraries maintained outside of the Prometheus project](/docs/instrumenting/clientlibs/), refer to their respective documentation.
