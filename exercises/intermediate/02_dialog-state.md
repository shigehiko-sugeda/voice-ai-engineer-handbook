# Dialog State設計演習

対応教材: [Dialog Manager](../../docs/06_dialog-manager.md) / [Dialog State図](../../diagrams/dialog-state.md)

## ケース

ユーザーが「田中さんに電話して」と発話し、連絡先候補が3人見つかりました。

- 候補提示後の入力Timeoutは8秒。
- 再質問は最大2回。
- 候補確定後、発信前に確認する。
- ユーザーはいつでも「やめて」と言える。
- 連絡先検索の古い応答が、キャンセル後に届く可能性がある。

## 課題

1. Mermaidの`stateDiagram-v2`または状態遷移表でState machineを書いてください。
2. 各Stateで受け付けるEvent、Timeout、遷移先を示してください。
3. Session終了時に破棄するContextと解放する資源を挙げてください。
4. キャンセル後の遅延応答を破棄する条件を書いてください。

<details>
<summary>ヒント1</summary>

Listening、Disambiguating、Confirming、Executing、Endingを出発点にしてください。

</details>

<details>
<summary>ヒント2</summary>

Session IDだけでなく、非同期検索を識別するRequest IDと、現在のStateも照合します。

</details>

## 自己チェック

- 候補選択と発信確認を別Stateにしたか。
- Timeout、Retry上限、キャンセルからIdleへ戻れるか。
- 古い検索結果を新しいSessionへ適用しないか。
- TTS Queue、Context、Mic、Audio Focusの整理を含めたか。
- State遷移理由をログに残せるか。
