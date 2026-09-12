# 第4章: iOS 9におけるレイアウト設計（Flexboxとレガシー手法）

本章では、Webページ内の要素を横並びや上下左右にきれいに配置するための **「レイアウト設計」** について解説します。
iOS 9 (Mobile Safari 9) 環境では、現代の最新仕様（CSS Grid や `gap` プロパティなど）がそのまま使えないケースが多いため、**Flexboxでの `-webkit-` ベンダープレフィックス併記** や **レガシー手法（Float, Clearfix, Margin等の工夫）** を理解しておくことが不可欠です。

Textastic のプレビューで挙動を確認しながら、iOS 9でも崩れない堅牢なレイアウトテクニックを学んでいきましょう。

---

## 1. Webレイアウトの進化と配置手法の概要

Webの歴史において、要素の配置（レイアウト）を行う手法は以下のように変化してきました。

1. **Table レイアウト（歴史的背景）**: `<table>` タグで画面全体を枠線で区切る古い手法。
2. **Float レイアウト（レガシー手法）**: `float: left;` と Clearfix ハックを用いてブロックを横並びにする手法。
3. **Flexbox（フレックスボックス）**: 1次元（横方向または縦方向）のレイアウトを柔軟に制御する手法。**iOS 9における主力のレイアウト手法**。
4. **CSS Grid (モダン手法)**: 2次元（縦横）のグリッドレイアウト。※ iOS 10.3 以降対応のため、**iOS 9では使用不可**。

本章では、iOS 9環境で最も重要となる **Flexbox** と、その補完・基礎理解のための **レガシー手法 (Float / Position / Inline-block)** に焦点を当てます。

---

## 2. iOS 9における Flexbox の基本と `-webkit-` プレフィックス

Flexbox は要素の横並び、均等配置、上下中央揃えなどを極めてシンプルに実現できる仕組みです。
親要素（コンテナ）に `display: flex;` を指定し、子要素（アイテム）の並び順やサイズ指定を行います。

### 2.1 ベンダープレフィックス (`-webkit-`) の必須指定
iOS 9 (Safari 9) の WebKit エンジンでは、標準の `display: flex` に加えて **`-webkit-` プレフィックス** の併記が必要です。プレフィックスを省略すると、iOS 9環境ではレイアウトが崩れたり適用されなかったりします。

```css
/* Flexコンテナの基本設定 */
.flex-container {
  display: -webkit-flex; /* iOS 9 向けベンダープレフィックス */
  display: flex;         /* 標準構文 */

  -webkit-flex-direction: row; /* 横並び（デフォルト） */
  flex-direction: row;

  -webkit-justify-content: space-between; /* 両端に寄せて均等配置 */
  justify-content: space-between;

  -webkit-align-items: center; /* 上下中央揃え */
  align-items: center;

  -webkit-flex-wrap: wrap; /* 折り返しを許可 */
  flex-wrap: wrap;
}

/* Flexアイテムの設定 */
.flex-item {
  -webkit-flex: 1; /* 均等に伸縮 */
  flex: 1;
}
```

---

## 3. iOS 9の `gap` 非対応問題と余白設計

現代の Flexbox 開発では `gap: 16px;` の1行で要素間の余白を指定できますが、**`gap` プロパティは iOS 14.5 で導入されたため、iOS 9では完全に無視されます。**

iOS 9で要素間に余白（ギャップ）を作るためには、以下の代替テクニックを使用します。

### 手法 A: 隣接兄弟セレクタによるマージン指定
横並びのアイテム同士の間だけに余白を作りたい場合に適しています。

```css
/* 横並びアイテム */
.flex-item {
  -webkit-flex: 1;
  flex: 1;
}

/* 2つ目以降のアイテムの左側にだけマージンを設定 */
.flex-item + .flex-item {
  margin-left: 16px;
}
```

### 手法 B: ネガティブマージン & パディング法（カードグリッド等）
複数行に折り返す要素群（グリッド表示）で、上下左右の余白を均等に保ちたい場合のレガシー標準テクニックです。

```css
/* コンテナ（親）：外側の枠線を揃えるために左右にマイナスマージンを設定 */
.card-grid {
  display: -webkit-flex;
  display: flex;
  -webkit-flex-wrap: wrap;
  flex-wrap: wrap;
  margin-left: -8px;
  margin-right: -8px;
}

/* アイテム（子）：左右にパディングを設定し、疑似的にギャップを作る */
.card-item {
  -webkit-flex: 0 0 50%; /* 幅50%に固定 */
  flex: 0 0 50%;
  max-width: 50%;
  padding-left: 8px;
  padding-right: 8px;
  padding-bottom: 16px;
  box-sizing: border-box;
}
```

---

## 4. レガシーレイアウト手法 (Float, Inline-block, Position)

