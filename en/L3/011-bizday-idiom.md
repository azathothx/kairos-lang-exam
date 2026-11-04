---
source_sha: 28799e53bd57
id: L3-011
title: L3-011
form: output-prediction
visibility: public
spec_refs: ["§4.5（結合子）", "reference/combinators.md"]
spec_head: v1.1.0
verified: impl
difficulty: 2
---

## Question

The business-day idiom. Give the firing points when this is materialized over the evaluation range 2026-08-10 (Mon)..2026-08-17
(2026-08-11 is a Tuesday · Mountain Day).

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays = [2026-08-11] covering: 2026-08-01..2026-09-01
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
everyDay \ (satSun | holidays)
```

## Answer

2026-08-10, 2026-08-12, 2026-08-13, 2026-08-14

## Explanation

Read **from the inside out**: `satSun | holidays` builds "the union of weekends and holidays", which is subtracted from `everyDay`.
Of the Mon–Fri days in the evaluation range, only Mountain Day (8/11 Tue) is in the union and drops out. Saturday 8/15 and Sunday 8/16 drop out
on the satSun side. This `everyDay \ (satSun | holidays)` is the basic form of business-day derivation, and in practice it is
declared as the `nonWorking` of a calendar entity (§3.9 — at Grade 3 it is enough to be able to read the form).
Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-08-10..2026-08-17
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays = [2026-08-11] covering: 2026-08-01..2026-09-01
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
everyDay \ (satSun | holidays)
```

```text
2026-08-10
2026-08-12
2026-08-13
2026-08-14
# 被覆サマリ
#   holidays covering 2026-08-01..2026-09-01 残走路 16 日
```
