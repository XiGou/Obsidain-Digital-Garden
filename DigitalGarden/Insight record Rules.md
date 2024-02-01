IOPS 突升+绝对数值高
CPU 占满
Mem 超过 Quota
Util 占满
重启

docker 部署prometheus




```yaml

alerting:
  alertmanagers:
  - api_version: v1
    path_prefix: /prometheus
    static_configs:
    - targets:
      - 169.254.169.254:9903
rule_files:
- alerting_rules.yml
- recording_rules.yml
scrape_configs:
- job_name: chunk-exporter.volume
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v1/prometheus/volume
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 0s
  static_configs:
  - targets:
    - 10.1.18.74:10202
    - 10.1.18.75:10202
    - 10.1.18.73:10202
    - 10.1.18.76:10202
- job_name: tuna-exporter.cpu
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/cpu
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 25s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.memory
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/memory
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 25s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.disk
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/disk
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 25s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.disk_status
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/disk_status
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 25s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.network
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/network
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 25s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.network_gruff_metrics
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/network_gruff_metrics
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 15s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.ntp
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/ntp
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 25s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.cluster
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/cluster
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 25s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.host_gruff_metrics
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/host_gruff_metrics
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 5m
  scrape_timeout: 30s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.misc
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/misc
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 25s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.ovs_bond
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/ovs_bond
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 25s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.event
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/event
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 10s
  scrape_timeout: 8s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: tuna-exporter.cluster_gruff_metrics
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/cluster_gruff_metrics
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 5m
  scrape_timeout: 30s
  static_configs:
  - targets:
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.73:10404
    - 10.1.18.76:10404
- job_name: octopus-exporter.endpoint
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /metrics
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 15s
  static_configs:
  - targets:
    - 10.1.18.74:9900
    - 10.1.18.75:9900
    - 10.1.18.73:9900
    - 10.1.18.76:9900
- job_name: timemachine-exporter.timemachine
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v3/prometheus/timemachine
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 0s
  static_configs:
  - targets:
    - 10.1.18.74:9912
    - 10.1.18.75:9912
    - 10.1.18.73:9912
- job_name: siren-exporter.notifier
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v3/metrics
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 5m
  scrape_timeout: 2m
  static_configs:
  - targets:
    - 10.1.18.74:9903
    - 10.1.18.75:9903
    - 10.1.18.73:9903
    - 10.1.18.76:9903
- job_name: meta-exporter.basic
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v1/prometheus/basic
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 0s
  static_configs:
  - targets:
    - 10.1.18.74:10104
    - 10.1.18.75:10104
    - 10.1.18.73:10104
- job_name: meta-exporter.volume
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v1/prometheus/volume
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 5m
  scrape_timeout: 0s
  static_configs:
  - targets:
    - 10.1.18.74:10104
    - 10.1.18.75:10104
    - 10.1.18.73:10104
- job_name: chunk-exporter.zbs_chunk
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v1/prometheus/chunk
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 0s
  static_configs:
  - targets:
    - 10.1.18.73:10202
    - 10.1.18.74:10202
    - 10.1.18.75:10202
    - 10.1.18.76:10202
- job_name: tuna-exporter.account
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v2/exporter/tuna/account
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 5m
  scrape_timeout: 58s
  static_configs:
  - targets:
    - 10.1.18.73:10404
    - 10.1.18.74:10404
    - 10.1.18.75:10404
    - 10.1.18.76:10404
- job_name: l2ping-access-exporter.l2ping_access
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v3/prometheus/l2ping_access
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 0s
  static_configs:
  - targets:
    - 10.1.18.73:9916
    - 10.1.18.74:9916
    - 10.1.18.75:9916
    - 10.1.18.76:9916
- job_name: netreactor-exporter.netreactor
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v3/prometheus/netreactor
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 0s
  static_configs:
  - targets:
    - 10.1.18.73:9914
    - 10.1.18.74:9914
    - 10.1.18.75:9914
    - 10.1.18.76:9914
- job_name: l2ping-storage-exporter.l2ping_storage
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /api/v3/prometheus/l2ping_storage
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  scrape_timeout: 0s
  static_configs:
  - targets:
    - 10.1.18.73:9915
    - 10.1.18.74:9915
    - 10.1.18.75:9915
    - 10.1.18.76:9915
- job_name: svcresctld.service
  metric_relabel_configs:
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  metrics_path: /metrics/services
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 1m
  scrape_timeout: 58s
  static_configs:
  - targets:
    - 10.1.18.73:10413
    - 10.1.18.74:10413
    - 10.1.18.75:10413
    - 10.1.18.76:10413
- basic_auth:
    password: HC!r0cks
    username: prometheus
  job_name: prometheus
  metric_relabel_configs:
  - action: drop
    regex: prometheus_rule_group.*
    source_labels:
    - __name__
  - action: labeldrop
    regex: _job
  - action: labeldrop
    regex: _instance
  relabel_configs:
  - source_labels:
    - job
    target_label: _job
  - regex: (\d+\.\d+\.\d+\.\d+):(\d+)
    source_labels:
    - __address__
    target_label: _instance
  scrape_interval: 30s
  static_configs:
  - targets:
    - 127.0.0.1:9090
```