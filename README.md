# test

## Daily AI/ML Hot-Topics Digest

A reusable Claude Code command that scans Twitter/X and the broader web for the
day's hottest AI/ML topics and delivers a digest as a push notification.

- **Command:** `/daily-ai-ml-digest` (defined in
  `.claude/commands/daily-ai-ml-digest.md`)
- **What it does:** runs web searches scoped to the last ~48h, synthesizes the
  5–7 most-discussed AI/ML topics (each with a one-line "why it's hot" + link),
  pushes a short headline summary, and prints the full digest in the session.

### Run it manually

In any Claude Code session on this repo, run:

```
/daily-ai-ml-digest
```

### Schedule it daily (~8am)

Because Claude Code on the web runs in an ephemeral container, the durable way to
run this every day is a **scheduled session** in the Claude Code web UI:

1. Open this repo in Claude Code on the web.
2. Create a new **scheduled session / automation**:
   - **Repository:** `ehan416/test`
   - **Branch:** `claude/daily-twitter-ai-ml-topics-gotlqa`
   - **Schedule:** daily, ~8:00am (your local timezone)
   - **Prompt:** `/daily-ai-ml-digest`
3. Save. Each morning it will generate and push the digest.

> Push notifications reach your phone only when **Remote Control** is connected.
> The full digest is always printed in the session transcript regardless.
