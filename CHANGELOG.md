# Changelog

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
