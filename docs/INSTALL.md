# Install

## Requirements

- macOS 27 or later (`LSMinimumSystemVersion` 27.0), **Apple silicon**
- For macOS guests only: a macOS guest installed once from an IPSW with FluxVM (not verified in 0.1.0)

## Download and verify

1. Download `Agent-Sandboxes-0.1.0.dmg` and `Agent-Sandboxes-0.1.0.dmg.sha256` from the [release page](https://github.com/zyvorai/agent-sandboxes/releases/tag/v0.1.0).
2. Optional: verify the checksum.

   ```
   cd ~/Downloads
   shasum -a 256 -c Agent-Sandboxes-0.1.0.dmg.sha256
   ```

3. Open the DMG and drag **AgentSandboxes** to **Applications**.

The app is Developer ID signed and notarized by Apple.

## First launch

Agent Sandboxes is a menu-bar app. It starts its own bundled FluxVM engine on `127.0.0.1:7799`, with state in `~/Library/Application Support/Agent Sandboxes/`. A FluxVM you already run (port 7788) is not touched. The first Linux sandbox downloads its base image.

## Licence key (optional)

The app works without a key. For a 30-day trial key write to **sales@zyvor.dev**, choose **Licence…**, paste the key and press Verify and Save. See [Licence and trial](LICENSING.md).

## Uninstall

Quit the app, delete `AgentSandboxes.app`, and optionally delete `~/Library/Application Support/Agent Sandboxes/`.
