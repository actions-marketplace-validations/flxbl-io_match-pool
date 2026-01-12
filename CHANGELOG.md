# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## 1.0.0 (2026-01-12)


### Features

* add manual trigger to CI workflow ([57d42e1](https://github.com/flxbl-io/match-pool/commit/57d42e181314b1e7d8905c70f5642bcc350ba728))
* initial release of match-pool action ([941597e](https://github.com/flxbl-io/match-pool/commit/941597ef9d5091cbaaab9d83fd3c3e7e88fde809))


### Bug Fixes

* use correct sfp review-envs rules match command ([fcc48b8](https://github.com/flxbl-io/match-pool/commit/fcc48b8adfcc0c7ba6a7a2398ee9d08919f6829f))
* use GHA_TOKEN for release-please ([efbdd48](https://github.com/flxbl-io/match-pool/commit/efbdd48314e89bb1f62419b98bda5927d8c5a924))

## [Unreleased]

## [1.0.0] - 2025-01-10

### Added

- Initial release as standalone Marketplace action
- Match pool assignment rules via SFP Server review-envs API
- Support for branch and domain pattern matching
- Fallback pool option when no rules match
- Rich outputs including pool-tag, pool-type, matched status, and full JSON result
- Aligned header with sfp-pro CLI style

### Features

- **Rule Matching**: Query SFP Server to match pool assignment rules
- **Pattern Support**: Branch and domain pattern matching
- **Fallback Support**: Optional fallback pool when no rules match
- **Detailed Outputs**: pool-tag, pool-type, matched, match-result (JSON)

[Unreleased]: https://github.com/flxbl-io/match-pool/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/flxbl-io/match-pool/releases/tag/v1.0.0
