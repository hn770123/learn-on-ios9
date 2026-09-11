# 第2章: HTML/CSSの基礎構造とページの仕組み

本章では、Webページの土台となるHTMLとCSSの基本構造、およびブラウザがこれらをどのように解釈して画面に表示（レンダリング）するのか、その仕組みを解説します。
iOS 9 (Mobile Safari) の Textastic 環境で実際にコードを入力・変更し、プレビュー機能で確認しながら学習を進めていきましょう。

---

## 1. Webページの全体像：HTMLとCSSの役割分担

Webページは主に **HTML (HyperText Markup Language)** と **CSS (Cascading Style Sheets)** という2つの言語によって構成されています。

* **HTML**: ページの「構造」と「意味（コンテンツ）」を定義します。（文章、見出し、画像、リンク、ボタン等）
* **CSS**: ページの「見た目（デザイン）」や「レイアウト」を定義します。（文字の色やサイズ、背景色、配置等）

### 例え話で理解する関係性
* **HTML**: 建物の「骨組み」や「壁・ドア」
* **CSS**: 外壁の「塗装」や「インテリアの装飾」

---

## 2. HTMLの基本構造とドキュメントツリー (DOM)

HTMLは「タグ」と呼ばれる記号（`<tagname>...</tagname>`）を使って要素を囲むことで文書構造を定義します。

### 2.1 最小限のHTML5テンプレート

以下は、標準的なHTML文書の基本構文です。iOS 9のTextasticで `index.html` などのファイルを作成する際、まずこの骨組みを用意します。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <!-- 文字コード指定（文字化け防止） -->
  <meta charset="UTF-8">
  <!-- モバイル（iOS）向けのビューポート指定 -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>第2章 ハンズオン</title>
  <!-- 外部CSSファイルの読み込み -->
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <!-- 実際に画面に表示されるコンテンツ -->
  <header>
    <h1>マイWebサイト</h1>
  </header>
  <main>
    <p>はじめてのHTML/CSS実験です。</p>
  </main>
</body>
</html>
```

### 2.2 主要タグの役割
* `<!DOCTYPE html>`: この文書がHTML5規格で書かれていることをブラウザに宣言します。
* `<html>`: HTML文書のルート（根）となる要素です。
* `<head>`: 画面には直接表示されないメタ情報（タイトル、文字コード設定、外部CSSのリンク等）を格納します。
* `<body>`: ブラウザの画面上に実際に表示されるコンテンツすべてを記述します。
* `<meta name="viewport" ...>`: **iOSなどのモバイル端末でページを最適な倍率・幅で表示させるために必須の指定です。**

---

## 3. CSSの適用方法と基本文法

CSSをHTMLに適用する方法には「インライン指定」「内部スタイルシート」「外部スタイルシート」の3種類があります。メンテナンス性や再利用性の観点から、**外部スタイルシート (`<link>` タグでの読み込み)** が推奨されます。

### 3.1 CSSの基本文法
CSSは **セレクタ (Selector)**、**プロパティ (Property)**、**値 (Value)** の組み合わせで記述します。

```css
/* セレクタ { プロパティ: 値; } */
p {
  color: #333333; /* 文字色 */
  font-size: 16px; /* フォントサイズ */
}
```

* **セレクタ**: 「どの要素にスタイルを適用するか」を指定（例: `p`, `.class-name`, `#id-name`）
* **プロパティ**: 「何を変化させるか」（例: `color`, `background-color`, `font-size`）
* **値**: 「どのように変化させるか」（例: `red`, `#ff0000`, `16px`）

---

## 4. ブラウザがページを描画する仕組み（レンダリングの流れ）

ブラウザがHTMLファイルを受け取ってから、画面にピクセルとして表示するまでの内部処理は以下の通りです。

```
[HTMLファイル]  --> DOMツリー構築   \
                                  +--> [レンダリングツリー] --> レイアウト計算 (Reflow) --> 描画 (Paint)
[CSSファイル]   --> CSSOMツリー構築 /
```

