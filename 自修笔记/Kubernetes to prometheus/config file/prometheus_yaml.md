```
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
- job_name: 'prometheus'

  # metrics_path defaults to '/metrics'
  # scheme defaults to 'http'.

  static_configs:
  - targets: ['192.168.154.129:9090']

- job_name: 'centos7-2'

  scrape_interval: 10s

  static_configs:
  - targets: ['192.168.154.130:9100']
    labels:
          instance: node02


- job_name: 'kubernetes-nodes'


  scheme: https
  kubernetes_sd_configs:
  - role: node
    api_server: https://192.168.154.129:6443
    bearer_token_file: /usr/local/prometheus/prometheus.token
    tls_config:
      insecure_skip_verify: true
  bearer_token_file: /usr/local/prometheus/prometheus.token
  tls_config:
    insecure_skip_verify: true

  relabel_configs:
  - action: labelmap
    regex: __meta_kubernetes_node_label_(.+)

- job_name: 'kubernetes-cadvisor'


  scheme: https
  metrics_path: /metrics/cadvisor
  kubernetes_sd_configs:
  - role: node
    api_server: https://192.168.154.129:6443
    bearer_token_file: /usr/local/prometheus/prometheus.token
    tls_config:
      insecure_skip_verify: true
  bearer_token_file: /usr/local/prometheus/prometheus.token
  tls_config:
    insecure_skip_verify: true

  relabel_configs:
  - action: labelmap
    regex: __meta_kubernetes_node_label_(.+)

```



---
#
#
<meta http-equiv="refresh" content="5">