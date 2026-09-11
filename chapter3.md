# 第3章: CSSスタイリング基礎（ボックスモデルと装飾）

本章では、CSSの最も重要な基本概念である **「ボックスモデル (Box Model)」** と、Webページ上の要素を装飾する基本プロパティについて解説します。
iOS 9 (Mobile Safari) の Textastic 環境で実際に幅や余白、装飾の値を変更し、表示結果の変化を体験しながら理解を深めていきましょう。

---

## 1. CSSボックスモデルの基本構造

HTMLのすべての要素は、画面上では四角い「箱（ボックス）」として扱われます。このボックスは中心から外側に向かって以下の4つの領域（層）で構成されています。

```
+-----------------------------------+
|              MARGIN               |  (外側の余白)
|  +-----------------------------+  |
|  |           BORDER            |  |  (枠線)
|  |  +-----------------------+  |  |
|  |  |        PADDING        |  |  |  (内側の余白)
|  |  |  +-----------------+  |  |  |
|  |  |  |     CONTENT     |  |  |  |  (テキストや画像などのコンテンツ)
|  |  |  +-----------------+  |  |  |
|  |  +-----------------------+  |  |
|  +-----------------------------+  |
+-----------------------------------+
```

### 1.1 4つの領域の役割
1. **Content (コンテンツ領域)**
   * テキストや画像など、要素の実際の領域です。`width` や `height` プロパティでサイズを指定します。
2. **Padding (内側余白)**
   * コンテンツ領域と Border（枠線）の間に位置する余白です。背景色を設定すると、この Padding 領域まで背景色が塗られます。
3. **Border (枠線)**
   * Padding の外側を取り囲む線です。線の太さ、スタイル（実線、破線など）、色を指定できます。
4. **Margin (外側余白)**
   * Border のさらに外側に位置する、他の要素との間隔を作るための余白です。Margin は常に透明（シースルー）であり、隣り合う要素とのディスタンスを調整します。

---

## 2. `box-sizing` プロパティの仕組み

要素の幅 (`width`) や高さ (`height`) を指定した際、それが「どの領域までのサイズを意味するのか」を決定するのが `box-sizing` プロパティです。

### 2.1 `content-box` (デフォルト動作)
`box-sizing: content-box;` はブラウザのデフォルト設定です。この場合、指定した `width` は **コンテンツ領域のみ** に適用されます。

* **要素全体の計算上の幅**:
  `全体の幅 = width + padding(左右) + border(左右)`

> **例**: `width: 200px; padding: 20px; border: 5px solid #000;` と指定した場合、画面上で実際に占有する全体の幅は **`200 + 40 + 10 = 250px`** となります。

### 2.2 `border-box` (推奨されるモダン設定)
`box-sizing: border-box;` を指定すると、`width` に **Padding と Border が含まれる** ようになります。

* **要素全体の計算上の幅**:
  `全体の幅 = width (PaddingやBorderが含まれる)`

> **例**: `width: 200px; padding: 20px; border: 5px solid #000;` と指定しても、全体の幅は **`200px` のまま固定** され、コンテンツ領域が自動的に縮小されます (`200 - 40 - 10 = 150px`)。

### 実務でのベストプラクティス
現代のWeb制作（およびiOS 9対応開発）では、全要素に対して `box-sizing: border-box;` を一括適用する「リセットCSS」手法が標準的です。これにより、レイアウト崩れを防ぎ、直感的なサイズ計算が可能になります。

```css
/* 全要素に border-box を適用する一括指定 */
*, *::before, *::after {
  box-sizing: border-box;
}
```

---

## 3. テキストと要素の装飾プロパティ

ページの見た目を整えるための主要なCSS装飾プロパティです。

### 3.1 色の設定 (`color`, `background-color`)
* **`color`**: テキストの色を指定します。
* **`background-color`**: 要素の背景色を指定します。

```css
p {
  color: #333333; /* Hexカラーコード */
  background-color: rgba(0, 0, 0, 0.05); /* RGBA (赤, 緑, 青, 不透明度) */
}
```

### 3.2 フォントとテキスト指定 (`font-family`, `font-size`, `font-weight`, `line-height`)
* **`font-family`**: 使用するフォントの種類を指定します。
* **`font-size`**: 文字の大きさを指定します (`16px`, `1rem` 等)。
* **`font-weight`**: 文字の太さを指定します (`normal`, `bold`, `600` 等)。
* **`line-height`**: 行の高さを指定します（単位なしの数値 `1.5`〜`1.6` 程度が読みやすい推奨値です）。

```css
body {
  font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
  font-size: 16px;
  font-weight: normal;
  line-height: 1.6;
}
```

