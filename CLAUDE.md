# CLAUDE.md (Root System Instructions for Claude Code)

このファイルは、Claude Code 等のAIエージェントが `FX_AI_SYSTEM` にアクセスした際に最初に読み込まれるエントリーポイントです。

> [!CAUTION] 
> **絶対ルールの継承 (CRITICAL)**
> 本システムにおける「外科手術の原則」「Windows環境でのUTF-8強制」「誠実性と事実確認の原則」など、システム開発と運用の全ルールは `GEMINI.md` に定義されています。
> 作業を開始する前に、必ず `GEMINI.md` を読み込み、そこに記載されているルールを**最優先**で厳守してください。

## 1. ナレッジベース（brain）の運用について
本システムでは、`brain/` フォルダ内で「Andrej Karpathy方式」に基づく自律的知識管理システム（第二の脳）を運用しています。
`brain/` フォルダ内のファイルの整理や参照を行う際は、必ず `brain/CLAUDE.md` を読み込み、そこに記載されているハイブリッド運用ルール（raw / wiki / reports）に従ってください。
- 既存の `MEMORY.md` や `daily/` などのレガシーファイルは**絶対に破壊・移動しない**こと。

## 2. スキル (Skills) の利用
Claude Code から利用可能な全てのスキル（`kb-compile`, `kb-report`, `kb-lint` などのナレッジ系スキルのほか、`web3-polymarket`, `quant-finance-research`, `systematic-debugging` などの既存の開発・分析スキル群）は `.claude/skills/` 配下にインストールされています。
タスクを実行する際は、まず `/` (スラッシュ) コマンドでこれらのスキルが利用できないか確認し、積極的にスキルをロードして活用してください。
