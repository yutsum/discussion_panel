# Discussion Panel Skills

このリポジトリは、構造化ブレインストーミングのためのリポジトリローカル Codex スキルパックです。

3つの要素で構成されています:

- `VS Code / Codex` — インタラクションUI
- `.codex/skills/` — ペルソナとオーケストレーションスキル
- `brainstorms/` — 出力の保存先

## 収録スキル一覧

- `brainstorm-setup`: ディスカッション開始前に参加者を選択する
- `brainstorm-research-brief`: ディスカッション前に共有外部コンテキストを収集する
- `brainstorm-moderator`: セッション全体をオーケストレーションする
- `visionary-founder`: 非対称な機会を探す
- `steve-jobs`: フォーカス・センス・エンドツーエンドのプロダクト品質を問い詰める
- `jensen-huang`: プラットフォームレバレッジ・エコシステム・フルスタック優位性を問い詰める
- `elon-musk`: ファーストプリンシプルによる再設計・スピード・垂直統合を問い詰める
- `analyst`: 専用ワークスペースに関連資料を保存し、最新の事実を確認してブリーフを更新する。リサーチが必要な場面を除き、通常の議論ラウンドには参加しない
- `contrarian`: コンセンサスの前提を崩す
- `systems-architect`: 運用上の複雑さを問い詰める
- `customer-psychologist`: モチベーション・信頼・採用行動に注目する
- `strategist-sunzi`: ポジショニングと間接的優位性を探る
- `synthesis-editor`: 発散した出力を意思決定メモに変換する

## リポジトリ構成

```text
.codex/
  skills/
    brainstorm-setup/
    brainstorm-research-brief/
    brainstorm-moderator/
    visionary-founder/
    steve-jobs/
    jensen-huang/
    elon-musk/
    analyst/
    contrarian/
    systems-architect/
    customer-psychologist/
    strategist-sunzi/
    synthesis-editor/
brainstorms/
prompts/
```

## 使い方

### Claude Code（CLI）

Claude Code は `.codex/skills/` を自動的に読み込みます。このディレクトリでセッションを開始します:

```sh
cd /path/to/discussion_panel
claude
```

続いて `/brainstorm` スラッシュコマンドを使います:

```text
/brainstorm B2B SaaSスタートアップはエンタープライズ営業とセルフサーブ成長のどちらを優先すべきか？
```

参加者をインラインで指定することもできます:

```text
/brainstorm [トピック] with steve-jobs, jensen-huang, elon-musk, analyst
```

省略した場合はデフォルトセット（`visionary-founder`、`analyst`、`contrarian`、`systems-architect`、`customer-psychologist`、`strategist-sunzi`）が使われます。

各ペルソナはそれぞれ独立したコンテキストを持つサブエージェントとして動作します。モデレーターがターンを調整し、各ラウンドを `brainstorms/<topic-slug>/transcript.md` に追記したあと停止します。ユーザーは返答・方向転換・無言での続行を選べます。

ディスカッション後に統合を依頼する場合:

```text
/brainstorm brainstorms/saas-growth-strategy/transcript.md のトランスクリプトを統合してください
```

### Codex / VS Code（Codex 有効）

1. このリポジトリを Codex または Codex 有効の VS Code で開く
2. チャットを開始する
3. 以下のいずれかを選ぶ:
   - `brainstorm-setup` で先に参加者を選択する
   - 現在の外部事実が必要な場合は先に `brainstorm-research-brief` を使う
   - `brainstorm-moderator` を直接使ってデフォルト参加者セットで始める
4. 具体的なトピックを入力する
5. 各参加者が1ラウンド発言する
6. ディスカッションを続けるか、後から別途サマリーを依頼する

`analyst` 以外の参加者は `brainstorms/<topic-slug>/agent-notes/` 以下にプライベートなスクラッチパッドを持てます。これらはコンパクトな作業メモ用であり、トランスクリプトには含まれません。

`analyst` は `brainstorms/<topic-slug>/analyst/` に専用ワークスペースを持ちます:

- `brief.md` — 全参加者が参照する共有ファクトベース
- `sources/` — ユーザーが渡したファイルや保存対象の資料
- `index.md` — 保存資料の一覧と由来
- `notes.md` — analyst 専用の抽出メモ
- `qa-log.md` — 回答履歴

### 参加者を先に選ぶ

このプロンプトをそのまま使えます:

```text
Use the `brainstorm-setup` skill.

Topic:
「生成AIを活用した法人向け学習プラットフォームの戦略」

Let me choose which participants to include before the discussion starts.
After I choose, start the brainstorming session with only those participants.
Record the discussion to a transcript file.
```

`prompts/select-participants.md` から始めることもできます。

### 共有リサーチから始める

このプロンプトをそのまま使えます:

```text
Use the `brainstorm-research-brief` skill.

Topic:
「生成AIを活用した法人向け学習プラットフォームの戦略」

Search for current official and primary sources.
Create a shared brief for all participants.
Save it to `brainstorms/saas-growth-strategy/analyst/brief.md`.
Then continue into brainstorming using that shared context.
```

