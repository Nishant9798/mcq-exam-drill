# MCQ Exam Drill

A single-file **bilingual (English + मराठी)** MCQ practice site with a **737-question bank** aimed at Maharashtra competitive exams and school revision, designed as an OMR answer sheet — the scannable bubble form used in real exams. Answer a bubble and it inks in immediately: **green with a ✓ if correct, red with an ✕ and a shake if wrong**, with the correct option outlined in dashed green so you learn it on the spot.

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

- **Bilingual throughout** — all 737 questions, their options, the verdicts, the answer key and the page chrome carry Marathi.
- **10 questions per sheet**, drawn at random, with the four options shuffled every time — so a question's correct answer moves position between attempts.
- **Instant marking.** Red for wrong, green for right, with a ✓/✕ stamp. The answer key row is revealed as soon as you commit.
- **Live readouts** for score, correct streak, and elapsed time.
- **Bubble progress strip** across the top, filling red/green as you work through the sheet.
- **Keyboard answering** — <kbd>A</kbd>–<kbd>D</kbd> or <kbd>1</kbd>–<kbd>4</kbd> to pick, <kbd>Enter</kbd> to advance.
- **Graded summary** with percentage, time taken, best streak, and a togglable answer key showing your answer against the correct one for every question.
- **Light and dark themes**, following your OS preference with a manual override.
- Respects `prefers-reduced-motion`.

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

**The bundled bilingual bank of 737 questions is the default source.**

There is also optional support for **Open Trivia DB** (`opentdb.com`), which offers a few thousand questions across ~24 categories — but it is **English-only**, and no Marathi trivia API exists. Because bilingual sheets are the point, the live feed is off by default. To enable it, flip one constant near the top of the script:

```js
const USE_LIVE_API = true;   // draws English-only questions instead
```

With it on, the API is tried first and the bilingual bank serves as the fallback if the request fails.

The bank is aimed at Maharashtra competitive exams (MPSC, Talathi, police bharti) as well as general school revision, across 25 subjects:

| Subject | Q | Subject | Q | Subject | Q |
|---|---|---|---|---|---|
| Indian polity | 71 | Chemistry | 40 | Science | 17 |
| Maharashtra history | 60 | Marathi literature | 40 | Literature | 16 |
| Indian history | 60 | Geography (world) | 36 | Computing | 16 |
| Maharashtra geography | 50 | Economics | 35 | Astronomy | 11 |
| Indian geography | 50 | Physics | 34 | Sport | 9 |
| Static GK | 50 | Maharashtra polity | 30 | Art / Music | 10 |
| Biology | 48 | History (world) | 24 | Language / Mythology | 10 |
| | | Maths | 19 | General | 1 |

At 10 questions a sheet, that is **73 sheets before any question repeats**.

Maharashtra-focused coverage includes the Sahyadri and state rivers, districts and divisions, Shivaji Maharaj and the Marathas, the Peshwas, the social reformers (Phule, Ambedkar, Shahu Maharaj, Karve, Agarkar), the Samyukta Maharashtra movement, state administration from talathi to collector, the sant tradition and Marathi literature, and Marathi grammar and dialects.

## Adding your own questions

Each entry in the `BANK` array is `[subject, question, correctAnswer, [threeWrongAnswers]]`. Any of those text fields may be either a **plain string** (rendered once, for anything identical in both languages) or an **`[english, marathi]` pair** (rendered as two stacked lines):

```js
// bilingual — question and every option differ between languages
["Biology",
  ["Which organ produces insulin?", "इन्शुलिन कोणता अवयव तयार करतो?"],
  ["The pancreas", "स्वादुपिंड"],
  [["The liver", "यकृत"], ["The kidney", "मूत्रपिंड"], ["The thyroid", "थायरॉइड ग्रंथी"]]],

// question is bilingual, but the options are numbers — single strings
["Maths",
  ["What is 12 × 12?", "12 × 12 किती होतात?"],
  "144", ["124", "132", "156"]],
```

Because plain strings are still accepted everywhere, the older English-only format keeps working unchanged — that is also how live API questions render.

Question text accepts inline HTML, so book and artwork titles can be italicised with `<em>`. Option order is randomised at runtime — do not try to control the position of the correct answer.

## Notes

- The whole app is one file: markup, CSS custom properties for both themes, and the logic in a single IIFE.
- If you deploy this behind a strict Content Security Policy, the outbound API call is blocked and the page silently falls back to the local bank. The source label in the footer tells you which one is active.
