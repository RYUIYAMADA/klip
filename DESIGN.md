---
project: klip
version: 0.14.0
inherits: ryuiyamada-design-system（グローバルDS）
updated: 2026-06-18
---

# DESIGN.md — klip

> このファイルは Claude Code / Codex が UI を作るとき**毎回最初に読む**設計契約。
> グローバルDS（`~/Desktop/ryui-workspace/projects/tools/ryuiyamada-design-system/`）を継承し、
> **このプロジェクト固有の差分だけ**ここに書く。global と矛盾する時はこのファイルが優先。

## 1. このプロダクトは何か
- 何をするものか: macOS クリップボードマネージャーアプリ「Klip」の配布用ランディングページ。GitHub Releases へのダウンロード誘導と、機能・ショートカット・初回セットアップの説明を1ページで行う。
- 主な利用者: Klip を使いたい macOS ユーザー（非エンジニア含む）。GitHub からアプリを落として使い始める導線を通る。
- 利用デバイス/環境: PC（macOS）中心。モバイルでも閲覧されるためレスポンシブ必須。
- トーン: 落ち着いた業務用 + Apple 系プロダクトページの清潔感。誇張せず「コピーした全部が手元に残る」という機能価値を静かに伝える。

## 2. デザイン原則（守るべき判断軸）
1. **静かな信頼感** — 派手な装飾より余白と整列で品質を伝える。グラデーション・glassmorphism は使わない（global準拠。ツールとしての堅実さを損なうため）。
2. **ベネフィット先行のコピー** — 見出しは機能名でなく「何が嬉しいか」を先に言う（例: 「コピーするたび履歴が溜まる。⌘⇧V で呼び出してすぐ貼れる。」）。
3. **キーボード文化を視覚化** — Klip はキーボード操作が核。`<kbd>` でキーを物理キーらしく見せ、ショートカット早見表を主要セクションに置く。
4. **アクセントは一点投下** — ネイビー（`--color-accent`）を基調に、オレンジ（`--color-orange`）は「強調語1つ」「警告ステップ」だけに使う。乱用しない。
5. **アプリ画面は macOS クローム付きで見せる** — スクリーンショットは信号機ドット付きウィンドウ枠（`.app-mock`）に入れ、実物感を出す。

## 3. トークン（`style.css` の `:root` が正本）
素のCSS値でハードコードせず必ず変数を使う。以下は実装済みの実値。
```css
/* Color */
--color-bg:        #ffffff;   /* ページ背景・nav・hero */
--color-surface:   #f2f2f2;   /* セクション交互背景（features/shortcuts） */
--color-surface2:  #ede8e4;   /* アイコン台座・eyebrow 背景 */
--color-border:    #e2ddd7;   /* 罫線・kbd枠・カード境界 */
--color-text:      #333333;   /* 本文・見出し */
--color-text-sub:  #858585;   /* 補足・説明文 */
--color-accent:    #073365;   /* 主要: ブランドネイビー・CTA・リンク */
--color-accent-hv: #052748;   /* accent の hover */
--color-orange:    #f7581d;   /* 強調語・警告ステップ番号のみ */
--color-success:   #1e8e3e;   /* チェックリストのチェック */
--color-warning-bg:#fff7ed;   /* 警告ボックス背景 */
--color-warning-bd:#fed7aa;   /* 警告ボックス枠 */
--color-white:     #ffffff;
/* Radius */ --radius: 8px; --radius-lg: 14px;
/* Shadow */ --shadow-sm/md/lg（カードは sm、アプリモックは lg）
/* Font */ --font: 'Noto Sans JP', system-ui, -apple-system, 'Helvetica Neue', sans-serif;
/* Layout */ --max-width: 880px;
```
※ macOS ウィンドウクローム再現色（`#1c1c1e` `#2c2c2e` 信号機ドット `#ff5f57/#ffbd2e/#28c840`）は OS UI 再現用の固定値として例外的に許容（変数化しない）。それ以外の素の HEX 追加は禁止。

