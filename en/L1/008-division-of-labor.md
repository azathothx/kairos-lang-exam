---
source_sha: 465b78084e18
id: L1-008
title: L1-008
form: concept
visibility: public
spec_refs: ["§7.7", "§7.8（発報層との分業）"]
spec_head: v1.1.0
verified: spec-line
difficulty: 1
---

## Question

A dispatch system that embeds Kairos stopped and restarted 30 minutes later. Regarding the handling of firings whose scheduled time passed while it was stopped,
choose the one correct statement of **what the language specification defines**.

- (a) The language detects the missed fire and automatically fires it late
- (b) Firings during the stop are lost under the language specification
- (c) **The language guarantees up to the deterministic enumeration of the point sequence (the same definition · the same data · the same range gives the same result);
  detecting what was missed, delayed execution and idempotence are the responsibility of the firing layer**
- (d) After the restart the evaluation is recomputed from the stop time, so the result changes

## Answer

**(c)**

## Explanation

The division-of-labor loop of spec §7.7/§7.8: the language = providing the **deterministic enumeration** "these are the firing points in this range"
(the conditions of determinism are the same definition · the same evaluation range · **the same data** — asof carries it, and external is
relative to the same snapshot. A result that changes when the data is updated is not a break of determinism but an update of the supply.
Re-materializing leaves the schedule unchanged — so the firing layer can detect missed fires **by computation**, by matching the sequences before and after the stop).
Executing the firings, at-least-once, idempotence and delay policy are the design domain of the firing layer. (d) is wrong
because of determinism itself — evaluation does not depend on the execution time (forms relative to the execution origin are also deterministic as a computation from "the injected instant" · §7.7).
