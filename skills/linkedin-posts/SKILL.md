---
name: linkedin-posts
description: "Help any professional plan and write LinkedIn posts following a weekly content mix: company culture, personal brand, AI landscape, reposts, and product updates. Supports multiple personas, each with their own voice profile."
when_to_use: "Invoke when the user wants to write, plan, or brainstorm LinkedIn posts. Trigger phrases include: linkedin post, write a post, post ideas, weekly content, what should I post."
---

# LinkedIn Posts

This skill helps any professional plan and write their weekly LinkedIn content. It supports multiple personas — each with their own voice profile and memory file — and generates hooks natively using behavioral psychology frameworks.

## Persona System

Personas are created on first run and stored as memory files. Each persona has:
- A name and role
- A company and what it does
- A target LinkedIn audience
- A content focus (the main angles and topics they write about)

Multiple personas are supported. The active one is tracked in `~/linkedin-persona`.

---

## Setup check

When this skill is first invoked, run both checks before doing anything else.

**1. Persona selection**

```bash
cat ~/linkedin-persona 2>/dev/null
```

If the file exists, read the slug and load `~/linkedin-memory-[slug].md` silently. Proceed without asking.

If the file does not exist, run the **persona creation flow**:

> "Let's set you up. I'll ask a few questions to build your persona profile — this only happens once.
>
> 1. Who are we writing for? (name)
> 2. What's their role? (e.g. Founder & CEO, Head of Marketing)
> 3. What's their company and what does it do? (e.g. Acme — B2B SaaS for HR teams)
> 4. Who's their LinkedIn audience? (e.g. HR leaders and SaaS founders)
> 5. What are their main content angles? (e.g. founder journey, product launches, AI takes)"

Ask all five questions at once. Wait for answers, then:
- Derive a slug from their first name (lowercase, no spaces): e.g. "Sarah Chen" → `sarah`
- Save the slug to `~/linkedin-persona`
- Create `~/linkedin-memory-[slug].md` with the persona profile pre-filled (see Memory file format)

**Adding a new persona:** If the user says "add a persona" or "write for someone else", run the creation flow again and update `~/linkedin-persona` to the new slug.

**Switching personas:** If the user explicitly asks to switch ("write for [name] instead"), update `~/linkedin-persona` to that slug and load their memory file. Run the creation flow if that persona doesn't exist yet.

**2. Memory file**

```bash
cat ~/linkedin-memory-[slug].md 2>/dev/null
```

If the file exists, load it silently — use the persona profile and voice & style notes when drafting, and do not ask for writing samples again.

If the file does not exist (new persona), ask:

> "Do you have any existing LinkedIn posts or writing samples I can learn [name]'s voice from? Paste them here, or press Enter to skip."

If they share samples, extract voice & style patterns and save them under `## Voice & Style Notes`. If they skip, create the file with empty sections so it's ready for confirmed posts.

Do not repeat this check on subsequent invocations in the same session.

---

## Memory file format

Path: `~/linkedin-memory-[slug].md`

```markdown
# LinkedIn Writing Memory — [Name]

## Persona Profile
- **Name:** [Name]
- **Role:** [Role]
- **Company:** [Company name and what it does]
- **Audience:** [Primary LinkedIn audience]
- **Content focus:** [Key angles and topics]

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

---

## Weekly Content Mix

Each week targets five post types.

| Slot | Type | Best source material |
|---|---|---|
| 1 | **Company culture** | New hires, milestones, events/partnerships, employee wins |
| 2 | **Personal brand** | Personal experience, problem solved this week, POV on something |
| 3 | **AI landscape** | Reaction to a specific event, prediction on AI's future |
| 4 | **Repost** | Useful resource, or an ICP's post worth amplifying |
| 5 | **Product** | Launch, new feature, new article/resource, infographic/carousel |

---

## Workflow

The default mode is **weekly planning**: run the Weekly Extraction interview, map the output to a 3-4 post week plan, then draft each post in order. This is the right flow for most sessions.

Only skip to a single post if the user explicitly asks to write one specific post right now.

### Step 1 — Mine the week or jump to a topic

Ask:

> "Want to plan your full week of content? I'll interview you about your week and we'll map out 3-4 posts. Or if you already have a specific topic, just tell me and we'll write that."

**If they want the full week plan:** run the Weekly Extraction interview, then proceed to Step 1b.

**If they have a specific topic:** ask which slot it fits and jump to Step 2.

### Step 1b — Map the week plan

After the Weekly Extraction interview, present the 5 ideas and immediately follow with a **week plan**: select the best 3-4 ideas, assign each a slot and PPP type, and propose a drafting order. Format it like this:

```
## Your week — [dates or "this week"]

