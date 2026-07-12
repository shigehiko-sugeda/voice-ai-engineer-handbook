# 音声パイプライン・ログ解析演習

対応教材: [全体像](../../docs/01_overview.md) / [評価指標](../../docs/11_metrics.md) / [Voice AIレビュー](../../cheatsheets/voice-ai-review.md)

## 申告された不具合

高速走行中、「エアコンを22度にして」と発話したところ、画面はListeningのままになり、音楽も元の音量へ戻りませんでした。

## ログ

```text
12:00:00.000 session=S42 wake score=0.91 result=accepted
12:00:00.030 session=S42 focus request=mic owner=voice result=granted
12:00:00.040 session=S42 focus request=speaker mode=duck result=granted
12:00:00.120 session=S42 asr state=streaming
12:00:01.400 session=S42 asr partial="エアコンを22"
12:00:01.900 session=S42 vad state=speech_end
12:00:04.900 session=S42 asr error=server_timeout
12:00:04.910 session=S42 dialog event=ASR_ERROR state=Listening
12:00:12.910 session=S42 dialog event=NO_INPUT_TIMEOUT state=Listening
```

## 課題

1. ログから確定できる事実と、まだ推測にすぎないことを分けてください。
2. 最初の異常地点と、後続で不足している処理を挙げてください。
3. 追加で必要なAudio、ASR、Dialog、Audio Focusログを挙げてください。
4. 修正案と、再発防止試験を提案してください。

<details>
<summary>ヒント1</summary>

`ASR_ERROR`を受けた後もDialogのStateが変わっていません。エラー処理と資源解放を分けて確認してください。

</details>

<details>
<summary>ヒント2</summary>

音楽が戻らない原因を断定するには、Speaker FocusのReleaseと復帰結果のログが必要です。

</details>

## 自己チェック

- ASR TimeoutとDialog Stateの問題を区別したか。
- ログにないFocus解放失敗を断定していないか。
- ユーザー通知、State遷移、Mic・Speaker解放を修正案に含めたか。
- 高速走行、通信遅延、サーバーTimeoutを試験条件に含めたか。
