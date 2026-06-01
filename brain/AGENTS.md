# AGENTS.md (Vault Operating Manual — brain/)

このファイルは **brain/**（Vault）全体のエージェント向け指示書です。  
OpenAI Codex は `brain/` 配下で作業する際、ルートの `AGENTS.md` に続いて本ファイルを読み込みます。  
Claude Code 利用時は `brain/CLAUDE.md` も同内容の参照先です。

> **ルールの優先順位**
> 本ファイルはナレッジベースの整理・運用に特化したサブマニュアルです。
> 外科手術の原則、UTF-8 強制、誠実性などの絶対原則は、プロジェクトルートの **`GEMINI.md`** が常に最優先です。競合時は `GEMINI.md` を優先してください。

## Owner

- 氏名: ユーザー
- 業務ドメイン: FX・暗号資産のクオンツトレード開発、AI合議システムの運用
- 言語: 日本語で書く。引用や固有名詞のみ英語可

## Vault Layout (ハイブリッド構造)

現在の `brain/` は、既存のシステムログ（非破壊）と Karpathy 方式の第二の脳（自動整理）のハイブリッド構造です。

### Karpathy System (自動整理エリア)

- `brain/raw/` — 整理しない。ユーザーが放り込むだけ（生ログ、外部記事など）
- `brain/wiki/` — エージェントが書く構造化ナレッジベース
- `brain/reports/` — 問いへの回答をファイルとして残す場所

### Legacy System (既存運用エリア — 破壊禁止)

- `brain/daily/` — 既存の日次レポート群。**編集・移動・削除禁止**
- `brain/MEMORY.md` 等 — 既存コアファイル。**手動指示がない限り移動・削除禁止**

`raw/` および Legacy System のファイルは原則 **読み取り専用**（削除・改名・移動禁止）です。  
`wiki/` と `reports/` はエージェントが自由に作成・編集して構いません。

## Compile Rules (wiki/ への書き方)

- 1 概念につき 1 ファイル (kebab-case、例: `ai-consensus-rules.md`)
- frontmatter に `sources: [raw/xxx.md]` と書く
- 必ず 1 段落以内の定義から始める
- 関連ページへの `[[wikilink]]` を最低 2 つ
- 索引: `wiki/index.md` に概念名と 1 行サマリを追記

## Report Rules (reports/ への書き方)

- ファイル名: `{YYYY-MM-DD}_{topic-slug}.md`
- frontmatter に `used_wiki_pages: [...]` を必ず書く
- 結論を冒頭に置く (リード)
- 出典として参照した wiki ページへのリンクを末尾に列挙

## Style

- 体言止めと「ですます」を混ぜすぎない
- 箇条書きと散文のリズムをつける
- 業務固有の用語 (例: Alpha-Convergence, Fortress) はそのまま使う
- 抽象論より具体例を優先する

## Lint Rules (週次の Health Check で確認)

- 矛盾する記述があれば `issues.md` に列挙
- 出典なしの数値があれば指摘
- 3 個以上の raw で言及されている概念に専用ページがなければ作成提案

## Codex スキル連携

- raw → wiki: ルートから `$kb-compile`
- wiki → reports: `$kb-report {問い}`
- wiki 監査: `$kb-lint`

スキル定義はリポジトリルートの `.agents/skills/` を参照してください。
