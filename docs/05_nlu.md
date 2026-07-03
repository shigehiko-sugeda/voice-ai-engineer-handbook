# NLU

## この章で学ぶこと

- Intent / Slot / Entity / Domain / Contextを理解する
- ASR、NLU、Dialog Managerの違い
- Multi-turn interactionとFallbackを設計する

## なぜ必要なのか

初回版では要点を整理します。後続の章や演習と対応づけながら、実務でレビューできる粒度まで拡張します。

## 全体像

初回版では要点を整理します。後続の章や演習と対応づけながら、実務でレビューできる粒度まで拡張します。

## 基本用語

初回版では要点を整理します。後続の章や演習と対応づけながら、実務でレビューできる粒度まで拡張します。

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

初回版では要点を整理します。後続の章や演習と対応づけながら、実務でレビューできる粒度まで拡張します。

## 設計時の注意点

初回版では要点を整理します。後続の章や演習と対応づけながら、実務でレビューできる粒度まで拡張します。

## レビュー観点

初回版では要点を整理します。後続の章や演習と対応づけながら、実務でレビューできる粒度まで拡張します。

## よくある不具合

初回版では要点を整理します。後続の章や演習と対応づけながら、実務でレビューできる粒度まで拡張します。

## 確認問題

1. この章の技術は音声パイプラインのどこで使われますか？
2. 車載環境で追加で注意すべき点を2つ挙げてください。
3. ログで確認すべき情報は何ですか？

## まとめ

初回版では要点を整理します。後続の章や演習と対応づけながら、実務でレビューできる粒度まで拡張します。
