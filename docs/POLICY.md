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
  "protectedPaths": ["ci/**"],
  "maxRisk": "medium",
  "secretRules": [{"name": "internal-token", "pattern": "corp_[A-Za-z0-9]{20}", "note": "issued by the platform team"}],
  "entropyScan": true
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
| `maxRisk` | `low`, `medium` or `high`. Keep refuses a change set whose [risk score](COMMAND-LINE.md#risk-report) is above this level. |
| `secretRules` | Your own credential patterns (`name`, `pattern`, optional `note`), added to the built-in scan. |
| `entropyScan` | Opt-in: also flags long random-looking tokens in added lines. |

Hooks run inside the sandbox, never on your Mac.

## Credential scan

Keep refuses added lines that look like credentials. The report names the rule and the line, never the text. The scan cannot be disabled by policy.

`secretRules` add patterns on top of the built-in scan and never replace it. `sbx policy check` rejects invalid regular expressions, patterns over 200 characters, patterns that match the empty string, back-references and nested quantifiers such as `(a+)+`; only the first 2000 characters of a line are matched. Findings show `custom: <name>` and the line. `entropyScan` flags tokens of 32 or more characters with high entropy that contain a letter and a digit, and skips hex hashes, lockfiles, integrity or checksum lines and `secretAllow` matches. It is deliberately conservative, so it will miss short or low-entropy secrets.

## Risk limit

`maxRisk` uses the deterministic change-set risk report (`sbx risk`). `--override-policy` bypasses it; the MCP server never can. The score is heuristic: a low score does not mean a change is safe.

## Overrides

The app asks "Keep Anyway"; `sbx keep` has `--override-policy` and `--ack-secrets`. The MCP server can override neither.

Check a policy with `sbx policy check <project>`. See what a policy would do to a finished session with `sbx policy test <id>` (it writes nothing). Create a starter policy with `sbx init <project> --preset node|python|swift|rust|go|docs-only|strict`; `sbx policy presets` lists them.
