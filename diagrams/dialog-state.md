# Dialog State

対話Sessionの代表的な状態遷移です。詳細は[Dialog Manager](../docs/06_dialog-manager.md)を参照してください。

```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> Listening: Wake Word / Voice Button
    Listening --> Understanding: ASR Final Result
    Listening --> Ending: No input / Cancel / Timeout
    Understanding --> Confirming: 実行前確認が必要
    Understanding --> Disambiguating: 候補が複数
    Understanding --> Executing: 情報が十分
    Understanding --> Responding: Unsupported / Low confidence
    Confirming --> Executing: Yes
    Confirming --> Ending: No / Cancel / Timeout
    Disambiguating --> Executing: 候補確定
    Disambiguating --> Listening: 再質問
    Disambiguating --> Ending: Cancel / Timeout / Retry上限
    Executing --> Responding: Success / Error
    Executing --> Ending: Cancel可能な処理を中止
    Responding --> Listening: Multi-turn継続
    Responding --> Ending: TTS完了
    Ending --> Idle: Context・Queue・Audio資源を解放
```

## 遷移に必ず持たせる情報

- Session ID、Request ID
- 遷移前State、Event、遷移後State、理由
- StateごとのTimeoutとRetry上限
- Contextと候補の生成・失効条件
- 遅延した非同期応答の採用・破棄条件

レビュー項目は[Dialog Managerチートシート](../cheatsheets/dialog-cheatsheet.md)を参照してください。
