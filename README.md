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

## Presets

- `default.json`: the shared settings, used with `local>bluebeetlept/renovate-config`.
- `php.json`: Renovate updates lock files on the PHP version the docker-dev containers run (8.4), not the newest one `composer.json` allows. Add `local>bluebeetlept/renovate-config:php` to the `extends` of a repository developed in docker-dev, and bump it here by hand when the containers move to a new PHP version.
