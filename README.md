# Kairos 言語検定 — 教材・サンプル問題 / Kairos Language Exam — Study Materials

スケジュール定義言語 **[Kairos](https://kairos-lang.org/)** の検定プロジェクトの公開リポジトリ。
級設計に基づく教材・サンプル問題を公開する（検定本体は準備中）。

Study materials and sample questions for the **Kairos** schedule definition language
certification (the certification itself is in preparation). The
**[language specification](https://kairos-lang.org/en/spec/) is always canonical** — these
materials never redefine it.

## 級構成（案） / Grades (draft)

| 級 | 主題 | できるようになること |
|---|---|---|
| **3 級（読解）** | 式が読める | Kairos 式を受け取って発火点列・意味を確認できる |
| **2 級（作文）** | 式が書ける | 業務要件からスケジュール定義を自分で書ける |
| **1 級（運用意味論）** | 運用まで設計できる | covering・評価註釈・external・決定性まで含めた設計ができる |

## サンプル問題 / Sample questions

- 3 級: [L3-001 各月の先頭日（出力予測）](L3/001-first-of-month.md) ·
  [L3-004 週窓と wkst（出力予測）](L3/004-week-wkst.md) ·
  [L3-007 日付リテラルの字句（概念）](L3/007-date-lexis.md) ·
  [L3-008 二層構造（概念）](L3/008-two-layers.md)
- 2 級: [L2-003 毎月の第 2 金曜（作文）](L2/003-second-friday.md)
- 1 級: [L1-002 空テーブルと「正当な空」（概念）](L1/002-empty-table.md)

各問題は言語仕様の §参照つき。実行可能な問題は末尾の「検証」節がそのまま
[リファレンス実装](https://github.com/azathothx/kairos-lang)で再現できる
（`# eval:` の範囲で実行すると期待出力と一致する）。

## 正本と検証 / Canonicity

- 言語の正本: [kairos-lang.org](https://kairos-lang.org/)（仕様・リファレンス・標準ライブラリ）
- 全問題はリファレンス実装での実行検証つき（仕様改訂時は `spec_head` で追従を機械検出）

License: Apache-2.0
