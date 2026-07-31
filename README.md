# MCQ Exam Drill

A single-file **bilingual (English + मराठी)** MCQ practice site with an **2891-question bank** aimed at Maharashtra competitive exams and school revision, designed as an OMR answer sheet — the scannable bubble form used in real exams. Answer a bubble and it inks in immediately: **green with a ✓ if correct, red with an ✕ and a shake if wrong**, with the correct option outlined in dashed green so you learn it on the spot.

No build step, no dependencies, no framework. One HTML file.

## Run it

Open the file directly:

```bash
xdg-open index.html      # Linux
open index.html          # macOS
```

Or serve it:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Features

- **Bilingual throughout** — all 2891 questions, their options, the verdicts, the answer key and the page chrome carry Marathi.
- **374 reasoning questions**, 29 of them diagram-based — drawn as inline SVG, so they still need no external files.
- **The sheet is set before it is drawn** — which subjects it comes from, how many questions it runs to (10 / 20 / 30 / 50), and how long there is to sit it.
- **A whole-sheet countdown**, quoted per question and multiplied by the sheet length — 60s × 20 questions is a 20-minute paper. When it expires the sheet grades itself and everything not reached is marked unattempted.
- Questions are **drawn at random**, with the four options shuffled every time — so a question's correct answer moves position between attempts.
- **No repeats until the bank runs out.** The bank is shuffled once and drawn down across sheets, so every one of the 2891 questions comes up before any of them comes up twice.
- **Instant marking.** Red for wrong, green for right, with a ✓/✕ stamp. The answer key row is revealed as soon as you commit.
- **An explanation on every question.** All 2891 carry a one- or two-line bilingual note, headed `WHY · कारण`, that appears under the options the moment you answer and again beside that question in the answer key. Where there is a rule to teach rather than a fact to restate, the note teaches it: *Double the term and add 1 each time: 31 × 2 + 1 = 63.*
- **Live readouts** for score, correct streak, and elapsed time.
- **Bubble progress strip** across the top, filling red/green as you work through the sheet.
- **Keyboard answering** — <kbd>A</kbd>–<kbd>D</kbd> or <kbd>1</kbd>–<kbd>4</kbd> to pick, <kbd>Enter</kbd> to advance.
- **Graded summary** — a score dial that sweeps to your percentage (green above 80, red below 50), boxed tallies for missed, percent, best run and time, and a togglable answer key. Every key row carries a coloured rail, so the misses can be found without reading a word.
- **Light and dark themes**, following your OS preference with a manual override. The stock is warm rather than blue-white, the way exam paper actually is, which also stops the long Devanagari lines from glaring.
- Respects `prefers-reduced-motion`.

## Setting the sheet

The page opens on a setup screen rather than straight into a paper. Everything on it is optional — the defaults are all 28 subjects, 10 questions, untimed — so a sheet is one click away, but the three settings are what make it drill a specific weakness rather than the whole bank.

**Subjects.** Every subject in the bank is a tick box carrying its question count, built from `BANK` at load rather than a hand-kept list — add questions in a new subject and its box appears on its own. `All` and `None` are there for the common case of drilling one thing: `None`, then `Reasoning`, gives a 150-question reasoning paper. The `Start` button is disabled while nothing is selected.

**Questions per sheet.** 10, 20, 30 or 50. A narrow selection can hold fewer questions than the sheet asks for — 50 questions from a subject with 19 — so the count is clamped to what is available and the setup screen says so before you start (*only 19 in this selection*) rather than quietly cutting the paper short.

**Time limit.** Untimed, or 90 / 60 / 30 seconds a question. The quote is per question but the clock is **one countdown for the whole sheet**, the way a real paper is timed: a hard question can be given four minutes at the cost of the rest. The masthead field switches from `Time` counting up to `Time left` counting down, and turns red for the last minute — or the last tenth of the paper, whichever is shorter.

When the countdown reaches zero the sheet grades itself where it stands. Questions never reached are logged **unattempted**, their bubbles left dashed on the progress strip, and the percentage is still out of the full sheet length — 6 of 20 on a paper you got halfway through is 30%, not 60%. The result screen leads with how many were left.

Settings are held for the session, so `Start a new sheet` draws another paper on the same terms and `Change the setup` goes back to alter them. They are not persisted — a reload returns to the defaults, along with a fresh shuffle.

