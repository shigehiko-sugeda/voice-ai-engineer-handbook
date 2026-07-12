# 設計レビュー練習

対応教材: [学習ロードマップ](../../docs/00_learning-roadmap.md) / [Voice AIレビュー](../../cheatsheets/voice-ai-review.md)

## レビュー対象

「音声で運転席の窓を半分開ける」機能について、次の設計案が提示されました。

```text
Wake Word
  → ASR
  → NLU: intent=OpenWindow, slot=seat:driver, amount:50
  → Dialog Manager
  → Vehicle API
  → TTS「窓を開けました」
```

追加仕様は次のとおりです。

- NLU Confidenceが0.5以上なら確認せず実行する。
- 走行中、降雨中、チャイルドロック中の扱いは未定。
- Vehicle APIへ送信できた時点で成功TTSを再生する。
- API Timeoutは5秒で、最大3回Retryする。
- ログにはASR文字列と最終結果だけを残す。
- 通話中や外部Assistant起動中の競合は未定。
- Process再起動時は進行中の要求を再送する。
- 対応車種、ソフトウェア版、設定の記録方法は未定。

## 課題

機能、品質、ログ、認証、安全性、運用の6観点でレビューしてください。

### 提出物

1. 観点ごとのレビュー質問を最低2問、合計12問以上。
2. リスクを重大・高・中・低で分類した表。
3. 最優先で修正する3項目と、その理由。
4. 修正後に必要な正常系・異常系試験。

| 優先度 | 観点 | 指摘・質問 | 想定影響 | 修正案 | 検証方法 |
|---|---|---|---|---|---|

<details>
<summary>ヒント1</summary>

「要求を送信できた」と「窓が実際に動いた」を分けてください。Retryで同じ操作が重複する可能性もあります。

</details>

<details>
<summary>ヒント2</summary>

低Confidence、走行中制限、Audio競合、状態遷移ログ、再起動後の古い要求を確認してください。

</details>

## 自己チェック

- 正常系だけでなく、拒否、Timeout、Cancel、通信断を確認したか。
- 誤操作と走行中の安全性を最優先で評価したか。
- API送信と実行完了を区別したか。
- Session ID、Request ID、State、Audio Focusログを要求したか。
- RetryのIdempotencyと再起動後の古い要求を扱ったか。
- 対象車種、ソフトウェア、設定の版を再現できるか。