1. **DOM (Document Object Model) ツリーの構築**
   ブラウザはHTMLテキストを読み込み、タグの階層構造を解析して「DOMツリー」と呼ばれる木構造のデータを作成します。
2. **CSSOM (CSS Object Model) ツリーの構築**
   CSSの指定を解析し、どの要素にどのようなスタイルが適用されるかのルールツリーを作成します。
3. **レンダリングツリーの形成**
   DOMツリーとCSSOMツリーを結合し、実際に画面に表示される要素のみを取りまとめたレンダリングツリーを作成します（`display: none` の要素などは除外されます）。
4. **レイアウト計算 (Reflow)**
   各要素の画面上での正確な位置やサイズ（幅、高さ）を計算します。
5. **描画 (Paint / Composite)**
   計算された位置・スタイルに従い、文字・色・画像などを画面のピクセルとして描画します。

---

## 5. 実験ハンズオン (Textasticでの操作体験)

それでは、iOS 9のTextastic（またはお手元のテキストエディタ）を使って、実際にコードを入力して挙動を確認してみましょう。

### 手順 1: サンプルファイル `samples/chapter2.html` の準備

Textasticで [`samples/chapter2.html`](./samples/chapter2.html) を開くか、新規ファイルとして以下のコードを作成します。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>第2章 実験ラボ</title>
  <style>
    /* 内部スタイルシートによるスタイリング */
    body {
      font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
      background-color: #f5f5f7;
      margin: 20px;
      color: #333;
    }

    .card {
      background-color: #ffffff;
      border-radius: 8px;
      padding: 16px;
      margin-bottom: 16px;
      border: 1px solid #ddd;
    }

    .highlight {
      color: #e65100;
      font-weight: bold;
    }
  </style>
</head>
<body>

  <h1>HTML/CSS 構造確認ラボ</h1>

  <div class="card">
    <h2>カード要素 1</h2>
    <p>これは1つ目のカードです。CSSの <span class="highlight">.card</span> クラスによって背景色と枠線、角丸が適用されています。</p>
  </div>

  <div class="card">
    <h2>カード要素 2</h2>
    <p>同じクラスを指定することで、複数の要素に同じデザインを一括適用できます。</p>
  </div>

</body>
</html>
```

### 手順 2: Textasticでのプレビュー確認と実験

1. **プレビュー画面を開く**: Textasticのメガネアイコン（プレビューボタン）をタップして、ブラウザ表示を確認します。
2. **実験1（スタイルの変更）**: `<style>` タグ内の `background-color: #f5f5f7;` を `background-color: #e0f7fa;` に変更し、保存後にプレビューを更新します。背景色が薄い青緑に変わることを確認します。
3. **実験2（要素の追加）**: `<body>` 内に 3つ目の `<div class="card">` を追加し、表示が自動的に他のカードと同じデザインになることを確認します。

---

## 6. iOS 9 (Mobile Safari) での注意点

本章で扱う基本構文は現代のWeb開発でもiOS 9でも共通ですが、以下の点に注意してください。

* **`-apple-system` フォント指定**: iOS 9で導入された San Francisco フォントを適用するために `font-family: -apple-system;` を使用します。
* **`viewport` メタタグ**: モバイル端末（iPad/iPhone）のSafariでは、`viewport` を正しく指定しないとPC用画面として極小サイズでレンダリングされてしまいます。

---

## 7. まとめ

* **HTML** はページの構造と意味を定義し、**CSS** はその見た目を指定します。
* ブラウザはHTMLとCSSを解析して **DOM** と **CSSOM** を生成し、それらを元にレイアウト計算を行って画面に描画します。
* 次章（第3章）では、CSSのより詳細な仕様である「ボックスモデル（Margin / Border / Padding）」や装飾について深掘りしていきます。
