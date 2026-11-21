# Changelog

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
