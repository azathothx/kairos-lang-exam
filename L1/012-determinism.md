---
id: L1-012
form: output-prediction
visibility: public
spec_refs: ["§7.8（消費ループ・決定性）", "ADR-31（from: 必須）", "reference/stride.md"]
spec_head: d1085ee
verified: impl
---

## 問題

隔週月曜の定義を、発報層が rolling horizon で**評価窓をずらしながら**繰り返し実体化する。

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> filter(d => weekday(d) == Mon) |> stride(2, from: 2026-01-05)
```

窓 A＝2026-01-01..2026-02-01・窓 B＝2026-01-15..2026-03-01 で別々に実体化したとき、
**重なり区間 [2026-01-15, 2026-02-01) の点**はどうなるか。

- (a) 窓によってずれ得る（stride の数え起こしが窓の先頭に依存するため）
- (b) **両窓で一致する**
- (c) 窓 B では隔週の位相が反転する
- (d) 重なり区間の点は窓 B にだけ現れる

## 正答

**(b)**。実出力＝窓 A: 2026-01-05, 2026-01-19／窓 B: 2026-01-19, 2026-02-02, 2026-02-16——
重なり区間の点はどちらも **2026-01-19 のみ**で一致。

## 解説

式は「時点の無限リストの定義」であり、評価窓は**切り取り**にすぎない——窓を動かしても重なる
範囲の点列は一致する（**決定性**。§7.8 の消費ループが成り立つ要件）。stride の位相を評価窓でなく
**`from:` が持つ**（ADR-31 で from: を必須にした帰結）ことがこれを支える。もし数え起こしが窓の
先頭に依存したら、発報層が評価するたびに隔週の位相が揺れる——(a) は「rolling horizon で使えない
定義」の姿で、Kairos はその形を言語仕様の段階で締め出している。
impl 実走検証済み（2026-08-02・公開 HEAD d1085ee）。

## 検証

窓 A:

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

窓 B（重なり区間 [2026-01-15, 2026-02-01) の点＝2026-01-19 が窓 A と一致）:

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
