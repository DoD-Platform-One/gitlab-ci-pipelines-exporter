# Gitlab-CI-Pipelines-Exporter

`gitlab-ci-pipelines-exporter` allows you to monitor your [GitLab CI pipelines](https://docs.gitlab.com/ee/ci/pipelines/) with [Prometheus](https://prometheus.io/) or any monitoring solution supporting the [OpenMetrics](https://github.com/OpenObservability/OpenMetrics) format.

You can find more information [on GitLab docs](https://docs.gitlab.com/ee/ci/pipelines/pipeline_efficiency.html#pipeline-monitoring) about how it takes part improving your pipeline efficiency.

### Prerequisites

`gitlab-ci-pipelines-exporter` requires a gitlab endpoint to monitor against, defined via an API endpoint at `.Values.config.gitlab.url` in the [values.yaml](../chart/values.yaml) file.  This requires a gitlab token that has API and read_repository permissions.  See <https://github.com/mvisonneau/gitlab-ci-pipelines-exporter/blob/master/docs/configuration_syntax.md> for details regarding additional configuration settings.

### Getting Started

If you are deploying this fresh alongside a bigbang gitlab deployment, set `.Values.gcpeJob.enabled` to `true` to enable the helm pre-deployment hook job that will automatically create a long-lived API token to be used by GCPE, and will update as you define additional `.Values.config.projects` project / groups to monitor within gitlab pipelines.

If you have an existing gitlab deployment, define the `.Values.config.gitlab.url` of your gitlab environment, and either directly implement the secret via the values at `.Values.config.gitlab.token`, or create a secret within the `gitlab-ci-pipelines-exporter` namespace with data key value `gitlabToken: <your base64 token here>`, and then plug that secret name into the values at `.Values.gitlabSecret`.  
