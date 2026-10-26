# 2 級（作文）サンプル問題 / Grade 2 sample questions

[Kairos 言語検定](../README.md)の 2 級——**式が書ける**（業務要件からスケジュール定義を自分で書く）——の
サンプル問題。教本は [2 級教本 — 作文](../textbook/L2.md)（各章末の「演習」がここの問題）。

- [L2-003 毎月の第 2 金曜（作文）](003-second-friday.md)——教本 第 2 章
- [L2-009 毎月の最終金曜（作文）](009-last-friday.md)——第 2 章
- [L2-011 五十日（作文）](011-gotobi.md)——第 4 章
- [L2-013 第 1・第 3 月曜（作文）](013-first-third-monday.md)——第 5 章
- [L2-014 毎月の最終営業日（作文）](014-last-bizday.md)——第 3 章
- [L2-015 支払期日（作文）](015-payday-following.md)——第 1 章

形式: **作文**＝要件文からスケジュール定義を書く。判定は同一出力（別解可）——自分の答えと模範解の出力を
[リファレンス実装](https://github.com/azathothx/kairos-lang)か [Playground](https://kairos-lang.org/playground/) で見比べる。
各問題は言語仕様の §参照つき——正本は常に[言語仕様](https://kairos-lang.org/spec/)。

CLI で再現するときは `npx kairos-lang list --from a --to b --tz Asia/Tokyo 定義.kairos`（`# eval: a..b` の a・b を入れる。`--tz` は必ず付ける＝CLI の既定は機械の時間帯で、違うと日付が前日の時刻付きで出る）。

← [検定トップ](../README.md)・教本 [3 級](../textbook/L3.md)・[2 級](../textbook/L2.md)・[1 級](../textbook/L1.md)
