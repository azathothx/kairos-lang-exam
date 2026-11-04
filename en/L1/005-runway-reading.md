---
source_sha: 575c844ea248
id: L1-005
title: L1-005
form: concept
visibility: public
spec_refs: ["§4.10（被覆サマリ・残走路）", "ADR-37"]
spec_head: v1.1.0
verified: spec-line
difficulty: 1
---

## Question

The coverage summary of an evaluation result said "`holidays covering 2026-01-01..2026-12-31 残走路 -5 日`" (runway −5 days).
Choose the one correct meaning of this **−5 days**.

- (a) The data update is 5 days late (asof is 5 days old)
- (b) **The end of the evaluation range exceeds the end of covering by 5 days** — the last 5 days of the result step
  outside the range of the data
- (c) covering expires in 5 days
- (d) The data for the past 5 days is missing

## Answer

**(b)**

## Explanation

Runway (残走路) = **the end of covering − the end of the evaluation range (evaluation to)** (the definition of the coverage summary in §4.10 〔source · covering ·
asof · completeness claim · **runway from evaluation to**〕). The baseline is not "from today" but "how far the evaluation reads"
— with the same data, extending the evaluation range shrinks the runway. Positive means "covering is still sufficient";
negative means "already reading outside the range" (an out-of-coverage annotation should be running alongside). The runway warning of the firing layer
fires by comparing this value with a threshold (spec §7.8).
