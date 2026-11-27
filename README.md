# Renovate Config

Shared [Renovate](https://docs.renovatebot.com/) configuration for Blue Beetle repositories.

## Usage

Reference this config in your repository's `renovate.json`:

```json
{
    "$schema": "https://docs.renovatebot.com/renovate-schema.json",
    "extends": [
        "config:recommended",
        "local>bluebeetlept/renovate-config"
    ],
    "rangeStrategy": "auto"
}
```

## Shared workflows

### Biome migrate (`.github/workflows/biome-migrate.yml`)

Renovate's `@biomejs/biome` bumps only change `package.json` and the lock file, and the hosted Renovate app can't run commands on its branches. This reusable workflow runs `biome migrate --write` at the bumped version (with `npx`, so nothing is installed) and commits the updated `biome.json` to the same pull request.

A repository uses it with `.github/workflows/renovate-biome-migrate.yml`:

```yaml
name: Renovate Biome migrate

on:
  pull_request:
    types: [opened, reopened, synchronize]

jobs:
  migrate:
    if: github.event.pull_request.user.login == 'renovate[bot]' && startsWith(github.head_ref, 'renovate/biomejs-biome')
    uses: bluebeetlept/renovate-config/.github/workflows/biome-migrate.yml@main
    secrets:
      BOT_TOKEN: ${{ secrets.BLUEBEETLE_BOT_PAT }}
```

The commit is pushed with `BOT_TOKEN` so the pull request's checks run again on it. Once a commit that isn't Renovate's is on its branch, Renovate stops rebasing that pull request by itself.

This repository is public, so any repository can call the workflow; it holds no secrets, they're passed in by the caller.

