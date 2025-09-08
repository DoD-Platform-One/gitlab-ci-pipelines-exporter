# Changelog

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.3.6-bb.11] - 2025-09-08

### Changed

- ironbank/bitnami/redis (source) 8.2.0 -> 8.2.1
- registry1.dso.mil/ironbank/bitnami/redis (source) 8.2.0 -> 8.2.1


## [0.3.6-bb.10] - 2025-09-04

### Changed

- Update BB redis chart 22.0.4-bb.0 -> 22.0.7-bb.0


## [0.3.6-bb.9] - 2025-09-02

### Changed

- - Updated NetworkPolicy template allow-istiod-egress for istio operator-less label

## [0.3.6-bb.8] - 2025-08-29

### Changed

- Update gluon 0.8.0 -> 0.8.4
- Update BB redis chart 22.0.3-bb.0 -> 22.0.4-bb.0
- Update redis-exporter v1.75.0 -> v1.76.0
- Update kubectl 18.2.2 -> 18.3.1

## [0.3.6-bb.7] - 2025-08-25

### Changed

- Update gluon 0.6.3 -> 0.8.0
- Update BB redis chart 21.2.9-bb.0 -> 22.0.3-bb.0
- Update redis-exporter v1.74.0 -> v1.75.0
- Update redis 8.0.3 -> 8.2.0
- Update kubectl 18.1.2 -> 18.2.2


## [0.3.6-bb.6] - 2025-07-17

### Changed

- Update BB redis chart 21.1.3-bb.0 -> 21.2.9-bb.0
- Update gluon 0.6.2 -> 0.7.0
- Update redis-exporter v1.73.0 -> v1.74.0
- Update redis 8.0.2 -> 8.0.3
- Update kubectl 18.0.1 -> 18.1.2
- Fix renovate issue that creates two MRs for one renovate.

## [0.3.6-bb.5] - 2025-06-06

### Changed

- Update BB redis chart to 21.1.3-bb.0

## [0.3.6-bb.4] - 2025-06-05

### Changed

- Update gluon 0.5.21 -> 0.6.2
- Update redis 8.0.1 -> 8.0.2

## [0.3.6-bb.3] - 2025-05-23

### Changed

- Update gluon to 0.5.21
- Update redis to 8.0.1
- Update redis-exporter to v1.73.0
- Update kubectl to 18.0.1

## [0.3.6-bb.2] - 2025-05-22

### Fixed

- Fixed renovate file to capture missing dependencies
- Fixed registry/repository tags in values.yaml to be renovate friendly

## [0.3.6-bb.1] - 2025-05-16

### Changed

- Update gitlab api token job to use live-in namespace instead of hardcoded to honor namespace deployment modification

## [0.3.6-bb.0] - 2025-04-01

### Changed

- Update redis upstream to BB maintained redis
- Update redis-exporter to v1.69.0
- Update redis to 7.4.2
- Update gluon to 0.5.15

## [0.3.5-bb.0] - 2025-01-28

### Changed

- Update gluon to 0.5.14
- Update redis to 7.4.2
- Update gitlab-ci-pipelines-exporter to v0.5.10

## [0.3.4-bb.12] - 2024-12-10

### Changed

- Update gluon to 0.5.12
- Update redis to 7.4.1
- Update gitlab-ci-pipelines-exporter to v0.5.9

## [0.3.4-bb.11] - 2024-12-06

### Changed

- Update the gitlab-ci-pipelines dashboard template to render to the config.gitlab.url endpoint instead of a learned var

## [0.3.4-bb.10] - 2024-12-03

### Changed

- downgrade redis-bb chart to match gitlab/postgresql to avoid a helper function conflict for labels

## [0.3.4-bb.9] - 2024-11-25

### Changed

- Added renovate definitions
- Moved redis-bb values to bigbang additions section in values.yaml
- Added the maintenance track annotation and badge

## [0.3.4-bb.8] - 2024-11-20

### Added

- Added gitlab CI jobs dashboard to package default grafana gitlab dashboards

## [0.3.4-bb.7] - 2024-11-18

### Fixed

- Fixed controlplaneCidr values indentation causing errors creating kube-egress-api netpol

## [0.3.4-bb.6] - 2024-11-15

### Changed

- Added kube api netpol

## [0.3.4-bb.5] - 2024-11-14

### Added

- Added kubernetes api netpol for create/update/patch secret job
- add create/update/patch verbs for hook job to secret

## [0.3.4-bb.4] - 2024-10-28

### Added

- Added internal service endpoint egress policy for gitlab communication

## [0.3.4-bb.3] - 2024-06-24

### Added

- Added https egress network policy

## [0.3.4-bb.2] - 2024-06-21

### Added

- Removed VS, added in granular inter-namespace, added in dashboard

## [0.3.4-bb.1] - 2024-06-13

### Added

- Adding in Security Drop all context and fixing tag
- Adding in auth, networkpolicies, peerauthentication, and vs
- added in tlsconfig for istio MTLS

## [0.3.4-bb.0] - 2024-06-07

### Added

- Initial creation of the chart in the sandbox
