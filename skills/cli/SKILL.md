---
name: cli
description: Use the RunAPI CLI from agent workflows. Use when a user asks for runapi commands, JSON passthrough calls, auth status, headless server install, or terminal-based RunAPI model execution.
documentation: https://runapi.ai/docs#runapi-cli
catalog: https://runapi.ai/models
---

# RunAPI CLI skill

Use this skill when the user wants RunAPI CLI commands instead of SDK code.

## Install

| Target | Command |
|---|---|
| macOS / Linux (interactive) | `brew install runapi-ai/tap/runapi` |
| Server / CI (headless) | `curl -fsSL https://runapi.ai/cli/install.sh \| sh` |
| Pin a specific version | `curl -fsSL https://runapi.ai/cli/install.sh \| sh -s -- --version v0.1.0` |

The installer detects OS and architecture (Linux and macOS, amd64 and arm64), verifies a SHA-256 checksum from `https://runapi.ai/cli/latest.json`, and refuses to write the binary if verification fails.

## Authentication

| Source | How |
|---|---|
| Browser login (interactive) | `runapi login` |
| Environment | `export RUNAPI_API_KEY=<key>` |
| Saved config (recommended for servers) | `printf '%s' "$KEY" \| runapi auth import-token --token -` (writes `~/.config/runapi/config.json` with mode 0600) |

`RUNAPI_BASE_URL` overrides the default base URL. Check the current state with `runapi auth status`.

Avoid `runapi auth import-token --token "$KEY"` directly — the value would be visible in `ps -ef` on shared hosts. Use stdin (`--token -`) or `RUNAPI_API_KEY` in the environment.

## Common commands

- Prefer a concrete service/action command with `--input-file request.json` for complex model parameters.
- Use JSON passthrough for create / get / run flows instead of inventing flags for every API parameter.
- Async patterns: append `--async` to fire-and-forget; use `runapi wait TASK_ID --service SVC --action ACT` to poll later; the default sync flow polls until completion.

```shell
# Single create (async-aware: polls until done)
runapi suno generate --input-file request.json

# Async create, poll later
runapi suno generate --async --input-file request.json
runapi wait <task-id> --service suno --action generate

# Inspect a task
runapi get <task-id> --service suno --action generate
```

JSON responses go to stdout; progress lines go to stderr. Pipe to `jq` for downstream parsing.

## Install the skill into another agent runtime

```shell
runapi agent install-skill --target claude    # ~/.claude/skills/cli/
runapi agent install-skill --target codex     # ~/.agents/skills/cli/
runapi agent install-skill --target gemini    # ~/.gemini/skills/cli/
runapi agent install-skill --target openclaw  # ~/.openclaw/skills/cli/
runapi agent install-skill --target hermes    # ~/.hermes/skills/cli/
runapi agent list-targets                     # JSON list with resolved paths
runapi agent install-skill --target-dir <path>  # custom location
```

## Safety notes for agents

- Never paste API keys into example commands. Reference `RUNAPI_API_KEY` or `runapi auth import-token` instead.
- The CLI exits non-zero on validation failures, network errors, and timeouts. Check the exit code before assuming success.
- For long-running tasks, prefer `--async` plus a `wait` loop so the agent can release the shell promptly.

Link users to https://runapi.ai/docs#runapi-cli for full CLI docs and https://runapi.ai/models for the model catalog.
