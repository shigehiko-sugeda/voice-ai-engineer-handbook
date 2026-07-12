# Chapter 07 TTS クイズ

対応教材: [本文](../docs/07_tts.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

TTS合成完了とSpeakerでの再生完了は同じEventとして扱える。

<details>
<summary>ヒント</summary>

合成後にQueue待ちやAudio Focus取得があります。

</details>

## 問題2（選択）

背景音を停止せず一時的に小さくする処理はどれですか。 A. Ducking / B. Endpoint / C. Slot / D. VAD

<details>
<summary>ヒント</summary>

TTSを聞きやすくする音量制御です。

</details>

## 問題3（穴埋め）

ユーザーへ提示する応答文を（　　　）という。

<details>
<summary>ヒント</summary>

TTSへ渡す内容です。

</details>

## 問題4（短答）

古いSessionのTTSをQueueから除去する理由を説明してください。

<details>
<summary>ヒント</summary>

キャンセル後に成功案内が流れる状況を考えます。

</details>

## 問題5（ケース）

Vehicle APIが失敗したのに「22度に設定しました」と再生されました。どの境界を修正しますか。

<details>
<summary>ヒント</summary>

応答文を生成する時点の入力を確認します。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。合成完了と再生開始・完了は別Eventです。
2. A。Duckingです。
3. Prompt。実行結果と一致させる必要があります。
4. 現在の状態と矛盾する遅延再生や誤案内を防ぐためです。
5. Vehicle APIの実行結果を確認してからPromptを生成し、Session/Request IDで結果とTTSを対応づけます。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
