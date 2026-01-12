# Match Pool Action

[![CI](https://github.com/flxbl-io/match-pool/actions/workflows/ci.yml/badge.svg)](https://github.com/flxbl-io/match-pool/actions/workflows/ci.yml)

A GitHub Action that matches pool assignment rules based on branch and domain patterns using `sfp review-envs rules match`.

## Usage

### Basic Usage

```yaml
- name: Match Pool
  id: match-pool
  uses: flxbl-io/match-pool@v1
  with:
    sfp-server-url: ${{ secrets.SFP_SERVER_URL }}
    sfp-server-token: ${{ secrets.SFP_SERVER_TOKEN }}
    branch: ${{ github.head_ref }}
    domain: core

- name: Use matched pool
  run: |
    echo "Pool Tag: ${{ steps.match-pool.outputs.pool-tag }}"
    echo "Pool Type: ${{ steps.match-pool.outputs.pool-type }}"
```

### With Fallback Pool

```yaml
- name: Match Pool
  id: match-pool
  uses: flxbl-io/match-pool@v1
  with:
    sfp-server-url: ${{ secrets.SFP_SERVER_URL }}
    sfp-server-token: ${{ secrets.SFP_SERVER_TOKEN }}
    branch: ${{ github.head_ref }}
    domain: core
    fallback-pool: default-pool
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `sfp-server-url` | URL to SFP Server (e.g., `https://your-org.flxbl.io`) | Yes | - |
| `sfp-server-token` | SFP Server application token | Yes | - |
| `repository` | Repository name (`owner/repo` format) | No | Current repository |
| `branch` | Branch to match against pool assignment rules | Yes | - |
| `domain` | Domain to match against pool assignment rules | Yes | - |
| `fallback-pool` | Pool to use if no matching rules are found | No | - |

## Outputs

| Output | Description |
|--------|-------------|
| `pool-tag` | The matched pool tag |
| `pool-type` | Type of pool (`SANDBOX` or `SCRATCH_ORG`) |
| `matched` | Whether a match was found (`true`/`false`) |
| `match-result` | Full match result as JSON |

## How It Works

```
+-----------------------------------------------------------+
|                       match-pool                          |
+-----------------------------------------------------------+
|                                                           |
|  1. Query SFP Server review-envs rules                    |
|     - Repository: owner/repo                              |
|     - Branch: feature/my-branch                           |
|     - Domain: core                                        |
|                                                           |
|  2. SFP Server evaluates rules                            |
|     - Branch pattern matching                             |
|     - Domain pattern matching                             |
|     - Priority ordering                                   |
|                                                           |
|  3. Return matched pool                                   |
|     +-- Match found -> pool-tag, pool-type                |
|     +-- No match -> fallback-pool (if provided)           |
|     +-- No match, no fallback -> Error                    |
|                                                           |
+-----------------------------------------------------------+
```

## Prerequisites

- **SFP Server**: Active SFP Server instance with review environment rules configured
- **Runtime**: `sfp` CLI must be available (use `sfops` Docker image)

## Example: PR Validation with Pool Matching

```yaml
jobs:
  validate:
    runs-on: ubuntu-latest
    container: ghcr.io/flxbl-io/sfops:latest

    steps:
      - uses: actions/checkout@v4

      - name: Match Pool
        id: match-pool
        uses: flxbl-io/match-pool@v1
        with:
          sfp-server-url: ${{ secrets.SFP_SERVER_URL }}
          sfp-server-token: ${{ secrets.SFP_SERVER_TOKEN }}
          branch: ${{ github.head_ref }}
          domain: core
          fallback-pool: dev-pool

      - name: Fetch Review Org
        run: |
          sfp pool:fetch \
            --pool ${{ steps.match-pool.outputs.pool-tag }} \
            --alias review-org
```

## Example: Dynamic Pool Selection

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    container: ghcr.io/flxbl-io/sfops:latest

    steps:
      - uses: actions/checkout@v4

      - name: Match Pool for Domain
        id: match-pool
        uses: flxbl-io/match-pool@v1
        with:
          sfp-server-url: ${{ secrets.SFP_SERVER_URL }}
          sfp-server-token: ${{ secrets.SFP_SERVER_TOKEN }}
          branch: ${{ github.ref_name }}
          domain: ${{ matrix.domain }}

      - name: Deploy to matched environment
        if: steps.match-pool.outputs.matched == 'true'
        run: |
          echo "Deploying to pool: ${{ steps.match-pool.outputs.pool-tag }}"
          echo "Pool type: ${{ steps.match-pool.outputs.pool-type }}"
```

## License

Copyright 2025 flxbl-io. All rights reserved. See [LICENSE](LICENSE) for details.

## Support

- [Documentation](https://docs.flxbl.io)
- [Issues](https://github.com/flxbl-io/match-pool/issues)
- [flxbl.io](https://flxbl.io)
