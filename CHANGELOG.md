# Changelog

## 0.7.0 (built, signed, notarized and stapled; not published here)

The downloads in this repository have not been updated to 0.7.0. This entry describes the 0.7.0 build.

### Added
- Risk report: `sbx risk <id>` (and the MCP tool `sandbox_risk`) scores a session's changes low, medium or high without a model, and the review screen shows a Risk badge. New policy key `maxRisk` makes Keep refuse a change set scoring above it. It is a heuristic, not a safety guarantee. See [docs/POLICY.md](docs/POLICY.md).
- Policy tools: `sbx policy test <id>` (and MCP `sandbox_policy_test`) lists every rule a change set would hit and writes nothing; `sbx policy presets` and `sbx init <project> --preset ...` write starter policies; `secretRules` add your own credential patterns; `entropyScan` is an opt-in extra check.
- Scheduled runs: `sbx run --schedule` and `sbx schedule add|list|status|run-now|remove`, one launchd agent per schedule. A scheduled run never keeps its own result: it ends in review. MCP `sandbox_schedules` is read-only.
- Keep as pull request: `sbx keep <id> --pr` pushes the kept changes as a branch and runs `gh pr create`. Without `--yes` it is a read-only preview. MCP has only the read-only `sandbox_pr_preview`; no MCP tool opens a pull request. The app has a Keep as Pull Request confirmation sheet.
- App windows for features that were command-line only: a Checkpoints tab with a restore preview, Compare Runs, an Audit Log with filters and CSV export, and policy tools in Settings.
- Documentation updated: [command line](docs/COMMAND-LINE.md), [policy](docs/POLICY.md), [FAQ](docs/FAQ.md).

### Verified and not verified
The app's self-tests pass: 33 suites, 1112 checks, 0 failed. The new app windows were compiled and covered by those tests, but not click-tested on a live desktop. 0.7.0 was not run against a real Linux VM; the tests use an engine stub, and the scheduling tests use a fake launchd host, so a real scheduled firing and real `launchctl` loading are not verified. Still not available: a network allow-list and egress log (needs the vmnet entitlement), extra Linux templates, macOS guest mounting, live output streaming, App Intents.

## Unreleased: 0.3.0 to 0.6.1 (built and notarized, not yet published here)

The downloadable release on this page is still v0.1.0. These features are in the 0.6.1 build.

### Added
- Review: per-hunk keep, a `+/-` totals line, a History tab with every run's exit code, duration and scrubbed output, Preview Keep, Rerun, Clone and Pin.
- Guardrails: project policy (`deny`, `allow`, size caps, hooks, `forceOffline`, `maxMinutes`, `protectedPaths`) and a credential scan on Keep. See [docs/POLICY.md](docs/POLICY.md).
- Checkpoints, run hooks with auto-checkpoint, saved profiles, `run --repeat` and compare, an audit log.
- Search, tags and notes; session bundles (export and import); run statistics and reports.
- Clean Up Sessions (bulk discard or forget with a preview) and a Diagnostics window.
- `sbx` command line and an MCP server. See [docs/COMMAND-LINE.md](docs/COMMAND-LINE.md).

### Not verified
The new app windows were compiled and covered by the app's self-tests but not click-tested on a live desktop. Live output streaming, a network allow-list, extra Linux templates and macOS guest mounting are still not available.

## v0.1.0 - 2026-11-20

First release of Agent Sandboxes for Mac, notarized, with a 30-day trial. Requires macOS 27 or later on Apple silicon.

### Added
- Disposable VM sandboxes: stage (APFS clone), run an agent command, review a per-file diff, keep or discard.
- Debian 13 and Agent micro Linux templates; macOS guest template with a two-guest limit.
- Conflict detection for files changed in the project during a run; path-safety rules on Keep.
- Bundled FluxVM 0.4.21 engine on 127.0.0.1:7799 with its own state folder. Only the virtualization entitlement.

### Not verified
macOS guest project mounting, IPSW install and boot. A Linux guest boot through the whole flow was verified. No vmnet networking, no USB passthrough.

Free 30-day trial key: sales@zyvor.dev. The app works without a key.
