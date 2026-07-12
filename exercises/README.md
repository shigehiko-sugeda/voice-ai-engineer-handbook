# 演習ガイド

本文を読んだ後に、設計判断とログ解析を自分の言葉で説明するための演習です。最初から模範解答を探さず、仮説を書いてからヒントを開いてください。

## 進め方

1. 対応する章の「この章で学ぶこと」と「全体像」を確認する。
2. ヒントを開かず、最初の仮説を書く。
3. 正常系だけでなく、Timeout、キャンセル、競合、通信断を検討する。
4. 判断に必要なログを挙げる。
5. 自己チェックで観点漏れを確認する。
6. 必要ならAI家庭教師へ「不足している観点だけ」を質問する。

## 難易度

- **初級**: 発話から意味構造やContextを抽出する。
- **中級**: 状態遷移、競合制御、ログから原因を設計・分析する。
- **上級**: 不完全な設計からリスクを発見し、修正案と検証方法を示す。
- **総合**: 機能、品質、ログ、認証、安全性、運用を横断してレビューする。

## 演習一覧

### 初級

- [Intent / Slot抽出演習](beginner/01_intent-slot.md)
- [Context演習](beginner/02_context.md)

### 中級

- [Audio Focus競合演習](intermediate/01_audio-focus.md)
- [Dialog State設計演習](intermediate/02_dialog-state.md)
- [音声パイプライン・ログ解析演習](intermediate/03_pipeline-log-analysis.md)

### 上級

- [アーキテクチャレビュー演習](advanced/01_architecture-review.md)
- [認証リスク抽出演習](advanced/02_certification-risk.md)
- [LLM車両連携・安全設計レビュー演習](advanced/03_llm-safety-review.md)

### 総合

- [設計レビュー練習](review/design-review-practice.md)

## AI家庭教師への依頼例

- 「私の回答に不足している観点を1つだけ教えてください」
- 「答えを言わず、次に確認すべきログを教えてください」
- 「機能、品質、ログ、認証、安全性、運用に分類して採点してください」
