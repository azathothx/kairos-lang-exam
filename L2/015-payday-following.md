---
id: L2-015
form: composition
visibility: public
spec_refs: ["§4.4（roll）", "§7.4"]
spec_head: 9e46dcf
verified: impl
---

## 問題

次の前提の下で、「**毎月 25 日。ただしその日が土日または祝日なら、翌営業日に繰り下げる**」
（請求書の支払期日の定番形）の本体式を書け。

```kairos
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays2026 = [2026-01-01, 2026-01-12, 2026-02-11, 2026-02-23, 2026-03-20,
                2026-04-29, 2026-05-03, 2026-05-04, 2026-05-05, 2026-05-06,
                2026-07-20, 2026-08-11, 2026-09-21, 2026-09-22, 2026-09-23,
                2026-10-12, 2026-11-03, 2026-11-23] covering: 2026..2026
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
bizDay = everyDay \ (satSun | holidays2026)

# ここに本体式を書く
```

## 正答

```kairos
everyDay |> within(month) |> nth(25) |> roll(Following, on: bizDay)
```

**判定基準**: 評価範囲 2026-07-01..2026-11-01 で 2026-07-27, 2026-08-25, 2026-09-25,
2026-10-26（別解可）。

## 解説

給料日（L2-001）との違いは規約の**向き**だけ——支払う側に有利な前倒しは Preceding・
受け取る側の期日は Following が商慣習の型。7/25（土）→ 7/27（月）・10/25（日）→ 10/26（月）。
「どちらへ倒すかは業務の意味が決める」ので、要件文の「繰り上げ／繰り下げ」を読み落とすと
Preceding/Following を取り違える——読解と作文の境目になる一語。
impl 実走検証済み（2026-08-01・公開 HEAD 9e46dcf）。

## 検証

```kairos
# eval: 2026-07-01..2026-11-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
holidays2026 = [2026-01-01, 2026-01-12, 2026-02-11, 2026-02-23, 2026-03-20,
                2026-04-29, 2026-05-03, 2026-05-04, 2026-05-05, 2026-05-06,
                2026-07-20, 2026-08-11, 2026-09-21, 2026-09-22, 2026-09-23,
                2026-10-12, 2026-11-03, 2026-11-23] covering: 2026..2026
satSun = everyDay |> filter(d => weekday(d) == Sat or weekday(d) == Sun)
bizDay = everyDay \ (satSun | holidays2026)

everyDay |> within(month) |> nth(25) |> roll(Following, on: bizDay)
```

```text
2026-07-27
2026-08-25
2026-09-25
2026-10-26
# 被覆サマリ
#   holidays2026 covering 2026-01-01..2026-12-31 残走路 61 日
```
