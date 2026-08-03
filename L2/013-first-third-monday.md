---
id: L2-013
form: composition
visibility: public
spec_refs: ["§4.5（結合子）", "§4.3（nth）", "§3.5（束縛）", "§4.8（糖衣定義）"]
spec_head: 44493ad
verified: impl
---

## 問題

「**毎月第 1・第 3 月曜**」（ごみ収集日の定番形）の本体式を書け（前提は wkst: Mon の @JP
のみ・データ不要）。

## 正答

```kairos
mondays = everyDay |> filter(d => weekday(d) == Mon)
(mondays |> within(month) |> nth(1)) | (mondays |> within(month) |> nth(3))
```

**判定基準**: 評価範囲 2026-09-01..2026-11-01 で 2026-09-07, 2026-09-21, 2026-10-05,
2026-10-19（別解可・束縛を使わない一本書きも可）。

## 解説

`nth` は序数を **1 つ**しか取らない（複数序数は言語の宿題＝F11）ので、「第 1 と第 3」は
**和 `|` で合成**するのが現行の正準形。月曜の列を束縛（`mondays = …`）してから 2 回使うと
重複が消えて読みやすい——束縛は式の整理にいつでも使ってよい（§3.5・§4.8）。第 2・第 4 型
（隔週集金など）も同型で書ける。impl 実走検証済み（2026-08-01・公開 HEAD 9e46dcf）。

## 検証

```kairos
# eval: 2026-09-01..2026-11-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
mondays = everyDay |> filter(d => weekday(d) == Mon)
(mondays |> within(month) |> nth(1)) | (mondays |> within(month) |> nth(3))
```

```text
2026-09-07
2026-09-21
2026-10-05
2026-10-19
```
