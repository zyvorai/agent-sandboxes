# Project policy and guardrails

Put a `.sandbox-policy.json` at the root of your **real** project. It is read from the project, never from the sandbox copy, so an agent cannot edit its own rules. Every key is optional.

```json
{
  "deny": [".github/**", ".env*"],
  "allow": ["src/**"],
  "maxFiles": 200,
  "maxBytes": 5000000,
  "maxFileBytes": 1000000,
  "before": "npm ci",
  "after": "npm test",
  "autoCheckpoint": true,
  "restoreOnFailure": false,
  "forceOffline": true,
  "maxMinutes": 30,
  "secretAllow": ["FAKE_KEY_FOR_TESTS"],
  "protectedPaths": ["ci/**"]
}
```

| Key | Effect |
| --- | --- |
| `deny` | Keep refuses paths matching these globs. |
| `allow` | When set, Keep may only write matching paths. `deny` still wins. |
| `maxFiles`, `maxBytes`, `maxFileBytes` | Caps on a change set; oversized binary files are called out. |
| `before` | Runs inside the sandbox before the agent and must succeed for the agent to start. |
| `after` | Always runs inside the sandbox after the agent; the agent's exit code is kept. |
| `autoCheckpoint` | Snapshot the sandbox copy before every run. |
| `restoreOnFailure` | Put that snapshot back when the agent exits non-zero. It discards that run's partial work, so it is opt-in. |
| `forceOffline` | Networking off for every run in the project. |
| `maxMinutes` | Lowers the time limit; never raises it. |
| `secretAllow` | Known-safe fixture lines the credential scan should skip. |
| `protectedPaths` | A run may not change these: the session is flagged and Keep refuses them. |

Hooks run inside the sandbox, never on your Mac.

## Credential scan

Keep refuses added lines that look like credentials. The report names the rule and the line, never the text. The scan cannot be disabled by policy.

## Overrides

The app asks "Keep Anyway"; `sbx keep` has `--override-policy` and `--ack-secrets`. The MCP server can override neither.

Check a policy with `sbx policy check <project>`; create a starter one with `sbx init <project>`.
