=============
RED in Golang
=============

Prometheus Query
================

- simple:

  ::

    order_mgmt_duration_seconds_sum{status_code='200'}

  ::

    order_mgmt_duration_seconds_sum{job=~".*"}
    or
    order_mgmt_database_duration_seconds_sum{job=~".*"}
    or
    order_mgmt_audit_duration_seconds_sum{job=~".*"}

- based on weave blog (https://www.weave.works/blog/of-metrics-and-middleware/):

  - QPS:

    ::

      sum(irate(order_mgmt_duration_seconds_count{job=~".*"}[1m])) by (status_code)

  - will give you the rate of requests returning 500s:

    ::

      sum(irate(order_mgmt_duration_seconds_count{job=~".*", status_code=~"5.."}[1m]))

  - by status_code:

    ::

      sum(irate(order_mgmt_duration_seconds_count{job=~".*"}[1m])) by (status_code)

  - 500s:

    ::

      sum(irate(order_mgmt_duration_seconds_count{job=~".*", status_code=~"5.."}[1m]))

  – will give you the 5-min moving 99th percentile request latency:

    ::

      histogram_quantile(0.99, sum(rate(order_mgmt_duration_seconds_count{job=~".*",ws="false"}[5m])) by (le))

Development
===========

The code is based on: https://github.com/microdevs/missy

Related Work
============

- - https://www.robustperception.io/combining-alert-conditions/
