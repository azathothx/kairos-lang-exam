# 3 級（読解）サンプル問題 / Grade 3 sample questions

[Kairos 言語検定](../README.md)の 3 級——**式が読める**（Kairos 式を受け取って発火点列・意味を確認する）——の
サンプル問題。教本は [3 級教本 — 読解](../textbook/L3.md)（各章末の「演習」がここの問題）。

- [L3-001 各月の先頭日（出力予測）](001-first-of-month.md)——教本 第 3 章
- [L3-004 週窓と wkst（出力予測）](004-week-wkst.md)——第 4 章
- [L3-007 日付リテラルの字句（概念）](007-date-lexis.md)——第 2 章
- [L3-008 二層構造（概念）](008-two-layers.md)——第 1 章
- [L3-011 営業日の定番形（出力予測）](011-bizday-idiom.md)——第 5 章
- [L3-013 和の重複排除（出力予測）](013-union-dedup.md)——第 5 章

形式: **出力予測**＝式と評価範囲から発火点列を答える（末尾の「検証」節を[リファレンス実装](https://github.com/azathothx/kairos-lang)か
[Playground](https://kairos-lang.org/playground/) で再現できる）・**概念**＝言語の構造や字句の理解を問う（仕様の該当行が根拠）。
各問題は言語仕様の §参照つき——正本は常に[言語仕様](https://kairos-lang.org/spec/)。

CLI で再現するときは `npx kairos-lang list --from a --to b --tz Asia/Tokyo 定義.kairos`（`# eval: a..b` の a・b を入れる。`--tz` は必ず付ける＝CLI の既定は機械の時間帯で、違うと日付が前日の時刻付きで出る）。

← [検定トップ](../README.md)・教本 [3 級](../textbook/L3.md)・[2 級](../textbook/L2.md)・[1 級](../textbook/L1.md)