### 3.3 枠線・角丸・影 (`border`, `border-radius`, `box-shadow`)
* **`border`**: 枠線の太さ・種類・色をまとめて指定します（例: `1px solid #ccc`）。
* **`border-radius`**: 角を丸くします（例: `8px`, `50%` で丸型）。
* **`box-shadow`**: 要素に影をつけます（水平オフセット, 垂直オフセット, ぼかし半径, 影の色）。

```css
.card {
  border: 1px solid #dddddd;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
```

---

## 4. iOS 9 (Mobile Safari) におけるスタイリングの注意点

iOS 9環境でスタイリングを行う際は、以下の点に注意が必要です。

1. **システムフォント指定 (`-apple-system`)**:
   iOS 9では標準システムフォントとして San Francisco が導入されました。`font-family: -apple-system;` を記述することで、iOS 9上で最も美しいシステムフォントが適用されます。
2. **`box-shadow` のパフォーマンス**:
   iOS 9のようなレガシー端末（古いiPadなど）では、過剰に大きなぼかし半径を持つ `box-shadow` や多数の影を適用すると、スクロール時の描画パフォーマンスが低下することがあります。シンプルかつ軽量な影を指定することが望ましいです。
3. **`-webkit-` ベンダープレフィックスの有無**:
   本章で扱った `box-sizing`, `border-radius`, `box-shadow`, `color`, `background-color` などの基本的なプロパティは、iOS 9でもプレフィックスなしで安定して動作します。

---

## 5. 実験ハンズオン (Textasticでの操作体験)

それでは、Textastic を使って実際のコードを入力し、ボックスモデルの計算変化やスタイリングの効果を確認してみましょう。

### 手順 1: サンプルファイル `samples/chapter3.html` の準備

Textasticで [`samples/chapter3.html`](./samples/chapter3.html) を開くか、新規ファイルとして以下のコードを作成します。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>第3章 ボックスモデル実験ラボ</title>
  <style>
    /* 全要素のボックスモデル初期化 */
    *, *::before, *::after {
      box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
      background-color: #f5f5f7;
      margin: 0;
      padding: 20px;
      color: #333333;
    }

    .container {
      max-width: 600px;
      margin: 0 auto;
    }

    /* デモボックス1: border-box */
    .box-border-box {
      box-sizing: border-box;
      width: 100%;
      padding: 20px;
      border: 5px solid #007aff; /* iOSブルーの枠線 */
      background-color: #ffffff;
      border-radius: 10px;
      margin-bottom: 20px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    /* デモボックス2: content-box */
    .box-content-box {
      box-sizing: content-box;
      width: 100%;
      padding: 20px;
      border: 5px solid #ff3b30; /* iOSレッドの枠線 */
      background-color: #ffffff;
      border-radius: 10px;
      margin-bottom: 20px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    .label {
      font-weight: bold;
      font-size: 18px;
      margin-bottom: 8px;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>ボックスモデル & 装飾ラボ</h1>

    <div class="box-border-box">
      <div class="label">1. box-sizing: border-box (青枠)</div>
      <p>幅 100% の中に Padding(20px) と Border(5px) が収まるため、親要素からはみ出しません。</p>
    </div>

    <div class="box-content-box">
      <div class="label">2. box-sizing: content-box (赤枠)</div>
      <p>幅 100% の外側に Padding と Border が追加されるため、画面右側にはみ出して横スクロールが発生します。</p>
    </div>
  </div>

</body>
</html>
```

### 手順 2: プレビューの確認と実験

1. **プレビューで比較**: Textasticのプレビュー画面を開きます。青枠のボックス (`border-box`) は画面内に綺麗に収まり、赤枠のボックス (`content-box`) は画面幅からはみ出していることを確認します。
2. **実験1（Paddingを変更してみる）**:
   `.box-border-box` の `padding: 20px;` を `padding: 40px;` に書き換えて保存・更新します。全体の幅は変わらず、内側の余白だけが広がることを確認します。
3. **実験2（装飾のカスタマイズ）**:
   `.box-border-box` に `background-color: #e5f2ff;` や `border-radius: 20px;` を追加し、背景色や角丸がどのように変化するか観察します。

---

## 6. まとめ

* **ボックスモデル** は Content、Padding、Border、Margin の 4 つの領域で構成されます。
* **`box-sizing: border-box;`** を使用すると、`width` に Padding と Border が含まれるようになり、レイアウトのサイズ調整が格段に扱いやすくなります。
* **`color`**, **`font-family`**, **`border`**, **`border-radius`**, **`box-shadow`** を組み合わせることで、直感的で美しいUIデザインを構築できます。
* 次章（第4章）では、これらのボックス要素を横並びや複雑な配置にレイアウトする **「iOS 9におけるレイアウト設計 (Flexboxとレガシー手法)」** について学んでいきます。
