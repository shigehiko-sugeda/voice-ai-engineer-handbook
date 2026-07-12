# Intent / Slot抽出演習

対応教材: [NLU](../../docs/05_nlu.md) / [NLUチートシート](../../cheatsheets/nlu-cheatsheet.md)

## ケース

運転者が「エアコンを22度にして」と発話しました。ASRの最終結果も同じ文字列です。車両は摂氏を使用し、設定可能範囲は16〜30度です。

## 課題

次の形式でNLU結果を書いてください。

```yaml
domain:
intent:
slots:
confidence_handling:
```

続いて、次の問いに答えてください。

1. Entityとして抽出する表現と、正規化後のSlotを分けてください。
2. 値が「32度」だった場合、NLUと実行側はそれぞれ何を担当しますか。
3. Confidenceが低い場合、Dialog Managerへ何を渡しますか。

<details>
<summary>ヒント1</summary>

ユーザーの目的、機能領域、温度の数値、単位を分けて考えてください。

</details>

<details>
<summary>ヒント2</summary>

NLUは値を構造化できますが、車両操作の最終的な許可と値域確認をNLUだけへ任せない設計が必要です。

</details>

## 自己チェック

- DomainとIntentを区別したか。
- Slotに数値型と単位があるか。
- 発話中のEntityと正規化後のSlotを区別したか。
- 低Confidence時に推測だけで実行していないか。
- 車両側の値域検証を考慮したか。