## 4. コンポーネント規約
- **Button / CTA**: 主CTA = `.hero-cta` / `.cta-btn`（ネイビー塗り or 白塗り、min-height 48px、DLアイコン付き）。nav の小CTA = `.nav-cta`（min-height 44px）。hover は背景を `--color-accent-hv` に。1画面で主CTAの役割は「ダウンロード」に統一。
- **Card**: `.feature-card` = 白背景・`--radius-lg`・`--shadow-sm`・余白 28px/24px。border は付けず影で浮かせる（border 多用禁止に準拠）。
- **kbd**: キーは必ず `<kbd>` で表現。白背景 + `--color-border` 枠 + 微小シャドウ + ネイビー文字。複合キーは1つの kbd 内にスペース区切り（例: `⌘ ⇧ V`）。
- **アプリモック**: `.app-mock`（ヒーロー大）/ `.app-mock-small`（検索・横並び）。ダーク枠 + 信号機ドット + タイトル。
- **警告ステップ**: `.setup-step--warning` は番号をオレンジに、本文に warning ボックス（`--color-warning-bg/bd`）。必須権限など見逃せない手順だけに使う。

## 5. レイアウト規約
- ブレークポイント: `max-width: 700px`（単一）。これ未満で features グリッドと search レイアウトを1カラム化。
- 最大幅: `--max-width: 880px`。全セクションは `<section>` + 内側 `.container`（左右 padding 24px）。
- セクション縦余白: 72〜80px（モバイルは 56px）。
- 背景は白とサーフェスグレーを交互に敷いてセクションを区切る（罫線でなく面で分ける）。
- 情報密度: ゆったり。説明文は `--color-text-sub` で本文より小さめ（13〜16px）にし、読む負担を下げる。

## 6. 禁止ルール（anti-pattern）
- 色・余白・フォントサイズを変数でなく素の値でハードコード → 禁止（macOS クローム再現色のみ例外）。
- グラデーション / glassmorphism / カードの border 多用 / font-weight 300以下 → 禁止（global + style.css 冒頭コメントで明示）。
- オレンジ（`--color-orange`）を広範囲・複数強調に使う → 禁止（強調語1つ・警告ステップのみ）。
- 絵文字をUIラベル・アイコン代わりに使う → 禁止（inline SVG モノクロ線で統一）。
- バージョン表記（`v0.14.0` / `Klip-v0.14.0.zip`）の更新漏れ → 禁止（VERSION と全箇所一致）。
- 新規の外部CDN・外部スクリプト追加 → 禁止（既存の Google Fonts は要確認事項。これ以上増やさない）。

## 7. アクセシビリティ
- コントラスト比 WCAG AA（本文4.5:1・大文字3:1）。ネイビー `#073365` on 白は十分、サブグレー `#858585` on 白は本文用途で AA ギリギリのため小文字長文には使い過ぎない。
- 装飾 SVG には `aria-hidden="true"`、画像には説明的 `alt` を必ず付ける（実装済み）。
- タップターゲット最小 44×44px（CTA は min-height で担保済み）。
- 日本語は `<br>` で文節改行 + `line-break: strict` の禁則処理（global準拠）。
- focus-visible リングは現状未確認・要確認（CTA/リンクにフォーカス可視を担保すること）。

## 8. Do / Don't
| ✅ Do | ❌ Don't |
|---|---|
| 背景は `var(--color-surface)` で面分け | `background:#f2f2f2` と素値で書く |
| 強調は `<em>` 1語をオレンジに | 段落全体をオレンジで塗る |
| キーは `<kbd>⌘ ⇧ V</kbd>` | キーを地の文 `Cmd+Shift+V` で書く |
| アイコンは inline SVG モノクロ線 | 絵文字 ⌨️ や多色アイコンを使う |
| バージョンは VERSION と全箇所一致 | 1箇所だけ更新して他を放置 |

## 9. AI（Claude/Codex）への指示
- UI実装前に必ずこのファイルと global DS、`CLAUDE.md` を読む。
- トークンは変数参照（素の値禁止）。§6 の禁止ルールに違反したら自己修正。
- 「読む負担を感じさせない、みてわかるレイアウト」を全画面のデフォルト前提にする。
- バージョン更新タスクでは VERSION を起点に `index.html` の全 `v0.14.0` 系表記を grep して漏れなく更新。
- 迷ったら §2 デザイン原則で判断。それでも決まらなければ実装を止めて PM に質問。

## 📜 更新履歴
- 2026-06-18 — 初版。既存 LP（v0.14.0）の実装実態（index.html / style.css）からトークン・規約・禁止事項を抽出して作成。