Filtering starts a fresh rotation, because the queue in progress holds subjects that are now excluded. The queues are kept per selection, though, so going back to a previous one resumes it where it left off instead of reshuffling and repeating questions already seen.

## Bilingual: English + मराठी

Every question and every answer option is shown in **English with the Marathi directly beneath it**, the way Maharashtra state exam papers (MPSC, Talathi, police bharti) are printed. Nothing to configure and no language switch to explain — both languages are always on the sheet.

Fields that are identical in both languages — numbers, years, chemical symbols, formulas — are stored as a single string and rendered once, so there is no pointless duplicate line under `144` or `NaCl`.

Devanagari uses system fonts (`Noto Sans Devanagari`, `Nirmala UI`, `Mangal`, `Lohit Devanagari` and others in a fallback chain) because the page loads no external resources. Windows, macOS, Android and most Linux distributions ship at least one of these.

> The Marathi was written to match school and competitive-exam usage: questions translated rather than transliterated, proper nouns in Devanagari, Latin digits for numbers and years. **It has not yet been proofread by a native speaker** — worth doing before students rely on it, particularly the science terminology, where Marathi textbooks vary between Marathi terms and Devanagari-spelled English ones.

Two editorial rules apply throughout the bank:

- **Nothing time-sensitive.** No current officeholders, record holders, or population figures, so answers don't go stale.
- **Nothing genuinely contested.** Questions with disputed answers are rephrased to be unambiguous — hence "longest river in Africa" and "largest *hot* desert".

Every entry is machine-checked: four unique options, no duplicate questions anywhere in the bank, and the correct answer verified to survive option shuffling.

## Where the questions come from

**The bundled bilingual bank of 2891 questions is the default source.**

There is also optional support for **Open Trivia DB** (`opentdb.com`), which offers a few thousand questions across ~24 categories — but it is **English-only**, and no Marathi trivia API exists. Because bilingual sheets are the point, the live feed is off by default. To enable it, flip one constant near the top of the script:

```js
const USE_LIVE_API = true;   // draws English-only questions instead
```

With it on, the API is tried first and the bilingual bank serves as the fallback if the request fails. The subject filter applies to the local bank only — the API's categories don't map onto the bank's subjects — but the sheet length and the countdown work the same either way.

The bank is aimed at Maharashtra competitive exams (MPSC, Talathi, police bharti) as well as general school revision, across 28 subjects:

| Subject | Q | Subject | Q | Subject | Q |
|---|---|---|---|---|---|
| Reasoning | 374 | Biology | 103 | Environment | 19 |
| Indian polity | 279 | Science | 91 | Literature | 16 |
| Marathi language | 244 | Chemistry | 77 | Sport | 9 |
| Maths | 240 | Physics | 72 | Art | 5 |
| Maharashtra history | 236 | Static GK | 66 | Music | 5 |
| Indian history | 216 | Geography (world) | 56 | Language | 5 |
| Maharashtra geography | 205 | Computing | 45 | Mythology | 5 |
| Economics | 179 | Astronomy | 34 | General | 1 |
| English language | 150 | Maharashtra polity | 30 |  |  |
| Indian geography | 105 | History (world) | 24 |  |  |

At 10 questions a sheet, that is **289 full sheets before any question repeats**, and the entire bank is seen within 290. Filtering to one subject shortens that to that subject's own rotation.

This is a rotation, not an independent draw per sheet: the bank is shuffled once and questions are taken off the front until it is exhausted, then reshuffled. Drawing each sheet from the full bank instead would be random but much weaker practice — in simulation it repeated 60% of draws over 200 sheets while leaving 95 questions unseen. The queue lives in memory only, so reloading the page starts a fresh shuffle.

Maharashtra-focused coverage includes the Sahyadri and state rivers, districts and divisions, Shivaji Maharaj and the Marathas, the Peshwas, the social reformers (Phule, Ambedkar, Shahu Maharaj, Karve, Agarkar), the Samyukta Maharashtra movement, state administration from talathi to collector, the sant tradition and Marathi literature, and Marathi grammar and dialects.

## Reasoning · बुद्धिमत्ता चाचणी

The largest single subject, at 150 questions, covering the types that actually appear in MPSC, Talathi and police bharti papers:

