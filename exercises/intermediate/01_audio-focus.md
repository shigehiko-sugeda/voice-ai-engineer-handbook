# Audio Focus競合演習

対応教材: [Audio FocusとArbitration](../../docs/09_audio-focus-and-arbitration.md) / [競合フロー図](../../diagrams/audio-focus-arbitration.md)

## ケース

次のEventが短時間に発生しました。

1. ナビ案内がSpeakerを使用中。
2. Siriが起動し、MicとSpeakerを要求。
3. 自社Assistantの「22度に設定しました」というTTSが再生待ち。
4. 2秒後に安全警告音が再生要求。
5. Siriは5秒後に終了。自社TTSの有効期限は3秒。

## 課題

次の表を完成させてください。

| Event | Mic所有者 | Speaker所有者 | ナビ | 自社TTS | 判定理由 |
|---|---|---|---|---|---|
| ナビ案内開始 |  |  |  |  |  |
| Siri起動 |  |  |  |  |  |
| 自社TTS要求 |  |  |  |  |  |
| 安全警告音 |  |  |  |  |  |
| Siri終了 |  |  |  |  |  |

さらに、Ducking、Pause、Stop、Queue、破棄、復帰の規則と、必要なログを設計してください。

<details>
<summary>ヒント1</summary>

MicとSpeakerの所有権は別々に考えます。安全警告音を通常のTTSと同じ優先度にしてよいでしょうか。

</details>

<details>
<summary>ヒント2</summary>

Siri終了時には、自社TTSがすでに期限切れです。「待っていたから再生する」とは限りません。

</details>

## 自己チェック

- Grant前に録音・再生していないか。
- 安全警告音の優先度を考慮したか。
- 自社TTSの期限切れを処理したか。
- Siri終了後のナビと音量の復帰を定義したか。
- Request ID、所有者、判定、Release理由をログへ含めたか。
