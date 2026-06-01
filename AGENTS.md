# AGENTS.md (Root — OpenAI Codex / 共通エージェント指示)

このファイルは **OpenAI Codex CLI** がセッション開始時に自動読み込みするエントリーポイントです。  
Claude Code では `CLAUDE.md`、Gemini 系では `GEMINI.md` が同様の役割を担います。

> **CRITICAL — 絶対ルールの継承**
> 「外科手術の原則」「Windows 環境での UTF-8 強制」「誠実性と事実確認の原則」など、開発・運用の全ルールは **`GEMINI.md`** に定義されています。
> 作業を開始する前に、必ず `GEMINI.md` を読み込み、そこに記載されているルールを**最優先**で厳守してください。

## 1. ナレッジベース（brain）の運用

`brain/` では Andrej Karpathy 方式の自律的知識管理（第二の脳）を運用しています。

- `brain/` 内の整理・参照時は **`brain/AGENTS.md`**（Claude 利用時は `brain/CLAUDE.md` も同等）を必ず読むこと。
- 既存の `MEMORY.md` や `daily/` などのレガシーファイルは**絶対に破壊・移動しない**こと。

## 2. スキル（Skills）の利用

ナレッジ管理スキルは **`.agents/skills/`** に配置されています（Claude Code 向けのコピーは `.claude/skills/` にもあります）。

| スキル | 用途 | Codex での呼び出し例 |
|--------|------|----------------------|
| `kb-compile` | `brain/raw/` → `brain/wiki/` へ体系化 | `$kb-compile` |
| `kb-report` | `brain/wiki/` からレポート生成 | `$kb-report 今週のバグと解決策をレポートに` |
| `kb-lint` | `brain/wiki/` の矛盾・欠落を監査 | `$kb-lint` |

タスクに合うスキルがあれば、明示的に `$スキル名` で呼び出すか、説明文に沿って暗黙的に利用してください。一覧は `/skills` でも確認できます。

## 3. Windows / UTF-8

ターミナル操作時は `GEMINI.md` 先頭の Windows 安定動作ルールに従うこと（PowerShell の UTF-8 前置、`PYTHONUTF8=1` など）。

## 4. 他エージェントとの対応表

| 役割 | Codex | Claude Code | 備考 |
|------|-------|-------------|------|
| ルート指示 | 本ファイル `AGENTS.md` | `CLAUDE.md` | Codex は AGENTS.md を自動連結 |
| 絶対ルール | `GEMINI.md`（手動で必読） | 同上 | 全エージェント共通 |
| brain 運用 | `brain/AGENTS.md` | `brain/CLAUDE.md` | 内容は同等 |
| スキル配置 | `.agents/skills/` | `.claude/skills/` | SKILL.md 形式は共通 |
