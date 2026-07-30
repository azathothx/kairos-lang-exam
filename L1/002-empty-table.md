---
id: L1-002
form: output-prediction
visibility: public
spec_refs: ["§3.8（空テーブル）", "ADR-45", "reference/table-literal.md"]
spec_head: 46a7730
verified: impl
---

## 問題

2027 年の祝日データが**まだ届いていない**状態を、次のように空テーブルで書いた。評価範囲
2027-01-04（月）..2027-01-11 で `bizDay` を実体化すると何が起きるか。

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays2027 = [] covering: 2027..2027
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
bizDay = everyDay \ (satSun | holidays2027)

bizDay
```

- (a) 空テーブルは静的エラー
- (b) **月〜金の 5 点が発火し、被覆サマリに holidays2027 の残走路が立つ（範囲外註釈は出ない）**
- (c) 全期間に範囲外註釈が付く
- (d) 祝日不明のため 1 点も発火しない

## 正答

**(b)**。実出力＝01-04〜01-08 の 5 点＋被覆サマリ（残走路 355 日）。

## 解説

空テーブルは covering 後置があれば**合法な「正当な空」**（ADR-45＝F98）——「2027 年に祝日は
ない、と主張している」のではなく「2027 年の範囲について空である（今のところ）」という被覆
主張つきの値。covering 内なので範囲外註釈は出ず、点は普通に発火する。データ未投入の告知は
註釈でなく**被覆サマリの残走路**が担う（尽きる前に更新せよという運用信号の器）。「まだ無い」
（正当な空）と「解決失敗」（SupplyError）の型区別は L1-003 へ。impl 実走検証済み（2026-07-30・
公開 HEAD 46a7730）。

## 検証

```kairos
# eval: 2027-01-04..2027-01-11
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays2027 = [] covering: 2027..2027
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
bizDay = everyDay \ (satSun | holidays2027)

bizDay
```

```text
2027-01-04
2027-01-05
2027-01-06
2027-01-07
2027-01-08
# 被覆サマリ
#   holidays2027 covering 2027-01-01..2027-12-31 残走路 355 日
```
