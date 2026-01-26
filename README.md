# Palpito Hunch - Organization Configuration

This repository contains organization-wide GitHub configurations and reusable workflows for all `palpito-hunch` repositories.

## Reusable Workflows

### Branch Flow Enforcement

**File:** `.github/workflows/branch-flow.yml`

Enforces the following merge flow across all repositories:

```
feature/* ──► develop ──► uat ──► main
                 ▲                  │
                 └──────────────────┘
                    (sync back)

hotfix/* ──► main (emergencies)
hotfix/* ──► uat (testing)
```

#### Allowed Flows

| Source | Target | Description |
|--------|--------|-------------|
| `develop` | `uat` | Regular promotion to UAT |
| `uat` | `main` | Release to production |
| `main` | `develop` | Sync production back to develop |
| `feature/*` | `develop` | Feature branch merge |
| `hotfix/*` | `main` | Emergency production fix |
| `hotfix/*` | `uat` | Test hotfix in UAT |

#### Usage

Add this workflow to any repository:

```yaml
# .github/workflows/branch-flow.yml
name: Branch Flow

on:
  pull_request:
    branches: [main, uat, develop]

jobs:
  check:
    uses: palpito-hunch/.github/.github/workflows/branch-flow.yml@v1
```

#### Versioning

| Tag | Description |
|-----|-------------|
| `@v1` | Stable version (recommended) |
| `@main` | Latest development version |

To update workflows across all repos, move the version tag:

```bash
# Update v1 to point to latest commit
git tag -d v1
git push origin :refs/tags/v1
git tag -a v1 -m "v1: Description of changes"
git push origin v1
```

## Branch Protection

All public repositories have branch protection enabled on `main`:

- Require 1 approval before merging
- Dismiss stale reviews when new commits are pushed
- Require "Branch Flow" status check to pass
- Enforce rules for administrators
- Block force pushes
- Block branch deletion

## Repositories

| Repository | Description | Visibility |
|------------|-------------|------------|
| [windows-bootstrap](https://github.com/palpito-hunch/windows-bootstrap) | Windows dev environment setup | Public |
| [macos-bootstrap](https://github.com/palpito-hunch/macos-bootstrap) | macOS dev environment setup | Public |
| [frontend-template](https://github.com/palpito-hunch/frontend-template) | Next.js frontend template | Public |
| [backend-template](https://github.com/palpito-hunch/backend-template) | Node.js backend template | Public |
| [ai-rules](https://github.com/palpito-hunch/ai-rules) | AI coding assistant rules | Public |
| [prediction-market-backend](https://github.com/palpito-hunch/prediction-market-backend) | Prediction market API | Public |

## Contributing

1. Create a feature branch
2. Make changes
3. Create a PR to `main`
4. Get approval from a team member
5. Merge
