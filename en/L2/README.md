---
source_sha: 09a35195fe0b
---

# Grade 2 (Writing) sample questions

Sample questions for Grade 2 of the [Kairos Language Exam](../README.md) — **writing expressions** (write a schedule definition yourself from business requirements).
The textbook is [Grade 2 Textbook — Writing](../textbook/L2.md) (the "Exercise" at the end of each chapter is a question here).

- [L2-003 Second Friday of each month (composition)](003-second-friday.md) — Textbook Chapter 2
- [L2-009 Last Friday of each month (composition)](009-last-friday.md) — Chapter 2
- [L2-011 Gotōbi (composition)](011-gotobi.md) — Chapter 4
- [L2-013 First and third Monday (composition)](013-first-third-monday.md) — Chapter 5
- [L2-014 Last business day of each month (composition)](014-last-bizday.md) — Chapter 3
- [L2-015 Payment due date (composition)](015-payday-following.md) — Chapter 1

Form: **composition** = write a schedule definition from a requirement sentence. Grading is by identical output (alternative answers accepted) — compare the output of your answer and the model answer
with the [reference implementation](https://github.com/azathothx/kairos-lang) or the [Playground](https://kairos-lang.org/en/playground/).
Every question cites the relevant § of the language specification — the [language specification](https://kairos-lang.org/en/spec/) is always canonical.

To reproduce with the CLI: `npx kairos-lang list --from a --to b --tz Asia/Tokyo definition.kairos` (put the a and b of `# eval: a..b`. Always pass `--tz` — the CLI defaults to the machine's time zone, and with a different one the dates come out as the previous day with a time).

← [Exam top](../README.md) · Textbooks: [Grade 3](../textbook/L3.md) · [Grade 2](../textbook/L2.md) · [Grade 1](../textbook/L1.md)
