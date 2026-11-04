---
source_sha: 2a5f2ffa077e
id: L1-011
title: L1-011
form: output-prediction
visibility: public
spec_refs: ["§4.10（評価註釈・輸送表）", "ADR-37 判断 4"]
spec_head: v1.1.0
verified: impl
difficulty: 3
---

## Question

We want a renewal check to fire 90 days after each contract date. The contract data has been obtained for 2026 and 2028 only;
**2027 has not been obtained** (there is a gap in the middle of the covering interval list). What comes out when this is materialized over the evaluation range
2028-01-01..2028-06-01?

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
dues = [2026-03-01, 2026-09-01] covering: 2026..2026, 2028..2028
dues |> shift(90, unit: day)
```

- (a) Zero points · no annotation (there are simply no 2028 contracts)
- (b) **Zero points plus an out-of-coverage annotation [2028-01-01, 2028-03-31)**
- (c) The missing 2027 is a static error
- (d) 90 days after the 2026 contracts (May and November 2026) come out

## Answer

**(b)**.

## Explanation

**Annotations are also transported by operators** (the transport table of ADR-37 decision 4). The gap in covering
[2027-01-01, 2028-01-01) is the claim "the contracts in this interval are **unknown**", and shift(90, unit: day) carries
that unknown interval too **as its image translated by +90 days** — of the image [2027-04-01, 2028-03-31), the part overlapping the evaluation range,
[2028-01-01, 2028-03-31), runs alongside as an annotation. The meaning is "**firings in January–March 2028 might have come from contracts in the
second half of 2027 that have not been obtained**". That no points come out, and that the reason none come out can be trusted, are
different things — the latter is what the annotation carries. (d) does not come out because it is outside the evaluation range (the image of 2026 stays within 2026).
Verified by running impl (the version is the spec_head at the top).

## Verification

```kairos
# eval: 2028-01-01..2028-06-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
dues = [2026-03-01, 2026-09-01] covering: 2026..2026, 2028..2028
dues |> shift(90, unit: day)
```

```text
# 0 点（[2028-01-01, 2028-06-01) に該当なし）
# ⚠ 範囲外 2028-01-01..2028-03-31（dues covering 2026-01-01..2026-12-31, 2028-01-01..2028-12-31）
# 被覆サマリ
#   dues covering 2026-01-01..2026-12-31, 2028-01-01..2028-12-31 残走路 214 日
```
