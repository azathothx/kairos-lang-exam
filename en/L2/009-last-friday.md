---
source_sha: e9ce62ec61d1
id: L2-009
title: L2-009
form: composition
visibility: public
spec_refs: ["§4.4（roll）", "reference/roll.md（匿名軸）"]
spec_head: v1.1.0
verified: impl
difficulty: 2
---

## Question

Write the body expression for "**the last Friday of every month**" (the premises are only @JP with wkst: Mon · no data needed).

## Answer

```kairos
monthEnd |> roll(Preceding, on: (everyDay |> filter(d => weekday(d) == Fri)))
```

**Grading criterion**: over the evaluation range 2026-08-01..2026-12-01, 2026-08-28, 2026-09-25, 2026-10-30,
2026-11-27 (alternative answers accepted — `everyDay |> filter(d => weekday(d) == Fri) |> within(month) |> last`
gives the same output).

## Explanation

Move the month-end day backward (Preceding) along the axis of "the sequence of Fridays" — the `on:` of roll accepts not only a named axis but also
an **anonymous axis** (匿名軸 · an inline stream expression) (reference/roll.md · guaranteed). If the month-end day is
a Friday it stays (a valid point does not move). The alternative with within + last is "the last of the Fridays within the month window" and gives the same
set. The roll form reads as "go back from the end of the month" and the within form as "select within the month" — only a difference in how it is read.
Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-08-01..2026-12-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
monthEnd |> roll(Preceding, on: (everyDay |> filter(d => weekday(d) == Fri)))
```

```text
2026-08-28
2026-09-25
2026-10-30
2026-11-27
```
