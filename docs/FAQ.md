# FAQ

**Does the agent touch my real project?** No. The VM mounts an APFS clone. Your project changes only on Keep, and only for files you ticked.

**Which agents work?** Any command you can run in the guest.

**Intel Macs?** No, Apple silicon only.

**macOS guests?** Template and two-guest limit are built; project mounting, IPSW install and boot are not verified in 0.1.0.

**Does it conflict with FluxVM?** No. It bundles FluxVM 0.4.21 and runs its own daemon on port 7799.

**Price?** INR 3,999 per Mac per year incl. 18% GST in India (about US$40 excl. tax outside India). Teams of 5 or more Macs: INR 3,399 per Mac per year (about US$34 excl. tax), minimum 5 Macs (INR 16,995, about US$171), by invoice. Buy via sales@zyvor.dev.

**Are the screenshots real?** Renders of the app's own views with invented sample data.

**Can I script it or limit what Keep writes?** The 0.7.0 build (not yet published here; the download is 0.1.0) has an `sbx` command line, an MCP server and a per-project policy file. See [COMMAND-LINE.md](COMMAND-LINE.md) and [POLICY.md](POLICY.md).

**Is there a risk score for a change?** The 0.7.0 build has `sbx risk <id>`: fixed rules (dependency files, install scripts, CI files, executable bits, obfuscated blobs, network calls and so on) give low, medium or high. It is a heuristic, not a guarantee, and a `maxRisk` policy key can make Keep refuse changes above a level.

**Can I run a sandbox on a schedule?** The 0.7.0 build can, with `sbx schedule`. A scheduled run never keeps its own result; it ends in review. Real `launchctl` loading and a timed firing were not verified.

**Can an agent open a pull request through MCP?** No. `sbx keep <id> --pr` is a command-line action (a dry run unless you add `--yes`), and MCP only has the read-only `sandbox_pr_preview`.

**Was 0.7.0 tested?** The app's self-tests pass (33 suites, 1112 checks, 0 failed). The app windows were not click-tested on a live desktop and 0.7.0 was not run against a real Linux VM.
