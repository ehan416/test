---
description: Scan X/Twitter and the web for today's hottest AI/ML topics and push a digest
allowed-tools: WebSearch, WebFetch, PushNotification
---

You are generating a **daily AI/ML hot-topics digest** sourced from Twitter/X and
the broader web, then delivering it as a push notification.

Today's date is provided in the session context — treat anything older than ~48
hours as stale and de-prioritize it.

## Step 1 — Gather
Run **at least 6–8** `WebSearch` queries to find what the AI/ML community is
buzzing about *right now*. The goal is **day-specific breaking news**, not
evergreen roundups.

Bias the queries toward freshness and specifics — include the **current month and
year** and concrete event types:
- `AI news today <Month> <day> <year>`
- `new AI model release this week <Month year>` (and name labs: OpenAI, Anthropic,
  Google DeepMind, Meta AI, Mistral, xAI, Alibaba/Qwen, Moonshot/Kimi)
- `AI announcement <Month year>` for each major lab
- `site:x.com OR site:twitter.com AI ML trending`
- `AI viral thread X this week` / `new AI term coined developers`
- prominent commentators who break news: `simonwillison.net`, `techcrunch.com`,
  `simonwillison AI <Month year>`, Andrej Karpathy, Addy Osmani.

**Critical — follow the leads.** When a result mentions a *specific proper noun or
newly coined term* you haven't already searched (a new model name, a person's
move, a phrase like "loop engineering"), run a **dedicated follow-up search** on
it before deciding what's hot. The hottest items are usually specific named things
(e.g. a model launched 2–4 days ago, an X-origin meme), and generic queries bury
them under SEO listicles. Treat "10 AI trends in <year>"-style listicles as weak
signal; prefer dated articles and original X posts.

If a specific public X thread or article looks central to a topic, optionally
`WebFetch` it for a sharper one-line summary. Don't block on any single source.

## Step 2 — Synthesize
Produce a digest of the **5–7 hottest AI/ML topics**:
- One bold headline per topic.
- One line on *why it's hot* (what happened / why people care).
- A link where available.
- Deduplicate overlapping items and order by how widely discussed each is
  (most-discussed first).

Keep it punchy and skimmable.

## Step 3 — Deliver
Call `PushNotification` (status `proactive`) with a **short headline summary** —
the top 3 topics in one line, under 200 characters, no markdown. Example shape:
`AI/ML today: 1) <topic> 2) <topic> 3) <topic>`.

Then print the **full 5–7 topic digest** as your final message so it's visible in
the session transcript (and survives even if the push isn't delivered).

If the push result says it wasn't sent, that's expected when Remote Control isn't
connected — the digest in the transcript is still the deliverable.
