---
name: linkedin-posts
description: Help a founder plan and write LinkedIn posts following a weekly content mix: company culture, personal brand, AI landscape, reposts, and product updates.
when_to_use: Invoke when the user wants to write, plan, or brainstorm LinkedIn posts. Trigger phrases include "linkedin post", "write a post", "post ideas", "weekly content", "founder post", "what should I post".
---

# LinkedIn Posts

This skill helps the founder plan and write their weekly LinkedIn content mix. It covers all five post types, drafts full posts (not just hooks), and integrates with the local hooks generator at `~/linkedin-hooks/` when a strong opening is needed.

## Setup check

When this skill is first invoked, check if the hooks generator is installed:

```bash
ls ~/linkedin-hooks/main.py 2>/dev/null
```

If the file doesn't exist, tell the user and offer to set it up:

```bash
git clone https://github.com/lucagalvani-galtea/linkedin-hooks.git ~/linkedin-hooks
pip install typer rich
```

Only run the setup if the user confirms. Once installed, proceed normally. Don't repeat this check on subsequent invocations in the same session.

## Weekly Content Mix

Each week targets five post types. When the user hasn't specified, ask what's happening this week and suggest which slots are most natural to fill first.

| Slot | Type | Best source material |
|---|---|---|
| 1 | **Company culture** | New hires, milestones, events/partnerships, employee wins |
| 2 | **Personal brand** | Personal experience, problem solved this week, POV on something |
| 3 | **AI landscape** | Reaction to a specific event, prediction on AI's future |
| 4 | **Repost** | Useful resource, or an ICP's post worth amplifying |
| 5 | **Product** | Launch, new feature, new article/resource, infographic/carousel |

## Workflow

### Step 1 — Identify the post type

If the user hasn't specified, ask:
- Which slot they want to fill (or show the list above)
- What's happening this week that fits that slot

### Step 2 — Gather raw material

Ask short, direct questions to pull out the real substance. Don't write anything until you have enough signal. For each type:

**Company culture**
- Who/what/where — name, role, event, milestone
- One specific detail that makes it real (not generic)
- Any number or date worth anchoring to

**Personal brand**
- What happened exactly — the situation, not the lesson
- What was surprising or uncomfortable about it
- The take they haven't heard from others

**AI landscape**
- The specific event or article being reacted to (link or title)
- The founder's actual opinion — agreement/disagreement and why
- What they think will happen next (the prediction, not the hedge)

**Repost**
- The original post or resource URL/author
- Why this is valuable for their audience specifically
- Optional: a short intro line the founder wants to add

**Product**
- What launched or changed — one concrete thing
- Who it's for and what problem it solves
- Any metric, number, or before/after that makes it tangible

### Step 3 — Draft the full post

Structure of a LinkedIn post:
1. **Hook** (1–3 lines) — stops the scroll
2. **Body** (3–8 lines) — delivers the substance
3. **Close** (1–2 lines, optional) — a thought, a question, a next step

**Hook:** Use the hooks generator for options when a strong opening matters. Run:
```bash
cd ~/linkedin-hooks && python main.py
```
Or draft a hook directly using the behavioral levers below.

**Body writing rules (apply to the whole post):**
- No AI-sounding phrases: "game-changer", "leverage", "delve", "navigate", "crucial", "pivotal", "in today's world", "Here's the thing:", "At the end of the day"
- No parallel bullet structures where every line is the same length
- No question bait at the end ("What do you think?", "Drop a comment")
- Use fragments deliberately. Vary sentence length.
- Odd specific numbers beat round estimates ("17 days" not "a few weeks")
- State opinions as facts. No "I think" or "In my opinion".
- Contractions are fine. Imperfect grammar is fine when it serves rhythm.

**Close:** End on a statement, not a question. If there's a CTA, make it specific ("Link in comments" or nothing at all).

### Step 4 — Tone

Default tone by post type unless the founder specifies otherwise:

| Post type | Default tone |
|---|---|
| Company culture | Conversational |
| Personal brand | Vulnerable or Direct |
| AI landscape | Provocative or Authoritative |
| Repost | Conversational |
| Product | Direct or Authoritative |

Tone definitions:
- **Direct** — short sentences, flat opinions, no hedging, no warmup
- **Conversational** — like texting a smart friend, contractions, thinks out loud
- **Provocative** — picks a fight with conventional wisdom, no softening, lets the claim hang
- **Vulnerable** — admits something most wouldn't say out loud, honest about being wrong or slow
- **Authoritative** — leads with specifics and evidence, precise numbers, no false modesty

### Step 5 — Iterate

After sharing the draft:
- Ask if the tone, hook, or body needs adjusting
- Offer to regenerate just the hook using the hooks generator if the opening isn't strong enough
- Suggest saving the post to a weekly content file if the user tracks their drafts

## Post type templates

### Company culture

```
[Specific person or event] just [specific thing that happened].

[1-2 lines of what this means or what it felt like — the human detail, not the PR version]

[Closing thought — no lesson, no moral, just a true observation]
```

### Personal brand

```
[Start with the situation, not the takeaway]

[What happened, in order — the uncomfortable or specific part]

[The conclusion or take — stated as fact, not offered as opinion]
```

### AI landscape

```
[The event or claim being reacted to — specific, not vague]

[The founder's actual position — disagree, agree, nuance — one sentence]

[Why — the reasoning, compressed. What this means going forward.]
```

### Repost intro

```
[One line on why this is worth reading — specific, not "great post"]

[One-sentence context for the founder's audience]

→ [Author name or link]
```

### Product

```
[What shipped — one sentence, no jargon]

[Who it's for and what it removes from their life]

[The number, the before/after, or the specific use case that makes it real]

[Link or CTA — or nothing if it speaks for itself]
```

## Hooks cheat sheet (quick reference)

Use these when drafting hooks without running the generator:

| Lever | Pattern |
|---|---|
| Curiosity gap | "Everyone does X. They're wrong." / "X is not what you think it is." |
| Loss framing | "You're leaving [outcome] on the table." / "Most [group] never notice [cost]." |
| Identity mirroring | "The reason [group] struggles with X isn't [obvious reason]." |
| Pattern interrupt | "[Conventional wisdom]. [One-sentence reframe]." |
| Competence signal | "Here's the [X]-step framework for [hard thing]." |

## What to avoid

- Generic culture posts ("So proud of our amazing team!")
- AI landscape takes that don't commit ("It's complicated, but...")
- Product posts that describe features instead of outcomes
- Any hook that could have been written by anyone

## Notes

- The hooks generator lives at `~/linkedin-hooks/` — use it when the opening isn't landing
- The founder's company is Galtea (AI product testing and evaluation platform)
- Audience: AI builders, founders, and product teams
- Weekly cadence: aim for all 5 slots, but 3 strong posts beat 5 weak ones
