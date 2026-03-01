# Ana's English Travels — Claude Code Handoff

## What this project is

A single-file HTML website for Ana Carolina's ESL class for adult learners in Brazil. It centralises class slides, homework, attendance tracking, and a class calendar — replacing a scattered Google Sheets/Docs/Slides workflow.

**Live file:** `index.html` — this is the entire site. No build tools, no dependencies, no backend. Pure HTML + CSS + vanilla JavaScript in one file.

---

## The students

- **Zilda** — travel + music goals, fear of speaking
- **Sandra** — travel + music goals, struggles memorising words
- **Monica** — wants confidence, strong writer but struggles with listening/speaking
- At least 2 are likely neurodivergent (Zilda: suspected ADHD, Monica: suspected autistic)

**Class schedule:** Mondays and Fridays, 1 hour, online via video call, twice a week.

---

## Site structure

The site is a single-page app with 7 sections, toggled by JS (`showSection(id)`):

| Nav label | Section ID | Description |
|---|---|---|
| 🏠 Home | `section-home` | Welcome banner + big nav buttons |
| 📅 This Week | `section-class` | Current slides link + what we're practising |
| ✏️ Homework | `section-homework` | Homework link + step-by-step instructions |
| 📚 Theory | `section-theory` | Master theory deck link + topic grid |
| 🗓️ Calendar | `section-calendar` | Monthly calendar + class log table |
| 🎵 Songs & Videos | `section-media` | BBC Learning English, Chicken Little, songs |
| 🗺️ Our Journey | `section-progress` | Unit progress bars |

---

## Design system

All colours are CSS variables defined in `:root`:

```css
--peach: #F5E6D8
--peach-mid: #EDD5BE
--peach-dark: #D4A882
--terracotta: #C07D54      /* primary accent */
--sage: #8FAF8A
--sage-light: #B8D4B3
--dusty-rose: #D4899A
--cream: #FBF6F0            /* page background */
--warm-brown: #5C3D2E       /* text */
--soft-gold: #E8C87A
```

**Fonts:** Playfair Display (headings/serif), Nunito (body) — loaded from Google Fonts.

**Theme:** Warm terracotta/peach, travel motifs (✈️ 🗺️ 🧳). Previously floral — travel theme is the current direction.

**Language:** Bilingual throughout. English primary, Portuguese subtitle under every label.

---

## The class log (most complex part)

The entire class history lives in a JavaScript array called `classLog` (around line 1020 in index.html). Each entry looks like:

```javascript
{
  date: '2025-03-14',          // ISO date string
  slidesTitle: '14/03 - Alphabet and Introductions',  // deck name
  unit: 'Alphabet, Greetings and Introduction',       // display name
  absent: [],                  // array of student names who were absent e.g. ['Zilda']
  note: '',                    // free text notes (Portuguese or English)
  slidesUrl: 'https://docs.google.com/presentation/...',  // Google Slides link
  homeworkUrl: 'https://docs.google.com/document/...',    // Google Docs link
  cancelled: true,             // optional — marks cancelled/vacation days
  month: 'mar25'               // legacy grouping key (not used for filtering anymore)
}
```

### Calendar colour coding (driven from the note/cancelled fields)

| Colour | Trigger |
|---|---|
| 🌸 Pink | note contains "fechamento" |
| 🌴 Green | note contains "vacation" or "férias" |
| Grey + ✕ | `cancelled: true` AND not vacation |
| Peach (default) | regular class day |

Clicking a coloured day opens a **modal popup** showing: unit title, attendance per student, slides link, homework link, notes.

### Calendar + class log are synced
The ← → month navigation arrows control BOTH the mini calendar grid AND the class log table below it. They share `currentYear` / `currentMonth` variables.

---

## What's been completed ✅

