---
source_sha: 8b421556dd4e
---

# Grade 3 (Reading) sample questions

Sample questions for Grade 3 of the [Kairos Language Exam](../README.md) — **reading expressions** (given a Kairos expression, confirm its firing points and meaning).
The textbook is [Grade 3 Textbook — Reading](../textbook/L3.md) (the "Exercise" at the end of each chapter is a question here).

- [L3-001 First day of each month (output prediction)](001-first-of-month.md) — Textbook Chapter 3
- [L3-004 Week windows and wkst (output prediction)](004-week-wkst.md) — Chapter 4
- [L3-007 Lexis of date literals (concept)](007-date-lexis.md) — Chapter 2
- [L3-008 The two layers (concept)](008-two-layers.md) — Chapter 1
- [L3-011 The business-day idiom (output prediction)](011-bizday-idiom.md) — Chapter 5
- [L3-013 Union deduplication (output prediction)](013-union-dedup.md) — Chapter 5

Forms: **output prediction** = given an expression and an evaluation range, answer the firing points (the "Verification" section at the end can be reproduced with the [reference implementation](https://github.com/azathothx/kairos-lang) or the
[Playground](https://kairos-lang.org/en/playground/)) · **concept** = tests understanding of the language's structure and lexis (the relevant specification line is the basis).
Every question cites the relevant § of the language specification — the [language specification](https://kairos-lang.org/en/spec/) is always canonical.

To reproduce with the CLI: `npx kairos-lang list --from a --to b --tz Asia/Tokyo definition.kairos` (put the a and b of `# eval: a..b`. Always pass `--tz` — the CLI defaults to the machine's time zone, and with a different one the dates come out as the previous day with a time).

← [Exam top](../README.md) · Textbooks: [Grade 3](../textbook/L3.md) · [Grade 2](../textbook/L2.md) · [Grade 1](../textbook/L1.md)
