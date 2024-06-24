# gitlab-ci-pipelines-exporter

![Version: 0.3.4-bb.2](https://img.shields.io/badge/Version-0.3.4--bb.2-informational?style=flat-square) ![AppVersion: v0.5.8](https://img.shields.io/badge/AppVersion-v0.5.8-informational?style=flat-square)

Prometheus / OpenMetrics exporter for GitLab CI pipelines insights

## Upstream References
* <https://github.com/mvisonneau/gitlab-ci-pipelines-exporter>

* <https://github.com/mvisonneau/helm-charts/tree/main/charts/gitlab-ci-pipelines-exporter>

## Learn More
* [Application Overview](docs/overview.md)
* [Other Documentation](docs/)

## Pre-Requisites

* Kubernetes Cluster deployed
* Kubernetes config installed in `~/.kube/config`
* Helm installed

Install Helm

https://helm.sh/docs/intro/install/

## Deployment

* Clone down the repository
* cd into directory
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
| config | object | `{"gitlab":{"url":"http://gitlab-webservice-default.gitlab.svc.cluster.local:8181"}}` | configuration of the exporter |
| gitlabSecret | string | `""` | name of a `Secret` containing the GitLab token in the `gitlabToken` field (required unless `config.gitlab.token` is specified) |
| webhookSecret | string | `""` | name of a `Secret` containing the webhook token in the `webhookToken` field (required unless `config.server.webhook.secret_token` is specified) |
| hostAliases | list | `[]` |  |
| serviceMonitor.enabled | bool | `true` | deploy a serviceMonitor resource |
| serviceMonitor.endpoints | list | `[{"interval":"10s","port":"http","scheme":"https","tlsConfig":{"caFile":"/etc/prom-certs/root-cert.pem","certFile":"/etc/prom-certs/cert-chain.pem","insecureSkipVerify":true,"keyFile":"/etc/prom-certs/key.pem"}}]` | endpoints configuration for the monitor |
| serviceMonitor.labels | object | `{}` | additional labels for the service monitor |
| serviceMonitor.annotations | object | `{}` | additional annotations for the service monitor BIG BANG ADDITIONS SCHEME AND TLSCONFIG |
| redis.enabled | bool | `true` | deploy a redis statefulset |
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
| monitoring.enabled | bool | `true` |  |
| istio.enabled | bool | `false` | Toggle istio integration |
| networkPolicies.enabled | bool | `true` |  |
| networkPolicies.ingressLabels.app | string | `"istio-ingressgateway"` |  |
| networkPolicies.ingressLabels.istio | string | `"ingressgateway"` |  |
| networkPolicies.controlPlaneCidr | string | `"0.0.0.0/0"` |  |
| bbtests.enabled | bool | `false` |  |

## Contributing

Please see the [contributing guide](./CONTRIBUTING.md) if you are interested in contributing.