| # | Slot | PPP type | Hook (idea) |
|---|---|---|---|
| 1 | Personal brand | Perspective | [hook from idea] |
| 2 | Product | Proof | [hook from idea] |
| 3 | AI landscape | Perspective | [hook from idea] |
| 4 | Company culture | Proof | [hook from idea] |
```

Aim for:
- 2 Perspective posts (personal brand, AI landscape)
- 1-2 Proof posts (product, company culture)
- Promo only if there's a launch or active offer

Then ask: "Want to start drafting? I'll go through them one by one."

---

## Weekly Extraction — Interview

Run this when the user doesn't have a topic ready. The goal is to surface content ideas from their real work week. Most professionals sit on a goldmine of insights but don't see it because it all feels "obvious" to them. Your job is to help them see what their audience would find valuable.

By the end, the user should feel a shift: from "I have nothing to say" to "I actually have too much."

### Interview rules

- Ask **one question at a time**. Acknowledge the answer briefly before moving on.
- Flag good material: "There's a post in there." Then move on.
- If an answer is flat, probe once with a more specific version. If still nothing, move on. Never push twice.
- Push for specifics — generic answers produce generic posts.
- **Budget: ~8 questions total** (6 starters + ~2 follow-ups). Don't burn follow-ups on dry areas.
- If the user types **STOP**, immediately skip to the output with whatever you have.

### Role-based framing

Before running the interview, read the persona's role and content focus from their profile. Use these to adapt question framing:

- **Founders / CEOs / CTOs:** Lean toward product decisions, company vision, builder perspective
- **Marketing / GTM / Growth roles:** Lean toward campaigns, experiments, distribution, marketing craft
- **Other roles:** Use the content focus from the persona profile as the primary lens

The questions below include role-specific variants where the framing meaningfully differs. Use the variant that fits.

### The 6 questions

**1. Product**

*Founder / builder:* "Anything ship, launch, or change at [company] this week? New feature, article, milestone — anything worth telling your audience about?"

*Marketing / GTM:* "Anything you shipped, tested, or launched on the marketing side this week? A campaign, a piece of content, an experiment — anything worth talking about?"

---

**2. Company culture**

"Any team news this week? New hire, a win worth celebrating, an event, a partnership — anything that happened with the people around you?"

---

**3. Personal brand (work)**

*Founder / builder:* "What was the hardest or most interesting thing you dealt with at work this week? A decision, a conversation, a problem — walk me through it."

*Marketing / GTM:* "What was the most interesting marketing or GTM challenge you worked on this week? A message that wasn't landing, a test you ran, a decision you made — walk me through it."

---

**4. Personal brand (life)**

"Anything happen outside work this week — something you did, experienced, or observed — that made you think differently about your work or the world?"

---

**5. AI landscape**

*Founder / builder:* "What's one thing you read, heard, or reacted to in AI this week — an article, a product, a take — that you actually had an opinion on?"

*Marketing / GTM:* "What's one thing in AI you reacted to this week — especially anything changing how marketing, distribution, or GTM works — that you actually had a take on?"

---

**6. Repost / curation**

"Anything you came across this week — a post, a resource, a tool — that you'd recommend to the people you're trying to reach?"

---

### Weekly Extraction output

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

After presenting the 5 ideas, proceed directly to Step 1b — map the week plan. Don't ask the user to pick one first; propose the full plan and let them adjust.

---

## Drafting flow

For each post in the week plan, go through this flow sequentially. After one post is confirmed or skipped, move to the next automatically.

### Gather raw material

If the post came from the Weekly Extraction interview, the raw material is already there — use it. Only ask follow-up questions if something specific is missing (a number, a name, a key detail).

If the post is on a topic the user brought in directly, ask short targeted questions to pull out the substance. Don't write anything until you have enough signal. For each type:

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
- The persona's actual opinion — agreement/disagreement and why
- What they think will happen next (the prediction, not the hedge)

**Repost**
- The original post or resource URL/author
- Why this is valuable for their audience specifically
- Optional: a short intro line the persona wants to add

**Product**
- What launched or changed — one concrete thing
- Who it's for and what problem it solves
- Any metric, number, or before/after that makes it tangible

### Draft the full post

Structure of a LinkedIn post:
1. **Hook** (1–3 lines) — stops the scroll
2. **Body** (3–8 lines) — delivers the substance
3. **Close** (1–2 lines, optional) — a thought, a question, a next step

**Hook:** Generate 5 hook variants using the Hook Generation framework below. Present them and let the user pick or ask for iterations before writing the body.

**Close:** End on a statement, not a question. If there's a CTA, make it specific ("Link in comments" or nothing at all).

### Tone

Default tone by post type unless the persona specifies otherwise:

| Post type | Default tone |
|---|---|
| Company culture | Conversational |
| Personal brand | Vulnerable or Direct |
| AI landscape | Provocative or Authoritative |
| Repost | Conversational |
| Product | Direct or Authoritative |

### Iterate, confirm, and move on

After sharing the draft:
- Ask if the tone, hook, or body needs adjusting
- When the user approves, save it to the persona's memory file under `## Confirmed Posts` with today's date and post type
- Then immediately move to the next post: "Post [N] done. Ready for post [N+1]? Here's what we planned: [slot] — [hook]"

