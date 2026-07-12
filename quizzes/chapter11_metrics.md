# Chapter 11 評価指標 クイズ

対応教材: [本文](../docs/11_metrics.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

平均Latencyが目標以内なら、遅いユーザー体験は十分に評価できる。

<details>
<summary>ヒント</summary>

分布の遅い側を確認します。

</details>

## 問題2（選択）

Intentが正しく分類された発話の割合を表す指標はどれですか。 A. Intent Accuracy / B. WER / C. SNR / D. CPU

<details>
<summary>ヒント</summary>

NLUの分類精度です。

</details>

## 問題3（穴埋め）

WERは置換、削除、（　　　）を基に計算する。

<details>
<summary>ヒント</summary>

余分な単語が入る誤りです。

</details>

## 問題4（短答）

False Wake Rateを報告するとき、分母を明記する理由を説明してください。

<details>
<summary>ヒント</summary>

観測時間と発話回数では値の意味が変わります。

</details>

## 問題5（ケース）

WERは改善したのにタスク成功率が低下しました。次に分解して確認する指標とログを挙げてください。

<details>
<summary>ヒント</summary>

文字化の後段を見ます。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。p95などのPercentileも確認します。
2. A。Intent Accuracyです。
3. 挿入。分母は正解単語数です。
4. 定義の違う数値を比較しないためで、観測時間・試行回数・条件を揃える必要があります。
5. Intent/Slot精度、Endpoint、Dialog State、Vehicle API結果、Latency、Context、Sessionログを確認します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
