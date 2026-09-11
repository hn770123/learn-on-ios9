# 今さら聞けない！0から始めるjs/css

このリポジトリはweb系の実装経験がほぼないITエンジニア向けに、jsとcssを0から解説することを目的とします。読み物では身につかないので、出先で気軽に変更とプレビューができるios9のiPadで動かすTextasticを使って、何を変えたら、どう変わるのか、実験しながら会得していくロードマップです。

---

## 目次

- [概要](#概要)
- [学習ロードマップ（作成予定コンテンツ）](#学習ロードマップ作成予定コンテンツ)
- [第2章: HTML/CSSの基礎構造とページの仕組み](#第2章-htmlcssの基礎構造とページの仕組み)
- [ドキュメント・ガイド一覧](#ドキュメントガイド一覧)
  - [iOS 9 対応 CSS 開発・完全リファレンスガイド](#ios-9-対応-css-開発完全リファレンスガイド)
  - [iOS 9 JavaScript 互換性・リファレンスガイド](#ios-9-javascript-互換性リファレンスガイド)

---

## 概要

本リポジトリでは、iOS 9 (Mobile Safari 9) の環境を基準とし、JavaScript および CSS の基本的な概念から実効的な互換性・フォールバック対応までを学べるコンテンツを提供しています。

---

## 学習ロードマップ（作成予定コンテンツ）

本リポジトリで順次作成を予定している学習コンテンツの目次（各章のタイトル）です。iOS 9（Textastic環境）で実際にコードを動かしながら学べる構成となっています。

- **第1章: 環境構築と基本操作**（iOS 9 Textasticでの実験環境づくり）
- **[第2章: HTML/CSSの基礎構造とページの仕組み](#第2章-htmlcssの基礎構造とページの仕組み)**
- **第3章: CSSスタイリング基礎**（ボックスモデルと装飾）
- **第4章: iOS 9におけるレイアウト設計**（Flexboxとレガシー手法）
- **第5章: JavaScript基礎構文**（変数・関数・制御構文）
- **第6章: DOM操作とイベントハンドリング**（動きのあるUI作成）
- **第7章: 非同期処理と通信基礎**（XMLHttpRequestとPromise）
- **第8章: 実践ハンズオン**（iOS 9互換コンポーネントの構築とデバッグ）

---

## 第2章: HTML/CSSの基礎構造とページの仕組み

本章では、Webページがどのように構成され、ブラウザ（iOS 9 Mobile Safari / Textasticのプレビュー機能）上でどのようにレンダリング（描画）されるのか、基礎的な仕組みと構造について解説します。

---

### 2.1 HTMLの基本構造（文書の骨組み）

HTML（HyperText Markup Language）は、Webページの**意味や骨組み（構造）**を記述するための言語です。タグと呼ばれる `<要素名>` でテキストやコンテンツを囲むことで構造化します。

以下は、最も標準的なHTML5ドキュメントの基本骨組みです。

```html
<!DOCTYPE html>
<!-- HTML5文書であることをブラウザに宣言する記述 -->
<html lang="ja">
  <head>
    <!-- headタグ内: ページのメタ情報（文字コード、タイトル、CSSの読み込みなど）を記述 -->
    <meta charset="UTF-8">
    <!-- iOS 9等のモバイル端末で正しい画面幅・拡大率で表示するための設定 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第2章 実験ページ</title>
    <!-- 外部CSSファイルの読み込み -->
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <!-- bodyタグ内: 実際にブラウザの画面に表示されるコンテンツを記述 -->
    <h1>HTML/CSSの基礎構造</h1>
    <p>この文章は本文（段落）です。</p>
  </body>
</html>
```

#### 各要素の役割
1. **`<!DOCTYPE html>`**: ドキュメントのタイプがHTML5であることをブラウザに伝えます。
2. **`<html>`**: 全体を囲むルート（最上位）要素です。`lang="ja"` で言語（日本語）を指定します。
3. **`<head>`**: 画面上には直接表示されない、ページの管理情報（タイトル、文字コード、外部ファイル参照など）をまとめます。
   * **`meta viewport`**: iOS Safari等のモバイルブラウザにおいて、画面サイズに応じたレスポンシブな描画を行うために必須のメタタグです。
4. **`<body>`**: ブラウザの画面上に実際にレンダリングされる見出し、本文、画像、リンクなどの全コンテンツを記述します。

---

### 2.2 HTML要素・タグと木構造（DOMツリー）

HTMLはタグの「入れ子構造（ネスト）」によって作られます。ブラウザはHTMLテキストを読み込むと、要素間の親子関係を解析して内部的に**DOM（Document Object Model）ツリー**と呼ばれる樹木構造を作成します。

```text
html
 ├── head
 │    ├── meta
 │    ├── title
 │    └── link
 └── body
      ├── h1
      └── p
```

* **親要素 (Parent)**: ある要素を直接包んでいる外側の要素（例: `body` は `h1` の親要素）
* **子要素 (Child)**: ある要素の直下にある内側の要素（例: `h1` は `body` の子要素）
* **階層関係（ツリー構造）**: この親子関係をもとに、CSSスタイルやJavaScriptによる操作が適用されます。

---

### 2.3 CSSの適用方法とリンクの仕組み

CSS（Cascading Style Sheets）は、HTMLの骨組みに対して**見た目（色、サイズ、配置、余白など）**を設定します。
HTMLにCSSを適用する方法は主に3つあります。

#### 1. 外部CSSファイルを読み込む（推奨）
HTMLの `<head>` 内で `<link>` タグを使って外部 `.css` ファイルを読み込みます。保守性やコードの見通しが良くなるため、最も一般的な手法です。

```html
<!-- HTMLファイル (index.html) -->
<head>
  <link rel="stylesheet" href="style.css">
</head>
```

```css
/* CSSファイル (style.css) */
/* 見出し1の文字色を青色にする設定 */
h1 {
  color: #0066cc;
}
```

#### 2. `<style>` タグによるインライン記述
HTMLの `<head>` 内に直接 `<style>` タグを書き、その中にCSSを記述します。単一ファイルで実験したい場合に便利です。

```html
<head>
  <style>
    /* ページ全体の背景色と文字スタイルを設定 */
    body {
      background-color: #f5f5f5;
      font-family: sans-serif;
    }
  </style>
</head>
```

#### 3. style属性による直接指定（インラインスタイル）
HTML要素の `style` 属性に直接CSSプロパティを指定します。特定の1要素だけに緊急で適用したい場合などを除き、原則として多様は避けます。

```html
<p style="color: red; font-weight: bold;">注意書きのテキストです。</p>
```

---

### 2.4 iOS 9 (Textastic) での実験ハンズオン

Textasticを使って、実際にコードを書き換えてブラウザ（プレビュー）での描画変化を確認してみましょう。

#### 実験用コード (index.html)
Textasticで以下の内容を含む `index.html` を作成（または既存ファイルを編集）し、プレビューで確認してください。

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8">
    <!-- iOS 9環境で適切な表示幅にする設定 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第2章 ハンズオン実験</title>
    <style>
      /* 全体背景と基本文字 */
      body {
        background-color: #eef2f5;
        font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
        padding: 16px;
      }

      /* メインカードコンテナ */
      .card {
        background-color: #ffffff;
        border-radius: 8px;
        padding: 20px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      }

      /* タイトルスタイル */
      .card-title {
        color: #333333;
        font-size: 20px;
        margin-top: 0;
      }

      /* 本文スタイル */
      .card-text {
        color: #666666;
        line-height: 1.6;
      }
    </style>
  </head>
  <body>
    <!-- カード型の構造を表すHTML -->
    <div class="card">
      <h2 class="card-title">HTMLとCSSの連動実験</h2>
      <p class="card-text">
        Textasticの画面で <code>.card</code> の <code>background-color</code> や <code>padding</code> を書き換えてプレビューを更新してみましょう。数値や色を変えることで描画がどのように変化するか観察します。
      </p>
    </div>
  </body>
</html>
```

#### 実験のポイント
1. **背景色の変更**: `.card` の `background-color: #ffffff;` を `#ffeb3b`（黄色）などに変えてプレビューを表示する。
2. **パディング（余白）の変更**: `padding: 20px;` を `padding: 40px;` に変え、枠内の余白が広がることを確認する。
3. **HTML構造の追加**: `<div class="card">` の中に `<button>ボタン</button>` などを追加して、HTMLの階層構造がどのように画面に反映されるか確かめる。

---

## ドキュメント・ガイド一覧

### iOS 9 対応 CSS 開発・完全リファレンスガイド
* **ファイル**: [ios9-css-guide.md](./ios9-css-guide.md)
* **概要**: iOS 9 (Safari 9) におけるCSSの互換性、利用可能な構文、非対応機能、Modern CSSとの比較やフォールバック戦略（Flexboxプレフィックス、`gap` 代替手法、`position: fixed` の挙動等）についてまとめています。

### iOS 9 JavaScript 互換性・リファレンスガイド
* **ファイル**: [ios9_javascript_guide.md](./ios9_javascript_guide.md)
* **概要**: iOS 9 (JavaScriptCore) で利用可能な ES5/ES6 構文や非対応な ES2016+ 構文（`async/await`, `fetch`, オプショナルチェイニング等）、および代替記法（`XMLHttpRequest`, `indexOf` 等）についてまとめています。
