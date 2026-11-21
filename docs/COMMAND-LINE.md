# Command line (`sbx`) and MCP

The app ships an `sbx` command line and an MCP server (stdio) so scripts and agents can drive sandboxes. Nothing here can write to your project except `keep`, and the MCP `sandbox_keep` tool is off unless the server is started with `--allow-keep`.

## Sessions

| Command | Does |
| --- | --- |
| `sbx run <project> [--profile P] [--repeat N] [--env-file F] -- <command>` | Stage and run |
| `sbx list [--tag T] [--search S] [--state S]` | List sessions (pinned first) |
| `sbx changes <id> [--stat]`, `sbx diff <id> [path] [--hunks]` | Review |
| `sbx keep <id> [--hunks path:0,2] [--dry-run]` | Write chosen changes to the project (`--dry-run` writes nothing) |
| `sbx discard <id>` | Delete the sandbox copy |
| `sbx rerun <id>`, `sbx clone <id>` | New session from an existing one's spec |
| `sbx pin <id>`, `sbx unpin <id>` | Pinned sessions are never pruned or bulk-removed |
| `sbx tag <id> --add T`, `sbx note <id> "text"` | Organise |
| `sbx checkpoint <id>`, `sbx restore <id>` | Save and restore the sandbox copy |
| `sbx compare <a> <b>` | Compare two runs' change sets |
| `sbx wait <id> [--timeout S]` | Block until the run ends; exit code follows the agent |
| `sbx export <id> -o run.sbxbundle`, `sbx import run.sbxbundle <project>` | Hand a session over (patch, runs, audit; no secret values or names) |

## Reports and housekeeping

- `sbx transcript <id> --format md|json`, `sbx history <id> [--run N]`
- `sbx audit [--since 7d] [--event keep] [--csv]`, `sbx report --days 7 --format md|csv`, `sbx stats [--days N] [--budget-minutes M]`
- `sbx status [--json] [--quiet-if-idle]`: one line for prompts and scripts
- `sbx discard --state failed|review|staged` and `sbx forget --state kept|discarded` (with `--tag`, `--older-than 7d`) only list what they would do until you add `--yes`; pinned and running sessions are always skipped
- `sbx prune [--dry-run] [--max-disk-gb N]`
- `sbx doctor [--json]`, `sbx logs [--lines N] [--daemon]` (credentials shown as `[redacted]`)
- `sbx profile save|list|show|export|import`, `sbx secrets`, `sbx init`, `sbx policy check`, `sbx completions zsh|bash`

## MCP

Start the server with `sbx mcp` (add `--allow-keep` to expose keep tools). Read-only tools include status, wait, transcript, report, stats, audit, compare, hunks, checkpoint, doctor, policy check, keep preview and prune preview (a dry run: the MCP server never prunes). No tool can override the project policy or the credential scan.

## Limits

The engine runs the agent command as one blocking request, so `sbx watch` follows state, not live output.
