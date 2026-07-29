---
id: L3-004
form: output-prediction
visibility: public
spec_refs: ["§3.6（week と wkst）", "reference/within.md"]
spec_head: 46a7730
verified: impl
---

## 問題

次の定義を評価範囲 2026-01-05..2026-01-26 で実体化したときの発火点列を答えよ
（2026-01-05 は月曜）。

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> within(week) |> first
```

## 正答

2026-01-05, 2026-01-12, 2026-01-19

## 解説

week 窓の切れ目は前文の `wkst: Mon` が決める——各週窓の先頭点＝月曜。wkst が Sun なら
同じ式で日曜列になる（式でなく前提が意味を変える＝premise 層の役割）。
impl 実走検証済み（2026-07-30・公開 HEAD 46a7730）。

## 検証

```kairos
# eval: 2026-01-05..2026-01-26
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> within(week) |> first
```

```text
2026-01-05
2026-01-12
2026-01-19
```
