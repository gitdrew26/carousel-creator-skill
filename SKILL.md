# Carousel Creator Skill — Drew's Health Shop + GR33N

Create high-converting Instagram/TikTok carousel posts and generate the actual slide images via Kie.ai Nano Banana 2 API.

## When to Use
Trigger when user says: "create a carousel", "make a carousel about [topic]", "carousel for [product]", "turn this into a carousel", "slideshow post"

---

## Generator Script
**Location:** `C:/Users/aever/Projects/carousel-generator/generate.py`
**API:** Kie.ai Nano Banana 2 (Gemini 3.1 Flash Image)
**API Key env var:** `KIE_API_KEY=344bf633c1228949c01ca1ca6d5786b0`

### Run Commands
```bash
# Single carousel by ID
KIE_API_KEY=344bf633c1228949c01ca1ca6d5786b0 python C:/Users/aever/Projects/carousel-generator/generate.py --carousel 4

# Custom JSON
KIE_API_KEY=344bf633c1228949c01ca1ca6d5786b0 python C:/Users/aever/Projects/carousel-generator/generate.py --custom slides.json
```

### Existing Carousels (pre-built in script)
| ID | Name | Style | Keyword | Status |
|----|------|-------|---------|--------|
| 1 | Myth Buster (Weight Loss After 40) | Green/Cream | GUIDE | Done |
| 2 | Problem Stacker (7 Hormone Signs) | Green/Cream | SYMPTOMS | Not run |
| 3 | Protocol Reveal (Morning Cortisol) | Green/Cream | MORNING | Not run |
| 4 | Soursop 52 Compounds | Whiteboard | SOURSOP | Done |

### Output Location
`C:/Users/aever/Projects/carousel-generator/output/<carousel-name>/slide_01.jpg ...`

---

## Two Visual Styles

### Style 1: Green/Cream (Drew's Health Shop brand)
- Dark forest green background `#1B4332`
- Warm cream text `#F5F0E8`
- Amber/gold accents `#C9882E` for numbers, highlights, labels
- Montserrat Bold headlines, Nunito Regular body
- 1080×1350px (4:5 portrait)
- Handle `@drewshealthshop` bottom-left, slide number bottom-right
- Use for: menopause, fat loss, hormone, protocol content

### Style 2: Whiteboard
- Realistic whiteboard takes up center 2/3 of image
- White board with thin gray frame, realistic marker texture
- Black dry-erase marker style text (slightly imperfect, human-looking)
- Key words highlighted with bright GREEN highlighter behind black text
- Arrows, underlines, circle callouts in black marker
- Handle and slide number below whiteboard in small clean text
- No humans, no hands — just the board
- Use for: educational content, ingredient reveals, science explainers

To use whiteboard style, set `"style": "whiteboard"` in the carousel definition.

---

## How to Add a New Carousel

### Option A — Add to script (permanent)
Add a new entry to the `CAROUSELS` dict in `generate.py`:
```python
5: {
    "name": "folder-name",
    "title": "Carousel Title",
    "keyword": "TRIGGERWORD",
    "style": "whiteboard",  # or omit for green/cream default
    "slides": [
        {
            "number": "1/7",
            "headline": "Hook goes here.",
            "body": "Supporting text here.",
            "style_note": "Visual direction for this slide"
        },
        ...
    ]
}
```
Then run: `python generate.py --carousel 5`

### Option B — Custom JSON (one-off)
Copy `custom_template.json`, fill in slides, run with `--custom`.

---

## Proven Carousel Formats (Research-Backed)

### Format 1: Myth Buster
Hook: "[X] things you've been told about [topic] that are completely wrong."
Structure: Slide 1 hook → Slide 2 bridge → Slides 3-7 MYTH/TRUTH pairs → Slide 8 pattern callout → Slide 9 CTA
Best for: Weight loss, hormones, nutrition misinformation

### Format 2: Problem Stacker
Hook: "If you have [X], [Y], and [Z] after 40, read this."
Structure: Hook → validate ("you're not crazy") → one symptom per slide with context → pivot → CTA
Best for: Menopause/perimenopause, hormone symptoms, pain points

### Format 3: Protocol Reveal
Hook: "My exact [protocol] for [outcome]. Save this."
Structure: Hook → why this matters → numbered steps (one per slide) → results frame → CTA
Best for: Morning routines, fasting, supplement stacks, workout protocols

### Format 4: Contrarian Science Drop
Hook: "Your doctor got this wrong." / "Stop doing [X]."
Structure: Hook → steelman conventional wisdom → counter-evidence → what to do instead → CTA
Best for: Cardio myths, diet myths, ingredient reveals

### Format 5: Identity Shift
Hook: "Your 40s aren't the beginning of the end."
Structure: Hook → old story → new story → actions → CTA
Best for: Mindset, empowerment, brand authority

---

## Hook Templates (Plug-and-Play)
1. `"If you have [X], [Y], and [Z] after 40, read this."`
2. `"Everything you've been told about [topic] in your 40s is wrong."`
3. `"[Stat]% of women in their 40s are [experiencing X]. Most don't know why."`
4. `"I wish someone had told me this about [topic] before 40."`
5. `"Stop [wrong action] after 40. [Right action] instead."`

---

## Slide Structure Rules
- 8-10 slides sweet spot (fewer = weak signal, more = completion drops)
- ONE idea per slide — never two
- 15-30 words max per slide body
- Add "→" swipe cue on slides 2-7 (boosts completion 15-30%)
- Slide number on every slide
- Final slide visually distinct — signals end, strong CTA

---

## CTA System
- **One CTA only** per carousel — never multiple asks
- Best combos: Save + keyword comment
- ManyChat keywords: `GUIDE`, `SYMPTOMS`, `MORNING`, `SOURSOP`, `RESET`
- Use a unique keyword per carousel to track which post drives which leads
- Always set up ManyChat automation BEFORE posting

---

## Brand Split

### GR33N Carousels
- CTA: link in bio → soursop gummy product page
- Topics: Black Seed Oil, Sea Moss, Soursop, plant medicine, immune health
- Style: Green/Cream or Whiteboard
- No health cure claims — use "supports," "clinically studied," "plant-based"

### Drew's Health Shop Carousels
- CTA: comment keyword → ManyChat → discovery call → $247 Reset Protocol
- Topics: Menopause, perimenopause, fat loss after 40, cortisol, protocols
- Style: Green/Cream or Whiteboard
- Voice: Trusted male health expert who studied female physiology

---

## Compliance Rules
- Never: "treats," "cures," "prevents," "heals," "balances your hormones"
- Always: "supports," "may support," "clinically studied," "formulated with"
- No wild medical claims — frame as natural support, not medical treatment
- No health guarantees

---

## Workflow (End-to-End)

1. **Get topic** — from user, reel script, or research file
2. **Choose format** — Myth Buster / Problem Stacker / Protocol / Contrarian / Identity
3. **Write slides** — hook, value slides, CTA. One idea per slide.
4. **Choose style** — Green/Cream (brand) or Whiteboard (educational)
5. **Add to script or JSON** — carousel ID or custom_template.json
6. **Run generator** — `python generate.py --carousel N`
7. **Check output** — review slides in `/output/<name>/`
8. **Post** — 10-11am or 2pm weekdays. Reply to every comment within 30 min.
9. **Track** — swipe-through rate (target 50%+), saves, keyword triggers in ManyChat
