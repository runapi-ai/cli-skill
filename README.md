# RunAPI CLI Skill

One binary, every AI model — no package installs, no language lock-in. This skill helps Claude Code, Codex, Gemini CLI, Cursor, and 50+ agents work with the `runapi` command-line client.

The canonical agent file is `skills/cli/SKILL.md`.

## Install the CLI

```bash
# macOS / Linux
brew install runapi-ai/tap/runapi

# or headless / CI
curl -fsSL https://runapi.ai/cli/install.sh | sh
```

## Install the skill

Pick one:

**A.** With [skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add runapi-ai/cli -g
```

**B.** Or manually: clone this repo and copy `skills/cli/` into your agent's skills directory (e.g. `~/.claude/skills/` for Claude Code).

## Quick examples

```bash
# Generate music (sync — polls until done)
runapi suno generate --input-file request.json

# Generate video (async — returns immediately)
runapi kling text-to-video --async --input-file request.json
runapi wait <task-id> --service kling --action text-to-video

# Check account balance
runapi account balance

# Receive webhook callbacks on your dev machine
runapi listen

# Install this skill into another agent runtime
runapi agent install-skill --target codex
```

JSON responses go to stdout; progress lines go to stderr. Pipe to `jq` for downstream parsing.

## What you can do

- **Call any AI model API** from the terminal — video, image, music, audio, speech, upscaling. Each service exposes `create`, `get`, and `run` subcommands with JSON input.
- **Authenticate** via browser login (`runapi login`), environment variable (`RUNAPI_API_KEY`), or headless token import for CI/servers.
- **Manage async tasks** — submit with `--async`, poll with `runapi wait`, or let the default sync flow handle polling automatically.
- **Receive callbacks locally** via `runapi listen` — test async workflows without deploying a public endpoint.
- **Install skills** into any agent runtime with `runapi agent install-skill --target <runtime>`.

## Routing

- CLI docs: https://runapi.ai/docs#runapi-cli
- Model catalog: https://runapi.ai/models
- CLI repository: https://github.com/runapi-ai/cli
- Skill repository: https://github.com/runapi-ai/cli-skill

## Agent rules

- Never paste API keys into commands. Use `RUNAPI_API_KEY` or `runapi auth import-token --token -` with stdin.
- Prefer `--input-file request.json` for complex parameters instead of inline JSON.
- For long-running tasks, use `--async` plus `runapi wait` so the agent can release the shell promptly.
- Link pricing and rate-limit answers to the variant page on https://runapi.ai/models, not this README.

## License

Licensed under the Apache License, Version 2.0.
