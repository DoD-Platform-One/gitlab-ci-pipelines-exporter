# Changelog

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---
##[0.3.4-bb.11] - 2024-12-06
### Changed
- Update the gitlab-ci-pipelines dashboard template to render to the config.gitlab.url endpoint instead of a learned var

##[0.3.4-bb.10] - 2024-12-03
### Changed
- downgrade redis-bb chart to match gitlab/postgresql to avoid a helper function conflict for labels

##[0.3.4-bb.9] - 2024-11-25
### Changed
- Added renovate definitions
- Moved redis-bb values to bigbang additions section in values.yaml
- Added the maintenance track annotation and badge

##[0.3.4-bb.8] - 2024-11-20
### Added
- Added gitlab CI jobs dashboard to package default grafana gitlab dashboards

##[0.3.4-bb.7] - 2024-11-18
### Fixed
- Fixed controlplaneCidr values indentation causing errors creating kube-egress-api netpol 

##[0.3.4-bb.6] - 2024-11-15
### Changed
- Added kube api netpol

##[0.3.4-bb.5] - 2024-11-14
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
