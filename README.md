# Parse Deploy Config Source

**Internal Action** - Parses deployment configuration sources for cross-repository deployments.

## Purpose

This action parses deployment source configurations to determine where Kubernetes manifests are located (same repo vs separate repo). Used internally by deploy workflows.

## Usage

```yaml
# Internal use in deploy workflows
- uses: KoalaOps/parse-deploy-config-source@v1
  with:
    deploy_config_source: "KoalaOps/k8s-configs:services/backend"
    deployment_ref: main
    environment: production
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `deploy_config_source` | Where to read deployment config from: `<repo>[@<ref>][:<path>]` | ❌ | '' |
| `deployment_ref` | Default git ref to use if not specified in deploy_config_source | ❌ | - |
| `service_dir` | Service directory for default path construction | ❌ | '.' |
| `environment` | Environment name for default path construction | ❌ | - |
| `current_repository` | Current repository | ❌ | `${{ github.repository }}` |
| `current_ref` | Current git ref | ❌ | `${{ github.ref }}` |

## Outputs

| Output | Description |
|--------|-------------|
| `repository` | The parsed repository (owner/repo format) |
| `ref` | The parsed git ref (branch or tag) |
| `path` | The parsed path to deployment configuration |
| `is_self` | Whether this is the current repository (true/false) |

## Config Source Format

Format: `<repo>[@<ref>][:<path>]`

- `repo`: "self" for current repo, or "owner/repo"  
- `@ref`: optional branch or tag (defaults to deployment_ref or repo default)
- `:path`: optional path to overlay/chart root (auto-discovered if omitted)

Examples:
- `self`
- `self:deploy/overlays/prod`
- `self:services/api/deploy/overlays/staging`
- `Acme/deployments:services/api/deploy/overlays/prod`
- `Acme/deployments@main:platform/payments/deploy/overlays/dev`

## Note

This is an internal action used by deploy workflows to handle cross-repository deployment configurations. Not intended for direct use.