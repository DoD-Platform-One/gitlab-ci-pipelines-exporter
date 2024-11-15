# Testing new GCPE version

## GCPE dashboard
- The following assumes a fresh gitlab development deployment on the latest tag, utilizing this packages [dev-overrides.yaml](./dev-overrides.yaml).
1. Begin with https://repo1.dso.mil/big-bang/product/packages/gitlab/-/blob/main/docs/DEVELOPMENT_MAINTENANCE.md?ref_type=heads#testing-new-gitlab-version, utilizing 
a fresh gitlab deployment.
2. Utilize this packages [dev-overrides.yaml](./dev-overrides.yaml) in place of gitlab dev-overrides, modifying the necessary branch name for GCPE dev-overrides. Check [bigbang gitlab tag page](https://repo1.dso.mil/big-bang/product/packages/gitlab/-/tags) for latest tag.
- If needed, modify gitlab tag in dev-overrides to your branch/desired tag for testing purposes.
3. Follow gitlabs standard testing document, creating all necessary resources.
4. Once group `test` and project `test1` exist with an enabled pipeline, visit https://grafana.dev.bigbang.mil
5. Sign in via SSO, and visit `Dashboards` panel.  
6. Visit `Gitlab CI Pipelines` panel, confirm that the previously tested jobs show up.  
7. Trigger additional pipeline(s) at https://gitlab.dev.bigbang.mil/test/test1/-/pipelines, and verify they are tracked in a `Running` or `Completed` state in grafana.



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

## chart/templates/bigbang/gitlab-exporter-token
- Added resources that allow for spinning up a fresh gitlab instance to integrate GCPE with.  This enables a pre-install hook job to create
a gitlab API token for use with GCPE to collect data on pipelines.
- Added clusterrole, rolebindings for gcpe and gitlab namespaces, and SA.

## chart/templates/bigbang/peerauthentication
- Allow metrics exception for prometheus metrics scraping.

## chart/templates/bigbang/networkpolicies
- Added allows for dns, egress to gitlab internal svc endpoint, egress to https for public gitlab endpoint scraping, ingress for metrics.
- Added default deny.

## chart/templates/bigbang/auth
- Added internamespace auth, allow metrics
- Added default deny

## chart/templates/bigbang/dashboards
- Added gitlab grafana dashboard
