# 外部アシスタント連携・認証観点チートシート

詳細は[認証観点](../docs/10_certification.md)を参照してください。本シートは一般的な設計観点です。認証手順や非公開仕様は推測せず、公開情報および契約上参照可能な正式資料を確認してください。

## 対象範囲

- CarPlay、Android Auto、Siri、Google Assistant
- Voice Button、Wake Word、Mic Routing、Audio Focus
- 起動・終了、Latency、Timeout、Error handling、復帰

## 要件管理の流れ

```text
正式資料 → 要件一覧 → 設計・実装 → 試験 → 証跡 → 課題修正
               ↑
       不明点は推測せず確認事項として管理
```

要件ごとに根拠資料、資料版、対象ソフトウェア版、責任者、試験項目、証跡を対応づけます。

## 起動・終了で見ること

- Voice Buttonの短押し・長押し・境界値と、起動対象。
- Wake WordとVoice Buttonが同時に発生した場合の重複防止。
- 外部Assistant起動中の自社ASR、Wake Word、TTSの抑制。
- ユーザーキャンセル、無発話、通信断、Timeoutの終了処理。
- 終了後のMic、Audio Focus、表示、音楽の復帰。

## Audioで見ること

- Mic Routingの所有者、Grant前の利用禁止、取得失敗時の動作。
- 通話、ナビ、警告音、音楽、自社・外部Assistantの競合規則。
- 停止、Ducking、Mute、Queue待ち、復帰の組み合わせ。
- Process停止や異常終了時の所有権回収。

## ログ・証跡の最小セット

- Button Event種別、押下・解放時刻、判定結果
- Assistantの状態遷移、Session ID、終了理由
- Mic Routingの要求元、接続先、Grant / Deny / Release
- Audio Focusの所有者、優先度、競合判定、復帰結果
- Latencyの測定起点・終点、Timeout種別
- 通信・権限・認識・実行のError code
- 車両、ソフトウェア、設定、資料の版情報

音声や個人情報を含む証跡は、保存期間、アクセス権、共有範囲を管理します。

## よくある不具合

- Siri起動中に自社ASRも起動する。
- 外部Assistantへマイクが渡らない。
- 短押し・長押しの境界で二つのAssistantが起動する。
- TTSと外部Assistantが同時に再生される。
- 通話中に音声認識が起動する。
- Timeout後もMicやAudio Focusが残る。
- 通信復帰後も再起動できない。
- 試験環境と版が記録されず再現できない。

## レビュー質問

1. 起動、終了、Timeout、再起動が状態遷移図にあるか。
2. 自社機能を停止・抑制する条件と解除条件は何か。
3. Mic RoutingとAudio Focusの所有権をログで証明できるか。
4. 同時起動、通信断、通話中、再接続を試験するか。
5. 要件、設計、試験、証跡、対象版を追跡できるか。
6. 不明な非公開要件を推測せず、確認先と期限を管理しているか。
