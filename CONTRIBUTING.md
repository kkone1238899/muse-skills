# Contributing

## The bar

This repo collects skills that survive **daily production use** — not ideas, not demos. Before submitting:

1. **Run it for at least a week** in a real workflow. If you haven't, it's not ready.
2. **Document the failure modes.** Every skill's `SKILL.md` must have a failure-modes table: what breaks in practice, why, and the fix. A skill with no documented failures hasn't been used enough.
3. **Keep it tool-agnostic where possible.** Prefer "call your TTS CLI" over "install my exact setup". When a specific tool is required, say so up front in Inputs.

## Skill format

```
skills/<skill-name>/
└── SKILL.md
```

`SKILL.md` frontmatter:

```yaml
---
name: <kebab-case-name>
description: <one paragraph: what it does + when to use it>
---
```

Body sections: purpose, Inputs, numbered Procedure, Failure modes table, Notes. Write in English; a Chinese translation is welcome but optional.

## Pull requests

- One skill per PR.
- Describe where and how long you've run it in production.
- Keep the diff focused: skill folder + README index entry.

## What gets rejected

- Link collections / "awesome lists" with no original instructions
- Skills that only work with the author's private setup
- Prompt dumps with no procedure or failure documentation
- Anything that hasn't actually been run
