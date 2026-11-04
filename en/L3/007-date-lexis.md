---
source_sha: 38cfb479e519
id: L3-007
title: L3-007
form: concept
visibility: public
spec_refs: ["§5.5（字句・日付リテラル）", "ADR-43"]
spec_head: v1.1.0
verified: spec-line
difficulty: 1
---

## Question

The date literal `2026-02-30` was written in Kairos source. Choose the one correct statement of what happens.

- (a) It is reinterpreted as 2026-03-02 (rollover)
- (b) Only that point is ignored at run time
- (c) **It is a lexical error and is rejected at parse time**
- (d) It is treated as an empty stream

## Answer

**(c)**

## Explanation

The value range of the date part is a lexical matter (§5.5 · ADR-43): the month is 01..12 and the day is only a day that exists under the month and the leap-year rule.
`2026-02-30` is rejected at the lexical layer, with no silent rollover — by guaranteeing "a written date exists",
the language lets all downstream semantics rest on that premise. For the same reason `23:59:60` (a leap second) is also
a lexical error (§5.5).
