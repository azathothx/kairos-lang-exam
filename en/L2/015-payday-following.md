---
source_sha: 02f60f0cee13
id: L2-015
title: L2-015
form: composition
visibility: public
spec_refs: ["§4.4（roll）", "§7.4"]
spec_head: v1.1.0
verified: impl
difficulty: 1
---

## Question

Under the following premises, write the body expression for "**the 25th of every month. But if that day is a weekend or a holiday, move it down to the following business day**"
(the standard form of an invoice payment due date).

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
everyDay |> within(month) |> nth(25) |> roll(Following, on: bizDay)
```

**Grading criterion**: over the evaluation range 2026-07-01..2026-11-01, 2026-07-27, 2026-08-25, 2026-09-25,
2026-10-26 (alternative answers accepted).

## Explanation

The only difference from the payday form written with move-up (Preceding) is the **direction** of the convention — moving up, which favors the paying side, is
Preceding; the receiving side's due date is Following, as commercial custom goes. 7/25 (Sat) → 7/27 (Mon) · 10/25 (Sun) → 10/26 (Mon).
"Which way to fall is decided by the business meaning", so missing "move up / move down" in the requirement sentence
swaps Preceding and Following — the one word on the boundary between reading and writing.
Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-07-01..2026-11-01
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

everyDay |> within(month) |> nth(25) |> roll(Following, on: bizDay)
```

```text
2026-07-27
2026-08-25
2026-09-25
2026-10-26
# 被覆サマリ
#   holidays2026 covering 2026-01-01..2026-12-31 残走路 61 日
```
