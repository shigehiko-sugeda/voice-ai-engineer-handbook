# Voice AIレビュー・チートシート

このシートは設計・不具合調査で「何を確認するか」をまとめます。処理の流れは[Voice AI全体フロー](../diagrams/voice-ai-overview.md)、詳細は[車載音声認識の全体像](../docs/01_overview.md)を参照してください。

## コンポーネント境界

| コンポーネント | 主な入力 | 主な出力 | レビューの焦点 |
|---|---|---|---|
| Wake Word / Voice Button | 音声、Button Event | Session開始要求 | 起動・抑制・重複防止 |
| Audio | マイク・再生信号 | ASR用音声 | AEC、VAD、Routing、欠落 |
| ASR | 音声 | Final Result | Endpoint、Confidence、Latency |
| NLU | ASR結果、Context | Intent / Slot | 正規化、曖昧性、Context期限 |
| Dialog Manager | NLU結果、State | 次Action | 確認、Timeout、Cancel、遅延応答 |
| Domain Service | 次Action、車両状態 | API要求 | 値域、権限、走行中制限 |
| Vehicle API | 構造化要求 | 完了、現在値、失敗理由 | 非同期性、重複、Retry |
| TTS | 応答文 | 再生音声 | 結果整合、Queue、割り込み |

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

## 関連図

- [Dialog State](../diagrams/dialog-state.md)
- [Audio FocusとArbitration](../diagrams/audio-focus-arbitration.md)
- [プロセス構成](../diagrams/process-architecture.md)
