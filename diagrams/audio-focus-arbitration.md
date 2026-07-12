# Audio FocusとArbitration

複数機能がマイクまたはスピーカーを要求した場合の調停フローです。詳細は[Audio FocusとArbitration](../docs/09_audio-focus-and-arbitration.md)を参照してください。

```mermaid
flowchart TD
    Request[Audio資源要求<br/>MicまたはSpeaker] --> Validate{要求は有効か}
    Validate -->|期限切れ・禁止状態| Deny[Deny / 破棄]
    Validate -->|有効| Owner{現在の所有者はいるか}
    Owner -->|No| Grant[Grant / Routing切替]
    Owner -->|Yes| Compare{競合規則と優先度を比較}
    Compare -->|低優先| Wait{待機可能か}
    Wait -->|Yes| Queue[期限付きQueue]
    Wait -->|No| Deny
    Compare -->|同時利用可能| Duck[Ducking / Mix]
    Compare -->|高優先| Preempt[現在の所有者を停止・Pause]
    Preempt --> Grant
    Duck --> Active[録音・再生中]
    Grant --> Active
    Queue --> Recheck[所有権解放時に再判定]
    Recheck --> Validate
    Active --> Release[Release]
    Release --> Resume{割り込まれた処理は有効か}
    Resume -->|再開可能| Restore[Resume / 音量復帰]
    Resume -->|Cancel・期限切れ| Drop[Queueと状態を破棄]
```

## 競合規則で定義すること

- 警告音、通話、ナビ、音楽、外部Assistant、自社Assistantの優先関係
- Stop、Pause、Ducking、Mix、Queue待ちの選択
- Grant前の利用禁止と、Releaseする主体
- 割り込み後のResume、再要求、破棄
- CancelとGrantの競合、所有Process停止時の回収

ログには要求ID、所有者、優先度、判定、Routing、Grant・Release時刻、復帰結果を残します。