Flexbox が導入される以前に使われていたレガシー手法も、iOS 9環境の保守や特定の配置ケースで役立ちます。

### 4.1 Float と Clearfix (`.clearfix`)
`float: left;` を使うと、要素を左に寄せて後続のテキストやブロックを回り込ませることができます。親要素の高さが潰れる問題を防ぐために `Clearfix` 擬似要素を指定します。

```css
/* Clearfix（高さ潰れ防止用ハック） */
.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

.column-left {
  float: left;
  width: 50%;
}

.column-right {
  float: right;
  width: 50%;
}
```

### 4.2 Inline-block レイアウト
`display: inline-block;` を指定すると、テキストのように横に並ぶブロック要素を作ることができます。
※ HTML上の改行コードによって要素間に「意図しない空隙（約4px）」が発生するため、親要素に `font-size: 0;` を指定するハックが必要になる場合があります。

### 4.3 Position (絶対配置・固定配置)
要素を通常の文書構造から切り離し、重ね合わせや固定表示を行うプロパティです。

* **`position: relative;`**: 通常の位置を基準とする（子要素の基準点になる）。
* **`position: absolute;`**: `relative` を持つ最も近い親要素を基準に、上下左右のピクセル位置で配置する。
* **`position: fixed;`**: ブラウザの画面（ビューポート）を基準に位置を固定する（ヘッダー・フッター等）。

---

## 5. iOS 9 (Mobile Safari) 特有のレイアウト注意点とバグ回避策

iOS 9環境でレイアウトを構築する際、遭遇しやすい特有のバグと解決策です。

### 5.1 Flexアイテム内のテキスト溢れバグ (`min-width: 0`)
iOS 9の Flexbox 実装では、Flexアイテム内の長い文字列や画像が存在すると、アイテムが親コンテナを突き破って拡大してしまう現象が発生します。

* **解決策**: Flexアイテムに対して明示的に `min-width: 0;` を設定します。

```css
.flex-item {
  -webkit-flex: 1;
  flex: 1;
  min-width: 0; /* iOS 9でのFlexアイテムテキスト溢れを防止 */
}
```

### 5.2 慣性スクロール (`-webkit-overflow-scrolling: touch`)
iOS 9の Safari では、`overflow: scroll;` や `overflow: auto;` を指定した領域のスクロールが重く（指を離すと即座に止まる）感じられます。

* **解決策**: `-webkit-overflow-scrolling: touch;` を指定することで、iOS標準の心地よい「慣性スクロール」を有効化できます。

```css
.scroll-container {
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch; /* iOS 9でのスムーズな慣性スクロール */
}
```

### 5.3 `position: fixed` とソフトウェアキーボードの衝突
iOS 9では、フォームの `<input>` にフォーカスが当たってソフトウェアキーボードが表示された際、`position: fixed;` で固定したヘッダーやフッターの位置が上下にずれるバグがあります。キーボード操作が発生する入力画面では、過度な `fixed` 固定を避ける設計が推奨されます。

---

## 6. 実験ハンズオン (Textasticでの操作体験)

Textastic を使って実際のコードを入力し、iOS 9互換の Flexbox レイアウトとギャップ代替テクニックを体験してみましょう。

### 手順 1: サンプルファイル `samples/chapter4.html` の準備

Textasticで [`samples/chapter4.html`](./samples/chapter4.html) を開くか、新規ファイルとして以下のコードを作成します。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>第4章 レイアウト設計ラボ</title>
  <style>
    /* 全要素のボックスモデル初期化 */
    *, *::before, *::after {
      box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
      background-color: #f5f5f7;
      margin: 0;
      padding: 16px;
      color: #333333;
    }

    h1 {
      font-size: 20px;
      margin-bottom: 16px;
    }

    h2 {
      font-size: 16px;
      color: #007aff;
      margin-top: 24px;
      margin-bottom: 12px;
    }

    /* --------------------------------------------------
     * 1. Flexbox ヘッダーレイアウト (左右両端揃え + 上下中央)
     * -------------------------------------------------- */
    .header-bar {
      display: -webkit-flex; /* iOS 9 プレフィックス */
      display: flex;
      -webkit-justify-content: space-between;
      justify-content: space-between;
      -webkit-align-items: center;
      align-items: center;
      background-color: #ffffff;
      padding: 12px 16px;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.08);
    }

    .logo {
      font-weight: bold;
      font-size: 18px;
      color: #1c1c1e;
    }

    .nav-btn {
      background-color: #007aff;
      color: #ffffff;
      border: none;
      padding: 6px 12px;
      border-radius: 6px;
      font-size: 14px;
    }

    /* --------------------------------------------------
     * 2. iOS 9互換 カードグリッド (ネガティブマージン法)
     * -------------------------------------------------- */
    .card-grid {
      display: -webkit-flex; /* iOS 9 プレフィックス */
      display: flex;
      -webkit-flex-wrap: wrap;
      flex-wrap: wrap;
      margin-left: -6px;  /* 外側の余白調整 */
      margin-right: -6px;
    }

    .card-item {
      -webkit-flex: 0 0 50%; /* iOS 9での幅50%固定 */
      flex: 0 0 50%;
      max-width: 50%;
      padding-left: 6px;   /* 疑似ギャップ（左右6pxで計12pxの間隔） */
      padding-right: 6px;
      margin-bottom: 12px;
      min-width: 0;        /* iOS 9 テキスト溢れ防止バグ回避 */
    }

    .card-content {
      background-color: #ffffff;
      padding: 16px;
      border-radius: 8px;
      border: 1px solid #e5e5ea;
    }

    .card-title {
      font-weight: bold;
      margin-bottom: 6px;
      color: #333;
    }

    .card-desc {
      font-size: 13px;
      color: #666;
      line-height: 1.4;
    }

    /* --------------------------------------------------
     * 3. 慣性スクロールエリア
     * -------------------------------------------------- */
    .scroll-box {
      height: 100px;
      background-color: #ffffff;
      border: 1px solid #d1d1d6;
      border-radius: 8px;
      padding: 12px;
      overflow-y: scroll;
      -webkit-overflow-scrolling: touch; /* iOS 9 ぬるぬるスクロール */
    }
  </style>
