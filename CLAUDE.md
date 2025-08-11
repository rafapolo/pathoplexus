# CLAUDE.md

## What This Repository Is

Pathoplexus is a specialized genomic database for viruses of public health importance. This repository is an **overlay** over the [Loculus](https://github.com/loculus-project/loculus) monorepo - it contains only Pathoplexus-specific customizations that are layered on top of the base Loculus codebase.

## How The Overlay Architecture Works

The key insight is that this repo doesn't contain a complete application - it's a set of customizations applied to Loculus:

1. **Version Pinning**: `pathoplexus_app/values.yaml` specifies exactly which Loculus commit to use
2. **Sync Process**: `./sync.sh` fetches that specific Loculus version and copies it to `monorepo/` (which is gitignored)
3. **Overlay**: Pathoplexus-specific files in `monorepo/` override the corresponding Loculus files
4. **Scope Control**: `monorepo/.gitignore` determines which files are Pathoplexus-specific vs inherited from Loculus

## Repository Structure

- **`monorepo/`**: Pathoplexus website customizations (overlays Loculus website code)
- **`loculus_values/`**: Configuration files that define how Pathoplexus differs from base Loculus
- **`pathoplexus_app/`**: Kubernetes deployment config and Loculus version reference
- **`data-integrity-tests/`**: Snakemake workflows for regression testing across environments
- **`scripts/`**: Database utility scripts

## Key Commands

```bash
# Sync with the pinned Loculus version
./sync.sh

# Start local development (after sync)
cd monorepo/website && npm ci && npm run dev

# Run data integrity tests
cd data-integrity-tests && snakemake -c1

# Clean up to see only Pathoplexus files
git clean -fX monorepo
```

## Configuration System

**Core Config**: `loculus_values/values.yaml` contains the main Pathoplexus configuration including:
- Supported organisms (cchf, west-nile, ebola variants, mpox, rsv-a/b, hmpv)
- Accession prefix ("PPD_")
- Authentication settings (ORCID integration)
- Website customizations and branding

**Environment Configs**: `loculus_values/environment_specific_values/` contains overrides for demo/staging/production deployments.

## Deployment Flow

Deployments are controlled by a separate `pathoplexus/loculus_deployments` repository:
1. This repo contains the code and configs
2. The deployments repo points to specific commit SHAs from this repo
3. ArgoCD watches the deployments repo and deploys accordingly

## Testing Infrastructure

Data integrity testing compares production vs staging data for all supported organisms using Snakemake workflows. The tests check both sequence data and metadata consistency across environments.
