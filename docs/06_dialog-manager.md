# Dialog Manager

## この章で学ぶこと

- State、Session、Contextの管理
- Confirmation、Disambiguation、Timeout
- Barge-inとError recovery

## なぜ必要なのか

一度の発話で情報が揃うとは限りません。候補選択、確認、割り込み、Timeoutを記憶せずに実装すると、古い要求の実行やマイクの解放漏れが起きます。Dialog Managerは対話を明示的な状態遷移として管理します。

## 全体像

```text
Idle → Listening → Understanding → Confirming / Disambiguating → Executing → Responding → Idle
          └──────── Cancel・Timeout・Error ────────────────────────────────┘
```

## 基本用語

- **Session**: 起動から終了までの対話単位。
- **State**: 現在待っている入力や処理を表す状態。
- **Confirmation**: 実行前にユーザーの同意を得ること。
- **Disambiguation**: 複数候補から一つを選ぶこと。
- **Barge-in**: TTS再生中のユーザー発話で応答を中断すること。

## 詳細解説

Dialog ManagerはNLU結果と現在の状態から、次のアクションを決めるコンポーネントです。

例:

```text
ユーザー: 田中さんに電話して
システム: 田中さんが3人います。どの田中さんですか？
```

この場合、IntentはCallContactですが、候補が複数あるためDisambiguation状態に遷移します。Sessionは対話の単位、Stateは現在の待ち受け状態、Contextは直前の意図や候補です。Timeout、Barge-in、Prompt、Error recoveryを明示的に設計します。

## 車載での利用例

「田中さんに電話して」で候補が3人なら、候補をSession Contextへ保存し、選択待ちへ遷移します。「2番目」なら候補を確定し、発信前確認が必要ならConfirmationへ進みます。「やめて」またはTimeoutなら候補を破棄してIdleへ戻します。

## 設計時の注意点

- 各Stateで受け付けるEventと、無効なEventの扱いを決める。
- StateごとにTimeout、Retry上限、終了時の資源解放を定義する。
- 非同期結果にSession IDを付け、古い応答を破棄する。
- Barge-in時にTTS停止、ASR開始、Audio Focus変更の順序を決める。

## レビュー観点

- **機能**: 正常、確認、曖昧、キャンセル、Timeoutの遷移があるか。
- **品質**: 応答Latencyと無限再質問の上限が定義されているか。
- **ログ**: 遷移前後のState、Event、Session ID、理由を残すか。
- **運用**: 再起動時に中途半端なSessionやAudio資源が残らないか。

## よくある不具合

- Timeout直前のASR結果が次のSessionで処理される。
- キャンセル後に非同期の車両API応答が届き、成功TTSを再生する。
- 再質問に上限がなく、ユーザーが対話から抜けられない。
- Barge-in後も古いTTSがQueueに残って再生される。

## 確認問題

1. 同名連絡先が3件ある場合のStateとEventを書いてください。
2. Timeout時に破棄・解放すべき情報と資源は何ですか。
3. 古い非同期応答を誤適用しない仕組みを説明してください。

## まとめ

Dialog Managerは対話をState、Event、Sessionで制御します。正常系だけでなく、曖昧性、割り込み、Timeout、遅延応答から安全にIdleへ戻れるかをレビューします。

## 関連クイズ

[この章の理解度を確認する](../quizzes/chapter06_dialog-manager.md)
