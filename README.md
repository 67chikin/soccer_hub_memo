 # プレミアリーグの選手情報サイト

# html

## 実践例：サッカー選手データベース（[[soccer_hub_memo]]）

### 1. 文書構造・メタデータ
- `<!DOCTYPE html>`：最新のHTML5仕様で書かれた文書であることをブラウザに伝える宣言。
- `<html lang="ja">`：ページ全体の言語が日本語であることを指定（検索エンジンや音声読み上げ、自動翻訳の誤作動防止に重要）。
- `<meta charset="UTF-8">`：文字化けを防ぐための文字コード指定。
- `<meta name="viewport" content="width=device-width, initial-scale=1.0">`：スマホやタブレットなど画面幅が異なる端末でも、表示倍率を1.0（等倍）にして正しくレスポンシブ表示させる必須タグ。

### 2. レイアウト・コンテナ設計
- `<div class="panel">`：検索フォーム全体をまとめる白い背景枠（カード型UIの親コンテナ）。
- `<div class="search-grid">`：3つの検索入力欄を横並び（[[CSS]]のGridレイアウト）にするためのラッパー要素。
- `<div id="status" class="status"></div>`：ファイル読み込みエラーなどの警告メッセージをJavaScriptから動的に流し込むための空コンテナ。
- `<div id="results" class="results"></div>`：検索結果の選手カードをJavaScriptで生成して挿入する出力先エリア。`id` はJSからの取得用、`class` はCSSでのデザイン適用と役割を分けている。

### 3. フォーム部品（ユーザー入力）
- `<input type="text" id="nameInput" placeholder="選手名 (例: さか, サカ, Saka...)">`
  - `type="text"`：1行のテキスト入力欄。
  - `placeholder`：未入力時に薄く表示される入力例・ガイドテキスト。
- `<select id="teamSelect">` / `<option value="">すべてのチーム</option>`
  - ドロップダウン（プルダウン）メニューを作る組み合わせ。初期状態では「すべてのチーム（値は空文字 `""`）」のみ用意し、残りのチーム選択肢はJSで後から `<option>` を追加している。

### 4. JavaScriptで動的に生成されるHTML要素
検索結果の描画時に、文字列テンプレートとして組み立てられるタグ群。

- `<div class="card">`：1選手分の情報をまとめるカード本体。
- `<div class="card-header">`：カード上部に「チーム名」と「背番号・ポジション」を横並び配置するためのヘッダー枠。
- `<span class="team-badge">` / `<span class="pos-badge">`
  - `<span>` は改行を伴わないインライン要素。短いテキストをラベル（バッジ）風に装飾して横に並べる際によく使う。
- `<ul class="player-details">` / `<li><span>国籍/代表：</span>...</li>`
  - 複数の詳細項目（国籍・生年月日・体格）を整理するための箇条書きリスト（Unordered List）。項目名の部分だけを `<span>` で囲むことで、CSSで太字・色変更を適用しやすくしている。
- `<pre class="raw-text">`
  - 整形済みテキスト（Preformatted Text）。改行や連続する半角スペースをブラウザに無視させず、元のテキストデータのレイアウトのまま画面に出力する。

 # css
 
 ## 実践例：サッカー選手データベース（[[soccer_hub_memo]]）

### 1. 全体レイアウトと配色設計
- **中央寄せコンテナの定番パターン**
  ```css
  body {
    max-width: 1000px; /* PCの大画面でも横に広がりすぎないよう上限を設定 */
    margin: 0 auto;    /* 上下0、左右autoで画面の中央に配置 */
    padding: 20px;     /* スマホ画面などで画面端に文字がくっつくのを防ぐ余白 */
  }