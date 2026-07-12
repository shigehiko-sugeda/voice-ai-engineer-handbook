# 認証リスク抽出演習

対応教材: [認証観点](../../docs/10_certification.md) / [認証チートシート](../../cheatsheets/certification-cheatsheet.md)

## レビュー対象

外部アシスタント連携について、開発チームから次の説明を受けました。

- Voice Button短押しで自社Assistant、長押しで外部Assistantを起動する。
- 長押し判定の時間は「だいたい1秒」で、境界値試験は未定。
- 外部Assistant起動要求と同時にMic Routingを切り替える。
- 自社ASRとWake Wordの停止完了は待たない。
- 通話中とナビ案内中の動作は未定。
- 通信Timeout後は画面を閉じるが、MicとAudio Focusの解放処理は記載されていない。
- 試験ログには成功・失敗だけを記録し、車両・ソフトウェア・資料の版は残さない。

## 課題

次の表でリスクを整理してください。

| 分類 | 確定している事実 | 未確定・要確認事項 | 想定リスク | 必要な試験・証跡 |
|---|---|---|---|---|

Voice Button、Wake Word、Mic Routing、Audio Focus、Timeout、再接続を必ず含めてください。非公開仕様の内容は推測せず、確認先が必要な事項として書いてください。

<details>
<summary>ヒント1</summary>

要件の値そのものではなく、「どの正式資料のどの版で確認するか」を書けます。

</details>

<details>
<summary>ヒント2</summary>

起動成功だけでなく、同時起動、境界値、通話中、通信断、Timeout後の所有権を確認してください。

</details>

## 自己チェック

- 公開・参照可能な情報と未確定事項を分けたか。
- 短押し・長押しの境界値試験を含めたか。
- 自社機能の停止完了と外部Assistant起動の順序を確認したか。
- Mic RoutingとAudio Focusの取得・解放を証明できるか。
- 要件、設計、試験、証跡、対象版を対応づけたか。
