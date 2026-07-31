---
id: L3-013
form: output-prediction
visibility: public
spec_refs: ["§4.5（結合子）", "reference/combinators.md"]
spec_head: 9e46dcf
verified: impl
---

## 問題

「毎月 25 日と毎週金曜の両方に発火したい」。評価範囲 2026-09-01..2026-10-01 で実体化すると
発火点は**何点**か（2026-09-25 は金曜・9 月の金曜は 4/11/18/25 の 4 本）。

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
(everyDay |> within(month) |> nth(25)) | (everyDay |> filter(d => weekday(d) == Fri))
```

- (a) 5 点（25 日の 1 点＋金曜の 4 点）
- (b) **4 点**
- (c) 8 点
- (d) 1 点（両方に該当する 9/25 のみ）

## 正答

**(b)**。実出力＝2026-09-04, 2026-09-11, 2026-09-18, 2026-09-25 の 4 点。

## 解説

和 `|` は**集合の和**——同じ点が両辺に現れても 1 点に正規化される（重複排除）。9/25 は
「25 日」としても「金曜」としても該当するが、発火点としては 1 点。「両方の予定が重なった日に
2 回発火」は起きない——at-least-once の発報と組むときにこの性質が効く。単純に「1＋4＝5 点」と
数えると (a) に落ちる。impl 実走検証済み（2026-07-31・公開 HEAD 9e46dcf）。

## 検証

```kairos
# eval: 2026-09-01..2026-10-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
(everyDay |> within(month) |> nth(25)) | (everyDay |> filter(d => weekday(d) == Fri))
```

```text
2026-09-04
2026-09-11
2026-09-18
2026-09-25
```
