# Chapter 05 NLU クイズ

対応教材: [本文](../docs/05_nlu.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

Intentは実行に必要な数値や単位、Slotはユーザーの目的を表す。

<details>
<summary>ヒント</summary>

IntentとSlotの役割を入れ替えていないか確認してください。

</details>

## 問題2（選択）

「エアコンを22度にして」のDomainとして最も適切なのはどれですか。 A. Media / B. Phone / C. Climate / D. Navigation

<details>
<summary>ヒント</summary>

機能領域を選びます。

</details>

## 問題3（穴埋め）

発話中の「22度」は（　　　）として抽出され、型と単位を持つSlotへ正規化される。

<details>
<summary>ヒント</summary>

発話中に現れる意味ある表現です。

</details>

## 問題4（短答）

Contextに有効期限とSession境界が必要な理由を説明してください。

<details>
<summary>ヒント</summary>

「もう一度」を前日の操作で解釈するとどうなるでしょうか。

</details>

## 問題5（ケース）

「もう少し下げて」の直前に空調と音量の両方を操作しています。どう処理しますか。

<details>
<summary>ヒント</summary>

一意に決まらない場合のDialog動作を考えます。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。Intentが目的、Slotが数値や単位などのパラメータです。
2. C。Climate Domainです。
3. Entity。正規化後は数値22とCelsiusなどになります。
4. 古い文脈を新しい要求へ誤適用し、別の機能や値を操作するのを防ぐためです。
5. 候補とConfidenceをDialog Managerへ渡し、「温度と音量のどちらですか」などの確認を行います。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
