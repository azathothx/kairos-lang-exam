---
source_sha: f2f7447cd3dc
id: L1-015
title: L1-015
form: output-prediction
visibility: public
spec_refs: ["§4.10（実効被覆域の分類器）", "ADR-37 判断 6/8", "ADR-15"]
spec_head: v1.1.0
verified: impl
difficulty: 3
---

## Question

When roll finds no landing point and the result is **empty**, Kairos distinguishes "why it is empty" by the shape of the output.
State the **difference in output** of the following two evaluations (both zero points) and the operational reading of each.
The premises are the same `@JP` as in the verification section (`premise JP` = Gregorian · `Asia/Tokyo`), and the evaluation range is 2027-01-01..2027-06-01.

【A】The covering of the axis data ends in 2026 (2027 has not been obtained):

```kairos
sekkiA = [2026-02-04, 2026-05-05] covering: 2026..2026
([2027-03-01] covering: ..) |> roll(Following, on: sekkiA)
```

【B】The covering of the axis data is open-ended (2026-01-01.. = the claim of **completeness** from then on):

```kairos
sekkiB = [2026-02-04, 2026-05-05] covering: 2026-01-01..
([2027-03-01] covering: ..) |> roll(Following, on: sekkiB)
```

## Answer

- 【A】= **empty plus an out-of-coverage annotation** (the evaluation range is outside the effective coverage of sekkiA). Reading: "the landing point is **unknown** —
  updating the data might produce a point. The supply needs updating".
- 【B】= **empty · no annotation** (legitimate empty). Reading: "it is **settled** that there is no axis point after 2026-05-05 —
  'nothing applicable' is the answer, and there is no need to wait for a data update".

## Explanation

The same "zero points" has three faces (the three branches of ADR-37 decisions 6/8): ① the **materialization horizon** (hit the edge of the computation range
to+400 days — reported by the `horizon-clip` **warning** · not the language's horizon) ·
② **outside the effective coverage** (実効被覆域 · unknown — an **out-of-coverage annotation** runs alongside) · ③ **legitimate exhaustion of complete data** (nothing applicable —
**an empty with nothing attached**. ADR-15 "distinguishing the accidental empty from the legitimate empty"). The operational branches correspond directly:
for ① revisit the design of the evaluation range · for ② update the supply · for ③ do nothing. Neither "empty, so something is wrong" nor
"empty, so normal" — **the output tells you the kind of empty** — and telling them apart is the operational semantics of Grade 1.
The first line of the output, `# 0 点（…に該当なし）`, is common to 【A】and 【B】(zero points do not stay silent); the difference shows in whether `# ⚠`
annotation lines follow.
Note also that an open end of coverage (`2026-01-01..`) is the strong claim "complete forever after" (attached carelessly,
it lies with face ③). ② and ③ are matched against actual output in the verification below · ① is a warning and so outside the verification
(in the doctest convention `#~>` covers it). Verified by running impl (the version is the spec_head at the top).

## Verification

【A】Outside the effective coverage = empty plus annotation:

```kairos
# eval: 2027-01-01..2027-06-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
sekkiA = [2026-02-04, 2026-05-05] covering: 2026..2026
([2027-03-01] covering: ..) |> roll(Following, on: sekkiA)
```

```text
# 0 点（[2027-01-01, 2027-06-01) に該当なし）
# ⚠ 範囲外 2027-01-01..2027-06-01（sekkiA covering 2026-01-01..2026-12-31）
# 被覆サマリ
#   (無名テーブル) covering ..（完結主張） 残走路 ∞
#   sekkiA covering 2026-01-01..2026-12-31 残走路 -151 日
```

【B】Legitimate exhaustion of complete data = empty without annotation:

```kairos
# eval: 2027-01-01..2027-06-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
sekkiB = [2026-02-04, 2026-05-05] covering: 2026-01-01..
([2027-03-01] covering: ..) |> roll(Following, on: sekkiB)
```

```text
# 0 点（[2027-01-01, 2027-06-01) に該当なし）
# 被覆サマリ
#   (無名テーブル) covering ..（完結主張） 残走路 ∞
#   sekkiB covering 2026-01-01..（完結主張） 残走路 ∞
```
