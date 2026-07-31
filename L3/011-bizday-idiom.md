---
id: L3-011
form: output-prediction
visibility: public
spec_refs: ["§4.5（結合子）", "reference/combinators.md"]
spec_head: 9e46dcf
verified: impl
---

## 問題

営業日の定番形。評価範囲 2026-08-10（月）..2026-08-17 で実体化したときの発火点列を答えよ
（2026-08-11 は火曜・山の日）。

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays = [2026-08-11] covering: 2026-08-01..2026-09-01
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
everyDay \ (satSun | holidays)
```

## 正答

2026-08-10, 2026-08-12, 2026-08-13, 2026-08-14

## 解説

**内側から**読む: `satSun | holidays` で「土日と祝日の和」を作り、`everyDay` から差し引く。
評価範囲の月〜金のうち山の日（8/11 火）だけが和に含まれ抜ける。土曜 8/15・日曜 8/16 は
satSun 側で抜ける。この `everyDay \ (satSun | holidays)` が営業日導出の基本形で、実務では
カレンダー実体の `nonWorking` として宣言する（§3.9——3 級では形が読めれば足りる）。
impl 実走検証済み（2026-07-31・公開 HEAD 9e46dcf）。

## 検証

```kairos
# eval: 2026-08-10..2026-08-17
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays = [2026-08-11] covering: 2026-08-01..2026-09-01
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
everyDay \ (satSun | holidays)
```

```text
2026-08-10
2026-08-12
2026-08-13
2026-08-14
# 被覆サマリ
#   holidays covering 2026-08-01..2026-09-01 残走路 16 日
```
