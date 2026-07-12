# Chapter 03 Wake Word クイズ

対応教材: [本文](../docs/03_wake-word.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

Wake Wordのしきい値を下げると、False WakeとMiss Wakeの両方が必ず減る。

<details>
<summary>ヒント</summary>

検出しやすさと誤起動はトレードオフです。

</details>

## 問題2（選択）

Wake検出直前の音声を保持し、後続発話の先頭欠けを防ぐ仕組みはどれですか。 A. Pre-roll / B. Ducking / C. Retry / D. Slot

<details>
<summary>ヒント</summary>

検出より前の音声を一時保存します。

</details>

## 問題3（穴埋め）

Wake Wordではない音で起動することを（　　　）という。

<details>
<summary>ヒント</summary>

誤起動を表す用語です。

</details>

## 問題4（短答）

通話中にWake Wordを抑制する場合、ログへ残す情報を挙げてください。

<details>
<summary>ヒント</summary>

検出の有無だけでなく、なぜ起動しなかったかを残します。

</details>

## 問題5（ケース）

Wake検出は成功しましたが「自宅へ」の先頭がASR結果から欠けます。確認箇所はどこですか。

<details>
<summary>ヒント</summary>

検出からASR開始までの時間を追ってください。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。一般に検出しやすくなる一方、False Wakeが増える可能性があります。
2. A。Pre-rollをASRへ渡して発話先頭を補います。
3. False Wake。起動語を検出できないことはMiss Wakeです。
4. 検出スコア、しきい値、通話状態、抑制理由、Session開始の有無、時刻を残します。
5. Pre-roll、Mic取得時刻、ASR準備完了時刻、音声Frame時刻、VAD開始点を確認します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
