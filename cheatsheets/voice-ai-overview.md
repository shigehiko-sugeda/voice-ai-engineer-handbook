# Voice AI Overview Cheatsheet

## 全体フロー

音声入力 → Wake Word → ASR → NLU → Dialog Manager → Skill / Domain → Vehicle API → TTS

## 各モジュールの役割

- Wake Word: 起動語を検出する。
- ASR: 音声を文字にする。
- NLU: 文字から意味を抽出する。
- Dialog Manager: 会話状態と次アクションを決める。
- Skill / Domain: Navigation、Phone、Media、Climateなどを担当する。
- Vehicle API: 車両機能との接点。
- TTS: 応答を音声で返す。

## 横断的な仕組み

Audio Focus、ログ、状態管理、タイムアウト、Fallback、権限、プライバシー。

## 評価指標

WER、Intent Accuracy、Slot F1、Latency、False Wake Rate、Miss Wake Rate、CPU、Memory。

## 認証で見るポイント

Voice Button、Mic Routing、Audio Focus、外部アシスタントとの競合、Timeout、Error handling。

## 用語ミニ辞典

- Intent: ユーザーの目的。
- Slot: 実行に必要な値。
- Context: 会話や車両状態の文脈。
- Barge-in: TTS中にユーザーが割り込んで話すこと。
