# renovate-config

Shareable [Renovate](https://docs.renovatebot.com/) configuration for Jubblin repositories: an org-level inherited config plus named presets under `presets/`.

## Consume presets

Extend a preset from another repository's Renovate config:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>jubblin/renovate-config//presets/github-actions",
    "github>jubblin/renovate-config//presets/swift"
  ]
}
```

Pin to a released tag once versions exist:

```json
{
  "extends": [
    "github>jubblin/renovate-config//presets/github-actions#v1.0.0"
  ]
}
```

### Available presets

| Path | Purpose |
| --- | --- |
| `presets/github-actions.json` | Group and automerge GitHub Actions updates |
| `presets/swift.json` | Group Swift Package Manager updates |

## Org inherited config

[`org-inherited-config.json`](org-inherited-config.json) is the org-wide inherited Renovate configuration (best practices, schedules, automerge, and default extends). Point your GitHub org Renovate app / Mend hosted config at this file as documented for [shared/org config](https://docs.renovatebot.com/getting-started/running/#global--shared-repository-configuration).

## Validate locally

Structure and schema checks (same idea as CI):

```bash
python3 -m json.tool org-inherited-config.json > /dev/null
python3 -m json.tool presets/*.json > /dev/null
```

Strict Renovate validation (`--no-global` treats these as presets, not self-hosted global config):

```bash
for f in org-inherited-config.json presets/*.json; do
  npx --yes --package renovate@44 -- renovate-config-validator --strict --no-global "$f"
done
```

## Releases

Merges to `main` that include releasable [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, etc.) trigger [semantic-release](https://semantic-release.org/). That updates `CHANGELOG.md`, creates a `vX.Y.Z` git tag, and publishes a GitHub Release. Pure `chore:` / `docs:` commits do not cut a release.

CI (`.github/workflows/ci.yml`) validates JSON structure and Renovate config on every PR and push to `main`.
