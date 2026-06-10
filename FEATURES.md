# nprep QBank — Feature List (Prototype)

> End-to-end feature inventory of the QBank flow as built in the prototype.
> Stack: React 18 + Vite 5 SPA · Manual screen routing via `screen` state in App.jsx · No backend

---

## Prototype-Level Controls

| Feature | Detail |
|---|---|
| **New / Returning User toggle** | Pill switch in the header of Home, Subject, and Videos screens. Shared state across the whole app — switching on any screen affects all others. |
| **New User state** | Zero progress bars, no Continue Watching, first-time onboarding copy, no stats shown |
| **Returning User state** | Real session data shown — daily Qs attempted, overall accuracy, chapter progress |

---

## 1. Home Screen (`/home`)

### Header & Stats
- User avatar (initials) + "Question Bank" title + search icon
- **First-time user** → "Start your first practice session" card with a direct Attempt CTA; stats area is empty with guidance copy
- **Returning user** → Stats strip showing **Daily Questions Attempted** + **Overall Accuracy %** (color-coded: green ≥70%, purple ≥50%, red <50%)

### Promotional
- **AIR 15 topper story banner** — gradient card, dismissible with ×, "Read More →" CTA

### Saved Questions Entry Point
- **First-time user** → Educational info box explaining the bookmark feature (still tappable)
- **Returning user** → Purple action button "Click Here to view your saved questions"

### Subject List
- List of all subjects with:
  - Per-subject progress bar (color reflects accuracy)
  - `X/Y Qs · Z% accuracy` or `not started` label
- **Subject Index sheet** — bottom sheet listing all subjects with numbered index and completion %

### Continue Where You Left Off (returning users only)
- Persistent sticky banner above the bottom nav
- Shows last session's subject + chapter name
- Resume CTA navigates to PreTest
- Dismissible with ×

### Navigation
- Bottom tab bar: Home · QBank · Videos · Tests · Buy

---

## 2. Subject Screen (`/subject`)

### Header
- Back to Home
- Subject title + edition badge (e.g., E5)
- Search icon + Chapter Index icon

### Toppers Banner (personalized)
- "★ Toppers complete this in 10 days" progress bar showing chapters done / total
- **Tappable CTA** → "View your study plan to finish in 10 days →"

### 10-Day Study Plan Sheet (on tap)
- Full-width bottom sheet with:
  - Overall progress bar
  - **Timeline view** — 7 rows with connecting vertical line
  - Each row: day range (e.g., Day 3–4), chapter name, question count, study tip
  - **Status dots**: green ✓ (completed), purple ▶ (up next), grey (upcoming)
  - Inline "Start now →" CTA on the current chapter
- Adapts automatically as chapters are completed

### Filter Tabs
- All · Free · Completed · Unattempted · Paused

### Chapter Index Sheet
- Bottom sheet with numbered chapter list
- Completion badges (Done / Paused) per chapter

### Chapter Cards
Three states, each styled distinctly:

| State | Card Content |
|---|---|
| **Unattempted** | Chapter name · Q count · Learn (if video available) + Attempt buttons |
| **Paused / In Progress** | Partial progress bar · Qs done/total · Resume + Learn buttons |
| **Completed** | Green border · Score summary (correct/total, %) · "Ahead of X% peers" badge · Re-Attempt + View Analysis buttons |

---

## 3. Pre-Test Screen (`/pretest`)

- Back to Subject
- **Mode selector** (2 options, pill cards):
  - **Guide Mode** — Solution visible immediately after each question
  - **Exam Mode** — Solution visible only after completing all questions
- Chapter info card (subject → chapter → total questions)
- **Topics list** — each topic shows Q count + PYQ count
- **"Revise first" video CTA** — warm amber card linking to chapter video
- **Let's Begin** CTA

---

## 4. Solve Screen (`/solve`) — Core QBank Experience

### Timer
- Per-question countdown (default 60s)
- Animated circular arc timer in the top bar
- On timeout → "Oops! You ran out of time" state card (question marked as skipped)

### Question Display
- Question text
- 4 answer options (A / B / C / D) as tappable cards
- Selected option highlighted in purple
- Correct/incorrect feedback shown immediately (Guide Mode) or after submit (Exam Mode)
- **Peer response distribution** — % bar shown on each option after answering (e.g., "67% of students chose B")
- PYQ badge on relevant questions

### Answer Blocking
- **Next button is locked** until the student selects an answer or runs out of time
- On attempted tap → "Select an answer to move on ↑" nudge message (auto-dismisses in 2.2s)
- Next button opacity dims to 0.5 when locked

### Skip / Timeout Nudge System
- **Attempts 1–3**: First timeout in each attempt auto-opens the skip survey after 700ms
- **All attempts**: Text CTA "Why did you skip this?" always visible after timeout
- **Attempt 4+**: Only text CTA shown (nudge not re-shown after behaviour is established)

### Skip Survey (modal)
- "Why did you skip this question?" title
- Options: Too confusing / Running out of time / Revise first / Other

### Navigation Dots
- Visual-only question dots (1 dot per question)
- Color-coded: answered / unanswered / skipped (timeout)
- Not tappable (no jumping allowed — designed to preserve sequential learning)

### Question Actions (top bar)
- **Bookmark** — save question; opens tag picker: Wrong / Important / Tricky / Revision
- **Question counter** (e.g., 3 / 21)

