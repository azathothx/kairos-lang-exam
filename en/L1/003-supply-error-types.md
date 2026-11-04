---
source_sha: 3c40703246cc
id: L1-003
title: L1-003
form: concept
visibility: public
spec_refs: ["§3.8（external・供給契約）", "ADR-45", "ADR-46"]
spec_head: v1.1.0
verified: spec-line
difficulty: 1
---

## Question

The resolver of `holidays = external(kind: dates)` failed at run time. Choose the one correct statement of
how the language specification treats this.

- (a) It is automatically reinterpreted as the empty table `[] covering: …` and evaluation continues
- (b) **A supply error (the SupplyError family) — distinguished by type from "not yet" (the empty table);
  it is not reinterpreted as empty**
- (c) If the previous resolved value is cached, it is silently used
- (d) covering is dropped and evaluation continues with the whole period unknown

## Answer

**(b)**

## Explanation

The core type distinction of ADR-45/46: **"legitimate empty"** = empty table plus covering (it is a value and evaluation continues ·
the runway is the operational signal), versus **"resolution failed"** = supply error (no value was obtained · surfaces as an error).
Reinterpreting a failure as empty turns it into the **false claim** "there are no holidays", so the language never does it
(why (a) and (d) are dangerous). Recovery strategies such as caching and degraded operation are **the responsibility of the firing layer**, not the
semantics of the language ((c) — the division of labor in spec §7.8).