| Type | Q | Type | Q |
|---|---|---|---|
| Number & letter series | 20 | Blood relations | 12 |
| Coding–decoding | 15 | Direction sense | 12 |
| Analogy | 15 | Syllogism | 10 |
| Odd one out | 12 | Ranking & order | 10 |
| **Figure series & counting** | 15 | Clock & calendar | 8 |
| **Mirror & water images** | 8 | Statement–conclusion | 7 |
| **Paper folding, embedded figures, dice** | 6 | | |

The 29 bold entries are **diagram questions**, drawn as inline SVG — no image files, no external requests, so the single-file property survives. Every stroke is `currentColor`, which means a figure is legible in both the light and dark themes and inks over to red or green with the rest of the option when the sheet is marked.

Where the four choices are themselves shapes — figure series, mirror images, embedded figures — the options are SVG too, because option text is rendered as HTML.

Series, coding-decoding and letter-based questions keep Latin letters and digits exactly as Maharashtra papers print them, so those options are stored as single strings rather than translated pairs.

## Adding your own questions

Each entry in the `BANK` array is `[subject, question, correctAnswer, [threeWrongAnswers], note]`. The fifth field is the explanation and is **optional** — an entry without one simply shows no panel, which is how live API questions arrive. Any of those text fields may be either a **plain string** (rendered once, for anything identical in both languages) or an **`[english, marathi]` pair** (rendered as two stacked lines):

```js
// bilingual — question, every option and the note differ between languages
["Biology",
  ["Which organ produces insulin?", "इन्शुलिन कोणता अवयव तयार करतो?"],
  ["The pancreas", "स्वादुपिंड"],
  [["The liver", "यकृत"], ["The kidney", "मूत्रपिंड"], ["The thyroid", "थायरॉइड ग्रंथी"]],
  ["Insulin is made by the beta cells in the islets of the pancreas.",
   "स्वादुपिंडातील बेटांमधील बीटा पेशी इन्शुलिन तयार करतात."]],

// question is bilingual, but the options are numbers — single strings
["Maths",
  ["What is 12 × 12?", "12 × 12 किती होतात?"],
  "144", ["124", "132", "156"],
  "12 × 12 = 144, the square of 12."],
```

A note that reads identically in both languages — a bare calculation, say — is stored as one string and rendered once, exactly like the option fields.

Write the note to earn its place. For reasoning, that means the **rule**, so the next question of that type becomes solvable; for recall subjects, a date, a cause or a nearby confusable that fixes the fact in place. Restating the answer in a longer sentence adds nothing.

Because plain strings are still accepted everywhere, the older English-only format keeps working unchanged — that is also how live API questions render.

Question text accepts inline HTML, so book and artwork titles can be italicised with `<em>`. Option order is randomised at runtime — do not try to control the position of the correct answer.

### Diagram questions

Because both question text and option text are rendered as HTML, a figure is just inline SVG wrapped in `<span class="fig">`. Put it at the **start of the English string** so the figure sits above the question and the two language lines stay together:

```js
["Reasoning",
  ["<span class='fig'><svg viewBox='0 0 60 60' …>…</svg></span>How many triangles are in the figure?",
   "दिलेल्या आकृतीत एकूण किती त्रिकोण आहेत?"],
  "8", ["4", "6", "10"]],
```

Two rules keep figures consistent with the rest of the page:

- **Stroke with `currentColor`, never a fixed colour.** That is what makes a figure work in both themes and inherit the graded red/green.
- **Add `class="fig small"`** for a single 60 × 60 figure; plain `fig` is sized for a wide series strip. Inside an option, `.fig` is capped narrower automatically.

An option that is itself a shape is a plain single string holding the SVG — there is nothing to translate, so it renders once:

```js
"<span class='fig small'><svg viewBox='0 0 60 60' …>…</svg></span>"
```

The bundled diagram questions were produced by a small generator script rather than written by hand, which is worth doing if you add many: rotations, polygon vertices and mirror transforms are easy to get subtly wrong in hand-written SVG.

## Notes

- The whole app is one file: markup, CSS custom properties for both themes, and the logic in a single IIFE.
- Diagrams are inline SVG markup, not images, so they survive a strict Content Security Policy and work offline like the rest of the page.
- If you deploy this behind a strict Content Security Policy, the outbound API call is blocked and the page silently falls back to the local bank. The source label in the footer tells you which one is active.
