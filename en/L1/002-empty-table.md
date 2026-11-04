---
source_sha: f38d993cbac8
id: L1-002
title: L1-002
form: output-prediction
visibility: public
spec_refs: ["§3.8（空テーブル）", "ADR-45", "reference/table-literal.md"]
spec_head: v1.1.0
verified: impl
difficulty: 2
---

## Question

The 2027 holiday data has **not arrived yet**. Someone in charge wrote it as the following empty table.
What happens when `bizDay` is materialized over the evaluation range 2027-01-04 (Mon)..2027-01-11?
(Whether this way of writing is right is discussed in the explanation.)

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays2027 = [] covering: 2027..2027
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
bizDay = everyDay \ (satSun | holidays2027)

bizDay
```

- (a) An empty table is a static error
- (b) **The 5 points Mon–Fri fire, and the coverage summary shows the runway of holidays2027 (no out-of-coverage annotation appears)**
- (c) An out-of-coverage annotation is attached to the whole period
- (d) Not a single point fires because the holidays are unknown

## Answer

**(b)**. Actual output = the 5 points 01-04 to 01-08 plus the coverage summary (runway 355 days).

## Explanation

An empty table with a trailing covering is a **legal "legitimate empty"** (正当な空 · ADR-45). But covering is
the claim "**complete within the range** · unknown outside it", and it does not touch the values (§3.8) — so this expression
ends up claiming "**I know 2027 completely, and there are zero holidays**". Being inside the claim,
no out-of-coverage annotation appears and the 5 points Mon–Fri fire as usual (= the machine behavior is (b)). The real holiday 2027-01-01
(New Year's Day) also **fires as a business day without annotation**, and the coverage summary returns the **false reassurance** of a +355-day runway.

The correct boot for "not arrived yet" is the **minimal claim of the observation day only** (of the order of `[] covering:
2026-08-02..2026-08-02`. The example takes the writing date 2026-08-02 as the observation day)
— when the evaluation reads 2027, an out-of-coverage annotation runs alongside and the runway is **immediately negative** = the operational signal "put the data in"
stands (the boot practice in reference/table-literal). The key to the empty-table vessel is "even with zero points, the coverage is only the range you
know" — the wider the coverage, the stronger the claim. The type distinction between "not yet" (legitimate empty) and "resolution failed"
(SupplyError) is in L1-003. Verified by running impl.

## Verification

```kairos
# eval: 2027-01-04..2027-01-11
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays2027 = [] covering: 2027..2027
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
bizDay = everyDay \ (satSun | holidays2027)

bizDay
```

```text
2027-01-04
2027-01-05
2027-01-06
2027-01-07
2027-01-08
# 被覆サマリ
#   holidays2027 covering 2027-01-01..2027-12-31 残走路 355 日
```
