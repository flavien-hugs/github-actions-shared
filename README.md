# GitHub Actions Shared

This repository contains shared GitHub Actions and reusable workflows used across our projects to standardize
CI/CD processes.

## Table of Contents
- [Available Actions](#available-actions)
  - [Bump Version](#bump-version)
  - [Cache Package](#cache-package)
  - [Docker Build & Push](#docker-build-and-push)
  - [Quality Code](#quality-code)
  - [Release](#release)
  - [Run Tests](#run-tests)
  - [Setup Python](#setup-python)
- [Reusable Workflows](#reusable-workflows)
  - [Shared CI Workflow](#shared-ci-workflow)
- [Usage](#usage)

---

## Available Actions

### Bump Version
Runs `semantic-release` to bump the project version and expose the new version.
- **Path**: `.github/actions/bump-version`
- **Inputs**:
  - `token` (Required): GitHub token to push tags.
- **Outputs**:
  - `version`: The new version after the bump.

### Cache Package
Sets up caching for Poetry and Pip dependencies to speed up workflows.
- **Path**: `.github/actions/cache-package`
- **Inputs**:
  - `cache-key` (Required): Key for the cache.

### Docker Build and Push
Builds and pushes a Docker image with proper tagging based on the Git ref (branch/tag).
- **Path**: `.github/actions/docker-build-push`
- **Inputs**:
  - `registry` (Required): Docker registry (e.g., `ghcr.io/org/repo`).
  - `username` (Required): Registry username.
  - `password` (Required): Registry password or token.
  - `version` (Required): Version to use for tagging.
- **Outputs**:
  - `image`: Full image name with primary tag.
  - `tags`: All tags applied to the image.

### Quality Code
Runs code quality checks using Black, Flake8, and Bandit.
- **Path**: `.github/actions/quality-code`
- **Inputs**: None.

### Release
Creates a new GitHub release on push to `main` using `semantic-release`.
- **Path**: `.github/actions/release`
- **Inputs**:
  - `github_token` (Required): GitHub token to publish the release.

### Run Tests
Runs tests using `pytest` with coverage and uploads the report as an artifact.
- **Path**: `.github/actions/run-tests`
- **Inputs**:
  - `token` (Required): GitHub token (used for submodules).

### Setup Python
Installs Python, Pipx, and Poetry, and prepares the environment.
- **Path**: `.github/actions/setup-python`
- **Inputs**: None.

---

## Reusable Workflows

### Shared CI Workflow
A complete CI pipeline including quality checks, testing, version bumping, release creation, and Docker image pushing.
- **Path**: `.github/workflows/ci-shared.yaml`
- **Secrets Required**:
  - `GHRC_REGISTRY_ADDR`: Docker registry address.
  - `GHRC_USERNAME`: Registry username.
  - `GHRC_PASSWORD`: Registry password.
  - `GH_SUBMODULE_TOKEN`: GitHub token for submodules and authenticated operations.

---

## Usage

To use an action in your repository:

```yaml
steps:
  - uses: flavien-hugs/github-actions-shared/.github/actions/setup-python@main
```

To use the shared CI workflow:

```yaml
jobs:
  ci:
    uses: flavien-hugs/github-actions-shared/.github/workflows/ci-shared.yaml@main
    secrets:
      GHRC_REGISTRY_ADDR: ${{ secrets.REGISTRY_ADDR }}
      GHRC_USERNAME: ${{ secrets.REGISTRY_USERNAME }}
      GHRC_PASSWORD: ${{ secrets.REGISTRY_PASSWORD }}
      GH_SUBMODULE_TOKEN: ${{ secrets.MY_GITHUB_TOKEN }}
```
