# Testing new GCPE version

## GCPE dashboard

- The following assumes a fresh gitlab development deployment on the latest tag, utilizing this packages [dev-overrides.yaml](./dev-overrides.yaml).
- Begin with <https://repo1.dso.mil/big-bang/product/packages/gitlab/-/blob/main/docs/DEVELOPMENT_MAINTENANCE.md?ref_type=heads#testing-new-gitlab-version>, utilizing
a fresh gitlab deployment.

## Upgrading to a new version

The below details the steps required to update to a new version of the Gitlab-CI-Pipeline-Exporter package.

1. Renovate may have already made changes in the development branch. If that is the case then just verify that the changes are correct as you go through these steps.

1. Discover the chart version tag that matches with the application version from the [upstream chart](https://github.com/mvisonneau/helm-charts/tree/main/charts/gitlab-ci-pipelines-exporter) by looking at the Chart.yaml. Do diff between old and new release tags to become aware of any significant chart changes. A graphical diff tool such as [Meld](https://meldmerge.org/) is useful. You can see where the current chart version and available versions are at under the `sources` section in Chart.yaml.`

1. Read the /CHANGELOG.md from the release tag from upstream [upstream chart](https://github.com/mvisonneau/helm-charts/tree/main/charts/gitlab-ci-pipelines-exporter). Take note of any special upgrade instructions, if any.
1. If Renovate has not created a development branch and merge request then manually create them.
1. Merge/Sync the new helm chart with the existing gitlab-ci-pipeline-exporter package code. A graphical diff tool like [Meld](https://meldmerge.org/) is useful. 
1. In ```/chart/values.yaml``` update all the  image tags to the new version. There are 4 images: gitlab-ci-pipeline-exporter, redis, redis-exporter and kubectl.
1. Update `chart/Chart.yaml` to the appropriate versions. The annotation version should match the ```appVersion```.

    ```yaml
    version: X.X.X-bb.X
    appVersion: X.X.X
    annotations:
      bigbang.dev/applicationVersions: |
        - gitlab-ci-pipelines-exporter: X.X.X
    dependencies:
    - name: gitlab-ci-pipelines-exporter
      version: X.X.X
      repository: https://charts.visonneau.fr
      alias: upstream
    - name: gluon
      version: X.X.X
      repository: oci://registry1.dso.mil/bigbang
    - name: redis
      version: X.X.X-bb.X
      repository: oci://registry1.dso.mil/bigbang/maintained
      condition: redis-bb.enabled
      alias: redis-bb
    ```

    ```yaml
    helm.sh/images: |
    - name: gitlab-ci-pipelines-exporter
      image: registry1.dso.mil/ironbank/opensource/gitlab-ci-pipelines-exporter:vX.X.X
    - name: redis
      image: registry1.dso.mil/ironbank/bitnami/redis:X.X.X
      condition: redis-bb.enabled
    - name: redis-exporter
      image: registry1.dso.mil/ironbank/bitnami/analytics/redis-exporter:vX.X.X
      condition: redis-bb.enabled
    - name: kubectl
      condition: gcpeJob.enabled
      image: registry1.dso.mil/ironbank/gitlab/gitlab/kubectl:X.X.X
    ```

1. Look in ```/chart/Chart.yaml``` at the dependencies and verify that you have the most recent versions of the all the listed applications. 
1. Run a helm dependency command to update the ```/chart/charts/x.x.x.tgz``` archives and create a new ```/requirements.lock``` file. You will commit the tar archives along with the requirements.lock that was generated.  This will contain the new upstream chart to commit if you have updated the Chart.yaml versioning for the upstream and other dependency charts.

    ```bash
    helm dependency update ./chart
    ```

1. Update `CHANGELOG.md` adding an entry for the new version and noting all changes.
1. Generate the `README.md` updates by following the [guide in gluon](https://repo1.dso.mil/platform-one/big-bang/apps/library-charts/gluon/-/blob/master/docs/bb-package-readme.md).
1. Open an MR in "Draft" status and validate that CI passes. This will perform a number of smoke tests against the package, but it is good to manually deploy to test some things that CI doesn't.
1. Once all manual testing is complete take your MR out of "Draft" status and add the review label.


## How to test gitlab-ci-pipeline-exporter

1. Utilize this packages [dev-overrides.yaml](./dev-overrides.yaml) in place of gitlab dev-overrides, modifying the necessary branch name for GCPE dev-overrides. Check [bigbang gitlab tag page](https://repo1.dso.mil/big-bang/product/packages/gitlab/-/tags) for latest tag.
    - If needed, modify gitlab tag in dev-overrides to your branch/desired tag for testing purposes.
1. Follow gitlabs standard testing document, creating all necessary resources.
1. Once group `test` and project `test1` exist with an enabled pipeline, visit <https://grafana.dev.bigbang.mil>
1. Sign in via SSO, and visit `Dashboards` panel.  
1. Visit `Gitlab CI Pipelines` panel, confirm that the previously tested jobs show up.  
1. Trigger additional pipeline(s) at <https://gitlab.dev.bigbang.mil/test/test1/-/pipelines>, and verify they are tracked in a `Running` or `Completed` state in grafana.
1. Once completed, be sure to update your `chart/Chart.yaml`, `CHANGELOG.md`, and `README.md`, as well as the `tests/dependencies.yaml` image and gitlab subchart tags.

## Modifications made to upstream chart

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
