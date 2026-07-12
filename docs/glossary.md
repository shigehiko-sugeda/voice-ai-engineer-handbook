# 用語集

各章で使う主要用語を、車載音声システムの文脈で説明します。製品やサービスによって定義が異なる場合は、対象システムの仕様を優先してください。

## 音声入力・起動

- **Sampling Rate**: 1秒間に音を測定する回数。単位はHz。
- **SNR（Signal-to-Noise Ratio）**: 目的の音声と雑音の強さの比。一般に、値が大きいほど音声を識別しやすい。
- **VAD（Voice Activity Detection）**: 入力音に人の発話が含まれる区間を検出する処理。
- **AEC（Acoustic Echo Cancellation）**: スピーカーの再生音がマイクへ回り込むエコーを抑える処理。
- **Beamforming**: 複数のマイクを使い、特定方向の音を強調する処理。
- **Wake Word**: 音声機能の受付を開始するための起動語。
- **False Wake**: Wake Wordではない音を起動語と誤判定すること。
- **Miss Wake**: 発話されたWake Wordを検出できないこと。
- **Pre-roll**: Wake Word検出直前の音声を一時保持し、後続発話の先頭欠けを防ぐ仕組み。

## 音声認識

- **ASR（Automatic Speech Recognition）**: 音声をテキストへ変換する技術またはコンポーネント。
- **Streaming ASR**: 音声を受け取りながら、認識結果を逐次更新する方式。
- **Partial Result**: 発話途中に出力される未確定の認識結果。
- **Final Result**: 発話終了の判定後に確定した認識結果。
- **Endpoint Detection**: 発話が終了した時点を検出する処理。
- **Confidence**: 認識・分類結果の信頼度を表す値。値の定義や比較方法は実装ごとに確認する。
- **WER（Word Error Rate）**: 単語の置換、削除、挿入を基にASRの誤りを測る指標。

## 意味理解

- **NLU（Natural Language Understanding）**: テキストからユーザーの目的やパラメータを抽出する技術またはコンポーネント。
- **Intent**: ユーザーが実行したい目的。例は温度設定、電話発信、目的地設定。
- **Slot**: Intentの実行に必要な型付きパラメータ。例は温度、連絡先、目的地。
- **Entity**: 発話中に現れる、人名、場所、数値などの意味を持つ表現。
- **Domain**: Climate、Phone、Navigation、Mediaなどの機能領域。
- **Context**: 直前の対話、画面、車両状態など、発話の解釈を補う情報。
- **Intent Accuracy**: 正しいIntentへ分類できた発話の割合。
- **Slot F1**: Slot抽出のPrecisionとRecallを一つにまとめた評価指標。

## 対話管理

- **Dialog Manager**: NLU結果と現在の状態から、確認、追加質問、実行などの次の動作を決めるコンポーネント。
- **Session**: 音声機能の起動から終了までを関連づける一連の対話単位。
- **State**: Listening、Confirming、Executingなど、システムが現在何をしているかを表す状態。
- **Event**: ユーザー発話、Timeout、実行結果など、Stateを変化させる出来事。
- **Confirmation**: 操作の実行前などに、ユーザーへ内容を確認すること。
- **Disambiguation**: 複数の候補から対象を一つに絞ること。
- **Multi-turn interaction**: 追加質問や確認を含む、複数回の発話による対話。
- **Barge-in**: TTS再生中のユーザー発話を受け付け、再生を中断して入力へ切り替えること。
- **Timeout**: 定めた時間内に入力や応答が得られず、待機を終了すること。
- **Fallback**: 認識失敗、通信断、Timeoutなどの際に、安全な終了や代替処理へ移ること。

## 音声出力・競合制御

- **TTS（Text-to-Speech）**: テキストを音声へ変換する技術またはコンポーネント。
- **Prompt**: TTSや画面を通してユーザーへ提示する応答内容。
- **Audio Focus**: マイク入力または音声再生を使用する権利。
- **Arbitration**: 複数の機能が音声資源を要求したとき、優先する要求を決める処理。
- **Ducking**: TTSや案内を聞きやすくするため、他の音を停止せず一時的に小さくすること。
- **Preemption**: 高優先度の要求が、現在の音声資源利用を中断すること。
- **Mic Routing**: マイク信号を自社ASR、外部アシスタント、通話などの利用先へ接続する制御。

## 車両連携・アーキテクチャ

- **Vehicle API**: 音声機能などの上位機能から車両機能を利用するためのインターフェース。
- **Domain Service**: 機能領域ごとの制約確認と、Vehicle APIへの変換を担う層。
- **ECU（Electronic Control Unit）**: 車両の特定機能を制御する電子制御ユニット。
- **Pipeline**: マイク入力から意味理解、車両実行、応答までをつなぐ処理の流れ。
- **IPC（Inter-Process Communication）**: 異なるプロセス間で要求やEventを交換する仕組み。
- **Queue**: 処理待ちの要求やEventを保持する待ち行列。
- **Backpressure**: 処理能力を超える入力を抑制し、QueueやMemoryの増加を防ぐ仕組み。
- **Request ID**: 非同期の要求と応答を対応づける識別子。
- **Idempotency**: 同じ要求が重複しても、結果を不正に重複させない性質。
- **Watchdog**: ProcessやComponentの停止、無応答を検知する監視機構。
- **Retry**: 一時的な失敗の後に処理を再実行すること。
- **Recovery**: 障害後に状態や資源を整え、利用可能な状態へ復旧すること。
- **Latency**: 定義した処理の開始から完了までにかかる時間。

## LLM連携

- **LLM（Large Language Model）**: 大量のテキストから学習し、自然言語の理解や生成を行うモデル。
- **Tool Calling**: LLMが利用する機能名と引数を、構造化された候補として出力する仕組み。
- **RAG（Retrieval-Augmented Generation）**: 検索した資料を入力へ加え、その情報を使って回答を生成する構成。
- **Agent**: 複数の判断やTool利用を組み合わせてタスクを進める処理主体。
- **Grounding**: 回答を取得資料や観測結果などの根拠へ結びつけること。
- **Guardrail**: LLMの入出力やTool実行を検査・制限する仕組み。

## ログ・レビュー

- **Correlation ID**: 複数のComponentやProcessをまたぐ一連の処理を追跡する識別子。
- **Traceability**: 要件、設計、実装、試験、結果の対応関係を追跡できる状態。
- **Evidence**: 試験結果を説明するログ、動画、設定、版情報などの証跡。
- **Fallback / Recoveryログ**: 失敗理由、代替処理、再試行、復旧結果を確認するための記録。
