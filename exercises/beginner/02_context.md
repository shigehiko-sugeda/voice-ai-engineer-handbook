# Context演習

対応教材: [NLU](../../docs/05_nlu.md) / [Dialog Manager](../../docs/06_dialog-manager.md)

## ケース

ユーザーが「もう少し下げて」と発話しました。ASR結果は正しいものとします。

直前の操作候補は次の3つです。

- エアコンを22度に設定した。
- 音楽の音量を20へ設定した。
- 地図の表示縮尺を変更した。

## 課題

1. 3つの直前操作ごとに、想定するDomain、Intent、必要なContextを書いてください。
2. そのContextを保存するSessionと有効期限を提案してください。
3. Contextだけで一意に決まらないケースと、その確認Promptを書いてください。
4. ASR結果だけでは判断できない理由を説明してください。

<details>
<summary>ヒント1</summary>

「下げる」の対象、変化量、単位、現在値が必要です。

</details>

<details>
<summary>ヒント2</summary>

古いContextを使うリスクも考えてください。車両再起動や新しいWake Word起動後まで残してよいでしょうか。

</details>

## 自己チェック

- 3種類の解釈をDomain別に書いたか。
- Contextの生成元、所有者、有効期限を示したか。
- 曖昧時の確認とキャンセルを考慮したか。
- 前SessionのContextを誤利用しないか。
