# Chapter 01 車載音声認識の全体像 クイズ

対応教材: [本文](../docs/01_overview.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

ASRが正しい文字列を返した場合、意図した車両機能の実行も必ず成功する。

<details>
<summary>ヒント</summary>

ASRより後段のNLU、Dialog Manager、Vehicle APIを考えてください。

</details>

## 問題2（選択）

ユーザーの発話をIntentとSlotへ変換する主なコンポーネントはどれですか。 A. ASR / B. NLU / C. TTS / D. Audio

<details>
<summary>ヒント</summary>

文字列を意味構造へ変換する段階です。

</details>

## 問題3（穴埋め）

一つの音声要求を複数コンポーネントで追跡するため、共通の（　　　）をログへ付与する。

<details>
<summary>ヒント</summary>

用語集のログ・レビューを確認してください。

</details>

## 問題4（短答）

Domain ServiceとVehicle APIの責務の違いを説明してください。

<details>
<summary>ヒント</summary>

制約判断と、車両との接点を分けます。

</details>

## 問題5（ケース）

Vehicle APIへ要求を送信できましたが、車両状態は変わりません。成功TTSを再生する前に何を確認しますか。

<details>
<summary>ヒント</summary>

送信成功と実行完了は別です。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。文字化が正しくても、意味解釈、実行条件、Vehicle APIで失敗する可能性があります。
2. B。NLUがASR文字列をIntentやSlotへ変換します。
3. Correlation ID。Session IDやRequest IDと組み合わせて追跡します。
4. Domain Serviceは値域・権限・実行可否を判断し、Vehicle APIは車両機能とのインターフェースを担います。
5. 完了応答、結果コード、状態反映、Request ID、Timeoutを確認し、実行成功を確認してから応答します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
