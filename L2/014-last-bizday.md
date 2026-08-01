---
id: L2-014
form: composition
visibility: public
spec_refs: ["§4.4（roll）", "§7.1"]
spec_head: 9e46dcf
verified: impl
---

## 問題

次の前提の下で、「**毎月の最終営業日**」の本体式を書け。

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
monthEnd |> roll(Preceding, on: bizDay)
```

**判定基準**: 評価範囲 2026-08-01..2026-12-01 で 2026-08-31, 2026-09-30, 2026-10-30,
2026-11-30（別解可——`bizDay |> within(month) |> last` も同一出力）。

## 解説

月末日を営業日軸へ Preceding（手前）に寄せる——月末が営業日ならそのまま（8/31 月・9/30 水・
11/30 月）、土日祝なら手前へ（10/31 土→10/30 金）。roll 形は spec §7.1「月末の 3 営業日前」の
前段そのもので、shift を足せばそのまま §7.1 になる。within＋last の別解は「月内の営業日の
最後」という同じ集合の選択子読み。impl 実走検証済み（2026-08-01・公開 HEAD 9e46dcf）。

## 検証

```kairos
# eval: 2026-08-01..2026-12-01
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

monthEnd |> roll(Preceding, on: bizDay)
```

```text
2026-08-31
2026-09-30
2026-10-30
2026-11-30
# 被覆サマリ
#   holidays2026 covering 2026-01-01..2026-12-31 残走路 31 日
```
