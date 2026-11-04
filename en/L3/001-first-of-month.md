---
source_sha: 7cf70acc3bd7
id: L3-001
title: L3-001
form: output-prediction
visibility: public
spec_refs: ["§4.2（窓）", "§4.3（選択子）", "reference/within.md", "reference/first.md"]
spec_head: v1.1.0
verified: impl
difficulty: 1
---

## Question

Give the firing points when the following Kairos definition is materialized over the evaluation range 2026-01-01..2026-04-01.

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> within(month) |> first
```

## Answer

```text
2026-01-01
2026-02-01
2026-03-01
```

(2026-04-01 is not included — the evaluation range `..2026-04-01` is a half-open interval)

## Explanation

`everyDay` generates the calendar-system-pure day ticks, `within(month)` partitions by month windows, and `first` selects
the first point of each window (window-relative = I4). So the 1st of each month fires. The evaluation range is half-open, so
the window for April is outside the range and it stops at 3 points. Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-01-01..2026-04-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> within(month) |> first
```

```text
2026-01-01
2026-02-01
2026-03-01
```
