# Diagrams

処理順序、状態遷移、所有権、プロセス境界をMermaidで視覚化します。図はGitHub上で読めるよう、Mermaidコードブロックを埋め込んだMarkdownとして管理します。

## 図の一覧

- [Voice AI全体フロー](voice-ai-overview.md): 起動から車両実行、TTS応答まで
- [Dialog State](dialog-state.md): 対話の正常系、確認、Timeout、キャンセル
- [Audio FocusとArbitration](audio-focus-arbitration.md): 音声資源の要求、競合、割り込み、復帰
- [プロセス構成](process-architecture.md): Process、IPC、Queue、監視の関係

## チートシートとの役割分担

- `diagrams/`: 処理や状態が「どう流れるか」を示す。
- `cheatsheets/`: 設計・ログ・不具合を「何で確認するか」を示す。
- `../assets/voice-ai-overview-cheatsheet.png`: README冒頭で全体を俯瞰する補助画像。

詳細な説明は`docs/`、レビュー時の確認項目は`cheatsheets/`を参照してください。
