drafts/ のテーマフォルダを docs/ に公開する。

## 引数

$ARGUMENTS — 公開するテーマフォルダ名（例: `20260219_testrun`）

## 処理フロー

1. **引数の検証**: `drafts/$ARGUMENTS/` が存在することを確認。存在しない場合はエラーメッセージを表示して終了
2. **ファイルコピー**: `drafts/$ARGUMENTS/` の中身を `docs/$ARGUMENTS/` にコピー（既存の場合は上書き）
   - prototype-*.html と *.md ファイルを対象にする
3. **auth gate 注入**: コピーした全 HTML ファイルの `<body>` タグ直後に以下の認証ゲートスクリプトを注入する（既に含まれている場合はスキップ）:
   ```
   <script>if(sessionStorage.getItem("proto_auth")!=="b78596825458be558740342d5db7145da954cdd9e8a5d3494dce43d5c019dfca"){location.replace("../index.html");}</script>
   ```
4. **オーバーレイ注入**: `prototype-index.html` **以外**の全 prototype-*.html に、auth gate 直後に以下のフローティングオーバーレイを注入する（既に含まれている場合はスキップ）:
   ```html
   <div style="position:fixed;bottom:16px;right:16px;z-index:99999;display:flex;gap:8px;align-items:center;">
     <span style="padding:6px 12px;background:rgba(0,0,0,0.75);color:#fff;border-radius:6px;font-size:12px;font-family:system-ui,sans-serif;white-space:nowrap;">プロトタイプ — ページ内リンクは無効です</span>
     <a href="prototype-index.html" style="display:inline-flex;align-items:center;gap:4px;padding:6px 14px;background:#00ADEE;color:#fff;border-radius:6px;font-size:13px;font-weight:600;text-decoration:none;box-shadow:0 2px 8px rgba(0,0,0,0.15);font-family:system-ui,sans-serif;">&#8592; 戻る</a>
   </div>
   ```
5. **themes.json 更新**: `docs/themes.json` を読み込み、該当テーマのエントリを追加または上書きする
   - `id`: フォルダ名
   - `title`: ユーザーに確認（デフォルト: フォルダ名をそのまま使用）
   - `description`: ユーザーに確認
   - `date`: フォルダ名の日付部分から推測、またはユーザーに確認
   - `fileCount`: コピーした prototype-*.html の数
6. **git add + commit**: 変更ファイルを個別に git add し、コミットメッセージ `Publish theme: $ARGUMENTS` でコミット
7. **push 確認**: ユーザーに `git push origin main` を実行するか確認する

## 注意事項

- `docs/index.html` と `docs/portal.html` は変更しない（portal.html は themes.json を動的に読むため更新不要）
- auth gate のリダイレクト先は `../index.html`（サブディレクトリ配置のため）
- コミット前に変更内容をユーザーに提示して確認を取ること
