# Modifications made to upstream chart

## chart/values.yaml - GCPE gitlab configuration
  ```yaml
  gitlab:
    url: "http://gitlab-webservice-default.gitlab.svc.cluster.local:8181"
    enable_health_check: false
    health_url: http://gitlab-webservice-default.gitlab.svc.cluster.local:8181
  project_defaults:
    pull:
      refs:
        tags:
          most_recent: 1
        merge_requests:
          enabled: true
          max_age_seconds: 28800
  projects:
    - name: test/test1
      pull:
        pipeline:
          jobs:
            enabled: true
        refs:
          merge_requests:
            enabled: true
            max_age_seconds: 43200
  ```

## chart/values.yaml - GCPE redis-bb configuration
  ```yaml
redis-bb:
  enabled: true
  auth:
    enabled: false
  istio:
    redis:
      enabled: false
  image:
    registry: registry1.dso.mil
    repository: ironbank/bitnami/redis
    tag: 7.2.4
    pullSecrets:
      - private-registry
  networkPolicies:
    enabled: true
    controlPlaneCidr: 0.0.0.0/0
  master:
    containerSecurityContext:
      enabled: true
      runAsUser: 1000
      runAsGroup: 1000
      runAsNonRoot: true
  replica:
    replicaCount: 0
    containerSecurityContext:
      enabled: true
      runAsUser: 1000
      runAsGroup: 1000
      runAsNonRoot: true
  commonConfiguration: |-
    # Enable AOF https://redis.io/topics/persistence#append-only-file
    appendonly no
    maxmemory 200mb
    maxmemory-policy allkeys-lru
    save ""
  ## Additional deployment labels
  podLabels: {}
  ```
