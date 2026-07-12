# アーキテクチャ

## この章で学ぶこと

- Pipeline、Component、Clean Architecture
- IPC、Thread、Queue、Actor pattern、State machine
- Logging、Watchdog、Retry、Fallback、Recovery

## なぜ必要なのか

音声Pipelineは複数の非同期処理と外部サービスをまたぎます。責務や所有権が曖昧だと、停止、遅延、重複、順序逆転が連鎖します。障害を閉じ込め、状態を観測し、安全に復旧できる構成が必要です。

## 全体像

```text
HMI ─┐
Audio ├─ IPC / Message Queue ─ Voice Assistant ─ Vehicle Integration
Nav ──┘                         │
                         State machine・Domain rules
```

プロセス境界は障害分離、更新単位、権限、性能を基に決めます。論理的なPipelineと実際のThread・Process構成を混同しません。

## 基本用語

- **IPC**: プロセス間で要求やEventを交換する仕組み。
- **Queue**: 処理待ちを保持し、順序と負荷を調整する構造。
- **Backpressure**: 処理能力を超える入力を抑制する仕組み。
- **Watchdog**: 停止や無応答を検知する監視。
- **Circuit Breaker**: 連続失敗時に外部呼び出しを一時停止する仕組み。

## 詳細解説

車載音声システムはPipelineとして見える一方、実装では複数プロセス・Thread・Queue・State machineで構成されます。ドメイン判断とASR、Audio、Vehicle APIなどの外部依存を分離し、交換や試験を可能にします。IPC方式は要求量、Latency、互換性、デバッグ性を基に選びます。

プロセス分離例:

- HMI Process
- Voice Assistant Process
- Audio Process
- Vehicle Integration Process
- Navigation Process

Watchdog、Retry、Fallback、Recoveryを設計し、異常時に黙って停止しない構成にします。

## 車載での利用例

Audio Processが停止した場合、Watchdogが検知し、Voice AssistantはListeningを終了してユーザーへ利用不可を示します。再起動後は古いQueueやFocus所有状態を破棄し、新しいSessionから受付を再開します。

## 設計時の注意点

- StateとQueueの所有者を一つにし、複数Threadから直接変更しない。
- IPCメッセージに版、Session ID、Request ID、期限を持たせる。
- Queue上限、Backpressure、重複排除、順序保証を定義する。
- Retry回数だけでなくIdempotencyと停止条件を確認する。

## レビュー観点

- **機能**: 責務、入出力、Stateの所有者が明確か。
- **品質**: IPCとQueueのLatency、CPU、Memory上限があるか。
- **ログ**: Processをまたぐ関連IDと状態遷移を追えるか。
- **運用**: Watchdog、再起動、Fallback、設定切り戻しを試験できるか。

## よくある不具合

- 無制限Queueで遅延とMemory使用量が増え続ける。
- Retryした非冪等な車両要求が二重実行される。
- Process再起動後に古い応答が新しいSessionへ適用される。
- Watchdogが生存だけを見て、処理停止を検知できない。

## 確認問題

1. Audio、Voice、Vehicleを分離する利点とコストを挙げてください。
2. Queueに必要な上限、順序、破棄方針を説明してください。
3. Process停止から復旧までに確認する状態とログは何ですか。

## まとめ

アーキテクチャレビューでは、責務と状態の所有者、IPC契約、Queue制御、障害検知、Fallback、復旧を確認します。正常時の図だけでなく、停止時の遷移を設計します。

## 関連クイズ

[この章の理解度を確認する](../quizzes/chapter12_architecture.md)
