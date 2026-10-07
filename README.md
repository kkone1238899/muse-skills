# muse-skills

Battle-tested skills and workflow templates for **Meta Muse** — the AI agent that works on your behalf.

No toy examples. Every skill in this repo is used in daily production by an AI agent running real workloads: narrated video production, social account operations, morning briefings, inbox triage. If a skill stops working in the real world, it gets fixed or removed here.

中文说明：[README_CN.md](./README_CN.md)

## What's inside

```
skills/
├── tts-narration/      # Reliable long-form narration: chunked TTS with
│                       # per-chunk verification, pacing control, concat
└── morning-briefing/   # Agent workflow template: overnight news → one
                        # tight morning brief you can read in 3 minutes
```

Each skill is a folder with a `SKILL.md` (name, description, instructions) — drop it into your agent's skills directory and it works.

## Install

```bash
git clone https://github.com/kkone1238899/muse-skills.git
# copy the skill you want into your agent's skills folder, e.g.
cp -r muse-skills/skills/tts-narration ~/.config/muse/skills/
```

Read the skill's `SKILL.md` first — it documents required tools, inputs, and failure modes.

## Why this exists

Most "awesome AI agent" lists are link collections. This repo is the opposite: a small set of skills that survive daily use. Contributions are welcome, but the bar is "I run this every day" — see [CONTRIBUTING.md](./CONTRIBUTING.md).

## Roadmap

- [ ] Social media operations skills (hook writing, reply playbooks)
- [ ] Video production pipeline skills (rendering, subtitles, QC)
- [ ] More workflow templates: inbox triage, travel planning, research digests

## License

MIT — use it however you want. See [LICENSE](./LICENSE).