- All 83 classes logged (March 2025 → March 2026)
- 80 Google Slides URLs linked
- 72 homework Google Doc URLs linked
- Attendance (✅/❌) per student per class
- Calendar colour coding (pink/green/grey/peach)
- Clickable day modal with full class details
- Synced month navigation (calendar + log table)
- Bilingual nav and all section headings
- Date format: dd/mm/yy throughout

---

## Immediate next steps (priority order)

### 1. Deploy to Netlify
- Go to netlify.com → sign up free → drag and drop `index.html`
- No configuration needed — it's a single static file
- Share the resulting URL with students via WhatsApp

### 2. Update "This Week's Class" section
The `section-class` content is **hardcoded** — Ana has to manually edit the HTML each week. Should be made easier. Options:
- A simple editable config block at the top of the JS (e.g. `const THIS_WEEK = { unit: '...', slidesUrl: '...' }`)
- Or a small admin panel / edit mode so Ana can update it without touching code

### 3. Fix the gap in the class log
There are no entries between **31/10/2025 and 17/11/2025** — about 2.5 weeks. Worth checking with Ana whether classes happened that aren't logged, or adding a note/cancelled entries for those dates.

### 4. Homework section improvements
Currently the homework section just links to a Google Doc and gives copy-paste instructions. Students struggle with the "make a copy" flow. Ideas:
- Embed the exercises directly on the page
- Or at minimum, make the instructions more visual/step-by-step with screenshots

### 5. Units 5–10 curriculum
Ana is finishing a review of Units 1–4. Units 5–10 haven't been built yet. Plan is to restructure around **functional/travel scenarios** rather than traditional grammar progression:
- At the airport
- At the hotel
- At the restaurant
- Asking for directions
- At the doctor
- Shopping

Update the "Our Journey" progress section (`section-progress`) as new units are taught.

---

## Curriculum reference

| Unit | Topics |
|---|---|
| 1 | Alphabet, pronunciation, greetings, numbers 1–20, pronouns, articles |
| 2 | Nouns, verbs (be/have/like/go), adjectives, days, colours, numbers 20–100, money, shopping |
| 3 | Present simple, times of day, meals, daily routines (used Chicken Little video — 9 classes) |
| 4 | Prepositions, adverbs of frequency, telling time, months, ordinal numbers, family |
| 5–10 | Not yet built — see above |

**Currently:** Major review of Units 1–4 in progress (started Jan 2026).

---

## Teaching approach (important for any content work)

- Heavy emphasis on **oral communication and listening**
- Grammar taught **in context**, not in isolation
- **Phonetic pronunciation guides** in slides (e.g. OHUEIS for "always")
- **Visual-first** — slides act as a chalkboard anchor
- No pressure, no perfection — confidence and comprehension are the goals
- Classes taught in a **mix of English and Portuguese**
- Max **3 exercises per homework** to keep review manageable
- Pacing is slower than standard adult learners — this is intentional and appropriate

---

## Tools in use

| Tool | Purpose |
|---|---|
| Google Slides | Class delivery (one deck per class + one master theory deck) |
| Google Docs | Homework (students make a copy, fill in, send link via WhatsApp) |
| Google Sheets | Class calendar/log (source of truth — this site mirrors it) |
| WhatsApp | Communication between classes |
| This website | Central hub — work in progress, not yet deployed |

---

## Files in this package

```
index.html          — The entire website (single file, ~1300 lines)
HANDOFF.md          — This document
data/calendar.csv   — Original class calendar export from Google Sheets
data/slides.csv     — CSV with extracted Google Slides URLs (column: Extracted URLs)
data/homework.csv   — CSV with extracted homework Google Doc URLs (column: Column 1)
```

The CSVs are included for reference — all their data has already been imported into `index.html`. They're useful if you need to re-import or verify links.

---

## Ana's contact context

Ana Carolina is Brazilian, based in London, teaching remotely to students in Brazil. She is not a professional teacher but has teaching experience. She is comfortable with Google Workspace but not with code — any admin/update workflows should be as simple as possible, ideally not requiring her to edit HTML directly.
