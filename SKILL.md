# Carousel Creator Skill

Create high-converting Instagram/TikTok carousel posts and generate the actual slide images with AI.

## When to Use
Trigger when user says: "create a carousel", "make a carousel about [topic]", "carousel for [product]", "turn this into a carousel", "slideshow post"

---

## Setup Required

1. Get an image generation API key (Kie.ai works well — [kie.ai](https://kie.ai))
2. Set `KIE_API_KEY` as an environment variable — never hardcode keys in your skill files or scripts
3. Point the generator script to your local path

---

## Visual Styles

### Style 1: Brand Color (Dark/Cream)
- Dark background color (forest green, navy, charcoal)
- Warm cream text
- Accent color for numbers, highlights, labels
- Montserrat Bold headlines, Nunito Regular body
- 1080×1350px (4:5 portrait)
- Handle bottom-left, slide number bottom-right
- Use for: authority content, protocols, brand posts

### Style 2: Whiteboard
- Realistic whiteboard takes up center 2/3 of image
- White board with thin gray frame, realistic marker texture
- Black dry-erase marker style text (slightly imperfect, human-looking)
- Key words highlighted with bright accent highlighter behind black text
- Arrows, underlines, circle callouts in black marker
- No humans, no hands — just the board
- Use for: educational content, ingredient reveals, science explainers

**Important:** always set the style explicitly in every carousel definition (e.g. `"style": "whiteboard"`). If your generator has a default style, a missing field silently renders the wrong look — easy to miss until after you've posted.

---

## Proven Carousel Formats

### Format 1: Myth Buster
Hook: "[X] things you've been told about [topic] that are completely wrong."
Structure: Slide 1 hook → Slide 2 bridge → Slides 3-7 MYTH/TRUTH pairs → Slide 8 pattern callout → Slide 9 CTA

### Format 2: Problem Stacker
Hook: "If you have [X], [Y], and [Z] — read this."
Structure: Hook → validate → one symptom per slide with context → pivot → CTA

### Format 3: Protocol Reveal
Hook: "My exact [protocol] for [outcome]. Save this."
Structure: Hook → why this matters → numbered steps (one per slide) → results frame → CTA

### Format 4: Contrarian Science Drop
Hook: "Your doctor got this wrong." / "Stop doing [X]."
Structure: Hook → steelman conventional wisdom → counter-evidence → what to do instead → CTA

### Format 5: Identity Shift
Hook: "Your [decade] isn't the beginning of the end."
Structure: Hook → old story → new story → actions → CTA

---

## Hook Templates (Plug-and-Play)
1. `"If you have [X], [Y], and [Z] — read this."`
2. `"Everything you've been told about [topic] is wrong."`
3. `"[Stat]% of [audience] are experiencing [X]. Most don't know why."`
4. `"I wish someone had told me this about [topic] before [age/event]."`
5. `"Stop [wrong action]. [Right action] instead."`

---

## Slide Structure Rules
- 8-10 slides sweet spot
- ONE idea per slide — never two
- 15-30 words max per slide body
- Add "→" swipe cue on slides 2-7 (boosts completion 15-30%)
- Slide number on every slide
- Final slide visually distinct — signals end, strong CTA

---

## CTA System
- One CTA only per carousel — never multiple asks
- Best combos: Save + keyword comment
- Use a unique keyword per carousel to track which post drives which leads
- Always set up your DM automation (ManyChat etc.) BEFORE posting
- **Never mention price on the CTA slide** — free or paid. Tell them what they get and how to get it. Price talk kills comment rates.

---

## Compliance Rules
- Never: "treats," "cures," "prevents," "heals," "balances your hormones"
- Always: "supports," "may support," "clinically studied," "formulated with"
- No wild medical claims — frame as natural support, not medical treatment

---

## Workflow (End-to-End)

1. Get topic — from user request, trending content, or research
2. Choose format — Myth Buster / Problem Stacker / Protocol / Contrarian / Identity
3. Write slides — hook, value slides, CTA. One idea per slide.
4. Choose style — Brand Color (authority) or Whiteboard (educational)
5. Generate images
6. Review output slides
7. Post — 10-11am or 2pm weekdays
8. Track — swipe-through rate (target 50%+), saves, keyword triggers in ManyChat

---

## Level Up: Fully Automated Daily Carousels

Once the manual workflow is dialed in, automate it. The architecture that works:

```
Scheduler (Task Scheduler / cron, runs 1 hr before each post slot)
   → picks today's topic from a deterministic rotation (date math, no database)
   → calls Claude API to write the slide JSON
   → drops the JSON into a queue folder (cloud-synced, e.g. Google Drive)

Watcher script (runs at login, polls the queue folder)
   → picks up queued JSON → generates all slide images
   → copies finished slides to a "ready" folder
   → sends a preview to Telegram for approval
   → you post manually (or wire up scheduling after you trust it)
```

Hard-won lessons baked into this design:

- **Date-stamp your output names** (`YYYYMMDD-slot1-topic-name`). Topic rotations repeat names over time, and a dedupe check against old output will silently skip real runs.
- **Retry your API calls** (5 attempts, 60s apart). Scheduled tasks fire the moment the machine wakes — often before the network is back. Without retries the run dies silently and you find out when nothing posts.
- **Send failures to Telegram, not just logs.** A scheduled task that exits "successfully" after doing nothing is the worst failure mode — you won't notice for days.
- **Generate 1 hour before the post slot.** Enough buffer to catch a failure and re-run, not so far ahead that content goes stale.
