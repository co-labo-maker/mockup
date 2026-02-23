# プロジェクト概要

Co-LABO MAKER 中古理化学機器ECプラットフォームのモックアップ・プロトタイプ

## リポジトリ構成

- `drafts/` — プロトタイプHTML作業ディレクトリ（gitignore対象）
- `docs/` — GitHub Pages 配信用（認証ゲート付き）

## 技術スタック

- スタンドアロンHTML（Tailwind CSS CDN）
- GitHub Pages デプロイ

## デザイン方針

- モバイルファースト
- 日本語UI
- ブランドカラー準拠（下記参照）

## ブランドカラー（Figma準拠）

- プライマリ: #00ADEE (colabo-blue-500)
- ホバー: #009CD6 (colabo-blue-600)
- アクセント: #EC1B23 (colabo-red-500)
- 背景: #F7F7F7 (colabo-gray-100)
- テキスト: #333333 (colabo-gray-900)
- ボーダー: #D8D8D8 (colabo-gray-300)

## プロトタイプ出力ルール

- 出力先: drafts/
- ファイル名: prototype-{名前}.html
- スタンドアロンHTML（外部依存なし）
- 公開時は `/publish` コマンドを使用

## 公開ワークフロー

### `/publish <theme-folder>` コマンド

drafts/ のテーマフォルダを docs/ に公開するコマンド。

```
/publish 20260219_testrun
```

処理内容:
1. `drafts/<theme-folder>/` → `docs/<theme-folder>/` にコピー
2. 全 HTML に auth gate（`../index.html` へリダイレクト）を注入
3. `docs/themes.json` にテーマメタデータを追加/更新
4. git commit（push はユーザーに確認）

### docs/ ディレクトリ構成

```
docs/
  index.html              ← ログインページ（portal.html へリダイレクト）
  portal.html             ← テーマ一覧ポータル（themes.json を動的読み込み）
  themes.json             ← テーマメタデータ
  20260223-category-ui/   ← テーマ別サブディレクトリ
    prototype-index.html
    prototype-*.html
    *.md
```
