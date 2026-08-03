# Kairos 言語検定 — 教材・サンプル問題 / Kairos Language Exam — Study Materials

スケジュール定義言語 **[Kairos](https://kairos-lang.org/)** の検定プロジェクトの公開リポジトリ。
級設計に基づく教材・サンプル問題を公開する（検定本体は準備中）。

Study materials and sample questions for the **Kairos** schedule definition language
certification (the certification itself is in preparation). The
**[language specification](https://kairos-lang.org/spec/) is always canonical** (Japanese
originals; the [English mirror](https://kairos-lang.org/en/spec/) follows them) — these
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
  [L3-008 二層構造（概念）](L3/008-two-layers.md) ·
  [L3-011 営業日の定番形（出力予測）](L3/011-bizday-idiom.md) ·
  [L3-013 和の重複排除（出力予測）](L3/013-union-dedup.md)
- 2 級: [L2-003 毎月の第 2 金曜（作文）](L2/003-second-friday.md) ·
  [L2-009 毎月の最終金曜（作文）](L2/009-last-friday.md) ·
  [L2-011 五十日（作文）](L2/011-gotobi.md) ·
  [L2-013 第 1・第 3 月曜（作文）](L2/013-first-third-monday.md) ·
  [L2-014 毎月の最終営業日（作文）](L2/014-last-bizday.md) ·
  [L2-015 支払期日（作文）](L2/015-payday-following.md)
- 1 級: [L1-002 空テーブルと「正当な空」（出力予測）](L1/002-empty-table.md) ·
  [L1-003 SupplyError の型区別（概念）](L1/003-supply-error-types.md) ·
  [L1-005 残走路の読み（概念）](L1/005-runway-reading.md) ·
  [L1-008 発報層との分業（概念）](L1/008-division-of-labor.md) ·
  [L1-011 註釈の輸送（出力予測）](L1/011-annotation-transport.md) ·
  [L1-012 決定性（出力予測）](L1/012-determinism.md) ·
  [L1-015 空の三分岐（出力予測）](L1/015-empty-trichotomy.md)

各問題は言語仕様の §参照つき。実行可能な問題は末尾の「検証」節がそのまま
[リファレンス実装](https://github.com/azathothx/kairos-lang)で再現できる
（`# eval:` の範囲で実行すると期待出力と一致する）。

## 教本 / Textbook

実行環境なしで試すなら [Playground](https://kairos-lang.org/playground/)——教本の「動かして読む」は
各例に「▶ Playground で開く」リンクつき。

- [3 級教本 — 読解](textbook/L3.md)（v1・言語 RC5 準拠・全章に実行検証つき「動かして読む」）
- [2 級教本 — 作文](textbook/L2.md)（v1・同上）
- [1 級教本 — 運用意味論](textbook/L1.md)（v1・同上——三つの級の教本はこれで完結）

## 正本と検証 / Canonicity

- 言語の正本: [kairos-lang.org](https://kairos-lang.org/)（仕様・リファレンス・標準ライブラリ）
- 実行可能な問題（出力予測・作文）はリファレンス実装での実行検証つき・概念問題は仕様行根拠
  （`verified: spec-line`）。仕様改訂時は `spec_head` で追従を機械検出

License: Apache-2.0