Keep momentum. The goal is to exit the session with 3-4 drafted posts, not 1 polished one.

---

## Hook Generation

Generate hooks natively using this framework. No external tool needed.

### How to generate hooks

When a hook is needed for a post:

1. **Infer** topic and audience from context. Ask only if something critical is missing.
2. **Identify the content goal** from the post's slot and PPP type:
   - Perspective posts (personal brand, AI landscape) → `thought_leadership` or `engagement`
   - Proof posts (product, company culture) → `save_worthy` or `audience_growth`
   - Promo posts → `engagement`
3. **Use the default tone** for the post type (or the persona's preference if stated).
4. **Generate 5 variants**, one per lever, using the framework below. Each must pass all 3 brain gates.
5. **Present them** in this format, then offer to regenerate specific ones.

### Output format

```
**Hook 1** — Curiosity Gap · [Surface/Social/Core]
[hook text]
Best for: [goal] | ⚠ [warning, or omit if none]

**Hook 2** — Loss Framing · [Surface/Social/Core]
[hook text]
Best for: [goal] | ⚠ [warning, or omit if none]

[...repeat for all 5 levers...]
```

After presenting all 5: "Which hook are you going with? Or I can rework specific ones — just give me the number and any direction."

### Regeneration loop

When the user asks to rework a hook:
- Note which slots to redo and any specific instruction ("more direct", "try loss framing", "less generic")
- Generate new variants only for those slots — meaningfully different from the existing ones
- Highlight the new hooks clearly
- Repeat until the user picks one or moves on

### The 3 brain gates

Every hook must pass these three gates, which fire in the first 300ms of reading before any conscious decision:

**Gate 1 — Pattern Recognition (0–100ms)**
Does this look like marketing? Generic hooks, corporate tone, and templates trigger instant dismissal.
Fix: be *unexpectedly familiar* — recognizable structure, unexpected angle.
Avoid: buzzwords, listicle openers without a specific claim, vague inspiration.

**Gate 2 — Identity Match (100–200ms)**
Is this for someone like me? Three identity layers, in order of depth:
- **Surface** — job title, industry, role → earns a glance
- **Social** — shared tribal experiences, group struggles → earns a share
- **Core** — values, beliefs, worldview → earns advocacy

Most hooks only hit Surface. The strongest hit Social or Core — they articulate what an audience already believes but has never seen written out loud.

**Gate 3 — Value Prediction (200–300ms)**
Is reading this worth the effort? Loss framing and curiosity gaps are the two most reliable mechanisms — both signal: "the cost of NOT reading is higher than the cost of reading."

### The 5 psychological levers

**1. Curiosity Gap**
Create a knowledge tension the brain needs to resolve. Contradict an assumption, expose a hidden pattern, or name something unnamed. Do not resolve the tension in the hook — let it hang.
Pattern: `"Everyone does X. They're wrong."` / `"X is not what you think it is."`

**2. Loss Framing**
Loss feels urgent; gain feels optional. Frame around what the reader is losing, missing, or getting wrong.
Pattern: `"You're leaving [outcome] on the table."` / `"Most [group] never notice [cost]."`

**3. Identity Mirroring**
Articulate the unspoken belief, frustration, or experience of the target tribe. The reader should feel "this person gets it" — not just "this is relatable."
Distinction: relatable = about them. Identity mirroring = about their worldview.
Pattern: `"The reason [group] struggles with X isn't [obvious reason]."`

**4. Pattern Interrupt**
Contradict the expected take on a familiar topic. Works best paired with a credible specific claim.
Pattern: `"[Conventional wisdom]. [One-sentence reframe]."`

**5. Competence Signal**
Promise a framework, process, or mental model that makes the complex manageable. Positions the author as a useful resource worth following.
Pattern: `"Here's the [X]-step framework for [hard thing]."`

### Content goal alignment

| Goal | Primary levers | Identity layer target |
|---|---|---|
| Thought leadership | Curiosity Gap + Pattern Interrupt | Core |
| Audience growth | Competence Signal | Social |
| Engagement | Identity Mirroring + Loss Framing | Social |
| Save-worthy | Competence Signal + specific numbered claims | Surface → Social |

### The Relatability Trap

Flag a hook that is in the trap if:
- It has no unique angle or claim attached
- Anyone could have written it
- The reader could share it without endorsing the author's perspective

Fix: use shared experience as a **setup**, then deliver a take the audience hasn't heard.

### Writing rules (apply to hooks and the full post)

**Banned phrases — never use:**
- "game-changer", "leverage" (as a verb), "delve", "navigate", "crucial", "pivotal"
- "in today's world", "in today's landscape"
- "it's not about X, it's about Y" as a formula
- "I want to share", "I'm excited to share", "Here's the thing:", "At the end of the day"
- "This is a reminder that", "The truth is", "Spoiler:"
- "What do you think?" or "Drop a comment" at the end

**Structural patterns that signal AI — avoid:**
- Perfect parallel structure in every line
- Every sentence complete and grammatically polished (fragments are human)
- Em dashes used as a crutch every few lines
- Motivational cadence: short punchy line. Slightly longer line. Emotional closer.
- Round numbers that feel estimated ("thousands of")
- Hedging opinions: "I think", "In my opinion" — state claims flat

**What human writing looks like:**
- Sentence fragments. Used deliberately.
- Specific odd numbers (not "3 weeks", but "17 days")
- Contractions and informal connectors (And. But. So.)
- Dramatically varied sentence length
- Opinions stated as facts, not offered as perspectives
- Word choices that are slightly unexpected, not the first synonym

---

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

[The persona's actual position — disagree, agree, nuance — one sentence]

[Why — the reasoning, compressed. What this means going forward.]
```

### Repost intro

```
[One line on why this is worth reading — specific, not "great post"]

[One-sentence context for the persona's audience]

→ [Author name or link]
```

### Product

```
[What shipped — one sentence, no jargon]

[Who it's for and what it removes from their life]

[The number, the before/after, or the specific use case that makes it real]

[Link or CTA — or nothing if it speaks for itself]
```

---

## Post strategy layer — Perspective / Proof / Promo

Every post falls into one of three strategic types.

| Type | Purpose | Reader stage | Offer visibility | Weekly target |
|---|---|---|---|---|
| **Perspective** | Build authority and attract peers | Cold → warm | None | 2/week |
| **Proof** | Build trust and belief, show results | Warm → want more | Soft | 2/week |
| **Promo** | Trigger action from ready buyers | Hot | Explicit | Occasional |

### Perspective posts
- Share lived experience, opinions, contrarian takes, personal stories
- No explicit mention of what you sell — authority builds through the voice, not the pitch
- Common mistakes: no specifics, no lived reality, sounds like a generic thought leader
- Maps to: **personal brand**, **AI landscape** slots
- Hook style: start with a situation or confession, not a lesson

### Proof posts
- Show client results, case studies, before/after, frameworks in action
- Soft mention of your offer is fine
- Common mistakes: hook doesn't speak to ICP, too broad, not specific enough
- Maps to: **product**, **company culture** slots
- Hook style: lead with a specific result or surprising outcome

### Promo posts
- Direct call to action — what you offer, who it's for, why now
- Common mistakes: no CTA, no ICP context, too many ideas in one post
- Maps to: **product** slot (launches, offers, sign-ups)
- Hook style: name the transformation or the before/after for a specific person

### Applying this to the weekly mix

- 2 Perspective posts (personal brand + AI landscape)
- 2 Proof posts (product + company culture)
- 1 flexible slot (repost, or Promo if there's something to push)

---

## What to avoid

- Generic culture posts ("So proud of our amazing team!")
- AI landscape takes that don't commit ("It's complicated, but...")
- Product posts that describe features instead of outcomes
- Perspective posts with no specifics — "no lived reality" is the #1 failure mode
- Proof posts where the hook doesn't speak directly to the ICP
- Promo posts with no CTA or too many ideas
- Any hook that could have been written by anyone

---

## Notes

- Weekly cadence: aim for all 5 slots, but 3 strong posts beat 5 weak ones
- Persona memory files: `~/linkedin-memory-[slug].md`
- Active persona: `~/linkedin-persona`
