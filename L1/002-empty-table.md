---
id: L1-002
form: output-prediction
visibility: public
spec_refs: ["§3.8（空テーブル）", "ADR-45", "reference/table-literal.md"]
spec_head: 44493ad
verified: impl
---

## 問題

2027 年の祝日データが**まだ届いていない**。ある担当者がそれを次のように空テーブルで書いた。
評価範囲 2027-01-04（月）..2027-01-11 で `bizDay` を実体化すると何が起きるか
（この書き方の是非は解説で扱う）。

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

空テーブルは covering 後置があれば**合法な「正当な空」**（ADR-45＝F98）。ただし covering は
「**範囲内は完全**・範囲外は未知」の主張で、値には触れない（§3.8）——つまりこの式は
「**2027 年を完全に把握しており、祝日はゼロ**」と主張してしまっている。主張の内側なので
範囲外註釈は出ず、月〜金の 5 点が普通に発火する（＝機械挙動は (b)）。実在の祝日 2027-01-01
（元日）も**無註釈で営業日として発火**し、被覆サマリは残走路 +355 日という**偽の安心**を返す。

「まだ届いていない」の正しい boot は**観測日当日のみの最小主張**（`[] covering:
2026-08-02..2026-08-02` 級）
——評価が 2027 年を読むと範囲外註釈が並走し、残走路は**即負**＝「データを入れよ」の運用信号が
立つ（reference/table-literal の boot 作法）。空テーブルの器は「点ゼロでも、覆域は知っている
範囲だけ」が要——覆域を広げるほど主張は強くなる。「まだ無い」（正当な空）と「解決失敗」
（SupplyError）の型区別は L1-003 へ。impl 実走検証済み。

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
