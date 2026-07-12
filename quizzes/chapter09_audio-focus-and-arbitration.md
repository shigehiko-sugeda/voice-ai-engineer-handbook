# Chapter 09 Audio FocusとArbitration クイズ

対応教材: [本文](../docs/09_audio-focus-and-arbitration.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

Audio Focusの優先度表があれば、割り込み後の復帰仕様は不要である。

<details>
<summary>ヒント</summary>

中断された音をどう戻すかも必要です。

</details>

## 問題2（選択）

高優先要求が現在の所有者を中断することを何といいますか。 A. Preemption / B. Grounding / C. Sampling / D. Confirmation

<details>
<summary>ヒント</summary>

所有権を奪う動作です。

</details>

## 問題3（穴埋め）

音を停止せず一時的に小さくする処理を（　　　）という。

<details>
<summary>ヒント</summary>

ナビやTTSとの共存で使います。

</details>

## 問題4（短答）

MicとSpeakerのAudio Focusを分けて管理する理由を説明してください。

<details>
<summary>ヒント</summary>

録音と再生で所有者が異なる場合があります。

</details>

## 問題5（ケース）

Siri終了後、自社TTSが期限切れでした。Queueをどう扱い、何をログに残しますか。

<details>
<summary>ヒント</summary>

待っていた要求を必ず再生するとは限りません。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。Resume、再要求、破棄、音量復帰まで定義します。
2. A。Preemptionです。
3. Ducking。PauseやStopとの使い分けが必要です。
4. 外部AssistantがMicを使い、警告音がSpeakerを使うなど、独立した競合判断が必要なためです。
5. TTSを破棄し、要求ID、期限、破棄理由、Focus解放、音量・ナビの復帰結果を残します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
