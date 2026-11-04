---
source_sha: c91146c69ee1
id: L1-012
title: L1-012
form: output-prediction
visibility: public
spec_refs: ["§7.8（消費ループ・決定性）", "ADR-31（from: 必須）", "reference/stride.md"]
spec_head: v1.1.0
verified: impl
difficulty: 2
---

## Question

The firing layer materializes a definition of every other Monday repeatedly on a rolling horizon, **sliding the evaluation window**.

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> filter(d => weekday(d) == Mon) |> stride(2, from: 2026-01-05)
```

When window A = 2026-01-01..2026-02-01 and window B = 2026-01-15..2026-03-01 are materialized separately,
what happens to **the points in the overlap [2026-01-15, 2026-02-01)**?

- (a) They can differ by window (because the counting of stride depends on the start of the window)
- (b) **They match in both windows**
- (c) In window B the biweekly phase is inverted
- (d) The points in the overlap appear only in window B

## Answer

**(b)**. Actual output = window A: 2026-01-05, 2026-01-19 / window B: 2026-01-19, 2026-02-02, 2026-02-16 —
the point in the overlap is **2026-01-19 only** in both, and they match.

## Explanation

An expression is "the definition of an infinite list of instants", and the evaluation window is only a **cut** — the point sequence in the overlapping
range matches even when the window moves (**determinism**. The requirement for the consumption loop of §7.8 to hold). What supports this is that the phase of stride is
**held by `from:`**, not by the evaluation window (a consequence of ADR-31 making from: required). If the counting depended on the start of the
window, the biweekly phase would wobble every time the firing layer evaluated — (a) is the shape of "a definition unusable on a rolling
horizon", and Kairos shuts that shape out at the level of the language specification.
Verified by running impl (the version is the spec_head at the top).

## Verification

Window A:

```kairos
# eval: 2026-01-01..2026-02-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> filter(d => weekday(d) == Mon) |> stride(2, from: 2026-01-05)
```

```text
2026-01-05
2026-01-19
```

Window B (the point in the overlap [2026-01-15, 2026-02-01) = 2026-01-19 matches window A):

```kairos
# eval: 2026-01-15..2026-03-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> filter(d => weekday(d) == Mon) |> stride(2, from: 2026-01-05)
```

```text
2026-01-19
2026-02-02
2026-02-16
```
