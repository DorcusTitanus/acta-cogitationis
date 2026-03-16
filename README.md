# Acta Cogitationis

A longitudinal dataset tracking cognitive patterns — word retrieval, working memory, pattern recognition — maintained as a personal record and potential public dataset.

## How to log

Each entry is a short structured block appended to the current year's log file (`entries/YYYY.md`). Low friction is the point. An entry can be three lines.

## Entry format

```
## YYYY-MM-DD | HH:MM | category

**Context:** (energy level, sleep, stress, time of day — whatever feels relevant)

What happened.

**Resolution:** (did the word come back? how? still missing?)
```

## Categories

- `retrieval` — word on the tip of your tongue, name you can't place
- `working-memory` — lost the thread, forgot why you walked into a room, dropped an item from a mental list
- `pattern` — typo you missed, metaphor that didn't land, joke timing off
- `orientation` — blanked on a date, sequence, or routine step
- `positive` — sharp day, fast recall, flow state, connections firing

The `positive` category matters. This isn't a deficit log. It's a full picture.

## Structure

```
acta-cogitationis/
  README.md
  entries/
    2026.md
  analysis/       ← generated summaries, frequency counts, charts
```

## Privacy note

This repo is private until the author decides otherwise.