</head>
<body>

  <h1>iOS 9 レイアウト設計ラボ</h1>

  <!-- 1. Flexbox ヘッダー -->
  <h2>1. Flexbox ヘッダー (両端揃え)</h2>
  <div class="header-bar">
    <div class="logo">My App</div>
    <button class="nav-btn">ログイン</button>
  </div>

  <!-- 2. カードグリッド (2列レイアウト) -->
  <h2>2. iOS 9 互換 2列カードグリッド</h2>
  <div class="card-grid">
    <div class="card-item">
      <div class="card-content">
        <div class="card-title">カード 1</div>
        <div class="card-desc">ネガティブマージン法により gap を使わずに余白を形成。</div>
      </div>
    </div>
    <div class="card-item">
      <div class="card-content">
        <div class="card-title">カード 2</div>
        <div class="card-desc">iOS 9 Safari でも左右均等に収まります。</div>
      </div>
    </div>
    <div class="card-item">
      <div class="card-content">
        <div class="card-title">カード 3</div>
        <div class="card-desc">min-width: 0 を指定してテキスト溢れを防ぎます。</div>
      </div>
    </div>
    <div class="card-item">
      <div class="card-content">
        <div class="card-title">カード 4</div>
        <div class="card-desc">折り返し (flex-wrap: wrap) の動作確認。</div>
      </div>
    </div>
  </div>

  <!-- 3. 慣性スクロール -->
  <h2>3. iOS 9 慣性スクロール</h2>
  <div class="scroll-box">
    <p>スクロールエリアの1行目です。</p>
    <p>2行目: -webkit-overflow-scrolling: touch を指定しています。</p>
    <p>3行目: iOS 9の画面上で指をフリックしてみてください。</p>
    <p>4行目: スムーズな慣性スクロールが体感できます。</p>
    <p>5行目: 最後の行です。</p>
  </div>

</body>
</html>
```

### 手順 2: プレビューの確認と実験

1. **Textasticでのプレビュー表示**: メガネアイコンをタップしてプレビューを表示します。
2. **ヘッダーの観察**: ロゴとボタンが左右両端にきれいに分かれて配置されていることを確認します。
3. **2列カードの観察**: 4つのカードが 2x2 の綺麗な格子状に並び、画面端からはみ出していないことを確認します。
4. **実験1（1列表示に変更）**:
   `.card-item` の `-webkit-flex: 0 0 50%; flex: 0 0 50%; max-width: 50%;` を `100%` に変更し、保存後にプレビューを更新します。カードが縦1列（スマホ向けレイアウト）に変化することを確認します。
5. **実験2（慣性スクロールの体感）**:
   「3. iOS 9 慣性スクロール」のボックス内を指でスワイプし、引っかかりなく軽快にスクロールすることを確認します。

---

## 7. まとめ

* **Flexbox** は iOS 9における最も重要かつ強力なレイアウト手法ですが、**`-webkit-` プレフィックス** の併記が必須です。
* iOS 9は **`gap` プロパティ非対応** のため、隣接兄弟マージン (`+`) やネガティブマージン＆パディング手法で余白を作ります。
* **`min-width: 0`** による Flexアイテムの文字溢れ防止、**`-webkit-overflow-scrolling: touch`** によるスクロール軽量化など、iOS 9特有の対策を講じることが重要です。
* 次章（第5章）からは、JavaScript の基本構文（変数、関数、制御構文）と iOS 9における互換性について学んでいきます。
