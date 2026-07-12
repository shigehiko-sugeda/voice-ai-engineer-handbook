# Chapter 13 LLM連携 クイズ

対応教材: [本文](../docs/13_llm-integration.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

LLMが返したTool Calling結果は構造化されているため、そのままVehicle APIへ渡してよい。

<details>
<summary>ヒント</summary>

構造化と安全な実行許可は別です。

</details>

## 問題2（選択）

検索した資料を入力へ加えて回答を生成する構成はどれですか。 A. RAG / B. AEC / C. VAD / D. IPC

<details>
<summary>ヒント</summary>

マニュアル検索などに使います。

</details>

## 問題3（穴埋め）

LLMの入出力やTool実行を検査・制限する仕組みを（　　　）という。

<details>
<summary>ヒント</summary>

安全境界を表す用語です。

</details>

## 問題4（短答）

従来型NLUを残す方が適する車両機能の特徴を説明してください。

<details>
<summary>ヒント</summary>

予測可能性、Latency、オフライン、安全性を考えます。

</details>

## 問題5（ケース）

RAGがマニュアル根拠を取得できませんでした。回答とログをどう扱いますか。

<details>
<summary>ヒント</summary>

根拠がない内容を断定しません。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。Schema、許可リスト、値域、権限、車両状態を決定的に検証します。
2. A。RAGです。
3. Guardrail。ただし単一機構だけに依存せず実行側でも検証します。
4. Intent/Slotが限定され、低Latency・オフライン・決定的動作が重要な操作です。
5. 取得できない旨を示し、必要なら確認先へ誘導します。検索Query、取得結果、モデル/Prompt版、Fallback理由を残します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
