---
source_sha: 59b143405eb1
id: L3-008
title: L3-008
form: concept
visibility: public
spec_refs: ["§2.1（二層構造）", "§3.1（premise）"]
spec_head: v1.1.0
verified: spec-line
difficulty: 1
---

## Question

Choose the one correct statement about the two layers of Kairos.

- (a) The premise layer weaves the schedule, and the body layer defines the calendar system
- (b) **The premise layer supplies premises such as the calendar system and calendars, and the body layer weaves the schedule with that vocabulary**
- (c) The two layers are syntactic decoration, and the same thing can be written in either layer
- (d) The body layer is evaluated before the premise layer

## Answer

**(b)**

## Explanation

The premise layer = supplying the premises (DDL-like · declarative · multi-line): it lists the conditions under which the interpretation of an expression holds —
the calendar system, calendars, tz, wkst and so on (the same shape as the preamble of a contract · §3.1). The body layer = weaving the schedule with the supplied
vocabulary (DML-like · pipes · one-line oriented). Just as the meaning of the same expression `everyDay |> within(week) |> first` changes with wkst
(L3-004), **the meaning of an expression is relative to the premises** — this separation is the backbone of the design of Kairos (§2.1).
