## Extend the HTTP Inbound/Outbound metrics with minimal and maximal request latency

By default, the inspect Ocelot Java Agent does not collect a minimum, maximum or percentile response time per time interval per HTTP endpoint. It is possible to expand the existing metric definitions with views to gather those additional metrics.

Below metric definitions result in two metrics for HTTP inbound/outbound requests:
* http_(in|out)_responsetime_min (quantile 0)
* http_(in|out)_responsetime_max (quantile 1)

```
inspectit:
  metrics:
    definitions:
      '[http/in/responsetime]':
        unit: ms
        views:
          '[http/in/responsetime]':
            aggregation: QUANTILES
            quantiles: [0, 1]
            tags: 
              'http_path': true
              'http_status': true
              'http_method': true
              'error': true
      '[http/out/responsetime]':
        unit: ms
        views:
          '[http/out/responsetime]':
            aggregation: QUANTILES
            quantiles: [0, 1]
            tags:
              'http_host': true
              'http_path': true
              'http_status': true
              'http_method': true
              'error': true
```

Percentile response times are possible by specifying quantiles within the range [0, 1]. 0.9 is the 90th percentile, 0.99 is the 0.99 percentile.

Further information can be found at [Custom Metrics](https://inspectit.github.io/inspectit-ocelot/docs/metrics/custom-metrics). Above configuration is available as [YAML file](http-metrics-min-max.yml).

