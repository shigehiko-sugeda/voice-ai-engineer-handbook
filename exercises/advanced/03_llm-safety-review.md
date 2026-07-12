# LLM車両連携・安全設計レビュー演習

対応教材: [LLM連携](../../docs/13_llm-integration.md) / [車両連携](../../docs/08_vehicle-integration.md)

## レビュー対象

チームは、ユーザー発話をLLMへ送り、返されたTool Calling結果をVehicle APIへ渡す機能を提案しています。

```text
発話 → LLM → tool_name / arguments → Vehicle API → TTS
```

設計案には次の特徴があります。

- LLMが返したTool名と引数をそのまま実行する。
- Tool定義にJSON Schemaはない。
- 読み取り操作と車両制御で権限を分けない。
- Prompt内の車両マニュアル記述も命令として扱える。
- モデルTimeout時は3回Retryし、その間Micを保持する。
- 通信断時の従来型NLU Fallbackはない。
- モデルとPromptの版をログへ残さない。

## 課題

1. 機能、品質、ログ、安全性、Privacy、運用に分けてリスクを抽出してください。
2. LLMとVehicle APIの間に必要な決定的な検証処理を図示してください。
3. 読み取り、可逆操作、不可逆・安全関連操作の確認方針を分けてください。
4. Timeout、通信断、モデル更新時のFallbackと切り戻しを設計してください。

<details>
<summary>ヒント1</summary>

Tool Calling結果は実行命令ではなく候補です。許可リスト、Schema、値域、権限、車両状態を通常コードで検証します。

</details>

<details>
<summary>ヒント2</summary>

取得した文書やユーザー入力に、Policyを変更したり新しいToolを許可したりする権限を与えないでください。

</details>

## 自己チェック

- Toolと引数の許可リスト・Schema検証があるか。
- 走行状態、権限、値域、確認要否を実行直前に検証するか。
- Prompt InjectionとGrounding不足を扱ったか。
- 音声、位置、連絡先、車両状態の送信範囲を最小化したか。
- モデル・Prompt版、検証結果、実行結果をログへ残すか。
- 停止スイッチ、Fallback、版切り戻しがあるか。
