---
source_sha: 76daa57fe943
id: L2-003
title: L2-003
form: composition
visibility: public
spec_refs: ["§4.6（filter）", "§4.3（nth）", "§7.2（第 2 営業日の次の金曜＝nth の前に絞る形が同型）"]
spec_head: v1.1.0
verified: impl
difficulty: 1
---

## Question

Write the body expression for "**the second Friday of every month**" (the premises are only @JP with wkst: Mon · no data needed).

## Answer

```kairos
everyDay |> filter(d => weekday(d) == Fri) |> within(month) |> nth(2)
```

**Grading criterion**: over the evaluation range 2026-01-01..2026-04-01, 2026-01-09, 2026-02-13, 2026-03-13 (alternative answers accepted).

## Explanation

Filter to Fridays only, then select the 2nd element in the month window — the point is that filter first narrows "what is counted within the window"
(doing within |> nth(2) first gives "the 2nd of each month"). Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-01-01..2026-04-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> filter(d => weekday(d) == Fri) |> within(month) |> nth(2)
```

```text
2026-01-09
2026-02-13
2026-03-13
```
