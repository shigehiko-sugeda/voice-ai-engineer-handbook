# Voice AI全体フロー

Wake WordまたはVoice Buttonによる起動から、車両実行結果をTTSで返すまでの論理フローです。詳細は[車載音声認識の全体像](../docs/01_overview.md)を参照してください。

```mermaid
flowchart LR
    User[ユーザー] -->|発話| Mic[Mic / Audio入力]
    User -->|ボタン操作| Button[Voice Button]

    Mic --> Wake[Wake Word検出]
    Wake --> Start{起動可能か}
    Button --> Start

    Start -->|Yes| Focus[Mic取得 / Audio Focus]
    Start -->|No: 通話・競合・禁止状態| Suppress[起動を抑制]
    Suppress --> Idle[Idle]

    Focus --> ASR[ASR Final Result]
    ASR --> NLU[NLU<br/>Domain / Intent / Slot]
    Context[(Session Context)] --> NLU
    NLU --> DM[Dialog Manager]
    DM -->|情報不足・曖昧| Confirm[確認 / 追加質問]
    Confirm --> ASR
    DM -->|実行| Domain[Domain Service<br/>制約・権限・値域確認]
    Domain --> API[Vehicle API]
    API --> Result{実行結果}
    Result -->|Success| Success[成功応答生成]
    Result -->|Error / Timeout| Failure[失敗・Fallback応答生成]
    Success --> TTS[TTS]
    Failure --> TTS
    TTS -->|Audio Focus取得| Speaker[Speaker]
    Speaker --> User
    Speaker --> PlaybackDone[再生完了]
    PlaybackDone --> End[資源解放 / Session終了]
    End --> Idle

    DM -->|Cancel / Timeout| End
```

## 読み方

- Wake WordとVoice Buttonは、どちらもSession開始の入口です。
- ASRは文字列、NLUは意味構造、Dialog Managerは次Actionを決めます。
- Domain Serviceは、LLMやNLUの結果をそのまま実行せず、制約と権限を検証します。
- Vehicle APIへの送信成功と車両での実行成功は区別します。
- TTS合成完了と再生完了を区別し、通常時は再生完了後にAudio Focusを解放します。
- Cancel、Timeout、Errorでも、Mic、Audio Focus、Contextを整理してIdleへ戻します。

レビュー項目は[Voice AIレビュー・チートシート](../cheatsheets/voice-ai-review.md)を参照してください。
