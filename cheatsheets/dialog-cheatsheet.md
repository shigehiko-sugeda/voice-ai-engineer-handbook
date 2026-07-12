# Dialog Managerチートシート

詳細は[Dialog Manager](../docs/06_dialog-manager.md)を参照してください。

## 一言でいうと

Dialog Managerは、NLU結果と現在のStateから「次に聞く、確認する、実行する、終了する」を決めます。

```text
Idle → Listening → Understanding → Confirming / Disambiguating
 ↑                                               ↓
 └──── Cancel / Timeout / Error ← Responding ← Executing
```

## 重要概念

| 用語 | 確認すること |
|---|---|
| Session | 起動から終了までを何で識別するか |
| State | 現在何を待っているか、所有者は誰か |
| Event | 各Stateで受理・無視する入力は何か |
| Confirmation | どの操作で実行前確認が必要か |
| Disambiguation | 候補の保持、選択、再質問をどう行うか |
| Barge-in | TTS停止とASR開始をどの順序で行うか |
| Timeout | 何秒待ち、何を破棄・解放して終了するか |

## 状態遷移の原則

- Stateごとに受け付けるEventと遷移先を定義する。
- Timeout、Retry回数、キャンセル、Errorの遷移を正常系と同時に書く。
- 非同期要求へSession IDとRequest IDを付ける。
- 終了済みSessionへ届いた遅延応答は破棄する。
- 終了時にContext、TTS Queue、Mic、Audio Focusを整理する。

## ケース: 同名連絡先

```text
「田中さんに電話して」
  → 候補3件をContextへ保存
  → Disambiguating
  → 「2番目」: 候補確定
  → 必要ならConfirming
  → Executing

「やめて」またはTimeout
  → 候補を破棄
  → Audio資源を解放
  → Idle
```

## ログの最小セット

- Session ID、Request ID、Event時刻
- 遷移前State、Event、遷移後State、遷移理由
- 候補とContextの生成・失効時刻
- Timeout種別、Retry回数、キャンセル元
- TTS停止、ASR開始、Mic・Audio Focusの状態
- 非同期応答を採用または破棄した理由

## よくある不具合

- Timeout直前のASR結果を次のSessionで処理する。
- キャンセル後のVehicle API応答で成功TTSを再生する。
- 再質問に上限がなく、対話から抜けられない。
- Barge-in後に古いTTSがQueueから再生される。

## レビュー質問

1. 正常、確認、曖昧、キャンセル、Timeoutの遷移があるか。
2. 各Stateで受理するEventと無効Eventの扱いは明確か。
3. Session終了時に破棄・解放する情報と資源は何か。
4. 古い非同期応答を誤適用しない仕組みがあるか。
5. 再質問とRetryに上限があるか。
6. Process再起動後に安全なIdleへ戻れるか。
