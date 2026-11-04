---
source_sha: 7b322a42cd48
---

# Kairos Language Exam — Study Materials and Sample Questions

**Japanese is canonical.** This is the English mirror of the [Japanese original](../README.md); where the two differ, the Japanese text governs.

**Write the rule once; list the dates.**

Public repository of the certification project for the **[Kairos](https://kairos-lang.org/en/)** schedule definition language.
It publishes study materials and sample questions based on the grade design (the certification itself is in preparation).
The **[language specification](https://kairos-lang.org/en/spec/) is always canonical** (Japanese
originals; the English mirror follows them) — these materials never redefine it.

**Evaluator output is Japanese.** The lines the reference implementation prints — the coverage summary (`# 被覆サマリ`), warnings (`警告:`), annotations (`# ⚠`) and error messages — are canonical in Japanese, and the English pages reproduce them exactly as printed. Code blocks (including the comments inside them) and expected output are byte-identical to the Japanese original; only the prose is translated.

## Grades (draft)

| Grade | Theme | What you can do |
|---|---|---|
| **Grade 3 (Reading)** | Read expressions | Given a Kairos expression, confirm its firing points and meaning |
| **Grade 2 (Writing)** | Write expressions | Write a schedule definition yourself from business requirements |
| **Grade 1 (Operational semantics)** | Design for operation | Design including covering, evaluation annotations, external and determinism |

## Sample questions

- [Grade 3](L3/README.md): [L3-001 First day of each month (output prediction)](L3/001-first-of-month.md) ·
  [L3-004 Week windows and wkst (output prediction)](L3/004-week-wkst.md) ·
  [L3-007 Lexis of date literals (concept)](L3/007-date-lexis.md) ·
  [L3-008 The two layers (concept)](L3/008-two-layers.md) ·
  [L3-011 The business-day idiom (output prediction)](L3/011-bizday-idiom.md) ·
  [L3-013 Union deduplication (output prediction)](L3/013-union-dedup.md)
- [Grade 2](L2/README.md): [L2-003 Second Friday of each month (composition)](L2/003-second-friday.md) ·
  [L2-009 Last Friday of each month (composition)](L2/009-last-friday.md) ·
  [L2-011 Gotōbi (composition)](L2/011-gotobi.md) ·
  [L2-013 First and third Monday (composition)](L2/013-first-third-monday.md) ·
  [L2-014 Last business day of each month (composition)](L2/014-last-bizday.md) ·
  [L2-015 Payment due date (composition)](L2/015-payday-following.md)
- [Grade 1](L1/README.md): [L1-002 Empty table and the "legitimate empty" (output prediction)](L1/002-empty-table.md) ·
  [L1-003 Type distinction of SupplyError (concept)](L1/003-supply-error-types.md) ·
  [L1-005 Reading the runway (concept)](L1/005-runway-reading.md) ·
  [L1-008 Division of labor with the firing layer (concept)](L1/008-division-of-labor.md) ·
  [L1-011 Transport of annotations (output prediction)](L1/011-annotation-transport.md) ·
  [L1-012 Determinism (output prediction)](L1/012-determinism.md) ·
  [L1-015 The three kinds of empty (output prediction)](L1/015-empty-trichotomy.md)

Every question cites the relevant § of the language specification. For executable questions, the "Verification" section at the end
can be reproduced as is with the
[reference implementation](https://github.com/azathothx/kairos-lang)
(run the `# eval:` range with `--tz Asia/Tokyo` and the output matches the expected output. The CLI defaults to the machine's time zone, so always pass it outside Japan).

## Textbooks

To try things without a runtime, use the [Playground](https://kairos-lang.org/en/playground/) — every "Run it and read" example in the textbooks
has an "▶ Open in the Playground" link. The expected output in these materials is verified against the same version as the Playground (reference implementation 1.1.0, tracked 2026-11-04) —
running an example in the Playground shows the same output as printed here (when the implementation is updated, the materials follow).

- [Grade 3 Textbook — Reading](textbook/L3.md) (v1 · conforms to language 1.0 〈tracked 2026-11-04〉 · every chapter has an execution-verified "Run it and read")
- [Grade 2 Textbook — Writing](textbook/L2.md) (v1 · same)
- [Grade 1 Textbook — Operational semantics](textbook/L1.md) (v1.1 · same — with this the textbooks for the three grades are complete)

## Canonicity

- Canonical source of the language: [kairos-lang.org](https://kairos-lang.org/en/) (specification, reference, standard library)
- Executable questions (output prediction, composition) are execution-verified with the reference implementation; concept questions cite the specification line
  (`verified: spec-line`). When the specification is revised, `spec_head` detects the need to follow mechanically

License: Apache-2.0
