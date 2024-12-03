<!-- Warning: Do not manually edit this file. See notes on gluon + helm-docs at the end of this file for more information. -->
# gitlab-ci-pipelines-exporter

![Version: 0.3.4-bb.9](https://img.shields.io/badge/Version-0.3.4--bb.9-informational?style=flat-square) ![AppVersion: v0.5.8](https://img.shields.io/badge/AppVersion-v0.5.8-informational?style=flat-square) ![Maintenance Track: bb_maintained](https://img.shields.io/badge/Maintenance_Track-bb_maintained-yellow?style=flat-square)

Prometheus / OpenMetrics exporter for GitLab CI pipelines insights

## Upstream References
- <https://github.com/mvisonneau/gitlab-ci-pipelines-exporter>

* <https://github.com/mvisonneau/helm-charts/tree/main/charts/gitlab-ci-pipelines-exporter>

## Upstream Release Notes

The [upstream chart's release notes](https://github.com/mvisonneau/gitlab-ci-pipelines-exporter/blob/main/CHANGELOG.md) may help when reviewing this package.

## Learn More

- [Application Overview](docs/overview.md)
- [Other Documentation](docs/)

## Pre-Requisites

- Kubernetes Cluster deployed
- Kubernetes config installed in `~/.kube/config`
- Helm installed

Install Helm

https://helm.sh/docs/intro/install/

## Deployment

- Clone down the repository
- cd into directory

```bash
helm install gitlab-ci-pipelines-exporter chart/
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| replicas | int | `1` | amount of desired pod(s) replica(s) |
| image.repository | string | `"registry1.dso.mil/ironbank/opensource/gitlab-ci-pipelines-exporter"` | image repository |
| image.tag | string | `"v0.5.8"` | image tag tag: <default to chart version> |
| image.pullPolicy | string | `"IfNotPresent"` | image pullPolicy |
| image.pullSecrets | list | `[]` | Optional array of imagePullSecrets containing private registry credentials Ref: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/ |
| image.pullCredentials | object | `{}` | Automatically create a secret with the credentials and use it Cannot be used in conjunction of image.pullSecrets |
| customLabels | object | `{}` | Custom labels to add into metadata |
| labels | object | `{}` | additional labels for the service |
| annotations | object | `{}` | additional annotations for the service |
| podLabels | object | `{}` | additional labels for the pods |
| podAnnotations | object | `{}` | additional annotations for the pods |
| service.type | string | `"ClusterIP"` |  |
| service.port | int | `80` |  |
| service.labels | object | `{}` |  |
| service.annotations | object | `{}` |  |
| resources | object | `{}` | resources to allocate to the pods |
| strategy | object | `{"type":"RollingUpdate"}` | deployment strategy type |
| livenessProbe.httpGet.path | string | `"/health/live"` |  |
| livenessProbe.httpGet.port | int | `8080` |  |
| readinessProbe.httpGet.path | string | `"/health/ready"` |  |
| readinessProbe.httpGet.port | int | `8080` |  |
| readinessProbe.initialDelaySeconds | int | `5` |  |
| readinessProbe.timeoutSeconds | int | `5` |  |
| readinessProbe.failureThreshold | int | `3` |  |
| readinessProbe.periodSeconds | int | `30` |  |
| nodeSelector | object | `{}` | node selector for pod assignment # ref: https://kubernetes.io/docs/user-guide/node-selection/ |
| tolerations | list | `[]` | tolerations for pod assignment # ref: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/ |
| affinity | object | `{}` | affinity for pod assignment # ref: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity |
| securityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"enabled":true,"readOnlyRootFilesystem":true,"runAsGroup":1000,"runAsNonRoot":true,"runAsUser":1000}` | security context to apply to the pods # ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context BIG BANG ADDITIONS |
| containerSecurityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"enabled":true,"readOnlyRootFilesystem":true,"runAsGroup":1000,"runAsNonRoot":true,"runAsUser":1000}` | security context to apply to the containers # ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context |
| command | list | `["gitlab-ci-pipelines-exporter","run"]` | command for the exporter binary |
| args | list | `["--config","/etc/config.yml"]` | arguments for the exporter binary |
| envVariables | list | `[{"name":"GCPE_INTERNAL_MONITORING_LISTENER_ADDRESS","value":"tcp://127.0.0.1:8082"}]` | environment variables for the container |
| config | object | `{"gitlab":{"enable_health_check":false,"health_url":"http://gitlab-webservice-default.gitlab.svc.cluster.local:8181","url":"http://gitlab-webservice-default.gitlab.svc.cluster.local:8181"},"project_defaults":{"pull":{"refs":{"merge_requests":{"enabled":true,"max_age_seconds":28800},"tags":{"most_recent":1}}}},"projects":null}` | configuration of the exporter |
| gitlabSecret | string | `""` | name of a `Secret` containing the GitLab token in the `gitlabToken` field (required unless `config.gitlab.token` is specified) |
| webhookSecret | string | `""` | name of a `Secret` containing the webhook token in the `webhookToken` field (required unless `config.server.webhook.secret_token` is specified) |
| hostAliases | list | `[]` |  |
| serviceMonitor.enabled | bool | `false` | deploy a serviceMonitor resource |
| serviceMonitor.endpoints | list | `[{"interval":"10s","port":"http","scheme":"https","tlsConfig":{"caFile":"/etc/prom-certs/root-cert.pem","certFile":"/etc/prom-certs/cert-chain.pem","insecureSkipVerify":true,"keyFile":"/etc/prom-certs/key.pem"}}]` | endpoints configuration for the monitor |
| serviceMonitor.labels | object | `{}` | additional labels for the service monitor |
| serviceMonitor.annotations | object | `{}` | additional annotations for the service monitor BIG BANG ADDITIONS SCHEME AND TLSCONFIG |
| redis.enabled | bool | `false` | deploy a redis statefulset |
| redis.architecture | string | `"standalone"` | run in standalone or clustermode |
| redis.auth.enabled | bool | `false` | enable authentication |
| redis.metrics.enabled | bool | `false` | enable /metrics endpoint of the redis pods |
| redis.metrics.serviceMonitor.enabled | bool | `false` | deploy a serviceMonitor resource for the redis pods |
| redis.master.persistence.enabled | bool | `false` | persist data |
| ingress.enabled | bool | `false` | deploy a ingress to access the exporter pod(s) /webhook endpoint |
| ingress.ingressClassName | object | `{}` | ingressClassName to be used instead of the deprecated annotation kubernetes.io/ingress.class |
| ingress.annotations | string | `nil` | additional annotations for the ingress resource |
| ingress.path | string | `"/webhook"` | path on the exporter to point the root of the ingress |
| ingress.pathType | string | `"Prefix"` | pathType for the ingress |
| ingress.service.port.name | string | `"http"` | service port for the ingress |
| ingress.hosts | list | `["gcpe.example.com"]` | ingress hosts |
| ingress.tls | list | `[{"hosts":["gcpe.example.com"],"secretName":{}}]` | ingress tls hosts config |
| rbac | object | `{"clusterRole":"","enabled":false,"serviceAccount":{"name":""}}` | If your kubernetes cluster defined the pod security policy, then you need to enable this part, and define clusterRole based on your situation. |
| domain | string | `"dev.bigbang.mil"` |  |
| redis-bb.enabled | bool | `true` |  |
| redis-bb.auth.enabled | bool | `false` |  |
| redis-bb.istio.redis.enabled | bool | `false` |  |
| redis-bb.image.pullSecrets[0] | string | `"private-registry"` |  |
| redis-bb.metrics.enabled | bool | `true` |  |
| redis-bb.metrics.containerSecurityContext.enabled | bool | `true` |  |
| redis-bb.metrics.containerSecurityContext.runAsUser | int | `1001` |  |
| redis-bb.metrics.containerSecurityContext.runAsGroup | int | `1001` |  |
| redis-bb.master.containerSecurityContext.enabled | bool | `true` |  |
| redis-bb.master.containerSecurityContext.runAsUser | int | `1001` |  |
| redis-bb.master.containerSecurityContext.runAsGroup | int | `1001` |  |
| redis-bb.master.containerSecurityContext.runAsNonRoot | bool | `true` |  |
| redis-bb.replica.containerSecurityContext.enabled | bool | `true` |  |
| redis-bb.replica.containerSecurityContext.runAsUser | int | `1001` |  |
| redis-bb.replica.containerSecurityContext.runAsGroup | int | `1001` |  |
| redis-bb.replica.containerSecurityContext.runAsNonRoot | bool | `true` |  |
| redis-bb.replica.containerSecurityContext.capabilities.drop[0] | string | `"ALL"` |  |
| redis-bb.commonConfiguration | string | `"maxmemory 200mb\nsave \"\""` |  |
| gcpeJob.enabled | bool | `false` |  |
| gcpeJob.image.repository | string | `"registry1.dso.mil/ironbank/gitlab/gitlab/kubectl"` |  |
| gcpeJob.image.tag | string | `"17.3.6"` |  |
| gcpeJob.image.pullSecrets[0].name | string | `"private-registry"` |  |
| gcpeJob.image.securityContext.runAsUser | int | `65534` |  |
| gcpeJob.image.securityContext.runAsGroup | int | `65534` |  |
| istio.enabled | bool | `false` |  |
| istio.hardened.enabled | bool | `false` |  |
| istio.hardened.customAuthorizationPolicies | list | `[]` |  |
| istio.hardened.gitlab.enabled | bool | `true` |  |
| istio.hardened.gitlab.namespaces[0] | string | `"gitlab"` |  |
| istio.hardened.monitoring.enabled | bool | `false` |  |
| istio.hardened.monitoring.namespaces[0] | string | `"monitoring"` |  |
| istio.hardened.monitoring.principals[0] | string | `"cluster.local/ns/monitoring/sa/monitoring-grafana"` |  |
| istio.hardened.monitoring.principals[1] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-kube-alertmanager"` |  |
| istio.hardened.monitoring.principals[2] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-kube-operator"` |  |
| istio.hardened.monitoring.principals[3] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-kube-prometheus"` |  |
| istio.hardened.monitoring.principals[4] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-kube-state-metrics"` |  |
| istio.hardened.monitoring.principals[5] | string | `"cluster.local/ns/monitoring/sa/monitoring-monitoring-prometheus-node-exporter"` |  |
| istio.hardened.outboundTrafficPolicyMode | string | `"REGISTRY_ONLY"` |  |
| istio.hardened.customServiceEntries | list | `[]` |  |
| istio.mtls.mode | string | `"STRICT"` | STRICT = Allow only mutual TLS traffic, PERMISSIVE = Allow both plain text and mutual TLS traffic |
| istio.injection | string | `"disabled"` |  |
| networkPolicies.enabled | bool | `false` |  |
| networkPolicies.ingressLabels.app | string | `"istio-ingressgateway"` |  |
| networkPolicies.ingressLabels.istio | string | `"ingressgateway"` |  |
| networkPolicies.controlPlaneCidr | string | `"0.0.0.0/0"` |  |
| networkPolicies.additionalPolicies | list | `[]` |  |
| monitoring.enabled | bool | `false` |  |
| monitoring.namespace | string | `"monitoring"` |  |
| bbtests.enabled | bool | `false` |  |

## Contributing

Please see the [contributing guide](./CONTRIBUTING.md) if you are interested in contributing.

---

_This file is programatically generated using `helm-docs` and some BigBang-specific templates. The `gluon` repository has [instructions for regenerating package READMEs](https://repo1.dso.mil/big-bang/product/packages/gluon/-/blob/master/docs/bb-package-readme.md)._

