# LinkedIn Posts Skill

A Claude Code skill that helps any professional plan and write their weekly LinkedIn content.

## Install

```bash
npx skills add github:lucagalvani-galtea/linkedin-skill
```

## Usage

Type `/linkedin-posts` or say "help me write a LinkedIn post" in Claude Code.

## What it does

On first run, sets up a persona profile (name, role, company, audience, content focus) — works for any person or company. Supports multiple personas, each with their own voice profile and memory file.

Guides you through a weekly content mix of 5 post types:

| # | Type | Examples |
|---|---|---|
| 1 | Company culture | New hires, milestones, events, employee wins |
| 2 | Personal brand | Personal experience, problem solved, POV |
| 3 | AI landscape | Reaction to news, predictions |
| 4 | Repost | Useful resources, ICP posts worth amplifying |
| 5 | Product | Launches, new features, articles, carousels |

For each post it runs a **Weekly Extraction** interview to surface content ideas, drafts a full post (hook + body + close), and generates 5 hook variants using behavioral psychology frameworks (curiosity gap, loss framing, identity mirroring, pattern interrupt, competence signal) — no external tools needed.
