# kubernetes-monitoring

![Version: 0.0.1](https://img.shields.io/badge/Version-0.0.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.1.4](https://img.shields.io/badge/AppVersion-1.1.4-informational?style=flat-square)

A Helm chart for Kubernetes Monitoring

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| petewall | <pete.wall@grafana.com> |  |

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| https://grafana.github.io/helm-charts | grafana-agent | ~0.15.0 |
| https://opencost.github.io/opencost-helm-chart | opencost | ~1.14.5 |
| https://prometheus-community.github.io/helm-charts | kube-state-metrics | ~5.7.0 |
| https://prometheus-community.github.io/helm-charts | prometheus-node-exporter | ~4.17.4 |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| cluster.name | string | `"my-cluster"` | The name of this cluster, which will be set in all labels |
| externalServices.loki.host | string | `nil` | Loki host |
| externalServices.loki.password | string | `nil` | Loki basic auth password |
| externalServices.loki.username | string | `nil` | Loki basic auth username |
| externalServices.prometheus.host | string | `nil` | Prometheus host |
| externalServices.prometheus.password | string | `nil` | Prometheus basic auth password |
| externalServices.prometheus.username | string | `nil` | Prometheus basic auth username |
| extraConfig | string | `nil` | Extra configuration that will be added to the Grafana Agent configuration file. <details> <summary>+ Example</summary> An example extraConfig to discover and scrape metrics from a service: ```text discovery.relabel "my-service" {   targets = discovery.kubernetes.services.targets   rule {     source_labels = ["__meta_kubernetes_service_label_app_kubernetes_io_name"]     regex = "my-service"     action = "keep"   } }  prometheus.scrape "my-service" {   job_name   = "integrations/my-service"   targets    = discovery.relabel.my-service.output   forward_to = [prometheus.relabel.add_cluster_label.receiver] } ``` Note: "discovery.kubernetes.services" and       "prometheus.relabel.add_cluster_label" are pre-defined by this chart. </details> |
| kube-state-metrics.enabled | bool | `true` | Should this helm chart deploy Kube State Metrics to the cluster. Set this to false if your cluster already has Kube State Metrics, or if you do not want to scrape metrics from Kube State Metrics. |
| logs.cluster_events.enabled | bool | `true` | Scrape Kubernetes cluster events |
| logs.enabled | bool | `true` | Capture and forward logs |
| logs.pod_logs.enabled | bool | `true` | Capture and forward logs from Kubernetes pods |
| logs.pod_logs.loggingFormat | string | `"docker"` | The log parsing format. Must be one of null, 'cri', or 'docker' See documentation: https://grafana.com/docs/agent/latest/flow/reference/components/loki.process/#stagecri-block |
| metrics.cadvisor.allowList | list | See [Allow List for cAdvisor](#allow-list-for-cadvisor) | The list of cAdvisor metrics that will be scraped by the Agent |
| metrics.cadvisor.enabled | bool | `true` | Scrape container metrics from cAdvisor |
| metrics.cost.allowList | list | See [Allow List for OpenCost](#allow-list-for-opencost) | The list of OpenCost metrics that will be scraped by the Agent |
| metrics.cost.enabled | bool | `true` | Scrape cost metrics from OpenCost |
| metrics.enabled | bool | `true` | Capture and forward metrics |
| metrics.kube-state-metrics.allowList | list | See [Allow List for Kube State Metrics](#allow-list-for-kube-state-metrics) | The list of Kube State Metrics metrics that will be scraped by the Agent |
| metrics.kube-state-metrics.enabled | bool | `true` | Scrape cluster object metrics from Kube State Metrics |
| metrics.kube-state-metrics.service.isTLS | bool | `false` | Does this port use TLS? |
| metrics.kube-state-metrics.service.port | string | `"http"` | Name of the metrics port |
| metrics.kubelet.allowList | list | See [Allow List for Kubelet](#allow-list-for-kubelet) | The list of Kubelet metrics that will be scraped by the Agent |
| metrics.kubelet.enabled | bool | `true` | Scrape cluster metrics from the Kubelet |
| metrics.node-exporter.allowList | list | See [Allow List for Node Exporter](#allow-list-for-node-exporter) | The list of Node Exporter metrics that will be scraped by the Agent |
| metrics.node-exporter.enabled | bool | `true` | Scrape node metrics |
| metrics.podMonitors.enabled | bool | `true` | Include service discovery for PodMonitor objects |
| metrics.serviceMonitors.enabled | bool | `true` | Include service discovery for ServiceMonitor objects |
| node-exporter.enabled | bool | `true` | Should this helm chart deploy Node Exporter to the cluster. Set this to false if your cluster already has Node Exporter, or if you do not want to scrape metrics from Node Exporter. |
| opencost.enabled | bool | `true` | Should this Helm chart deploy OpenCost to the cluster. Set this to false if your cluster already has OpenCost, or if you do not want to scrape metrics from OpenCost. |
| opencost.opencost.prometheus.external.url | string | `"https://prom.example.com/api/prom"` | The URL for Prometheus queries. It should match externalService.prometheus.host + "/api/prom" |

## Allow List

Each metric source has an allow list, which is a list of metric names that will
be forwarded by the Grafana Agent to Prometheus. Any metric not on that list
will be ignored. Defaults have been supplied for the specific services and are
shown in the following sections.

If you want to allow all metrics, set this in the values file:

```yaml
allowList: ["*"]
```

### Allow List for Kube State Metrics

Visit the Kube State Metrics [documentation](https://github.com/kubernetes/kube-state-metrics/tree/main/docs#exposed-metrics) for the full list of metrics

* kube_daemonset.*
* kube_deployment_metadata_generation
* kube_deployment_spec_replicas
* kube_deployment_status_observed_generation
* kube_deployment_status_replicas_available
* kube_deployment_status_replicas_updated
* kube_horizontalpodautoscaler_spec_max_replicas
* kube_horizontalpodautoscaler_spec_min_replicas
* kube_horizontalpodautoscaler_status_current_replicas
* kube_horizontalpodautoscaler_status_desired_replicas
* kube_job.*
* kube_namespace_status_phase
* kube_namespace_status_phase
* kube_node.*
* kube_persistentvolumeclaim_resource_requests_storage_bytes
* kube_pod_container_info
* kube_pod_container_resource_limits
* kube_pod_container_resource_requests
* kube_pod_container_status_restarts_total
* kube_pod_container_status_waiting_reason
* kube_pod_container_status_waiting_reason
* kube_pod_info
* kube_pod_owner
* kube_pod_start_time
* kube_pod_status_phase
* kube_pod_status_phase
* kube_pod_status_reason
* kube_replicaset.*
* kube_resourcequota
* kube_statefulset.*

### Allow List for Node Exporter

* node_cpu.*
* node_exporter_build_info
* node_filesystem.*
* node_memory.*
* process_cpu_seconds_total
* process_resident_memory_bytes

### Allow List for Kubelet

Visit the Kubelet [documentation](https://github.com/kubernetes/kubernetes/blob/master/test/instrumentation/documentation/documentation.md) for the full list of metrics

* container_cpu_usage_seconds_total
* kubelet_certificate_manager_client_expiration_renew_errors
* kubelet_certificate_manager_client_ttl_seconds
* kubelet_certificate_manager_server_ttl_seconds
* kubelet_cgroup_manager_duration_seconds_bucket
* kubelet_cgroup_manager_duration_seconds_count
* kubelet_node_config_error
* kubelet_node_name
* kubelet_pleg_relist_duration_seconds_bucket
* kubelet_pleg_relist_duration_seconds_count
* kubelet_pleg_relist_interval_seconds_bucket
* kubelet_pod_start_duration_seconds_bucket
* kubelet_pod_start_duration_seconds_count
* kubelet_pod_worker_duration_seconds_bucket
* kubelet_pod_worker_duration_seconds_count
* kubelet_running_container_count
* kubelet_running_containers
* kubelet_running_pod_count
* kubelet_running_pods
* kubelet_runtime_operations_errors_total
* kubelet_runtime_operations_total
* kubelet_server_expiration_renew_errors
* kubelet_volume_stats_available_bytes
* kubelet_volume_stats_capacity_bytes
* kubelet_volume_stats_inodes
* kubelet_volume_stats_inodes_used
* kubernetes_build_info
* namespace_workload_pod
* rest_client_requests_total
* storage_operation_duration_seconds_count
* storage_operation_errors_total
* volume_manager_total_volumes

### Allow List for cAdvisor

Visit the cAdvisor [documentation](https://github.com/google/cadvisor/blob/master/docs/storage/prometheus.md)

* container_cpu_cfs_periods_total
* container_cpu_cfs_throttled_periods_total
* container_cpu_usage_seconds_total
* container_fs_reads_bytes_total
* container_fs_reads_total
* container_fs_writes_bytes_total
* container_fs_writes_total
* container_memory_cache
* container_memory_rss
* container_memory_swap
* container_memory_working_set_bytes
* container_network_receive_bytes_total
* container_network_receive_packets_dropped_total
* container_network_receive_packets_total
* container_network_transmit_bytes_total
* container_network_transmit_packets_dropped_total
* container_network_transmit_packets_total
* machine_memory_bytes

### Allow List for OpenCost

Visit the OpenCost [documentation](https://www.opencost.io/docs/prometheus#available-metrics) for the full list of metrics

* container_cpu_allocation
* container_gpu_allocation
* container_memory_allocation_bytes
* deployment_match_labels
* kubecost_cluster_info
* kubecost_cluster_management_cost
* kubecost_cluster_memory_working_set_bytes
* kubecost_http_requests_total
* kubecost_http_response_size_bytes
* kubecost_http_response_time_seconds
* kubecost_load_balancer_cost
* kubecost_network_internet_egress_cost
* kubecost_network_region_egress_cost
* kubecost_network_zone_egress_cost
* kubecost_node_is_spot
* node_cpu_hourly_cost
* node_gpu_count
* node_gpu_hourly_cost
* node_ram_hourly_cost
* node_total_hourly_cost
* opencost_build_info
* pod_pvc_allocation
* pv_hourly_cost
* service_selector_labels
* statefulSet_match_labels
