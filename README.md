# しいたけ栽培管理アプリ - GitHub Pages 版

スマホのホーム画面に独自アイコン（しいたけキャラ）を設定するため、HTMLを GitHub Pages にホストする構成。

## ファイル構成

```
github_pages/
├── index.html           # メインHTML（fetch経由でGAS API呼び出し）
├── manifest.json        # PWA マニフェスト
├── icon-192.png         # PWA用 192x192 アイコン
├── icon-512.png         # PWA用 512x512 アイコン
├── apple-touch-icon.png # iOS用 180x180 アイコン
├── favicon-32.png       # ブラウザタブ用 32x32
└── README.md            # 本ファイル
```

---

## デプロイ手順

### 1. GitHub リポジトリを作成

1. GitHub にログイン
2. 右上 `+` → `New repository`
3. リポジトリ名: 例 `shiitake-app`（任意）
4. **Public** を選択（Pages 無料プランは Public 必須）
5. `Create repository`

### 2. ファイルをアップロード

**方法A: ブラウザで直接アップロード（簡単）**
1. 作成したリポジトリのページで `uploading an existing file` リンクをクリック
2. `github_pages/` フォルダ内のファイル（**フォルダごとではなく中身のファイル6個**）をドラッグ＆ドロップ
3. 下の `Commit changes` をクリック

**方法B: git コマンド（慣れている人向け）**
```bash
git clone https://github.com/あなたのユーザー名/shiitake-app.git
cd shiitake-app
cp -r "G:/マイドライブ/しいたけ作業管理表作成/github_pages/"* .
git add -A && git commit -m "Initial commit" && git push
```

### 3. GitHub Pages を有効化

1. リポジトリの `Settings` タブ
2. 左サイドバーで `Pages`
3. `Source` → `Deploy from a branch`
4. `Branch` → `main` / `/ (root)` → `Save`
5. 数分待つと `https://あなたのユーザー名.github.io/shiitake-app/` でアクセス可能に

### 4. トークンを設定

セキュリティトークンを設定します。2箇所を**同じランダム文字列**に変更:

**① `index.html`**
```javascript
const API_TOKEN = 'CHANGE_THIS_TOKEN_TO_RANDOM_STRING';
```

**② GAS `コード.gs`**
```javascript
const API_TOKEN = 'CHANGE_THIS_TOKEN_TO_RANDOM_STRING';
```

推奨: ランダムな32文字以上。例:
```
k7Gm3Xn8pQw2RzYv6AhBsLe4Tj9Fdc1N
```

変更後、GitHub にコミット・GAS を保存＆再デプロイ。

### 5. GAS ウェブアプリを再デプロイ

- GASエディタで `デプロイ` → `デプロイを管理` → 鉛筆アイコン → 新バージョン → デプロイ
- `doPost` が追加されたので URL はそのまま使えます

### 6. スマホでホーム画面に追加

**iPhone (Safari):**
1. `https://あなたのユーザー名.github.io/shiitake-app/` を開く
2. 共有ボタン → `ホーム画面に追加`
3. しいたけキャラのアイコンでホーム画面に登録される 🍄

**Android (Chrome):**
1. URL を開く
2. メニュー → `ホーム画面に追加` / `アプリをインストール`
3. 同様にしいたけアイコンで登録

---

## 定期的なアクセスチェック

GASエディタで以下を実行するとログが確認できます:

```javascript
reviewAccessLog(7)  // 直近7日のレポート
```

または Claude に「アクセスログをチェックして」と依頼すれば、スプレッドシートの `アクセスログ` シートを読んで不審なアクセスを報告します。

---

## トラブルシューティング

**Q. アイコンがデフォルトのWebページアイコンになる**
- スマホのキャッシュ。一度アプリを削除して再度ホーム画面に追加

**Q. 「認証エラー」が出る**
- `index.html` と `コード.gs` の `API_TOKEN` が一致していない

**Q. データが送信されない**
- ブラウザのデベロッパーコンソールでエラー確認
- GASエディタで `実行ログ` を確認
- GAS のデプロイURLが最新か確認

**Q. CORS エラーが出る**
- fetch 呼び出しは `Content-Type: text/plain` にしてあるので通常は出ない
- もし出たら、GAS を新規バージョンで再デプロイしてみる
