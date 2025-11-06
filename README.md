<!-- Warning: Do not manually edit this file. See notes on gluon + helm-docs at the end of this file for more information. -->
# gitlab-ci-pipelines-exporter

![Version: 0.3.6-bb.17](https://img.shields.io/badge/Version-0.3.6--bb.17-informational?style=flat-square) ![AppVersion: v0.5.10](https://img.shields.io/badge/AppVersion-v0.5.10-informational?style=flat-square) ![Maintenance Track: bb_maintained](https://img.shields.io/badge/Maintenance_Track-bb_maintained-yellow?style=flat-square)

Prometheus / OpenMetrics exporter for GitLab CI pipelines insights

## Upstream References

- <https://github.com/mvisonneau/gitlab-ci-pipelines-exporter>
- <https://github.com/mvisonneau/helm-charts/tree/main/charts/gitlab-ci-pipelines-exporter>

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
| upstream.nameOverride | string | `"gitlab-ci-pipelines-exporter"` |  |
| upstream.image.repository | string | `"registry1.dso.mil/ironbank/opensource/gitlab-ci-pipelines-exporter"` |  |
| upstream.image.tag | string | `"v0.5.10"` |  |
| upstream.securityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"enabled":true,"readOnlyRootFilesystem":true,"runAsGroup":1000,"runAsNonRoot":true,"runAsUser":1000}` | Custom labels to add into metadata customLabels: {} app: gitlab-ci-pipelines-exporter securityContext -- security context to apply to the pods # ref: https://kubernetes.io/docs/tasks/configure-pod-container/security-context BIG BANG ADDITIONS |
| upstream.containerSecurityContext.allowPrivilegeEscalation | bool | `false` |  |
| upstream.containerSecurityContext.capabilities.drop[0] | string | `"ALL"` |  |
| upstream.containerSecurityContext.enabled | bool | `true` |  |
| upstream.containerSecurityContext.readOnlyRootFilesystem | bool | `true` |  |
| upstream.containerSecurityContext.runAsNonRoot | bool | `true` |  |
| upstream.containerSecurityContext.runAsUser | int | `1000` |  |
| upstream.containerSecurityContext.runAsGroup | int | `1000` |  |
| upstream.config.gitlab.url | string | `"http://gitlab-webservice-default.gitlab.svc.cluster.local:8181"` |  |
| upstream.config.gitlab.enable_health_check | bool | `false` |  |
| upstream.config.gitlab.health_url | string | `"http://gitlab-webservice-default.gitlab.svc.cluster.local:8181"` |  |
| upstream.config.project_defaults.pull.refs.tags.most_recent | int | `1` |  |
| upstream.config.project_defaults.pull.refs.merge_requests.enabled | bool | `true` |  |
| upstream.config.project_defaults.pull.refs.merge_requests.max_age_seconds | int | `28800` |  |
| upstream.config.projects | string | `nil` |  |
| upstream.gitlabSecret | string | `"gitlab-ci-exporter-token"` | Use the below gitlabSecret if enabling a fresh deployment with gitlab |
| upstream.serviceMonitor.enabled | bool | `false` |  |
| upstream.serviceMonitor.endpoints[0].port | string | `"http"` |  |
| upstream.serviceMonitor.endpoints[0].interval | string | `"10s"` |  |
| upstream.serviceMonitor.endpoints[0].scheme | string | `"https"` |  |
| upstream.serviceMonitor.endpoints[0].tlsConfig.caFile | string | `"/etc/prom-certs/root-cert.pem"` |  |
| upstream.serviceMonitor.endpoints[0].tlsConfig.certFile | string | `"/etc/prom-certs/cert-chain.pem"` |  |
| upstream.serviceMonitor.endpoints[0].tlsConfig.keyFile | string | `"/etc/prom-certs/key.pem"` |  |
| upstream.serviceMonitor.endpoints[0].tlsConfig.insecureSkipVerify | bool | `true` |  |
| upstream.redis.enabled | bool | `false` |  |
| domain | string | `"dev.bigbang.mil"` |  |
| redis-bb.enabled | bool | `true` |  |
| redis-bb.cleanUpgrade.enabled | bool | `true` |  |
| redis-bb.networkPolicies.enabled | bool | `true` |  |
| redis-bb.upstream.auth.enabled | bool | `false` |  |
| redis-bb.upstream.global.imagePullSecrets[0] | string | `"private-registry"` |  |
| redis-bb.upstream.install | bool | `true` |  |
| redis-bb.upstream.architecture | string | `"standalone"` |  |
| redis-bb.upstream.cluster.enabled | bool | `false` |  |
| redis-bb.upstream.metrics.enabled | bool | `true` |  |
| redis-bb.upstream.metrics.image.registry | string | `"registry1.dso.mil"` |  |
| redis-bb.upstream.metrics.image.repository | string | `"ironbank/bitnami/analytics/redis-exporter"` |  |
| redis-bb.upstream.metrics.image.tag | string | `"v1.79.0"` |  |
| redis-bb.upstream.metrics.image.pullSecrets | list | `[]` |  |
| redis-bb.upstream.metrics.resources.limits.cpu | string | `"250m"` |  |
| redis-bb.upstream.metrics.resources.limits.memory | string | `"256Mi"` |  |
| redis-bb.upstream.metrics.resources.requests.cpu | string | `"250m"` |  |
| redis-bb.upstream.metrics.resources.requests.memory | string | `"256Mi"` |  |
| redis-bb.upstream.metrics.containerSecurityContext.enabled | bool | `true` |  |
| redis-bb.upstream.metrics.containerSecurityContext.runAsUser | int | `1001` |  |
| redis-bb.upstream.metrics.containerSecurityContext.runAsGroup | int | `1001` |  |
| redis-bb.upstream.metrics.containerSecurityContext.runAsNonRoot | bool | `true` |  |
| redis-bb.upstream.metrics.containerSecurityContext.capabilities.drop[0] | string | `"ALL"` |  |
| redis-bb.upstream.serviceAccount.automountServiceAccountToken | bool | `false` |  |
| redis-bb.upstream.securityContext.runAsUser | int | `1001` |  |
| redis-bb.upstream.securityContext.fsGroup | int | `1001` |  |
| redis-bb.upstream.securityContext.runAsNonRoot | bool | `true` |  |
| redis-bb.upstream.image.registry | string | `"registry1.dso.mil"` |  |
| redis-bb.upstream.image.repository | string | `"ironbank/bitnami/redis"` |  |
| redis-bb.upstream.image.tag | string | `"8.2.2"` |  |
| redis-bb.upstream.image.pullSecrets[0] | string | `"private-registry"` |  |
| redis-bb.upstream.master.resources.limits.cpu | string | `"250m"` |  |
| redis-bb.upstream.master.resources.limits.memory | string | `"256Mi"` |  |
| redis-bb.upstream.master.resources.requests.cpu | string | `"250m"` |  |
| redis-bb.upstream.master.resources.requests.memory | string | `"256Mi"` |  |
| redis-bb.upstream.master.containerSecurityContext.enabled | bool | `true` |  |
| redis-bb.upstream.master.containerSecurityContext.runAsUser | int | `1001` |  |
| redis-bb.upstream.master.containerSecurityContext.runAsGroup | int | `1001` |  |
| redis-bb.upstream.master.containerSecurityContext.runAsNonRoot | bool | `true` |  |
| redis-bb.upstream.master.containerSecurityContext.capabilities.drop[0] | string | `"ALL"` |  |
| redis-bb.upstream.sentinel.resources.limits.cpu | string | `"250m"` |  |
| redis-bb.upstream.sentinel.resources.limits.memory | string | `"256Mi"` |  |
| redis-bb.upstream.sentinel.resources.requests.cpu | string | `"250m"` |  |
| redis-bb.upstream.sentinel.resources.requests.memory | string | `"256Mi"` |  |
| redis-bb.upstream.volumePermissions.resources.limits.cpu | string | `"250m"` |  |
| redis-bb.upstream.volumePermissions.resources.limits.memory | string | `"256Mi"` |  |
| redis-bb.upstream.volumePermissions.resources.requests.cpu | string | `"250m"` |  |
| redis-bb.upstream.volumePermissions.resources.requests.memory | string | `"256Mi"` |  |
| redis-bb.upstream.sysctlImage.resources.limits.cpu | string | `"250m"` |  |
| redis-bb.upstream.sysctlImage.resources.limits.memory | string | `"256Mi"` |  |
| redis-bb.upstream.sysctlImage.resources.requests.cpu | string | `"250m"` |  |
| redis-bb.upstream.sysctlImage.resources.requests.memory | string | `"256Mi"` |  |
| gcpeJob.enabled | bool | `false` |  |
| gcpeJob.image.repository | string | `"registry1.dso.mil/ironbank/gitlab/gitlab/kubectl"` |  |
| gcpeJob.image.tag | string | `"18.5.1"` |  |
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

