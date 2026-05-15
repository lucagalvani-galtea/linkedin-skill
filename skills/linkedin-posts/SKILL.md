---
name: linkedin-posts
description: "Help a founder plan and write LinkedIn posts following a weekly content mix: company culture, personal brand, AI landscape, reposts, and product updates."
when_to_use: "Invoke when the user wants to write, plan, or brainstorm LinkedIn posts. Trigger phrases include: linkedin post, write a post, post ideas, weekly content, founder post, what should I post."
---

# LinkedIn Posts

This skill helps the founder plan and write their weekly LinkedIn content mix. It covers all five post types, drafts full posts (not just hooks), and integrates with the local hooks generator at `~/linkedin-hooks/` when a strong opening is needed.

## Setup check

When this skill is first invoked, run both checks before doing anything else.

**1. Hooks generator**

```bash
ls ~/linkedin-hooks/main.py 2>/dev/null
```

If missing, offer to install it:

```bash
git clone https://github.com/lucagalvani-galtea/linkedin-hooks.git ~/linkedin-hooks
pip install typer rich
```

Only run if the user confirms.

**2. Memory file**

```bash
cat ~/linkedin-memory.md 2>/dev/null
```

If the file exists, load it silently — use the voice & style notes when drafting and do not ask for writing samples again.

If the file does not exist, ask:

> "Do you have any existing LinkedIn posts or writing samples I can learn your voice from? Paste them here, or press Enter to skip."

If they share samples, extract voice & style patterns (sentence length, vocabulary, structural habits, what they avoid) and create `~/linkedin-memory.md` with the structure below. If they skip, create the file with empty sections so it's ready for confirmed posts.

Do not repeat this check on subsequent invocations in the same session.

## Memory file format

Location: `~/linkedin-memory.md`

```markdown
# LinkedIn Writing Memory

## Voice & Style Notes
<!-- Extracted from writing samples. Updated when new samples are shared. -->


## Confirmed Posts
<!-- Appended after each post the user confirms as done. -->
```

### How to update the memory file

**After extracting voice notes from samples:**
Append bullet points under `## Voice & Style Notes`. Focus on patterns that are specific and actionable — sentence rhythm, word choices they favour or avoid, structural habits, recurring angles. Not generic observations.

**After a post is confirmed:**
When the user says a post is done, approved, or ready to publish, append it under `## Confirmed Posts`:

```markdown
### YYYY-MM-DD — [post type]
[full post text]
```

Use today's date. Always append — never overwrite existing entries.

**When new samples are shared mid-session:**
Extract additional patterns and merge them into `## Voice & Style Notes`.

## Weekly Content Mix

Each week targets five post types.

| Slot | Type | Best source material |
|---|---|---|
| 1 | **Company culture** | New hires, milestones, events/partnerships, employee wins |
| 2 | **Personal brand** | Personal experience, problem solved this week, POV on something |
| 3 | **AI landscape** | Reaction to a specific event, prediction on AI's future |
| 4 | **Repost** | Useful resource, or an ICP's post worth amplifying |
| 5 | **Product** | Launch, new feature, new article/resource, infographic/carousel |

## Workflow

### Step 1 — Identify the post type or mine the week

Ask the user:

> "Do you already have a topic in mind, or would you like me to interview you about your week to find one?"

**If they have a topic:** ask which slot it fits and proceed to Step 2.

**If they don't have a topic** (or say things like "I don't know what to write", "nothing interesting happened", "help me find content"): run the Content Mine interview below, then use the output to pick the best-fit slot and proceed to Step 2 with the chosen idea as the raw material.

---

## Content Mine — Weekly Extraction Interview

Run this when the user doesn't have a topic ready. The goal is to surface content ideas from their real work week. Most professionals sit on a goldmine of insights but don't see it because it all feels "obvious" to them. Your job is to help them see what their audience would find valuable.

By the end, the user should feel a shift: from "I have nothing to say" to "I actually have too much."

### Interview rules

- Ask **one question at a time**. Acknowledge their answer briefly before moving on.
- If they give a rich answer, flag the gold: "There's definitely a post in there." Then move on.
- If they give a flat "nothing really" answer, probe once with a more specific version. If still nothing, move on. Never push twice.
- Push for specifics. If they say "I had a tough conversation," ask what made it tough, what was said, what happened. Generic answers produce generic ideas.
- **Budget: ~12 questions total** (8 starters + ~4 follow-ups). Don't burn follow-ups on dry territories.
- If the user types **STOP** at any point, immediately skip to the output with whatever material you have.

### The 8 territories

**1. Conversations**
"Think about the conversations you had this week — with colleagues, clients, your team. Did any of them spark a thought? Maybe a question someone asked that surprised you, or a pattern you noticed across multiple conversations?"

**2. Events & News**
"What actually happened at work this week? Any meetings that mattered, decisions made, launches, shifts in direction?"

**3. The Hard Problem**
"What was the hardest thing you dealt with this week, professionally? Walk me through it — what happened and how did you handle it?"

**4. Processes & Tactics**
"Did you create or update any documents, processes, templates, or workflows? Anything you built that helped you or your team work better?"

**5. Learning & Mind Changes**
"Did you learn something new this week? Or change your mind about something you used to believe?"

**6. Contrarian Take**
"Was there a 'best practice' or common piece of advice you heard this week that you actually disagree with?"

**7. Values & Boundaries**
"Did you say 'No' to anything this week? A project, a client, a meeting, a request? What was it and why?"

**8. Curation**
"What's one thing you read, watched, or listened to this week that you found yourself telling someone else about?"

### Content Mine output

Once the interview is done (or STOP), generate **5 content ideas** using this structure for each:

```
## Idea #[N]: [Hook — a phrase that could open the post]

- **Source:** [Specific moment from the interview]
- **Target audience:** [Who would care — be specific]
- **The angle:** [One sentence: the post's core message]
- **Content type:** [Case Study | Contrarian Take | Tactical How-To | Personal/Vulnerable | Curation | Observation]
- **Best slot:** [Which of the 5 weekly slots this fits]
```

**Rules for the output:**
- Exactly 5 ideas. Nothing after Idea #5 — no bonus ideas, next steps, or strategy sections.
- At least 3 different content types across the 5 ideas.
- Every idea must reference something the user actually said — no invented details.
- The hook must be strong enough to stop a scroll.

After presenting the 5 ideas, ask: "Which of these do you want to turn into a post?" Then proceed to Step 2 with that idea as the raw material.

---

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

### Step 5 — Iterate and confirm

After sharing the draft:
- Ask if the tone, hook, or body needs adjusting
- Offer to regenerate just the hook using the hooks generator if the opening isn't strong enough
- When the user is happy with the post, ask: "Want me to save this to memory?" — if yes, append it to `~/linkedin-memory.md` under `## Confirmed Posts` with today's date and post type

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
