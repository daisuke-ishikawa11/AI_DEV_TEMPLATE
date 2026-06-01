# CLAUDE.md (Vault Operating Manual)

このファイルはエージェントに対する Vault（brain）全体の指示書です。
AIとして `brain/` にアクセスする際はこの指示に従ってください。

> [!CAUTION] 
> **ルールの優先順位**
> 本ファイルは「ナレッジベース（brain）の整理・運用」に特化したサブマニュアルです。
> システム開発における絶対原則（外科手術の原則、UTF-8強制、誠実性、各種運用ルールなど）については、プロジェクトルートの `GEMINI.md` のルールが常に最優先で適用されます。本ファイルの内容が全体ルールと競合する場合は、全体ルールを優先してください。

## Owner
- 氏名: ユーザー
- 業務ドメイン: FX・暗号資産のクオンツトレード開発、AI合議システムの運用
- 言語: 日本語で書く。引用や固有名詞のみ英語可

## Vault Layout (ハイブリッド構造)
現在の `brain/` は、既存のシステムログ（非破壊）とKarpathy方式の第二の脳（自動整理）のハイブリッド構造です。

### Karpathy System (自動整理エリア)
- `brain/raw/`      ← 整理しない、私が放り込むだけの場所（生ログ、外部記事など）
- `brain/wiki/`     ← あなた (エージェント) が書く、構造化された知識ベース
- `brain/reports/`  ← あなた (エージェント) が問いに対する答えをファイルとして残す場所

### Legacy System (既存運用エリア - 破壊禁止)
- `brain/daily/`    ← 既存の日次レポート群。**編集・移動・削除禁止**。
- `brain/MEMORY.md` 等 ← 既存のコアファイル。**手動指示がない限り移動・削除禁止**。

`raw/` および Legacy System のファイルは原則として **読み取り専用**（削除・改名・移動禁止）です。
`wiki/` と `reports/` はあなたが自由に作成・編集して構いません。

## Compile Rules (wiki/ への書き方)
- 1 概念につき 1 ファイル (kebab-case、例: ai-consensus-rules.md)
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
