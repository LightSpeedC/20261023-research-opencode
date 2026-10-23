# OpenCode の調査

オープンソースの AI コーディングエージェント OpenCode の素性・機能・使い勝手を、Web の公開情報から調べた記録

> 📅 作成: 2026-10-23 / 更新: 2026-10-23

[⌂](../)

1. [概要](#1-概要)
2. [素性](#2-素性)
3. [由来とソースの出所](#3-由来とソースの出所)
4. [機能と使い勝手](#4-機能と使い勝手)
5. [Claude との関係](#5-claude-との関係)
6. [評価と出典](#6-評価と出典)

## 1. 概要

OpenCode はターミナルで動く**オープンソースの AI コーディングエージェント**。Claude Code と同じ種類の道具だが、特定の LLM 提供元に縛られない。

## 2. 素性

| 項目 | 内容 |
|---|---|
| 開発元 | Anomaly（GitHub 組織 anomalyco）。SST を作ったチームが母体 |
| リポジトリ | [anomalyco/opencode](https://github.com/anomalyco/opencode)（旧 sst/opencode から移転） |
| ライセンス | MIT |
| 人気 | GitHub のスター約 22 万、フォーク約 2.9 万（2026-10-23 にリポジトリのページで確認） |
| 公式サイト | [opencode.ai](https://opencode.ai/) |

> [!NOTE]
> **同名の別プロジェクト**: Go 製の同名プロジェクトがあり、Charm 社の「Crush」になったとされる（未確認）。二次情報の「Go 製の CLI」という説明は、こちらとの混同の可能性がある。

## 3. 由来とソースの出所

### どこで生まれたか

| 日付（JST） | 出来事 |
|---|---|
| 2025-03-17 | Kujtim Hoxha 氏が Go 製のエージェント「TermAI」を公開（[opencode-ai/opencode](https://github.com/opencode-ai/opencode)。現在はアーカイブ済み） |
| 2025 年春 | SST の開発者 2 名（dax 氏・adam 氏）が参加し、「OpenCode」への改名を提案。dax 氏がドメインを買ったとされる |
| 2025-05-01 | 現在の OpenCode のリポジトリが作られる。TypeScript で書かれ、GitHub 上のフォークではない |
| 2025-05-21 | Charm 社が Go 版を引き継ぐ形で [Crush](https://github.com/charmbracelet/crush) のリポジトリを作る |

日付は GitHub の API で取ったリポジトリの作成日。経緯は双方の主張と二次情報による。

### 分裂の経緯（双方の主張が食い違う）

- **Charm 社の主張**（同社 CEO の投稿）: 元の作者は Charm 社の OSS ライブラリ（Bubble Tea など）で TermAI を作った。同社が作者を雇おうとしたところ、SST 側が名前を保ったまま別の組織へ移し、AUR のパッケージも引き取った
- **SST 側の主張**（第三者による伝聞）: 2 人は主要な貢献者だった。dax 氏のコミットはまとめられて履歴から消された。OSS のまま続けたかったので、同じ名前で TypeScript に作り直した

### クリーンルームか

クリーンルームではない。元のコードを見ていない人が仕様だけで作った、という資料は見つからない。作り直した開発者は元のプロジェクトに深く関わっていた。発想・名前・操作画面は元のプロジェクトを受け継ぎ、言語は Go から TypeScript に変わった。コードの流用の有無は突き合わせていない。

### Claude Code のソースが出所という噂

- 一般の検索・Reddit を含めた広い検索のどちらでも、そうした噂・告発は見つからなかった（X は見ていない）
- 2026-03-31、Claude Code 2.1.88 の npm パッケージにソースマップ（`cli.js.map`、59.8 MB）が含まれ、約 51.2 万行・約 1,900 ファイルが復元できる状態になった。Anthropic は「人為的な誤り」と説明した。Anthropic がソースマップを出してしまったのは 3 回目とする報道もある（二次情報）
- それより前の 2025-03-10、難読化を解いた Claude Code を GitHub に載せた開発者に、Anthropic が DMCA の削除要求を出している（TechCrunch、2025-04-25）
- 現在の OpenCode のリポジトリは 2025-05-01 に作られ、2026 年の流出より 1 年近く前。2025 年 3 月の件の後だが、OpenCode の側が使ったという報道・告発は無い
- 流出をきっかけにした書き直し（claw-code、流出コードに他社モデル対応を足した OpenClaude 等）は別のプロジェクトで、OpenCode とは関係ない
- 流出したコードには OpenCode の名前が 2 ファイル以上にあり、「OpenCode の自動スクロールに合わせる」というコメントもあったと報じられている。Claude Code の側が OpenCode を参考にしていた、という向きの話（報じたのは 1 本で、裏付けは無い）

> [!IMPORTANT]
> **未確認**: 2025 年 2 月ごろにもソースマップが含まれていた件があったとする記憶があるが、検索では見つからなかった。「3 回目」の報道がこれを指すかは不明。

## 4. 機能と使い勝手

### 使える形

ターミナル画面（TUI）・デスクトップアプリ（ベータ版。Mac・Windows・Linux）・IDE 拡張の 3 通り。

### エージェント

- `build`: 既定。ファイル編集・コマンド実行ができる
- `plan`: 読み取り専用。編集は既定で拒否、bash は実行前に確認（Tab キーで切り替え）
- `@general`: 調べものや複数手順の作業を任せるサブエージェント

### LLM と料金

- API キーを設定すれば、どの提供元のモデルも使える
- Zen: 公式が動作を確かめたモデルを従量課金で使える
- Go: 公開モデルを月 $10 で使える（金額は二次情報）

### 設定と操作

`/init` で `AGENTS.md` を作る。`/undo`・`/redo` で変更の取り消し・やり直し。テーマ・キー割り当て・独自コマンドも設定できる。

### 仕組み

起動すると裏でサーバ（既定 `127.0.0.1:4096`）が立ち、TUI はその利用者側として動く。`opencode serve` でサーバだけを動かせ、OpenAPI 3.1 で外部から自動化できる。

> [!WARNING]
> **外部公開の注意**: 外からつながる場所でサーバを公開すると、画面操作（`/tui`）や認証情報の設定（`PUT /auth/:id`）も外から行える。公開するなら `OPENCODE_SERVER_PASSWORD` でパスワードを設定する。

### 導入

- install スクリプト、npm・bun・pnpm・yarn、Homebrew、Arch のパッケージ等
- Windows は WSL を推奨。直接入れるなら Chocolatey・Scoop・npm 等
- 版 1 と版 2 は同じ `opencode` コマンドのため同時に入れられない

### 権限

- 道具ごとに `allow`（そのまま実行）・`ask`（確認）・`deny`（拒否）を `opencode.json` の `permission` で決める。対象は `read`・`edit`・`glob`・`grep`・`shell`・`task`・`webfetch`・`websearch`・`external_directory`（プロジェクト外）・`doom_loop`（同じ操作の繰り返し）等
- **既定はほとんどが `allow`**。`ask` は `doom_loop` と `external_directory` だけ。`.env` の読み取りは拒否（`.env.example` は可）
- コマンドは `"git *": "allow"`・`"rm *": "deny"` のように型で書く。最後に当たった規則が勝つので、全体の既定を先に書く
- 確認には「今回だけ」「このセッション中は常に」「拒否」で答える。`--auto` を付けると確認を飛ばす（`deny` は効く）
- エージェントごとに上書きできる

> [!WARNING]
> **版 2 の既定**: 移行の資料によると、版 2 では `external_directory` の既定が `allow` に変わり、プロジェクトの外へも確認なしで出る。版 1 と同じにするには規則を書く必要がある。

### MCP

- 手元で動かすもの（`type: local`、`command` を指定）と、ネット越しのもの（`type: remote`、`url` を指定）の両方に対応
- ネット越しのものは 401 を受けると OAuth を始め、トークンを保存する。`opencode mcp auth`・`list`・`logout`・`debug` で管理する
- MCP の道具は文脈を消費する。GitHub の MCP のように大量に消費するものもある

### LSP

- 版 1: 約 35 言語の LSP を内蔵し、診断をエージェントへ返す。既定は無効で `"lsp": true` で有効にする。一部は自動でダウンロードする（`OPENCODE_DISABLE_LSP_DOWNLOAD` で止める）
- 公式も「LSP が常に役立つとは限らない」とし、lint・型検査のコマンドを `AGENTS.md` に書いて走らせる方法を勧めている
- **版 2 では LSP の設定は残るが、言語サーバも診断も動かない**（移行の資料）

### 設定ファイル

次の順に重ね合わせ、後のものが勝つ。置き換えではなく統合される。

1. 組織の既定（`.well-known/opencode`）
2. 全体の設定 `~/.config/opencode/opencode.json`
3. 環境変数 `OPENCODE_CONFIG` で指したファイル
4. プロジェクトの `opencode.json`
5. `.opencode` フォルダ（エージェント・コマンド・プラグイン）
6. 環境変数 `OPENCODE_CONFIG_CONTENT`
7. 管理者が置く設定（Windows は `%ProgramData%\opencode`）
8. macOS の MDM の構成プロファイル（最優先）

### Claude Code の資産を読む

- 版 1 は、`AGENTS.md` が無ければ `CLAUDE.md` を、全体の `AGENTS.md` が無ければ `~/.claude/CLAUDE.md` を読む。`~/.claude/skills/` も読む
- 止めるには `OPENCODE_DISABLE_CLAUDE_CODE=1`（全部）・`_PROMPT`・`_SKILLS`（一部）
- 版 2 は `AGENTS.md` だけを読む（移行の資料）

### 通信とデータ

- プロキシは `HTTPS_PROXY`・`HTTP_PROXY`。TUI と手元のサーバの通信を通さないよう `NO_PROXY=localhost,127.0.0.1` が要る。社内の CA は `NODE_EXTRA_CA_CERTS`。NTLM・Kerberos の認証には対応せず、対応した LLM ゲートウェイを挟むよう案内している
- **既定で起動時に更新をダウンロードする**。`"autoupdate": false` で止め、`"notify"` で通知だけにする
- ソースにある無効化の環境変数: `OPENCODE_DISABLE_AUTOUPDATE`・`_MODELS_FETCH`（モデル一覧の取得）・`_FREE_MODELS`・`_PROJECT_CONFIG`・`_FILEWATCHER` 等（GitHub のコード検索で確認）
- テレメトリ: 版 1 に試験的な OpenTelemetry の出力設定があり、版 2 では無視される。OpenCode 自身が利用データを送るかを書いた公式の説明は見つからなかった

### 版 1 → 版 2 の主な変更

- プラグインの API が変わり、版 1 のプラグインはそのまま動かない。サーバの API も変わった
- 設定はそのまま読めるが、名前が変わる（`permission` → `permissions`、`bash` → `shell`、`mcp` → `mcp.servers` 等）
- TODO は既定で無効。委ねたコマンドは背面で動く

## 5. Claude との関係

- 2026 年 1 月ごろから、Claude の Pro・Max 契約のログイン（OAuth）を他社製の道具から使えなくなった。2 月には規約で禁止と明記されたと報じられている
- OpenCode で Claude を使うには、Claude Console の API キー（従量課金）が要る

> [!IMPORTANT]
> **確度**: この経緯は二次情報が中心で、正確な日付は確かめきれていない。

## 6. 評価と出典

### 向き・不向き

- 向く: 複数の LLM を使い分けたい、ローカル・社内の LLM を使いたい、外部から自動化したい
- 向かない: Claude の定額契約だけで使いたい（その用途は Claude Code が前提）

### 調べきれなかったこと

- 会話の共有（share）機能: 公式資料のページが見つからない（404）。何がどこへ上がるか不明
- テレメトリ: OpenCode 自身が利用データを送るかの公式の説明が見つからない
- X 上の噂: 見ていない
- 分裂についての SST 側本人の発言: 見つからない

### 出典

- [anomalyco/opencode（GitHub）](https://github.com/anomalyco/opencode)
- [OpenCode Docs](https://opencode.ai/docs)
- [OpenCode Docs: Server](https://opencode.ai/docs/server)
- [OpenCode Docs: Permissions](https://opencode.ai/docs/permissions)
- [OpenCode Docs: MCP servers](https://opencode.ai/docs/mcp-servers)
- [OpenCode Docs: LSP](https://opencode.ai/docs/lsp)
- [OpenCode Docs: Config](https://opencode.ai/docs/config)
- [OpenCode Docs: Rules](https://opencode.ai/docs/rules)
- [OpenCode Docs: Network](https://opencode.ai/docs/network)
- [OpenCode: 版 1 からの移行（GitHub）](https://github.com/anomalyco/opencode/blob/HEAD/services/www/src/docs/content/migrate-v1.mdx)
- [TechCrunch: Anthropic の DMCA 削除要求（2025-04-25）](https://techcrunch.com/2025/04/25/anthropic-sent-a-takedown-notice-to-a-dev-trying-to-reverse-engineer-its-coding-tool/)
- [NodeSource: Claude Code のソース流出](https://nodesource.com/blog/anthropic-claude-code-source-leak-bun-bug)
- [Claude Code Source Code Is Online: 512,000 Lines](https://pasqualepillitteri.it/en/news/581/claude-code-source-leak-npm-512000-lines)
- [Habr: OpenClaude](https://habr.com/en/articles/1018234)
- [OpenCode vs Claude Code（morphllm）](https://morphllm.com/comparisons/opencode-vs-claude-code)
- [rywalker.com: OpenCode](https://rywalker.com/research/opencode)
- [heyuan110: OpenCode review](https://www.heyuan110.com/posts/ai/2026-03-13-opencode-ai-coding-agent-review)
- [noqta: developer guide 2026](https://noqta.tn/en/blog/opencode-open-source-ai-coding-agent-developer-guide-2026)
- [Anthropic's Third-Party Claude Crackdown（falcao.org）](https://falcao.org/posts/anthropic-claude-access-crackdown-ecosystem-fallout/)
- [alternativeto: subscription auth ban](https://alternativeto.net/news/2026/2/anthropic-officially-bans-using-subscription-authentication-for-third-party-claude-use)
- [Sovereign Magazine](https://www.sovereignmagazine.com/article/anthropic-blocks-openclaw-claude-subscriptions)
- [Hacker News スレッド（Charm と OpenCode の分裂）](https://hn.svelte.dev/item/44483251)
- [rywalker.com: Crush](https://rywalker.com/research/crush)
- [horadecodar.com.br: OpenCode no Reddit](https://horadecodar.com.br/opencode-reddit/)
- [Sovereign Magazine: Claude Code source code leak](https://www.sovereignmagazine.com/article/anthropic-claude-code-source-code-leak)
- [Claude Code Source Leak: A Timeline（Kilo）](https://blog.kilo.ai/p/claude-code-source-leak-a-timeline)
- [Open Source For You](https://www.opensourceforu.com/2026/04/anthropic-leak-exposes-claude-code-as-an-open-source-learning-event/)

[⌂](../)
