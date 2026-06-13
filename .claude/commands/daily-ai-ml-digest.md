---
description: Scan X/Twitter and the web for today's hottest AI/ML topics and push a digest
allowed-tools: WebSearch, WebFetch, PushNotification, Bash(git*), Read, Write
---

You are generating a **daily AI/ML hot-topics digest** sourced from Twitter/X and
the broader web, then delivering it as a push notification.

This runs **every day**, so the job is to surface **what's new since yesterday** —
not to re-report the same ongoing stories all week. Treat anything older than
~48 hours as stale, and use a dedup log to avoid repeats.

## Step 0 — Load recent history (dedup)
Read `.claude/state/recent-topics.md` if it exists (it's a log of topics already
reported over the past ~7 days, one `YYYY-MM-DD | topic headline` per line).
Use it to recognize what you've already sent. If the file doesn't exist yet,
treat history as empty.

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

### Find the originating tweet + engagement
For each hot topic, also run a targeted `WebSearch` to locate the **originating /
most-viral X post**, scoping with `allowed_domains: ["x.com", "twitter.com"]`.
Useful query shapes:
- `<topic or person> tweet x.com likes reposts`
- `<exact quote or coined phrase> x.com`
- `<lab/person handle> announcement x.com`

Capture the **direct status URL** (e.g. `https://x.com/<user>/status/<id>`) and any
**engagement numbers** (likes / reposts / replies / quotes / views) that appear in
the search snippet. Note: search snippets are the reliable source for these counts
— **`WebFetch` on x.com typically returns 403** (no authenticated X access here),
so don't rely on fetching the tweet page; record only the numbers search surfaces,
and omit counts when none are available rather than guessing.

## Step 2 — Synthesize (new-first, with dedup)
Classify each candidate topic against the Step 0 history:
- **NEW** — not in the log. These are the priority.
- **ONGOING** — already reported, but with a *material new development* today
  (e.g. new benchmark, major reaction, follow-up release). Include only if there's
  genuinely something new; lead the line with what changed.
- **STALE** — already reported, nothing new today. **Drop it** (don't repeat).

Produce a digest of the **5–7 hottest topics**, prioritizing NEW items, then
ONGOING-with-news. Each item:
- One bold headline (prefix ONGOING items with `(cont.)`).
- One line on *why it's hot* / what's new.
- The **originating X post link** (`https://x.com/.../status/...`) when found, with
  **engagement numbers in parentheses** if the search surfaced them
  (e.g. `(13K reposts, 9.6K quotes, 1K likes)`). Add a supporting article link too.
- Order by virality — prefer the engagement numbers as the ranking signal when
  available, otherwise by how widely discussed the topic is.

If a genuinely quiet day yields fewer than 5 new items, that's fine — send fewer
rather than padding with stale repeats.

## Step 3 — Deliver
Call `PushNotification` (status `proactive`) with a **short headline summary** —
the top 3 topics in one line, under 200 characters, no markdown. Example shape:
`AI/ML today: 1) <topic> 2) <topic> 3) <topic>`.

Then print the **full digest** as your final message so it's visible in the
session transcript (and survives even if the push isn't delivered).

If the push result says it wasn't sent, that's expected when Remote Control isn't
connected — the digest in the transcript is still the deliverable.

## Step 4 — Update the dedup log
Append each topic you reported today (NEW and ONGOING) to
`.claude/state/recent-topics.md`, one line per topic as `YYYY-MM-DD | headline`
(create the file/dir if needed). Then **prune** any lines older than 7 days from
today so the log stays small. Commit and push so tomorrow's run sees it:

```
git add .claude/state/recent-topics.md
git commit -m "Update AI/ML digest dedup log (YYYY-MM-DD)"
git push origin claude/daily-twitter-ai-ml-topics-gotlqa
```

If there's nothing new to report, skip the commit.
