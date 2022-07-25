## Version

- rancher: 2.6.5
- prometheus-operator: 19.0.3
- mongodb-exporter: 0.33 (percona)

Prometheus operator được cài vào cụm bằng app (auto run helm chart).

Doc của prometheus operator ghi rằng, mặc định prom sẽ chỉ scan vào các ServiceMonitor hoặc PodMonitor trong cùng ns với prom, có release tag là prometheus-operator. Có thể tắt nó bằng cách set `prometheus.prometheusSpec.podMonitorSelectorNilUsesHelmValues` and `prometheus.prometheusSpec.serviceMonitorSelectorNilUsesHelmValues` to `false` (xem doc thêm).

