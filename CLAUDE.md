# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

Single-file static landing page for **toi**（合同会社Do. が提供する「思考のOS」を身につけるコーチング/教育サービス）. The entire site is one HTML file served via GitHub Pages:

- Production URL: https://sghh02.github.io/toi-lp/lp.html
- Deploy mechanism: push to `main` → GitHub Pages serves `lp.html` directly. No build step, no CI.

## Working with the code

- Everything lives in `lp.html` — markup, inline `<style>`, and inline `<script>`. There is no bundler, framework, package.json, or asset pipeline. Edits happen directly in this single file.
- To preview locally, open `lp.html` in a browser (e.g. `open lp.html`) or run any static server from the repo root (e.g. `python3 -m http.server`).
- There are no tests, linters, or formatters configured. Do not introduce tooling unless the user asks.

## Structure inside `lp.html`

Sections appear in this order in the body and share CSS custom properties defined in `:root` (`--ink`, `--paper`, `--accent`, font families, etc.). When restyling, prefer editing these variables over hardcoding values.

- `nav` — fixed header, toggles `.scrolled` class on window scroll
- `.hero` → `.problem` → `.framework#framework` → `.phases#phases` → `.pricing#pricing` → `.phases#target` → `.faq#faq` → `.cta#apply` → `footer`
- Tokusho (特定商取引法) modal at the bottom, opened via `openTokusho()` / closed via `closeTokusho()`

Inline script responsibilities (all at the end of `<body>`):
- Nav scroll class toggle
- IntersectionObserver-based `.reveal` → `.visible` scroll animations
- `toggleFaq(el)` accordion (only one item open at a time)
- Tokusho modal open/close + overlay click-to-close

## Content conventions

- Copy is Japanese; keep edits in Japanese and preserve the existing tone (断定的・短文・強調は `<em>` とアクセントカラー).
- Pricing, 事業者情報, and 特商法 table content are legally meaningful — confirm with the user before changing numbers, company name (合同会社Do.), representative (坂田 海斗), or refund policy.
- External CTAs currently anchor to `#apply` in-page; if wiring a real form/Stripe link, update both the hero and final CTA buttons together.

## CSS 編集ルール

- 色は `:root` の `--ink / --paper / --paper-warm / --accent / --accent-glow / --muted / --border / --card-bg` を経由する。新しい色を足したいときは変数を先に定義してから参照する。
- フォントは `--font-display / --font-body / --font-mono` を使い、`font-family` の直指定を避ける。
- サイズは既存のパターン（`rem` / `clamp()`）に揃える。マジックナンバー `px` を増やさない。
- メディアクエリは `lp.html` の `<style>` 末尾にまとめて書かれている。新規スタイルもそのブロック内に追記してブレイクポイントの散逸を防ぐ。

## アクセシビリティ最低ライン

- 見出し階層を守る（`<h1>` は 1 回、セクション見出しは `<h2>`）。
- 画像追加時は `alt` を付与。純粋な装飾画像は `alt=""` とする。
- モーダルの閉じるボタンは `aria-label="閉じる"` を付ける。
- アクセントカラー (`--accent`) を細い文字のメインテキスト色に使わない（コントラスト不足）。

## 編集後の QA チェック

コピー/デザイン修正後は以下を確認してから完了を報告する:

- [ ] ブラウザで表示確認（`open lp.html` または `python3 -m http.server 8000` → `http://localhost:8000/lp.html`）
- [ ] レスポンシブ確認（幅 375px / 768px / 1280px の 3 点）
- [ ] `nav` が scroll で `.scrolled` になること、FAQ アコーディオン、特商法モーダル開閉が壊れていないこと
- [ ] アンカーリンク (`#framework` `#phases` `#pricing` `#faq` `#apply`) が対応 section にジャンプすること
- [ ] 閉じタグのバランス（section・modal の入れ子が崩れていない）

ブラウザ確認ができない状況で完了報告するときは、その旨を明示する（成功を勝手に宣言しない）。

## コミットメッセージ規約

- `feat: ...` — 新セクション / 新機能の追加
- `fix: ...` — タイポ、リンク切れ、表示崩れの修正
- `style: ...` — CSS のみの見た目調整
- `docs: ...` — README / CLAUDE.md など非コード変更
- `chore: ...` — 設定ファイル・gitignore など雑務

## 変更時に必ずユーザー確認

以下は法的/ブランド的に重要なため、テキスト変更前に必ずユーザーに確認する:

- 社名「合同会社Do.」、代表者「坂田 海斗」
- 料金（本コース：月額30,000円(税別) / 継続プラン：月額5,000円(税別)）
- 特商法テーブルの記載内容全般（返品・キャンセルポリシー含む）
