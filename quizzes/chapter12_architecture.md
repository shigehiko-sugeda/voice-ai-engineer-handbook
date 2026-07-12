# Chapter 12 アーキテクチャ クイズ

対応教材: [本文](../docs/12_architecture.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

Queueを無制限にすれば、要求を捨てずに安定動作できる。

<details>
<summary>ヒント</summary>

入力が処理能力を超え続けた場合を考えます。

</details>

## 問題2（選択）

Processが存在しているが処理が停止している状態を検知するのに有効なのはどれですか。 A. Heartbeatと進捗監視 / B. PID確認だけ / C. WER / D. Slot F1

<details>
<summary>ヒント</summary>

生存と処理進捗を分けます。

</details>

## 問題3（穴埋め）

Process間でEventや要求を交換する仕組みを（　　　）という。

<details>
<summary>ヒント</summary>

Inter-Process Communicationの略です。

</details>

## 問題4（短答）

非冪等な車両要求を無条件Retryしてはいけない理由を説明してください。

<details>
<summary>ヒント</summary>

電話発信や操作が二重に起きる可能性があります。

</details>

## 問題5（ケース）

Voice Process再起動後、古いQueueを再処理する設計です。レビュー項目を挙げてください。

<details>
<summary>ヒント</summary>

期限、Session、所有権、重複を見ます。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。LatencyとMemoryが増え続けるため、上限とBackpressureが必要です。
2. A。Healthと処理進捗、Queue滞留などを監視します。
3. IPC。版、ID、期限などの契約が必要です。
4. 既に成功していた場合に重複実行するため、結果確認、Idempotency、Retry条件が必要です。
5. Session/Request ID、期限、Idempotency、State初期化、Audio Focus回収、破棄ログ、復旧試験を確認します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
