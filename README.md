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
      BOT_TOKEN: ${{ secrets.RENOVATE_PR_TOKEN }}
```

The commit is pushed with `BOT_TOKEN` so the pull request's checks run again on it (a push with the job's own token wouldn't start them). Give it a fine-grained token with only *Contents: read and write* on the calling repositories. It is only ever handed to the push step: the checkout keeps no credentials and Biome runs with install scripts off, so the npm package never sees it. Each organisation keeps that token as a `RENOVATE_PR_TOKEN` secret; on a free plan, organisation secrets don't reach private repositories, so a private repository there needs it as a repository secret. Once a commit that isn't Renovate's is on its branch, Renovate stops rebasing that pull request by itself.

This repository is public, so any repository can call the workflow; it holds no secrets, they're passed in by the caller.

