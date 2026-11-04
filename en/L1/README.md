---
source_sha: ac0439969c3e
---

# Grade 1 (Operational semantics) sample questions

Sample questions for Grade 1 of the [Kairos Language Exam](../README.md) — **designing for operation** (covering, evaluation annotations, external, determinism).
The textbook is [Grade 1 Textbook — Operational semantics](../textbook/L1.md) (the "Exercise" at the end of each chapter is a question here).

- [L1-002 Empty table and the "legitimate empty" (output prediction)](002-empty-table.md) — Textbook Chapter 1
- [L1-003 Type distinction of SupplyError (concept)](003-supply-error-types.md) — Chapter 5
- [L1-005 Reading the runway (concept)](005-runway-reading.md) — Chapter 3
- [L1-008 Division of labor with the firing layer (concept)](008-division-of-labor.md) — Chapter 4
- [L1-011 Transport of annotations (output prediction)](011-annotation-transport.md) — Chapter 3
- [L1-012 Determinism (output prediction)](012-determinism.md) — Chapter 4
- [L1-015 The three kinds of empty (output prediction)](015-empty-trichotomy.md) — Chapter 2

Forms: **output prediction** = given an expression and an evaluation range, answer the output (the "Verification" section at the end can be reproduced with the [reference implementation](https://github.com/azathothx/kairos-lang) or the
[Playground](https://kairos-lang.org/en/playground/)) · **concept** = tests understanding of the specification (the relevant specification line is the basis).
Every question cites the relevant § of the language specification — the [language specification](https://kairos-lang.org/en/spec/) is always canonical.

To reproduce with the CLI: `npx kairos-lang list --from a --to b --tz Asia/Tokyo definition.kairos` (put the a and b of `# eval: a..b`. Always pass `--tz` — the CLI defaults to the machine's time zone, and with a different one the dates come out as the previous day with a time).

← [Exam top](../README.md) · Textbooks: [Grade 3](../textbook/L3.md) · [Grade 2](../textbook/L2.md) · [Grade 1](../textbook/L1.md)
