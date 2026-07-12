# 車載Voice AI 全体像チートシート

詳細は[車載音声認識の全体像](../docs/01_overview.md)を参照してください。

## 全体フロー

```text
Wake Word ─┐
Voice Button ─┴→ Mic取得 → ASR → NLU → Dialog Manager → Domain Service → Vehicle API
                              ↑            │                    ↓
                           Context         └→ 応答生成 → TTS → Speaker
```

Vehicle APIへの要求送信と、車両での実行完了は別です。成功・失敗を確認してから応答します。

## 各コンポーネントの責務

| コンポーネント | 入力 | 主な判断・処理 | 出力 |
|---|---|---|---|
| Wake Word / Voice Button | 音声またはボタンEvent | 起動条件、抑制条件 | Session開始要求 |
| Audio | マイク・再生信号 | AEC、VAD、Routing | ASR用音声 |
| ASR | 音声 | 文字化、Endpoint判定 | Partial / Final Result |
| NLU | ASR結果、Context | Domain、Intent、Slot抽出 | 意味構造、Confidence |
| Dialog Manager | NLU結果、State | 確認、候補選択、実行、終了 | 次Action |
| Domain Service | 次Action、車両状態 | 値域、権限、実行可否 | Vehicle API要求 |
| Vehicle API | 構造化要求 | 車両機能との連携 | 完了、現在値、失敗理由 |
| TTS | 応答文 | 合成、Queue、割り込み | 再生音声 |

## 車載固有の競合

- 競合対象: 警告音、通話、ナビ案内、音楽、Siri、Google Assistant、自社Assistant。
- 入力: マイクの所有者とMic Routingを一意にする。
- 出力: 停止、Pause、Ducking、待機、継続のどれかを定義する。
- 復帰: 割り込まれた処理を再開、再要求、破棄のどれにするか決める。
- 異常時: TimeoutやProcess停止後にMicとAudio Focusを回収する。

## 主な評価指標

| 対象 | 指標・確認項目 |
|---|---|
| Wake Word | False Wake Rate、Miss Wake Rate、起動Latency |
| ASR | WER、Endpoint誤り、Final Result Latency |
| NLU | Intent Accuracy、Slot Precision / Recall / F1 |
| 対話 | タスク成功率、再質問回数、キャンセル成功率 |
| End-to-end | p50 / p95 Latency、車両実行成功率 |
| Resource | CPU、Memory、Queue長、通信量 |

平均値だけでなく、停車・市街地・高速、座席、空調、音楽、通信状態などの条件別に確認します。

## ログの最小セット

- Correlation ID、Session ID、Request ID
- Event時刻、遷移前後のState、終了理由
- ASRのPartial / Final ResultとConfidence
- NLUのDomain、Intent、Slot、候補、Confidence
- Mic Routing、Audio Focus所有者、Grant / Deny / Release理由
- Vehicle APIの要求値、結果コード、現在値
- TTSのPrompt ID、再生開始・終了・停止理由
- ソフトウェア、モデル、設定、車両の版情報

## 設計レビュー6観点

- **機能**: 起動から終了までと、確認、キャンセル、再試行が定義されているか。
- **品質**: 精度とLatencyの目標、測定条件、合否判定があるか。
- **ログ**: コンポーネントをまたいで要求とStateを追跡できるか。
- **認証**: Voice Button、Wake Word、Mic Routing、Audio Focusの確認事項があるか。
- **安全性**: 走行中制限、誤操作、低Confidence時の確認があるか。
- **運用**: 通信断、Process停止、設定不整合から復旧・切り戻しできるか。

## 切り分けの入口

| 現象 | 最初に確認する場所 |
|---|---|
| 起動しない・勝手に起動する | Wakeスコア、抑制理由、Voice Button Event |
| 聞き間違い | 入力音声、AEC / VAD、ASR Final Result |
| 意図と違う機能が動く | ASR結果、NLU Intent / Slot、Context |
| 対話が終わらない | Dialog State、Timeout、古い非同期応答 |
| 車両が動かない | 実行条件、Vehicle API結果、状態反映 |
| 音が重なる・復帰しない | Audio Focus、Routing、Queue、解放ログ |
