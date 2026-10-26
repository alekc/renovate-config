# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) presets for alekc's repositories. Renovate runs
self-hosted (renovate-operator, GitHub App `alekc-bot`) with a global base config: majors wait for
Dependency Dashboard approval, releases must be 2 days old, branches are created between 00:00 and
06:00 Europe/London, and known vulnerabilities are fixed straight away. Each repository picks one
preset on top of that:

| Preset | Use it when | Effect |
| --- | --- | --- |
| `github>alekc/renovate-config:ci-automerge` | PRs run real tests | Minor, patch and digest updates merge once checks pass |
| `github>alekc/renovate-config:build-automerge` | PRs only build | Actions, Docker images and patch updates merge once the build passes |
| `github>alekc/renovate-config:manual-grouped` | No CI, or you want to review everything | All non-major updates as one PR on Monday, nothing merges by itself |

A repository's `renovate.json` then stays short:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>alekc/renovate-config:ci-automerge"]
}
```

Renovate merges eligible PRs itself on its next run (`platformAutomerge` is off), so no branch
protection or GitHub auto-merge setting is needed. A repository with no CI checks only automerges
with `"ignoreTests": true`.
