---
id: L3-001
form: output-prediction
visibility: public
spec_refs: ["§4.3（窓と選択子）", "reference/within.md", "reference/first.md"]
spec_head: 46a7730
verified: impl
---

## 問題

次の Kairos 定義を評価範囲 2026-01-01..2026-04-01 で実体化したとき、発火点列を答えよ。

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> within(month) |> first
```

## 正答

```text
2026-01-01
2026-02-01
2026-03-01
```

（2026-04-01 は含まれない——評価範囲 `..2026-04-01` は右開区間）

## 解説

`everyDay` が暦法純粋な日刻みを生成し、`within(month)` が月窓でパーティション化、`first` が
各窓の先頭点を選ぶ（窓相対＝I4）。よって各月の 1 日が発火する。評価範囲は右開なので
4 月分の窓は範囲に入らず、3 点で止まる。impl 実走で検証済み（2026-07-28・公開 HEAD 46a7730）。

## 検証

```kairos
# eval: 2026-01-01..2026-04-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> within(month) |> first
```

```text
2026-01-01
2026-02-01
2026-03-01
```
