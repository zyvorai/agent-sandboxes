# Command line (`sbx`) and MCP

The app ships an `sbx` command line and an MCP server (stdio) so scripts and agents can drive sandboxes. Nothing here can write to your project except `keep`, and the MCP `sandbox_keep` tool is off unless the server is started with `--allow-keep`.

## Sessions

| Command | Does |
| --- | --- |
| `sbx run <project> [--profile P] [--repeat N] [--env-file F] -- <command>` | Stage and run |
| `sbx list [--tag T] [--search S] [--state S]` | List sessions (pinned first) |
| `sbx changes <id> [--stat]`, `sbx diff <id> [path] [--hunks]` | Review |
| `sbx keep <id> [--hunks path:0,2] [--dry-run]` | Write chosen changes to the project (`--dry-run` writes nothing) |
| `sbx keep <id> --pr [--remote origin] [--base main] [--title T] [--draft] [--yes]` | Keep as a pull request. Without `--yes` it is a dry run that prints the branch, remote (credentials stripped), base, title and body, and changes nothing |
| `sbx risk <id> [--paths a,b] [--json]` | Risk score of the changes; exit 1 when high |
| `sbx discard <id>` | Delete the sandbox copy |
| `sbx rerun <id>`, `sbx clone <id>` | New session from an existing one's spec |
| `sbx pin <id>`, `sbx unpin <id>` | Pinned sessions are never pruned or bulk-removed |
| `sbx tag <id> --add T`, `sbx note <id> "text"` | Organise |
| `sbx checkpoint <id>`, `sbx restore <id>` | Save and restore the sandbox copy |
| `sbx compare <a> <b>` | Compare two runs' change sets |
| `sbx run <project> --schedule NAME` | Run on behalf of a schedule (the launchd agent calls this) |
| `sbx wait <id> [--timeout S]` | Block until the run ends; exit code follows the agent |
| `sbx export <id> -o run.sbxbundle`, `sbx import run.sbxbundle <project>` | Hand a session over (patch, runs, audit; no secret values or names) |

## Reports and housekeeping

- `sbx transcript <id> --format md|json`, `sbx history <id> [--run N]`
- `sbx audit [--since 7d] [--event keep] [--csv]`, `sbx report --days 7 --format md|csv`, `sbx stats [--days N] [--budget-minutes M]`
- `sbx status [--json] [--quiet-if-idle]`: one line for prompts and scripts
- `sbx discard --state failed|review|staged` and `sbx forget --state kept|discarded` (with `--tag`, `--older-than 7d`) only list what they would do until you add `--yes`; pinned and running sessions are always skipped
- `sbx prune [--dry-run] [--max-disk-gb N]`
- `sbx doctor [--json]`, `sbx logs [--lines N] [--daemon]` (credentials shown as `[redacted]`)
- `sbx profile save|list|show|export|import`, `sbx secrets`, `sbx completions zsh|bash`

## Policy tools

- `sbx init <project> [--preset node|python|swift|rust|go|docs-only|strict] [--note] [--force]` writes a starter `.sandbox-policy.json` and prints what each key does; `--note` also writes `.sandbox-policy.notes.txt`. `sbx policy presets` lists the presets.
- `sbx policy check <project>` validates a policy file.
- `sbx policy test <id> [--json]` is a dry run: for each changed path it lists every rule that would hit (deny, allow miss, protected path, size caps, credential findings by rule and line only) and says whether Keep would be blocked. It writes nothing and exits 1 when Keep would be blocked.

## Risk report

`sbx risk <id>` scores a session's changes low, medium or high with fixed rules and no model. Rules: dependency manifests and lockfiles, install scripts in `package.json`, CI workflow files, executable bits, new shell scripts, base64 or obfuscated blobs, added `curl | sh`, non-localhost network calls, large binary additions, dotfiles and git hooks, and symlinks pointing outside the project. Each finding gives a rule id, path, line and a fixed reason, never the matched text. One high finding makes the score high; any medium makes it medium, and five or more mediums count as high. A low score is not a guarantee the change is safe. See `maxRisk` in [POLICY.md](POLICY.md).

## Scheduled runs

```
sbx schedule add NAME --profile P --project DIR (--at HH:MM [--weekdays mon,wed] | --every 1h|1d|7d) [--notify] [--max-concurrent N] [--allow-overlap]
sbx schedule list | status NAME | run-now NAME | remove NAME
```

Each schedule is a per-user launchd agent that runs `sbx run` with the profile. Names use lower-case letters, digits and dashes, up to 31 characters; the project must be an absolute folder that passes the same checks as staging. A scheduled run is tagged `scheduled:NAME`, is never kept automatically (it ends in review), and is written to the audit log. By default a run is skipped while the previous run of the same schedule is still going; `--allow-overlap` with `--max-concurrent N` allows up to N. Real `launchctl` loading and a real timed firing have not been verified.

## Keep as pull request

`sbx keep <id> --pr` keeps the changes as a branch (every Keep policy and credential check applies), pushes it to the project's remote without forcing, and runs `gh pr create`. Without `--yes` it only previews. It refuses when the remote or `gh` is missing, the credential scan flags something, policy is violated, or the touched files have uncommitted edits. The body holds your note, tags, a run summary and the last output lines with secrets scrubbed.

## MCP

Start the server with `sbx mcp` (add `--allow-keep` to expose keep tools). Read-only tools include status, wait, transcript, report, stats, audit, compare, hunks, checkpoint, doctor, policy check, keep preview, prune preview (a dry run: the MCP server never prunes), `sandbox_risk`, `sandbox_policy_test`, `sandbox_schedules` (it cannot add, run or remove schedules) and `sandbox_pr_preview` (no MCP tool opens a pull request). No tool can override the project policy or the credential scan.

## Limits

The engine runs the agent command as one blocking request, so `sbx watch` follows state, not live output.