議論中にファクトチェックや情報更新が予想される場合は、参加者に `analyst` を含めてください。`analyst` は通常ラウンドでは発言せず、ブリーフ更新・検証依頼・資料取り込み・参加者が記憶に頼らず根拠が必要な場面でのみ参加します。

`analyst` にローカル資料を使わせたい場合は、対象ファイルのパスまたは内容を渡し、該当ラウンド前に `brainstorms/<topic-slug>/analyst/sources/` に保存します。

### 最小プロンプト

このプロンプトをそのまま使えます:

```text
Use the `brainstorm-moderator` skill.

Topic:
「AIを使った古典兵法・経営戦略の学習アプリ」

Run a multi-persona brainstorming session using all available persona skills.
Keep it turn-based and simple.
Each participant should speak in sequence for 2-4 sentences.
Each participant should refer to prior speakers by name when relevant.
Do not include moderator lines in the transcript.
If a claim depends on current company realities, market facts, product facts, financial status, or brand assets, consult the analyst brief first and ask `analyst` to refresh it if needed.
Record the discussion to `brainstorms/<topic-slug>/transcript.md`.
After one round, stop and let me optionally reply or continue silently.
Do not summarize unless I explicitly ask.
Append each new round at the end of the transcript file.
```

`prompts/brainstorm.md` から始めることもできます。

### 前回のセッションを続ける

以前のディスカッションから継続する場合は、既存のトランスクリプトを貼り付けて継続プロンプトを使います:

```text
Use the `brainstorm-moderator` skill.

Continue this brainstorming session from the prior transcript.

Topic:
<トピック>

Prior transcript:
<既存のトランスクリプトまたはサマリーを貼り付ける>

Continue with one more round, append it to the transcript, then stop.
```

`prompts/continue-brainstorm.md` から始めることもできます。

## 推奨プロンプトスタイル

トピックは絞り込み、制約を明示してください。

良い例:

- `日本のSME向けB2B AI営業コーチング`
- `3ヶ月の資金で作る日常戦略学習コンシューマーアプリ`
- `有料獲得なしの社内リサーチツール`

弱い例:

- `AIスタートアップのアイデア`
- `教育系で何か`

追加すると有効な制約:

- ターゲットユーザー
- 地域
- 時間軸
- 流通チャネル
- 予算
- 技術的制約

## モデレーターが生成するもの

想定フローは以下のとおりです:

1. ターゲットアウトカムを1文で明確にする
2. 外部事実が重要な場合は analyst brief を読み込む
3. 前回のトランスクリプトがあれば継続する
4. 参加者ごとのプライベートスクラッチパッドと analyst ワークスペースを更新する
5. 各ペルソナのターンを収集する
6. 事実更新・資料追加・不確かな記憶への依存が起きた場合は analyst brief を再取得する
7. ラウンドをトランスクリプトに追記する
8. 関連する場合は参加者が先の発言者を名指しで参照できるようにする
9. 止まってユーザーがコメントするか次の動きを選べるようにする
10. 統合は明示的に依頼されたときのみ行う

`analyst` が参加している場合は、通常の議論の声としてではなくオンデマンドのリサーチ機能として扱います。参加者は企業固有の最新の主張について自身の記憶を信頼せず、analyst ワークスペース由来の brief を引用するか `analyst` に更新を依頼することが期待されます。

ライブラウンドの出力形式:

- `Topic`
- `Analyst workspace`
- `Shared brief file`
- `Scratchpad directory`
- `Transcript file`
- `Round N transcript`
- `User options`

最終統合は、ユーザーが明示的に依頼したときのみ `synthesis-editor` に渡します。

## 出力の保存

各ブレインストーミングセッションは `brainstorms/` 以下の独自サブディレクトリに保存します。

推奨ファイル構成:

- `transcript.md` — 累積ディスカッション履歴
- `analyst/brief.md` — 共通リサーチコンテキスト
- `analyst/sources/` — ユーザー提供ファイルと保存資料
- `analyst/index.md` — analyst の資料台帳
- `agent-notes/` — 参加者ごとのプライベートスクラッチパッド
- `notes.md` — 任意の生メモ
- `decision-memo.md` — 任意の統合アウトプット
- `experiments.md` — フォローアップ検証事項

例:

```text
brainstorms/
  strategy-learning-app/
    analyst/
      brief.md
      index.md
      notes.md
      qa-log.md
      sources/
    agent-notes/
    transcript.md
    notes.md
    decision-memo.md
    experiments.md
```

## 設計思想

これらのスキルは、ペルソナを安定した意思決定レンズを持つロールプレイキャラクターとして扱うときに最もよく機能します。

推奨:

- 認識可能な話し方のスタイル
- 安定した評価レンズ
- 各ターンでの具体的な判断
- 関連する場合は先の発言者への明示的な参照

避けるべきこと: 空虚な模倣、キャッチフレーズ多用のパロディ、明確な意思決定レンズを持たないペルソナ。
