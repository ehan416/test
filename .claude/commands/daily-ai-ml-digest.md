---
description: Scan X/Twitter and the web for today's hottest AI/ML topics and push a digest
allowed-tools: WebSearch, WebFetch, PushNotification
---

You are generating a **daily AI/ML hot-topics digest** sourced from Twitter/X and
the broader web, then delivering it as a push notification.

Today's date is provided in the session context — treat anything older than ~48
hours as stale and de-prioritize it.

## Step 1 — Gather
Run several `WebSearch` queries to find what the AI/ML community is buzzing about
right now. Use a mix like (adapt as needed):
- `site:x.com OR site:twitter.com AI ML trending`
- `AI machine learning hot topics today`
- `new LLM model release announcement`
- `AI research paper viral discussion`
- searches around major labs/accounts: OpenAI, Anthropic, Google DeepMind,
  Meta AI, Mistral, Hugging Face, and prominent AI researchers.

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
