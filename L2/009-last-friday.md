---
id: L2-009
form: composition
visibility: public
spec_refs: ["§4.4（roll）", "reference/roll.md（匿名軸）"]
spec_head: 9e46dcf
verified: impl
---

## 問題

「**毎月の最終金曜**」の本体式を書け（前提は wkst: Mon の @JP のみ・データ不要）。

## 正答

```kairos
monthEnd |> roll(Preceding, on: (everyDay |> filter(d => weekday(d) == Fri)))
```

**判定基準**: 評価範囲 2026-08-01..2026-12-01 で 2026-08-28, 2026-09-25, 2026-10-30,
2026-11-27（別解可——`everyDay |> filter(d => weekday(d) == Fri) |> within(month) |> last`
も同一出力）。

## 解説

月末日を「金曜の列」を軸に前方（Preceding）へ寄せる——roll の `on:` は名前付きの軸だけでなく
**匿名軸**（インラインのストリーム式）を受ける（reference/roll.md・保証済み＝F7）。月末日が
金曜ならそのまま（有効点は動かない）。within＋last の別解は「月窓の中の金曜の最後」で同じ
集合になる。roll 形は「月末から遡る」・within 形は「月内で選ぶ」という読み下しの違いだけ。
impl 実走検証済み（2026-07-31・公開 HEAD 9e46dcf）。

## 検証

```kairos
# eval: 2026-08-01..2026-12-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
monthEnd |> roll(Preceding, on: (everyDay |> filter(d => weekday(d) == Fri)))
```

```text
2026-08-28
2026-09-25
2026-10-30
2026-11-27
```
