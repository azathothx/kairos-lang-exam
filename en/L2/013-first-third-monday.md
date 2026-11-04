---
source_sha: 2cad561573f3
id: L2-013
title: L2-013
form: composition
visibility: public
spec_refs: ["§4.5（結合子）", "§4.3（nth）", "§3.5（束縛）", "§4.8（糖衣定義）"]
spec_head: v1.1.0
verified: impl
difficulty: 2
---

## Question

Write the body expression for "**the first and third Monday of every month**" (the standard form of a garbage-collection day) (the premises are only @JP with wkst: Mon ·
no data needed).

## Answer

```kairos
mondays = everyDay |> filter(d => weekday(d) == Mon)
(mondays |> within(month) |> nth(1)) | (mondays |> within(month) |> nth(3))
```

**Grading criterion**: over the evaluation range 2026-09-01..2026-11-01, 2026-09-07, 2026-09-21, 2026-10-05,
2026-10-19 (alternative answers accepted · a one-line form without a binding is also accepted).

## Explanation

`nth` takes only **one** ordinal (multiple ordinals are homework for the language), so "the first and the third" is
**composed with the union `|`** as the current canonical form. Binding the sequence of Mondays (`mondays = …`) and using it twice
removes the duplication and reads better — bindings may be used to tidy an expression (§3.5 · §4.8. Except factoring out a subexpression that contains a lambda variable — the lambda variable of the caller is not visible from the right-hand side of a binding = ADR-53 · point-dependent values go in a binding with parameters). The second-and-fourth type
(biweekly collection and the like) is written in the same shape. Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2026-09-01..2026-11-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
mondays = everyDay |> filter(d => weekday(d) == Mon)
(mondays |> within(month) |> nth(1)) | (mondays |> within(month) |> nth(3))
```

```text
2026-09-07
2026-09-21
2026-10-05
2026-10-19
```
