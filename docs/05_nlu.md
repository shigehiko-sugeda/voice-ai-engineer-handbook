# NLU

## この章で学ぶこと

- Intent / Slot / Entity / Domain / Contextを理解する
- ASR、NLU、Dialog Managerの違い
- Multi-turn interactionとFallbackを設計する

## なぜ必要なのか

ASRが正しく文字化しても、その文字列を誤ったIntentやSlotへ変換すれば別の車両機能が動きます。NLUの責務とDialog Managerの判断を分けることで、誤操作の原因と対策を具体化できます。

## 全体像

```text
ASR最終結果 + Context → Domain分類 → Intent分類 → Entity抽出・正規化
                                              ↓
                              Intent / Slot / Confidence → Dialog Manager
```

## 基本用語

- **Intent**: ユーザーの目的。
- **Slot**: 実行に必要な型付きパラメータ。
- **Entity**: 発話中から抽出する意味のある表現。
- **Domain**: Climate、Phoneなどの機能領域。
- **Context**: 直前の対話や車両状態など、解釈を補う情報。
- **Fallback**: 意味を確定できない場合の代替動作。

## 詳細解説

NLU（Natural Language Understanding）は、ASRが出力したテキストから「ユーザーが何をしたいか」を構造化する処理です。

## 主要概念

- **Intent**: ユーザーの目的。例: `SetTemperature`、`CallContact`、`NavigateToDestination`。
- **Slot**: Intentを実行するためのパラメータ。例: 温度、連絡先名、目的地。
- **Entity**: テキスト中で意味を持つ表現。Slotに正規化される候補。
- **Domain**: 機能領域。例: Climate、Phone、Navigation、Media。
- **Context**: 直前の会話、現在の画面、車両状態、前回操作などの文脈。
- **Confidence**: NLU結果の信頼度。しきい値未満なら確認やFallbackを使う。
- **Fallback**: IntentやSlotが確定できない場合の代替動作。
- **Multi-turn interaction**: 1回で完結せず、追加質問や確認を挟む対話。

## Intent / Slot例

ユーザー発話: 「エアコンを22度にして」

```yaml
Intent: SetTemperature
Domain: Climate
Slot:
  Temperature: 22
  Unit: Celsius
```

この例では、ASRは「エアコンを22度にして」という文字列を出します。NLUはそれをClimateドメインのSetTemperature意図として解釈し、TemperatureとUnitを抽出します。

## Contextが必要な例

ユーザー発話: 「もう少し下げて」

この発話だけでは、温度を下げるのか、音量を下げるのか、地図縮尺を下げるのか判断できません。直前に「エアコンを22度にして」と言っていれば、ContextからClimateの温度調整だと推定できます。直前に音量操作をしていればMediaのVolumeDownかもしれません。

## NLUとASRの違い

ASRは音声を文字にする処理です。NLUは文字列をIntent、Slot、Domainなどの意味構造に変換します。ASR誤りがあるとNLUも影響を受けるため、ASR confidenceとNLU confidenceを分けてログに残すことが重要です。

## NLUとDialog Managerの違い

NLUは「この発話が何を意味するか」を推定します。Dialog Managerは「その結果を受けて次に何をするか」を決めます。例えばSlotが不足していればDialog Managerが追加質問を出します。候補が複数あればDisambiguationを行います。

## 設計の勘所

- Intent名は実装都合ではなくユーザー目的に合わせる。
- Slotは型、単位、範囲、正規化ルールを定義する。
- Confidenceしきい値はドメインごとに調整する。
- 危険または不可逆な操作は確認を挟む。
- Fallbackは「分かりません」だけでなく、再発話例を提示する。
- Contextは便利だが、古い文脈の誤利用を避けるため有効期限を持つ。

## 車載での利用例

「少し下げて」は、直前が空調操作なら温度、音楽操作なら音量を指す可能性があります。NLUは候補を返し、Contextが十分でなければDialog Managerが「温度を下げますか」と確認します。推測だけで操作を実行しません。

## 設計時の注意点

- Slotごとに型、単位、範囲、既定値、正規化規則を定義する。
- Contextに有効期限とSession境界を設け、古い値を再利用しない。
- 低Confidence、複数候補、必須Slot不足の扱いを分ける。
- 走行中制限や権限は、NLU結果を信頼せず実行側でも検証する。

## レビュー観点

- **機能**: 発話バリエーションとIntent・Slotの対応が仕様化されているか。
- **品質**: Intent AccuracyとSlot F1をDomain別に測るか。
- **ログ**: ASR入力、Context、候補、Confidence、採用結果を残すか。
- **安全性**: 曖昧な結果から危険または不可逆な操作を実行しないか。

## よくある不具合

- 「22度」を文字列のまま渡し、単位や許容範囲を解釈できない。
- 前SessionのContextで「もう一度」を誤解釈する。
- 未知の発話を高Confidenceで既存Intentへ強制分類する。
- ASR誤りとNLU誤分類を同じエラーとして集計する。

## 確認問題

1. 「エアコンを22度にして」のIntent、Domain、Slotを記述してください。
2. 「もう少し下げて」を即時実行できない条件は何ですか。
3. NLU誤分類をASR誤りと区別するログを挙げてください。

## まとめ

NLUはASR文字列を実行可能な意味構造へ変換します。型と正規化、Contextの寿命、曖昧時の確認、ASRとは別の評価とログが重要です。

## 参考資料

- [Intents — Dialogflow ES, Google Cloud](https://cloud.google.com/dialogflow/es/docs/intents-overview): Intent、Training Phrase、Parameter、Entity、Responseの具体的な構成例を確認できます。
- [Input and output contexts — Dialogflow ES, Google Cloud](https://cloud.google.com/dialogflow/es/docs/contexts-input-output): ContextによるIntent MatchingとLifespanの考え方を確認できます。

これらはDialogflowの製品仕様です。本章では、特定製品に依存しないNLU設計概念を説明しています。

## 関連クイズ

[この章の理解度を確認する](../quizzes/chapter05_nlu.md)
