---
source_sha: 0dae404d077b
id: L2-011
title: L2-011
form: composition
visibility: public
spec_refs: ["§4.6（フィルタ）", "§4.9（暦座標糖衣 dayNo）"]
spec_head: v1.1.0
verified: impl
difficulty: 2
---

## Question

Write the body expression for "**gotōbi** (五十日) = the 5th, 10th, 15th, 20th, 25th and 30th of every month" (the premises are only @JP with wkst: Mon ·
no data needed). A form that does not line up nth six times is preferred.

## Answer

```kairos
everyDay |> filter(d => dayNo(d) mod 5 == 0)
```

**Grading criterion**: over the evaluation range 2026-09-01..2026-10-01, 2026-09-05, 2026-09-10, 2026-09-15,
2026-09-20, 2026-09-25, 2026-09-30 (alternative answers accepted — the 6-way union `nth(5) | nth(10) | …` gives the same output.
Including the 31st is wrong).

## Explanation

"Days that are multiples of 5" is done in one stroke with the remainder of the calendar-coordinate sugar `dayNo(d)` (the day number within the month the point belongs to) — shorter than a union of
enumerations, and the intent (days divisible by 5) becomes the expression as is. The 31st drops out naturally because `31 mod 5 == 1`.
Gotōbi are customary days for settlement and collection, and the variant combined with a business-day roll (gotōbi that are business days) is a standard
form in practice. Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-09-01..2026-10-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> filter(d => dayNo(d) mod 5 == 0)
```

```text
2026-09-05
2026-09-10
2026-09-15
2026-09-20
2026-09-25
2026-09-30
```
