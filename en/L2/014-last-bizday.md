---
source_sha: 044e91442d37
id: L2-014
title: L2-014
form: composition
visibility: public
spec_refs: ["§4.4（roll）", "§7.1"]
spec_head: v1.1.0
verified: impl
difficulty: 1
---

## Question

Under the following premises, write the body expression for "**the last business day of every month**".

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays2026 = [2026-01-01, 2026-01-12, 2026-02-11, 2026-02-23, 2026-03-20,
                2026-04-29, 2026-05-03, 2026-05-04, 2026-05-05, 2026-05-06,
                2026-07-20, 2026-08-11, 2026-09-21, 2026-09-22, 2026-09-23,
                2026-10-12, 2026-11-03, 2026-11-23] covering: 2026..2026
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
bizDay = everyDay \ (satSun | holidays2026)

# ここに本体式を書く
```

## Answer

```kairos
monthEnd |> roll(Preceding, on: bizDay)
```

**Grading criterion**: over the evaluation range 2026-08-01..2026-12-01, 2026-08-31, 2026-09-30, 2026-10-30,
2026-11-30 (alternative answers accepted — `bizDay |> within(month) |> last` gives the same output).

## Explanation

Move the month-end day to the business-day axis with Preceding (backward) — if the month end is a business day it stays (8/31 Mon · 9/30 Wed ·
11/30 Mon); if it is a weekend or holiday it moves back (10/31 Sat → 10/30 Fri). The roll form is exactly the first stage of spec §7.1 "3 business days before the end of the month",
and adding shift gives §7.1 as is. The alternative with within + last is the selector reading of the same set, "the last of the business days within
the month". Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-08-01..2026-12-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays2026 = [2026-01-01, 2026-01-12, 2026-02-11, 2026-02-23, 2026-03-20,
                2026-04-29, 2026-05-03, 2026-05-04, 2026-05-05, 2026-05-06,
                2026-07-20, 2026-08-11, 2026-09-21, 2026-09-22, 2026-09-23,
                2026-10-12, 2026-11-03, 2026-11-23] covering: 2026..2026
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
bizDay = everyDay \ (satSun | holidays2026)

monthEnd |> roll(Preceding, on: bizDay)
```

```text
2026-08-31
2026-09-30
2026-10-30
2026-11-30
# 被覆サマリ
#   holidays2026 covering 2026-01-01..2026-12-31 残走路 31 日
```
