# Fortress AI Development Template 🚀

このテンプレートは、AIエージェント（**OpenAI Codex** / Claude Code / Gemini）を用いたソフトウェア開発において、**「上位1%の自律型エージェント開発環境」**を即座に構築するためのスターターキットです。

元OpenAIのAndrej Karpathy氏が提唱する「ファイルシステム上でのLLM運用（コンパイラ・図書館係）」の手法を取り入れつつ、開発を安全に進めるための「外科手術の原則」を統合しています。

## 🌟 特徴 (Features)

1. **自律的ナレッジベース（第二の脳）**
   AIを単なるチャットボットとしてではなく、情報を自ら整理し続ける「コンパイラ」として運用するための3フォルダ構造 (`brain/raw`, `wiki`, `reports`) を搭載しています。
2. **デグレを完全に防ぐ「外科手術の原則」**
   システムルートの `GEMINI.md` と各エージェント向けエントリ（`AGENTS.md` / `CLAUDE.md`）によって、AIに「既存コードの不要なリファクタリングを禁止し、ピンポイントで追加のみを行う」ことを強制し、プロジェクト破壊を防ぎます。
3. **秘伝のタレ「ナレッジ管理スキル」同梱**
   `.agents/skills/`（および Claude 向け `.claude/skills/`）に、知識を自動でコンパイル・監査する専用スキル（`kb-compile`, `kb-report`, `kb-lint`）をプリインストール済みです。

---

## 📁 フォルダ構成 (Directory Structure)

```bash
.
├── .agents/
│   └── skills/           # Codex / 共通 SKILL.md スキル（推奨の正本）
│       ├── kb-compile/
│       ├── kb-lint/
│       └── kb-report/
├── .claude/
│   └── skills/           # Claude Code 向け（.agents と同等の kb-* スキル）
├── .codex/
│   └── config.toml       # Codex プロジェクト設定（フォールバックファイル名など）
├── brain/                # 第二の脳（ナレッジベース）
│   ├── AGENTS.md         # brain 専用指示（Codex）
│   ├── CLAUDE.md         # brain 専用指示（Claude Code）
│   ├── raw/              # [手動] 会議メモ、ログ、記事を「ただ放り込む」場所
│   ├── reports/          # [AI自動] 問いに対する回答が蓄積される場所
│   └── wiki/             # [AI自動] 構造化されたナレッジが生成される場所
├── AGENTS.md             # Codex 向けルートエントリ（自動読み込み）
├── CLAUDE.md             # Claude Code 向けルートエントリ
└── GEMINI.md             # 全エージェント共通の絶対ルール
```

---

## ⚡ クイックスタート

```bash
git clone https://github.com/daisuke-ishikawa11/AI_DEV_TEMPLATE.git
cd AI_DEV_TEMPLATE
```

新規プロジェクトへ展開する場合は、テンプレート一式をコピーしたうえで `GEMINI.md` と `AGENTS.md` / `CLAUDE.md` の Owner・ドメイン記述を自分のプロジェクト向けに書き換えてください。

---

## 🚀 使い方 (How to Use)

### 1. インストール
新しいプロジェクトフォルダを作成し、このリポジトリの中身をすべてコピー（または上記 `git clone`）してください。

**OpenAI Codex CLI** を使う場合:

