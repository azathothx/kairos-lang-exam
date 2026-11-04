---
source_sha: 8183cb1f2b4f
id: L3-013
title: L3-013
form: output-prediction
visibility: public
spec_refs: ["§4.5（結合子）", "reference/combinators.md"]
spec_head: v1.1.0
verified: impl
difficulty: 2
---

## Question

"We want to fire on both the 25th of every month and every Friday." When this is materialized over the evaluation range 2026-09-01..2026-10-01,
**how many** firing points are there? (2026-09-25 is a Friday · the Fridays in September are the 4th, 11th, 18th and 25th — four of them.)

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
(everyDay |> within(month) |> nth(25)) | (everyDay |> filter(d => weekday(d) == Fri))
```

- (a) 5 points (1 point for the 25th plus 4 points for the Fridays)
- (b) **4 points**
- (c) 8 points
- (d) 1 point (only 9/25, which satisfies both)

## Answer

**(b)**. Actual output = the 4 points 2026-09-04, 2026-09-11, 2026-09-18, 2026-09-25.

## Explanation

Union `|` is **set union** — even when the same point appears on both sides, it is normalized to one point (deduplication). 9/25
qualifies both as "the 25th" and as "a Friday", but as a firing point it is one point. "Firing twice on a day where both schedules
coincide" does not happen — this property matters when combined with at-least-once dispatch. Counting naively as "1 + 4 = 5 points"
leads to (a). Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-09-01..2026-10-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
(everyDay |> within(month) |> nth(25)) | (everyDay |> filter(d => weekday(d) == Fri))
```

```text
2026-09-04
2026-09-11
2026-09-18
2026-09-25
```
