# Chapter 06 Dialog Manager クイズ

対応教材: [本文](../docs/06_dialog-manager.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

Sessionが終了していても、同じIntentなら遅れて届いた非同期応答を適用してよい。

<details>
<summary>ヒント</summary>

古い要求と現在の対話を区別してください。

</details>

## 問題2（選択）

同名連絡先が複数見つかった場合のStateとして適切なのはどれですか。 A. Disambiguating / B. Idle / C. Sampling / D. Ducking

<details>
<summary>ヒント</summary>

候補を一つに絞る状態です。

</details>

## 問題3（穴埋め）

TTS再生中のユーザー発話で再生を中断し、入力へ切り替えることを（　　　）という。

<details>
<summary>ヒント</summary>

割り込み発話の用語です。

</details>

## 問題4（短答）

StateごとにTimeoutを定義する理由を説明してください。

<details>
<summary>ヒント</summary>

待ち続けた場合の資源とUXを考えます。

</details>

## 問題5（ケース）

キャンセル直後にVehicle APIの成功応答が届きました。何を確認し、どう扱いますか。

<details>
<summary>ヒント</summary>

現在のSessionとStateを照合します。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。Session ID、Request ID、Stateを照合し、終了済み要求の応答は破棄します。
2. A。候補選択を待つDisambiguatingへ遷移します。
3. Barge-in。TTS停止とASR開始の順序も設計します。
4. 無限待機を防ぎ、Context、Mic、Audio Focusなどを確実に整理してIdleへ戻すためです。
5. Session IDとRequest ID、Cancel可否、現在Stateを確認し、終了済みなら成功TTSを出さず応答を破棄・記録します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
