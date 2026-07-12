# NLUチートシート

詳細は[NLU](../docs/05_nlu.md)を参照してください。

## 一言でいうと

NLUは、ASRが文字化した発話を「何を、どの値で実行したいか」という構造へ変換します。

```text
ASR Final Result + Context
        ↓
Domain → Intent → Entity抽出 → Slot正規化
        ↓
Intent / Slot / Confidence → Dialog Manager
```

## 重要概念

| 用語 | 意味 | 例 |
|---|---|---|
| Domain | 機能領域 | Climate |
| Intent | ユーザーの目的 | `SetTemperature` |
| Entity | 発話中の意味ある表現 | 「22度」 |
| Slot | 型・単位を持つ実行パラメータ | `Temperature: 22`, `Unit: Celsius` |
| Context | 解釈を補う直前の対話や車両状態 | 直前に空調を操作 |
| Confidence | NLU結果の信頼度 | 低ければ確認へ |

## 判断の基本

- ASR誤りとNLU誤分類を分ける。
- Slotの型、単位、値域、正規化規則を定義する。
- 必須Slot不足はDialog Managerへ渡し、追加質問する。
- 複数候補や低Confidenceでは確認し、推測だけで実行しない。
- ContextにはSession境界と有効期限を設ける。
- 走行中制限や権限は実行側でも再検証する。

## 例

```yaml
utterance: エアコンを22度にして
domain: Climate
intent: SetTemperature
slots:
  temperature: 22
  unit: Celsius
```

「もう少し下げて」は、直前が空調なら温度、Mediaなら音量の可能性があります。Contextで一意に決まらなければ確認します。

## ログの最小セット

- Session ID、NLUモデル・設定版
- 入力に使ったASR Final Result
- 使用したContextとその生成時刻
- Domain、Intent、Slotの候補とConfidence
- 正規化前後のSlot値
- 採用結果、Fallback・確認へ進んだ理由

## よくある不具合

- ASR結果は正しいが、別DomainまたはIntentへ分類する。
- 「22度」を文字列のまま渡し、単位や値域を検証できない。
- 前SessionのContextで「もう一度」を誤解釈する。
- 未知の発話を高Confidenceで既存Intentへ強制分類する。

## レビュー質問

1. 発話バリエーションとIntentの対応が定義されているか。
2. Slotの型、単位、値域、正規化は明確か。
3. 低Confidence、必須Slot不足、複数候補の動作は何か。
4. Contextはいつ生成され、いつ失効するか。
5. ASR誤りとNLU誤分類をログで区別できるか。
6. 危険または不可逆な操作を曖昧な結果から実行しないか。
