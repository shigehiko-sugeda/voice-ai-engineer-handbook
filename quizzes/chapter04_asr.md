# Chapter 04 ASR クイズ

対応教材: [本文](../docs/04_asr.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

Streaming ASRのPartial Resultは後から変化するため、不可逆な操作の確定には適さない。

<details>
<summary>ヒント</summary>

PartialとFinalの違いを考えてください。

</details>

## 問題2（選択）

発話終了を判断する処理はどれですか。 A. Endpoint Detection / B. Arbitration / C. RAG / D. Beamforming

<details>
<summary>ヒント</summary>

ASRが結果を確定するタイミングに関係します。

</details>

## 問題3（穴埋め）

単語の置換・削除・挿入からASR誤りを測る指標は（　　　）である。

<details>
<summary>ヒント</summary>

Word Error Rateの略です。

</details>

## 問題4（短答）

ASR ConfidenceとNLU Confidenceを別々にログへ残す理由を説明してください。

<details>
<summary>ヒント</summary>

文字化と意味分類の失敗を分けます。

</details>

## 問題5（ケース）

「東京駅を経由して横浜へ」が「東京駅を経由して」で確定されました。確認すべき設定とログは何ですか。

<details>
<summary>ヒント</summary>

発話途中の間を終端と誤判定した可能性があります。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ○。車両操作の確定には原則としてFinal Resultを使います。
2. A。Endpoint Detectionが発話終端を判断します。
3. WER。正解単語数を分母にします。
4. ASR誤りとNLU誤分類の原因・対策が異なり、切り分けと評価を別々に行うためです。
5. Endpoint設定、VAD、音声区間、Partial/Final Result、単語時刻、確定理由、走行ノイズを確認します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
