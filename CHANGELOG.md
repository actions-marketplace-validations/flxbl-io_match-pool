# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
