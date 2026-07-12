# Chapter 10 認証観点 クイズ

対応教材: [本文](../docs/10_certification.md)

先に自分の回答を書き、必要な場合だけヒントを開いてください。解答と解説は全問回答後に確認します。

## 問題1（○×）

非公開の認証要件が不明な場合、一般的な実装から要件値を推測して試験すればよい。

<details>
<summary>ヒント</summary>

教材の注意事項を確認してください。

</details>

## 問題2（選択）

認証試験の再現性に最も必要な組み合わせはどれですか。 A. 成否だけ / B. 要件・対象版・試験・証跡 / C. TTS文だけ / D. WERだけ

<details>
<summary>ヒント</summary>

Traceabilityを考えます。

</details>

## 問題3（穴埋め）

マイク信号を自社ASRや外部Assistantへ接続する制御を（　　　）という。

<details>
<summary>ヒント</summary>

Micの利用先を切り替えます。

</details>

## 問題4（短答）

Voice Button短押し・長押しの境界値試験が必要な理由を説明してください。

<details>
<summary>ヒント</summary>

境界で二重起動や誤起動が起き得ます。

</details>

## 問題5（ケース）

外部AssistantがTimeoutして画面は閉じましたが、音楽が戻りません。確認する証跡を挙げてください。

<details>
<summary>ヒント</summary>

UI終了だけでAudio資源が戻るとは限りません。

</details>

## 解答と解説

<details>
<summary>回答後に開く</summary>

1. ×。正式資料と担当者への確認事項として管理し、推測で断定しません。
2. B。根拠資料、製品版、試験条件、証跡の対応が必要です。
3. Mic Routing。所有者とGrant/Releaseも記録します。
4. 判定時間付近で起動対象が不安定になり、自社・外部Assistantの同時起動や誤起動を防ぐためです。
5. 状態遷移、Mic Routing、Audio Focus Release、音量復帰、Timeout理由、各時刻、対象版を確認します。

</details>

## 自己チェック

- 用語だけでなく、入力・処理・出力を説明できたか。
- 正常系だけでなく、Timeout、競合、ログ、Fallbackを考えたか。
- 不確かな内容を断定せず、確認方法を示したか。
