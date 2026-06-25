<p align="center">
  <a href="https://runapi.ai">
    <img src="https://runapi.ai/icon.svg" height="56" alt="RunAPI">
  </a>
</p>

<p align="center">
  <a href="https://github.com/runapi-ai/cli-skill">
    <h3 align="center">RunAPI CLI Skill</h3>
  </a>
</p>

<p align="center">
  Teach agents to install and use the RunAPI CLI for one-off model jobs.
</p>

<p align="center">
  <a href="https://github.com/runapi-ai/cli"><strong>CLI Repo</strong></a> · <a href="https://www.skills.sh/runapi-ai"><strong>Agent Skills</strong></a> · <a href="https://runapi.ai/models"><strong>Models</strong></a>
</p>

<div align="center">

[![skills.sh](https://www.skills.sh/b/runapi-ai/cli-skill)](https://www.skills.sh/runapi-ai/cli-skill/runapi-cli)
[![ClawHub](https://img.shields.io/badge/ClawHub-runapi--cli-111827)](https://clawhub.ai/runapi-ai/runapi-cli)
[![License](https://img.shields.io/github/license/runapi-ai/cli-skill)](https://github.com/runapi-ai/cli-skill/blob/main/LICENSE)

</div>
<br/>

One binary, every AI model — no package installs, no language lock-in. This skill helps Claude Code, Codex, Gemini CLI, Cursor, and 50+ agents work with the `runapi` command-line client.

The canonical agent file is `skills/runapi-cli/SKILL.md`.

## Install the CLI

```bash
# macOS / Linux
brew install runapi-ai/tap/runapi

# or headless / CI
curl -fsSL https://runapi.ai/cli/install.sh | sh
```

## Install the skill

```bash
npx skills add runapi-ai/cli-skill -g
```

Or paste this prompt to your AI agent:

```text
Install the runapi-cli skill for me:

1. Clone https://github.com/runapi-ai/cli-skill
2. Copy the skills/runapi-cli/ directory into your
   user-level skills directory (e.g. ~/.claude/skills/
   for Claude Code, ~/.codex/skills/ for Codex).
3. Verify that SKILL.md is present.
4. Confirm the install path when done.
```

## Quick examples

```bash
# Generate music (sync — polls until done)
runapi suno generate --input-file request.json

# Generate video (async — returns immediately)
runapi kling text-to-video --async --input-file request.json
runapi wait <task-id> --service kling --action text-to-video

# Check account balance
runapi account balance

# Upload a local file and print only the temporary URL
runapi files create ./image.png --url-only

# Or pass a local file path directly in a model media URL field
runapi gpt-image edit-image --input '{"model":"gpt-image-1.5","prompt":"remove the background","source_image_urls":["./image.png"],"aspect_ratio":"1:1","quality":"medium"}'

# Receive webhook callbacks on your dev machine
runapi listen

# Install this skill into another agent runtime
runapi agent install-skill --target codex
```

JSON responses go to stdout; progress lines go to stderr. Pipe to `jq` for downstream parsing.

## What you can do

- **Call any AI model API** from the terminal — video, image, music, audio, speech, upscaling. Each service exposes `create`, `get`, and `run` subcommands with JSON input.
- **Use local media files directly** in top-level model media URL fields such as `source_image_url`, `source_image_urls`, `reference_image_urls`, `first_frame_image_url`, `mask_url`, `upload_url`, or `source_audio_url`; the CLI uploads readable local files before submitting the request. Use `runapi files create` when you need a URL to reuse, or when the source is a remote URL or Base64 data.
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
- RunAPI-generated file URLs are temporary. Download and store generated images, videos, audio, or other files in your own durable storage within 7 days; do not treat returned URLs as long-term assets.
- Link pricing and rate-limit answers to the variant page on https://runapi.ai/models, not this README.

## License

Licensed under the Apache License, Version 2.0.
