# 【最優先】Windows 安定動作ルール（作業前確認必須）

> [!IMPORTANT]
> このファイルは作業開始前に必ず読み、全てのターミナル操作に適用してください。
> 本ルールはブレイン（ナレッジベース）の `windows-utf8-stability` にも登録されています。

# Antigravity Windows 安定動作ルール

> [!IMPORTANT]
> **Hyperliquid 資金認識の鉄則 (Unified Account)**
> ユーザーは「統合口座 (Unified Account)」を使用しています。
> - Perp（先物）残高が $0 であっても、**Spot（現物）に残高（USDC等）があれば取引可能**です。
> - エージェントは「Perp残高 $0 = 取引不能」と誤認してはなりません。
> - 必ず `libs/hl_quant_lib.py` の `get_account_value()` を使用して合算値を評価してください。
> - 資金移動（Transfer）を促す提案は、Spot側も空である場合に限定してください。

ターミナルUTF-8強制: run_commandツール使用時、以下を必ず適用すること。
これを怠ると日本語の文字化けが原因で、無限ループやクラッシュが起きる。

- PowerShell: コマンド冒頭に `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8;` を付与
- CMD: コマンド冒頭に `chcp 65001 > nul &&` を付与
- Python実行時: `$env:PYTHONUTF8=1;` を前置（PowerShell環境）
- Docker実行時: `environment` に `PYTHONUTF8: 1` を必ず含める
- ファイル書き込み(Set-Content等): `-Encoding UTF8` を必ず明示

---

# 誠実性と事実確認の原則 (Honesty & Verification)

> [!CAUTION]
> ユーザーへの回答において「ごまかし」「未確認の推測」「事実の隠蔽」は、技術的ミス以上に重大なルール違反とみなされます。

1. **「思い込み」の禁止**: システムの挙動や数値の不備（N/Aや0など）に対し、「稼働初期だから」「仕様だから」といった推測のみで回答することを厳禁とする。
2. **一次ソース（生ログ）確認の義務化**: 回答の前に必ず `fx_consensus_log.jsonl` や `fx_training_data.jsonl` などの生ログを確認し、エビデンス（事実）を提示しなければならない。
3. **無知・不備の即時開示**: 不明点やコードの不具合の可能性を察知した際は、それをごまかさず、即座にユーザーへ報告し、調査・修正を提案しなければならない。
4. **「最短ルート」の誘惑に負けない**: 「説明で済ませる」ことよりも「不具合を特定し修正する」ことを優先し、安易な回答で責任を回避してはならない。
5. **調査の徹底 (Exhaustive Investigation)**: ユーザーの質問や指摘に対し、回答する前に必ずエージェントスキル（ログ確認、コード精読、検証スクリプト実行等）を駆使して深層まで調査を行うこと。調査なしの回答は、事実上の「さぼり」であり、重大なルール違反とみなす。
6. **ナレッジベース（Brain）の優先参照**: 作業開始前および不明点がある際は、必ず `d:\FX_AI_SYSTEM\brain` 内の記録（特に `MEMORY.md`）を確認すること。過去の教訓を無視した作業は、システムへの理解不足を招く重大な怠慢とみなす。

---

# Polymarket 運用ルール (2026 移行期)

- **V1/V2 の厳別**: 2026年4月28日の完全移行までは、`libs/polymarket_v2_config.py` の `DOMAIN_VERSION` を原則 `"1"` とし、`signature_type=0` (EOA) を使用すること。
- **資産の監視**: V1 運用中は `USDC.e` の残高を確認し、V2 (pUSD) と混同しないこと。
- **LLM エンコーディング・ガード**: LLM からの応答（特に `reason` フィールド）をログ出力・パースする際は、必ず Unicode 正規化 (NFC) および制御文字の除去を行うこと。

---

# 推奨ルール：**外科手術の原則** (Surgical Principles)

AI支援開発における「スコープクリープ（不要な修正によるデグレ）」を防止するための最優先ルールです。

## ルール1：「切開範囲」を事前に宣言する
修正前に必ず対象を明示する。
```markdown
【変更対象】[ファイル名] [クラス/関数名]
【変更範囲】[追加/修正内容の詳細]
【変更理由】[なぜこの変更が必要か]
```

## ルール2：既存コードへの変更は「追加のみ」を原則とする
- ✅ OK: 新しいメソッドを末尾に追加、新しいファイルを作成
- ❌ NG: 既存ロジックの改善、変数名・フォーマットの整理、「ついでにリファクタ」

## ルール3：1タスク = 1ファイル = 1コミット
1回の作業で複数ファイルを変更せず、1ファイルごとに動作確認と完了報告を行う。

## ルール4：「気になる箇所」は修正せず DEFER.md に記録する
本来の目的以外の修正箇所を見つけた場合は、修正せず `DEFER.md` に追記して Issue 化する。

---

# 日次報告・記録ルール (Daily Reporting Protocol)

1. **報告書の場所**: `d:\FX_AI_SYSTEM\brain\daily` を正本（SSOT）とする。他の場所（`docs/logs`等）には作成しない。
2. **ファイル命名**: `YYYY-MM-DD_final_report.md` を完了時の最終報告書として作成する。
3. **内容の統合**: その日の全ての作業（実装、修正、調査結果、証跡）を一つの最終報告書に集約する。
4. **メモリ更新**: 作業完了後、必ず `d:\FX_AI_SYSTEM\brain\MEMORY.md` にその日のサマリーと最終報告書へのリンクを追記する。
5. **同期**: 報告書作成とメモリ更新が完了したら、速やかに `git add`, `git commit`, `git push` を実行し、リモートと同期する。

---

# ターミナル操作の補助：PowerShell 版 grep 代替 (sls)

Windows 環境で `grep` が未インストールの場合、以下の `Select-String` (alias: `sls`) を使用すること。

| Linux/bash | PowerShell | 備考 |
|---|---|---|
| `grep "pattern" file` | `sls "pattern" file` | 基本検索 |
| `grep -r "pattern" dir` | `sls -Path "dir\**\*.py" -Recurse` | 特定拡張子の再帰検索 |
| `grep -n "pattern" file` | `sls "pattern" file` | 行番号は標準出力 |
| `cat file \| grep "pattern"` | `gc file \| sls "pattern"` | パイプライン連携 |
| `grep -v "pattern"` | `sls "pattern" file \| Where {!$_}` | 除外検索 |

---

# Polymarket V2 運用・パッチ管理ルール (2026-05-04 更新)

- **V2 署名整合性**: Polymarket V2 のメインネットコントラクト（`0xE111...`）は、署名対象に `expiration` を含めない 11 フィールド構造体を期待する。
- **外科的修正の維持**: SDK (`py-clob-client-v2`) のアップデートや再インストールが行われた場合、`expiration` が署名ハッシュに再混入する可能性がある。その際は必ず `d:\FX_AI_SYSTEM\scratch\restore_sdk_patch.py` を実行してパッチを再適用すること。
- **署名方式の固定**: V2 運用時は原則として `signature_type=0` (EOA) を使用し、`DOMAIN_VERSION` は必ず `"2"` を指定する。
