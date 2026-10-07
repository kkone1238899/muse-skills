---
name: morning-briefing
description: Compile an overnight briefing the user can read in 3 minutes. Use every morning (or on demand) to turn raw news, repo activity, and calendar into one tight brief. Pairs well with a cron job.
---

# morning-briefing

Turns the last ~12 hours of the world into a 3-minute read. Built to run unattended on a schedule.

## Inputs

- `topics`: 3–8 topics the user cares about (e.g. "AI model releases", "indie hacker revenue", "design clients")
- `sources`: where to look — search verticals, RSS, GitHub trending, specific accounts
- `locale`: user's language and timezone for timestamps

## Procedure

### 1. Collect (breadth first)

For each topic, gather candidate items from the last 12 hours:

- Web/news search per topic (use the `news` vertical, not general search)
- GitHub trending / release feeds if the topic is technical
- 5–10 items per topic max — you are collecting candidates, not reading everything

Record for each candidate: title, source, URL, published time, one-line why-it-might-matter.

### 2. Filter (ruthless)

Drop anything that fails any of these:

- Older than 12 hours (stale)
- No primary source (rumor laundering)
- "Company announces" press releases with no substance
- Duplicates — keep the earliest or the best-sourced version

Aim to keep **at most 3 items per topic**, 10 total. A briefing with 25 links is a search results page, not a briefing.

### 3. Write the brief

Format (in the user's language):

```markdown
## 早安 · Oct 7
**一句话**: <the single most important thing overnight, ≤30 words>

### <Topic 1>
- **<headline>** — <why it matters in one line> [source](url)
- ...

### <Topic 2>
...

### 今天
- <calendar items / deadlines today, if any>
```

Rules:

- Every bullet must answer "so what" — never just restate the headline.
- Numbers over adjectives: "star +1,050 in 24h" beats "growing fast".
- If nothing passed the filter for a topic, write "无大事" and move on — do not pad.
- Total reading time ≤ 3 minutes. Cut until it fits.

### 4. Deliver

- Send to the user's chat, or write to a dated file if running headless.
- Keep a 7-day archive; weekly review shows which topics consistently produce nothing (prune them).

## Failure modes

| Symptom | Cause | Fix |
|---|---|---|
| Brief is a link dump | Filter step skipped | Enforce the 3-per-topic cap |
| Everything is stale | Ran too late / sources slow | Run earlier; prefer primary sources |
| User never reads it | Too long or irrelevant topics | Cut to 3 minutes; ask which topics to drop |
| Missed the big story | Topic list too narrow | Add one "general tech news" catch-all topic |

## Scheduling

Runs well as a daily cron at the user's wake time minus 30 minutes. The skill is stateless — the scheduler handles timing, the skill handles judgment.
