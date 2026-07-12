# アーキテクチャレビュー演習

対応教材: [アーキテクチャ](../../docs/12_architecture.md) / [プロセス構成図](../../diagrams/process-architecture.md)

## レビュー対象

ある車載Voice AIは、HMI、Voice Assistant、Audio、Vehicle Integration、Navigationを別Processにします。

設計案には次の記載があります。

- Process間は非同期メッセージで通信する。
- すべての要求は共通Queueへ追加し、到着順に処理する。
- Queue長に上限はない。
- Vehicle APIがTimeoutした場合は成功するまでRetryする。
- WatchdogはProcessのPIDが存在するかを1秒ごとに確認する。
- Process再起動後は、停止前のQueueをそのまま再処理する。
- IPCメッセージにはIntentとSlotを含めるが、版、Session ID、Request ID、期限は含めない。
- 各Processはローカル時刻と独自形式でログを出力する。

目標は、音声起動からTTS開始までp95で2秒以内、Memory使用量を長時間一定範囲に保つことです。

## 課題

次の形式でレビュー結果を提出してください。

| 優先度 | 問題 | 発生する現象 | 修正案 | 検証方法・ログ |
|---|---|---|---|---|

最低8件を挙げ、IPC、Queue、Watchdog、Retry、Fallback、Recoveryを必ず含めてください。最後に、Process境界を維持する利点とコストも説明してください。

<details>
<summary>ヒント1</summary>

「処理が生きている」と「要求を進められる」は同じではありません。Watchdogが何を観測すべきか考えてください。

</details>

<details>
<summary>ヒント2</summary>

再起動後にすべてを再処理すると、電話発信や車両操作が重複する可能性があります。期限とIdempotencyを確認してください。

</details>

## 自己チェック

- 無制限QueueとBackpressureを指摘したか。
- メッセージ互換性、関連ID、期限を扱ったか。
- 非冪等な要求の無限Retryを止めたか。
- 生存監視だけでなく処理停滞を検知できるか。
- 再起動時の古いState、Queue、Audio所有権を整理したか。
- p95 LatencyとMemory目標の測定方法を示したか。
