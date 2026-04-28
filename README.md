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
