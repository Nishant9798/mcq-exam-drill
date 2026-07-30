# MCQ Exam Drill

A single-file MCQ practice site for students, designed as an OMR answer sheet — the scannable bubble form used in real exams. Answer a bubble and it inks in immediately: **green with a ✓ if correct, red with an ✕ and a shake if wrong**, with the correct option outlined in dashed green so you learn it on the spot.

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

- **10 questions per sheet**, drawn at random, with the four options shuffled every time — so a question's correct answer moves position between attempts.
- **Instant marking.** Red for wrong, green for right, with a ✓/✕ stamp. The answer key row is revealed as soon as you commit.
- **Live readouts** for score, correct streak, and elapsed time.
- **Bubble progress strip** across the top, filling red/green as you work through the sheet.
- **Keyboard answering** — <kbd>A</kbd>–<kbd>D</kbd> or <kbd>1</kbd>–<kbd>4</kbd> to pick, <kbd>Enter</kbd> to advance.
- **Graded summary** with percentage, time taken, best streak, and a togglable answer key showing your answer against the correct one for every question.
- **Light and dark themes**, following your OS preference with a manual override.
- Respects `prefers-reduced-motion`.

## Where the questions come from

Two sources, with automatic fallback:

1. **Open Trivia DB** (`opentdb.com`) — fetched live, a few thousand questions across ~24 categories. Requires the page to have network access.
2. **A built-in bank of 236 questions** — used whenever the API is unreachable.

The bundled bank is curated for school and general-knowledge revision:

| Subject | Questions | Subject | Questions |
|---|---|---|---|
| Geography | 36 | Physics | 16 |
| Biology | 26 | Literature | 16 |
| History | 24 | Computing | 16 |
| Chemistry | 20 | Astronomy | 11 |
| Maths | 19 | Art / Music | 10 |
| Science | 17 | Sport | 9 |
| | | Language / Mythology / Economics | 15 |

At 10 questions a sheet, that is 23 sheets before any question repeats.

Two editorial rules were applied to the bank:

- **No time-sensitive facts.** No current officeholders, record holders, or population figures, so answers don't go stale.
- **No contested facts.** Questions with genuinely disputed answers are phrased to be unambiguous — hence "longest river in Africa" and "largest *hot* desert".

## Adding your own questions

Each entry in the `BANK` array is `[subject, question, correctAnswer, [threeWrongAnswers]]`:

```js
["Biology", "Which organ produces insulin?", "The pancreas",
  ["The liver", "The kidney", "The thyroid"]],
```

The question string accepts inline HTML, so book and artwork titles can be italicised with `<em>`. Option order is randomised at runtime — do not try to control the position of the correct answer.

## Notes

- The whole app is one file: markup, CSS custom properties for both themes, and the logic in a single IIFE.
- If you deploy this behind a strict Content Security Policy, the outbound API call is blocked and the page silently falls back to the local bank. The source label in the footer tells you which one is active.