1. [Codex CLI](https://developers.openai.com/codex/) をインストールし、GitHub / OpenAI アカウントでログインする
2. リポジトリルートで `codex` を起動する（`AGENTS.md` と `.codex/config.toml` が自動適用される）
3. 初回セッションで「`GEMINI.md` を読んでルールを要約して」と依頼し、外科手術の原則などが読み込まれていることを確認する
4. ナレッジ整理は `$kb-compile`、レポートは `$kb-report …`、監査は `$kb-lint` を利用する

```bash
codex
# 動作確認の例:
# 「読み込んだ AGENTS.md と GEMINI.md の要点を箇条書きで」
# 「$kb-lint を実行して wiki の健康状態を確認して」
```

**Claude Code** を使う場合は、同ディレクトリで `claude` を起動し `/kb-compile` 等のスラッシュコマンドを利用します。

### 2. 日々の運用（情報のインプット）
会議の議事録、エラーログ、気になった外部記事、要件定義のメモなどは、**一切整理せずにただ `brain/raw/` フォルダの中に保存**してください。ファイル名も適当で構いません。

### 3. AIによるコンパイル（知識化）
週末や作業の区切りに、ターミナルでプロジェクトルートからAIを起動し、以下のコマンド（スキル）を打つだけです。

**OpenAI Codex の場合:**
```bash
codex
# プロンプト例:
$kb-compile
```

**Claude Code の場合:**
```bash
claude
> /kb-compile
```

AIが `brain/raw/` 内の雑多な情報をすべて読み込み、`brain/wiki/` へ綺麗に構造化されたマークダウンページとして自動生成・リンク付け（ウィキリンク）を行います。

### 4. 知識の引き出し

**Codex:**
```text
$kb-report 今週のバグとその解決策を、wikiの内容をもとにレポートにして
```

**Claude Code:**
```text
/kb-report 今週のバグとその解決策を、wikiの内容をもとにレポートにして
```

AIが `brain/wiki/` から関連知識を引き出し、`brain/reports/` にマークダウンファイルとして出力します。

---

## 🤖 エージェント別クイックリファレンス

| 項目 | OpenAI Codex | Claude Code | Gemini 等 |
|------|--------------|-------------|-----------|
| ルート指示 | `AGENTS.md`（自動） | `CLAUDE.md` | `GEMINI.md` を手動で最優先参照 |
| 絶対ルール | `GEMINI.md`（**必読**） | 同上 | 同上 |
| brain 指示 | `brain/AGENTS.md` | `brain/CLAUDE.md` | いずれかを参照 |
| スキル配置 | `.agents/skills/` | `.claude/skills/` + `.agents/skills/` | 環境に応じて |
| スキル呼び出し | `$kb-compile` または `/skills` | `/kb-compile` | プロンプトで SKILL 内容を参照 |

### 推奨週次フロー

1. 平日: メモ・ログ・記事を `brain/raw/` に放り込む（整理不要）
2. 週末: `$kb-compile`（または `/kb-compile`）で `brain/wiki/` を更新
3. 必要に応じて: `$kb-report` で調査・レポートを `brain/reports/` に出力
4. 品質確認: `$kb-lint` で矛盾・欠落を `brain/wiki/issues.md` に記録

---

## 🛡️ カスタマイズ (Customization)

プロジェクトの特性に合わせて、以下のファイルを修正してください。

- **`GEMINI.md`**: プロジェクト特有の開発ルールやコーディング規約を追記してください。
- **`AGENTS.md` / `CLAUDE.md` / `brain/AGENTS.md` / `brain/CLAUDE.md`**: Ownerの氏名やプロジェクトのドメイン名を書き換えてください。
- **`.codex/config.toml`**: 指示ファイルのフォールバック名やサイズ上限を調整できます。

### Codex で指示が効かないとき

- 作業ディレクトリが Git ルート（本リポジトリのルート）か確認する
- `AGENTS.md` が空でないか、`project_doc_max_bytes` で切り詰められていないか確認する（本テンプレートは 64 KiB に設定済み）
- スキルが一覧に出ない場合は Codex を再起動する（`.agents/skills/` の変更は再起動後に反映されることが多い）

---

## 📎 参考リンク

- 公開リポジトリ: https://github.com/daisuke-ishikawa11/AI_DEV_TEMPLATE
- [Codex — Custom instructions with AGENTS.md](https://developers.openai.com/codex/guides/agents-md)
- [Codex — Agent Skills](https://developers.openai.com/codex/skills/)

---

*This template was generated to empower AI-native development. Stop treating LLMs as search engines, start treating them as compilers.*
