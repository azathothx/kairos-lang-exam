---
id: L2-003
form: composition
visibility: public
spec_refs: ["§4.6（filter）", "§4.3（nth）", "§7.2"]
spec_head: 46a7730
verified: impl
---

## 問題

「**毎月の第 2 金曜**」の本体式を書け（前提は wkst: Mon の @JP のみ・データ不要）。

## 正答

```kairos
everyDay |> filter(d => weekday(d) == Fri) |> within(month) |> nth(2)
```

**判定基準**: 評価範囲 2026-01-01..2026-04-01 で 2026-01-09, 2026-02-13, 2026-03-13（別解可）。

## 解説

金曜だけに濾してから月窓で第 2 要素を選ぶ——「窓の中で数える対象」を filter が先に絞るのが要点
（within |> nth(2) を先にすると「各月の 2 日」になってしまう）。impl 実走検証済み（2026-07-30・
公開 HEAD 46a7730）。

## 検証

```kairos
# eval: 2026-01-01..2026-04-01
premise JP {
  calendar-system: Gregorian
  tz: "Asia/Tokyo"
  wkst: Mon
}

@JP
everyDay |> filter(d => weekday(d) == Fri) |> within(month) |> nth(2)
```

```text
2026-01-09
2026-02-13
2026-03-13
```