### Guide Content Panel (below question)
Always shows:
- **Explanation** — full text explanation + option-by-option distractor reasoning ("Why Other Options Were Wrong")
- **Clinical Relevance** — collapsible accordion
- **How to Approach** — collapsible accordion
- **Reference Book** — book name, edition, page number

### Bottom Navigation
- **Previous** (disabled on Q1)
- **Next** → guarded by answer-blocking logic
- **Submit** button on last question

### Submit Confirmation Dialog
- **Unanswered questions** → "You have X questions without an answer — please go back and attempt them"
- **All skipped (timeout)** → "X questions were skipped due to time running out"
- **All answered** → "Are you sure you want to submit?"

### Review Mode
- Entered via "View Solutions →" from Result screen
- All answers locked; correct/incorrect states shown
- Timed-out questions show the correct answer + explanation
- Navigation dots reflect full attempt state

### Session Stats (top of screen during attempt)
- Running count of: Correct · Incorrect · Skipped

---

## 5. Result / Performance Screen (`/result`)

*Single scrollable page — Summary hero at top, full analysis below.*

### Animated Hero Section (slides up on mount)
- Chapter context label ("Anatomical Terms · Applied Anatomy")
- **Reattempt notice** if this is not a first attempt
- **Adaptive headline**:
  - ≥80% → "Outstanding!"
  - ≥60% → "Good effort!"
  - ≥40% → "Keep going!"
  - <40% → "Don't give up!"
- **Semi-circle accuracy gauge** — SVG arc animates from 0% to actual accuracy (1.3s spring)
- **3 Fan stat cards** (Duolingo-style spread):
  - Correct (green, rotated −3°)
  - Questions / Total (purple, elevated)
  - Incorrect (red, rotated +3°)
- **Skipped row** (amber, shown only if questions timed out)
- "Scroll for full analysis ↓" hint

### Full Analysis Section (below divider)
- **Score summary card** — motivational copy + avg time per question + total session time + correct/wrong/skipped counts
- **PYQ Missed alert** — if any previous-year exam questions were answered incorrectly; lists them as tappable chips (exam name + year)
- **Study Focus** — per-topic weakness breakdown:
  - Topic name + wrong/skipped count + PYQ count
  - Accuracy progress bar
  - "Review" CTA per topic
- **Review These Questions** — expandable list of all wrong/skipped questions:
  - Question snippet (85 chars)
  - Your answer vs correct answer (in colour)
  - "Show N more ↓" / "Show less ↑" toggle
- **Experience Feedback** — 5-star rating + optional text note + Submit Feedback (in-screen confirmation on submit)

### Bottom CTAs (fixed)
- **Try Again** → opens Re-attempt confirm popup
- **View Solutions →** → enters Review Mode in Solve screen

### Re-attempt Confirm Popup
- Warns: "Only your first attempt scores are marked for review and analysis"
- Cancel / Try Again

---

## 6. Saved Questions Screen (`/saved`)

- Back to Home
- Total saved count badge
- **Tab toggle**: Questions · Videos
- **Filter chips**: All · Wrong · Important · Tricky · Revision (with per-tag count)
- Saved question cards:
  - Expandable — tap to show question text + explanation snippet
  - Tag label
  - Remove (unsave) with confirm dialog
- Empty state if no questions saved

---

## Cross-Cutting / App-Level Features

### Session Tracking
- First attempt data saved per chapter (`sessions` array)
- Reattempts tracked separately (`isReattempt` flag) — not overwritten
- `viewAnalysis()` restores first-attempt answers to show accurate historical performance
- `attemptCount` increments on every attempt (including reattempts)

### Screen Transition Animations
Driven by a `SCREEN_DEPTH` map — no animation library required.

| Navigation | Animation | Duration | Easing |
|---|---|---|---|
| Going deeper (Home → Subject → Solve) | Slide in from **right** | 260ms | iOS ease-out |
| Going back (any back button) | Slide in from **left** | 260ms | iOS ease-out |
| Result after submit | **Slide up** from below | 340ms | Ease-out expo (spring) |
| Try Again / View Solutions | Slide in from **right** | 260ms | iOS ease-out |

### Data Model (src/data.js)
| Export | Purpose |
|---|---|
| `SUBJECTS` | 10 nursing subjects with total Qs, progress, accuracy |
| `CHAPTERS` | 6 chapters per subject with state, progress, difficulty |
| `TOPICS` | 4 topics in Anatomical Terms with Q + PYQ count |
| `QUESTIONS` | 21 questions with options, correct answer, explanation, distractors, approach, reference book, PYQ metadata |
| `SAVE_TAGS` | Tag definitions for bookmarked questions |
| `VIDEO_SUBJECTS` | 12 video subjects with watched counts |
| `VIDEO_CHAPTERS` | Chapter + video lists for video player |

---

## Screens Summary

| Screen | Route key | Entry point |
|---|---|---|
| Home | `home` | App start |
| Subject | `subject` | Home → subject tap |
| Pre-Test | `pretest` | Subject → Attempt |
| Solve | `solve` | PreTest → Let's Begin |
| Result | `result` | Solve → Submit |
| Saved | `saved` | Home → Saved CTA |
| Videos | `videos` | Bottom nav |
| VideoSubject | `videosubject` | Videos → subject tap |
| VideoPlayer | `videoplayer` | VideoSubject → video tap |

---

*Last updated: June 2026 · nprep prototype v1*
