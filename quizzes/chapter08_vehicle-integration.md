# Chapter 08 車両連携 クイズ

対応教材: [本文](../docs/08_vehicle-integration.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

Vehicle APIへの送信が成功すれば、車両操作も完了したと判断できる。

<details>
<summary>ヒント</summary>

送信、受理、実行完了、状態反映を分けます。

</details>

## 問題2（選択）

同じ要求のRetryで不正な重複実行を防ぐ性質はどれですか。 A. Idempotency / B. Beamforming / C. Grounding / D. WER

<details>
<summary>ヒント</summary>

非同期APIと再試行に関係します。

</details>

## 問題3（穴埋め）

非同期の要求と応答を対応づける識別子を（　　　）という。

<details>
<summary>ヒント</summary>

Sessionより細かい要求単位です。

</details>

## 問題4（短答）

走行中制限をNLUだけでなく実行側でも検証する理由を説明してください。

<details>
<summary>ヒント</summary>

上流結果を完全には信頼しません。

</details>

## 問題5（ケース）

温度設定APIがTimeoutしました。Retry前に確認する項目を挙げてください。

<details>
<summary>ヒント</summary>

既に実行済みかもしれません。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。完了応答または状態更新の確認が必要です。
2. A。Idempotencyです。
3. Request ID。Session IDと併用します。
4. 誤分類、古いContext、不正入力があっても危険な操作を防ぐ最終安全境界にするためです。
5. Idempotency、現在値、Request ID、失敗種別、Retry上限、期限、重複時の影響を確認します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
