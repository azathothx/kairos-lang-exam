---
source_sha: 51909927d614
id: L3-004
title: L3-004
form: output-prediction
visibility: public
spec_refs: ["§3.6（week と wkst）", "reference/within.md"]
spec_head: v1.1.0
verified: impl
difficulty: 2
---

## Question

Give the firing points when the following definition is materialized over the evaluation range 2026-01-05..2026-01-26
(2026-01-05 is a Monday).

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> within(week) |> first
```

## Answer

2026-01-05, 2026-01-12, 2026-01-19

## Explanation

The cut of the week window is decided by `wkst: Mon` in the preamble — the first point of each week window = Monday. If wkst were Sun,
the same expression would give a sequence of Sundays (the premises, not the expression, change the meaning = the role of the premise layer).
Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-01-05..2026-01-26
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> within(week) |> first
```

```text
2026-01-05
2026-01-12
2026-01-19
```
