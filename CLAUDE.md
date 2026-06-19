# Project: klip

## 概要
macOS クリップボードマネージャーアプリ「Klip」の配布用ランディングページ（LP）。GitHub Releases への誘導・機能紹介・ショートカット早見表・初回セットアップ手順を1ページで提供する静的サイト。Klip 本体（macOS app）はこのリポジトリではなく `RYUIYAMADA/klip` の Releases で配布する。

## 技術スタック（実態）
- 素の HTML + CSS のみ（ビルドツール・フレームワーク・JS なし）
- `index.html`（単一ページ）+ `style.css`（CSS変数ベース）
- フォント: Google Fonts CDN（Noto Sans JP）を `<link>` で読込（後述・要確認）
- ホスティング: GitHub Pages 想定（origin = github.com/RYUIYAMADA/klip）
- VERSION: SemVer 管理（現在 0.14.0）。LP 内に `v0.14.0` 表記が複数箇所ハードコードされている

## ディレクトリ構成
```
klip/
├── index.html      # LP本体（単一ページ・セクション構成）
├── style.css       # 全スタイル（:root にCSS変数を集約）
├── VERSION         # SemVer（例: 0.14.0）
├── assets/
│   ├── klip-showcase-default.png   # ヒーローのアプリ画面
│   └── klip-showcase-search.png    # 検索機能のアプリ画面
└── .gitignore
```

## 命名・コーディング規則
- クラス名は日本語セクションに対応した BEM 風のケバブケース（`feature-card` / `setup-step--warning` / `app-mock-small`）。修飾は `--warning` のように `--` サフィックス
- CSS は `:root` の変数を必ず参照。素の HEX・px のハードコードは禁止（後述の例外を除く）
- セクションは `<section class="...">` + 内側 `.container`（max-width: var(--max-width)）の二層構造で統一
- アイコンは inline SVG（モノクロ線・`fill:none; stroke:currentColor` 相当）。`viewBox="0 0 24 24"`・`stroke-width` 1.8〜2.5・`stroke-linecap/linejoin: round` で統一
- 文言は日本語。コピーはベネフィット先行（「コピーした全部が、すぐ戻る。」等）
- レスポンシブは `@media (max-width: 700px)` の単一ブレークポイントでグリッドを1カラム化

## 禁止事項（PJ固有）
- バージョン更新時に `index.html` 内の `v0.14.0` / `Klip-v0.14.0.zip` 表記の更新漏れ → 禁止（`<title>`・hero-eyebrow・hero-cta・setup step1・footer-cta・footer の6箇所以上に散在。VERSION と必ず一致させる）
- JS の安易な追加 → 禁止（現状 JS ゼロ。動的挙動が本当に必要か検討してから入れる）
- ダウンロードリンクの URL を `releases/latest` 以外（固定タグ等）に変える → 原則禁止（常に最新版へ誘導する設計）
- アプリ本体のソースをこのリポジトリに混在させない（LP専用リポジトリ）
- mock 内の `#1c1c1e` / `#2c2c2e` / 信号機ドット色（`#ff5f57` 等）は macOS ウィンドウクロームの再現値。これら「OS UI 再現の固定色」のみ変数化の例外として許容するが、それ以外の素の HEX 追加は禁止

## グローバル設定の継承
`~/.claude/CLAUDE.md` の全ルールを継承する（plan 起点開発・Codex 実装レビュー必須／指摘ゼロまでループ・itshover モノクロ線アイコン・CSS変数必須でハードコード禁止・認知負荷最小・コードは綺麗/シンプル/高速）。本ファイルには上記の klip 固有事項のみ記載。UI 実装時は `DESIGN.md` を最優先で参照する。

### 注意（要確認・グローバル禁則との差分）
- グローバルの HTML 資料ルールは「外部 CDN・外部フォント禁止・自己完結 HTML」を求めるが、本 LP は Google Fonts CDN（Noto Sans JP）を外部読込している。**配布 LP のため意図的か、ローカル同梱に直すべきか未確定・要確認**。新規追加では外部依存を増やさない。
