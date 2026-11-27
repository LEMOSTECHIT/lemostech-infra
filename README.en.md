# lemostech-infra

Reusable GitHub Actions workflows for the LemosTech repositories.
One place to change the pipeline, and every repository follows — as long as they
point to a **tag**, never to `main`.

## Why it exists

Each application had (or would have) the same copied sequence: install Python,
install dependencies, run lint, run tests, sometimes with coverage. A copied
pipeline means fixing a problem requires editing N repositories — and that each
one drifts from the others over time.

Here the logic lives once. The application repository only declares **what is its
own**: where the code is, which dependencies file, which test command.

## What's in here

| File | Role |
|---|---|
| `.github/workflows/ci-python.yml` | Reusable CI: installs dependencies, runs Ruff (check and format) and pytest, with optional coverage |
| `.github/workflows/cd-deploy-receptor.yml` | Reusable CD: calls the server's deploy receptor (`deploy.lemostech.dev.br`) and requires HTTP 202 |
| `.github/workflows/lint-workflows.yml` | Checks the workflows themselves with `actionlint` on every push |
| `exemplos/itsm/` | How the ITSM looks after the migration (pilot) |
| `docs/guia-uso.md` | Usage rules, versioning and what should not be centralized |

## Quick usage

In the application repository:

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: LEMOSTECHIT/lemostech-infra/.github/workflows/ci-python.yml@v1
    with:
      python-version: "3.12"
      requirements-file: requirements.txt
      coverage-min: 70
      coverage-source: apps
```

## Golden rule

**Never point to `@main`.** A bad commit here would break the pipeline of every
repository at once. Use a tag (`@v1`), test the change in a pilot repository and
only then move the tag.

## Status

Created on 27/11/2026. Pilot: ITSM. Nothing has been published to GitHub yet.
