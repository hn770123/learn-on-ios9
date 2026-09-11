# iOS 9 対応 CSS 開発・完全リファレンスガイド

本ドキュメントは、iOS 9（Safari 9.0 〜 9.3 / 2015年〜2016年リリース）におけるCSSの互換性、利用可能な命令・構文、非対応機能、および現代（2026年現在）における開発アプローチとの比較・フォールバック戦略をまとめたWebエンジニア向けの包括的リファレンスです。

---

## 1. iOS 9 CSS環境の概要

iOS 9の標準ブラウザであるSafari 9は、CSS3の多くの機能をサポートしていますが、最新仕様（Flexbox仕様の最新形やCSS Grid、CSS変数など）への移行過渡期に位置しています。

### 主な特徴と注意点
* **ベンダープレフィックス (`-webkit-`) の多用**: Flexbox、Transform、Transition、Animationなど、多くの主要CSS3機能において `-webkit-` プレフィックスが必須です。
* **Flexbox仕様の過渡期**: iOS 9のFlexbox実装にはいくつかのアニメーションバグやサイズ計算の仕様差分（バグ）が存在します。
* **モダンCSS機能の未実装**: CSS Grid Layout、CSS Custom Properties (変数)、`gap` プロパティ、ロジカルプロパティ等は未対応または制限付きです。

---

## 2. iOS 9 で使える CSS 命令・構文まとめ

iOS 9（Safari 9）で正常にサポートされている、または `-webkit-` プレフィックスを伴うことで安全に使用できるCSS命令・構文の一覧です。

### 2.1 セレクタ・擬似クラス・擬似要素
* **基本セレクタ**: 要素名、クラス (`.class`)、ID (`#id`)、属性セレクタ (`[type="text"]`)
* **結合子**: 親子 (`>`), 子孫 (` `), 隣接 (`+`), 一般兄弟 (`~`)
* **擬似クラス**:
  * `:first-child`, `:last-child`, `:nth-child()`, `:nth-last-child()`, `:only-child`
  * `:hover`, `:active`, `:focus`, `:checked`, `:enabled`, `:disabled`
  * `:not()`（単一セレクタのみ。複雑なセレクタ `:not(.a, .b)` は非対応）
* **擬似要素**: `::before`, `::after`, `::first-line`, `::first-letter`, `::selection`

### 2.2 ボックスモデル・レイアウト
* **ボックスサイズ決定**: `box-sizing: border-box` / `content-box`
* **表示形式**: `display: block`, `inline`, `inline-block`, `none`, `table`, `table-cell`, `table-row`
* **配置**: `position: static`, `relative`, `absolute`, `fixed`
* **切り抜き・溢れ**: `overflow: visible` / `hidden` / `scroll` / `auto`
* **浮動レイアウト**: `float: left` / `right`, `clear: both` / `left` / `right`

### 2.3 フレックスボックス (Flexbox) ※`-webkit-` プレフィックス必須
iOS 9ではFlexboxをサポートしていますが、**`-webkit-` プレフィックスが必須**です。標準構文のみでは動作しません。

```css
.flex-container {
  display: -webkit-flex;
  display: flex;
  -webkit-flex-direction: row;
  flex-direction: row;
  -webkit-justify-content: space-between;
  justify-content: space-between;
  -webkit-align-items: center;
  align-items: center;
  -webkit-flex-wrap: wrap;
  flex-wrap: wrap;
}

.flex-item {
  -webkit-flex: 1;
  flex: 1;
}
```

### 2.4 変形・アニメーション・グラフィック
* **2D / 3D トランスフォーム**: `-webkit-transform` (例: `-webkit-transform: translate3d(0,0,0);`)
* **トランジション**: `-webkit-transition`
* **キーフレーム・アニメーション**: `@-webkit-keyframes` および `-webkit-animation`
* **角丸**: `border-radius`
* **ドロップシャドウ・テキストシャドウ**: `box-shadow`, `text-shadow`
* **グラデーション**: `-webkit-linear-gradient`, `-webkit-radial-gradient`（および標準 `linear-gradient`）
* **不透明度・背景**: `opacity`, `background-size: cover / contain`, `background-clip`

### 2.5 テキスト・フォント・メディアクエリ
* **メディアクエリ**: `@media screen and (max-width: 768px)`, `@media (-webkit-min-device-pixel-ratio: 2)` (Retinaディスプレイ対応)
* **Webフォント**: `@font-face` (WOFF, WOFF2 サポート)
* **テキスト自動サイズ調整防止**: `-webkit-text-size-adjust: 100%`
* **iOS独自慣性スクロール**: `-webkit-overflow-scrolling: touch`

---

## 3. iOS 9 で使えない (非対応・危険な) CSS 命令・構文まとめ

iOS 9では動作しない、または重大なレンダリングバグを引き起こすため**使用不可**（またはフォールバックが必須）なCSS命令・構文です。

### 3.1 非対応のレイアウト・モジュール
* **CSS Grid Layout (`display: grid`)**: iOS 10.3 (Safari 10.1) から対応。iOS 9では完全に動作しません。
* **Flexbox / Grid ギャップ (`gap`, `row-gap`, `column-gap`)**: Flexboxにおける `gap` は iOS 14.5 から対応。iOS 9では無効化されます。
* **ロジカルプロパティ (Logical Properties)**:
  * `margin-inline`, `margin-block`, `padding-inline`, `padding-block`, `inset` 等は非対応。`margin-left`, `top` 等の物理プロパティが必須です。
* **アスペクト比固定 (`aspect-ratio`)**: iOS 15 から対応。iOS 9では使用できません (`padding-top` ハックが必要)。
* **コンテナクエリ (`@container`, `container-type`)**: iOS 16 から対応。

### 3.2 非対応のカスタムプロパティ・関数・セレクタ
* **CSS Custom Properties / 変数 (`var(--main-color)`)**: iOS 9.3で部分サポートが始まりましたが実装が非常に不安定であり、iOS 9全般としては「使用不可」とみなす必要があります（iOS 10で正式サポート）。
* **Feature Queries (`@supports`)**: iOS 9 Safari 9.0で導入されたものの、評価フラグメントにバグがあり信頼性が低いため推奨されません。
* **モダンセレクタ / 擬似クラス**:
  * `:has()` (iOS 15.4+)
  * `:is()`, `:where()` (iOS 14+)
  * `:focus-within` (iOS 10.3+)
  * `:focus-visible` (iOS 15.4+)
* **モダンカラー関数・記法**:
  * カンマなし記法 (`rgb(255 0 0)`), `color-mix()`, `oklch()`, `lab()`, `hwb()` 等は非対応。従来スタイルの `rgb(255, 0, 0)` や Hexコード (`#ff0000`) を使用します。

### 3.3 非対応または制限のある装飾・フィルター
* **背景ぼかし (`backdrop-filter`)**: iOS 9では未対応（または著しいバグ発生）。
* **マスク・クリッピング**: `clip-path` (iOS 9では非常に制限的かつバグ多発)。
* **`contain` / `will-change`**: メモリ最適化プロパティは非対応または挙動不安定。

---

## 4. 今でも (2026年現在も) 現役の頻出命令・構文

iOS 9で動作し、かつ2026年現在のモダンWeb開発でも引き続き標準的・日常的に使用されているCSS命令・構文です。

| 分類 | 命令・構文 | iOS 9での注意点 / 現代との比較 |
| :--- | :--- | :--- |
| **ボックスモデル** | `box-sizing: border-box;` | 現代でもリセットCSSの基本。iOS 9でも完全に動作。 |
| **基本レイアウト** | `display: block; / inline-block; / none;` | Web開発の基礎構文として不変。 |
| **ポジショニング** | `position: relative; / absolute; / fixed;` | レイアウト構築の基礎。※iOS 9の `fixed` はキーボード表示時のバグに注意。 |
| **Flexbox** | `display: flex;`, `align-items`, `justify-content` | 現代レイアウトの中心。iOS 9対応には `-webkit-` プレフィックスの併記が必要。 |
| **視覚効果** | `opacity`, `border-radius`, `box-shadow` | デザイン表現の定番。プレフィックスなしで安定動作。 |
| **アニメーション** | `transform`, `transition`, `@keyframes` | iOS 9では `-webkit-` プレフィックスが必須だが、概念・記述自体は現代と同一。 |
| **レスポンシブ** | `@media (max-width: ...)` | ブレイクポイント制御の標準。現代でも現役。 |
| **単位** | `px`, `%`, `em`, `rem` | スタイリングの基本単位。`rem` はiOS 9でも問題なく利用可能。 |

---

## 5. 2026年現在では使われなくなった (iOS 9では使わざるを得ない) 命令・構文

2026年のモダン開発（iOS 15+ ターゲット等）では不要・非推奨となったものの、iOS 9をサポート対象に含める場合には**必須となるレガシーな命令・構文・手法**です。

### 5.1 大量の `-webkit-` ベンダープレフィックス
現代ではAutoprefixer等のビルドツールで自動付与・または削除されますが、iOS 9では以下のプロパティに `-webkit-` プレフィックスが絶対に欠かせません。

* `-webkit-display: flex` / `display: -webkit-flex`
* `-webkit-flex-direction`, `-webkit-justify-content`, `-webkit-align-items`
* `-webkit-transform`, `-webkit-transform-origin`
* `-webkit-transition`, `-webkit-transition-duration`
* `-webkit-animation`, `@-webkit-keyframes`
* `-webkit-user-select`
* `-webkit-filter`

### 5.2 Flexbox `gap` 代替テクニック（マージン＆ネガティブマージン）
2026年現在では `gap: 16px;` 1行で記述できますが、iOS 9は `gap` 非対応のため、以下のようなレガシー手法が必要です。

```css
/* 2026年現在のモダン記述 (iOS 9非対応) */
.container-modern {
  display: flex;
  gap: 16px;
}

/* iOS 9 対応記述 */
.container-legacy {
  display: -webkit-flex;
  display: flex;
  margin: -8px; /* ネガティブマージンで外枠調整 */
}
.container-legacy > .item {
  padding: 8px; /* パディングによる疑似ギャップ */
}

/* または隣接兄弟セレクタによるマージン指定 */
.flex-item + .flex-item {
  margin-left: 16px;
}
```

### 5.3 iOS独自属性 `-webkit-overflow-scrolling: touch`
iOS 9のSafariでモーダル内や特定要素のスクロールを滑らか（慣性スクロール）にするために必須だったプロパティです（iOS 13以降で不要・非推奨化）。

```css
.scrollable-area {
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch; /* iOS 9でぬるぬるスクロールさせるために必須 */
}
```

### 5.4 Floatレイアウトと Clearfix (`.clearfix`)
Flexboxの完全な代替、またはiOS 9における複雑なFlexbox計算バグを避けるため、2行・3行カラムをFloatで組むケースがありました。

```css
/* レガシー Clearfix ハック */
.clearfix::after {
  content: "";
  display: table;
  clear: both;
}
.column {
  float: left;
  width: 50%;
}
```

### 5.5 Table-cell レイアウト (`display: table-cell`)
上下中央揃え (`vertical-align: middle`) を確実に行うため、Flexboxの代わりに使われたレガシーテクニックです。

```css
.table-container {
  display: table;
  width: 100%;
}
.table-cell {
  display: table-cell;
  vertical-align: middle;
}
```

### 5.6 アスペクト比固定の `padding-top` (Padding-bottom) ハック
`aspect-ratio: 16 / 9;` が使えないため、パーセンテージパディングを利用したハックが用いられます。

```css
/* 16:9 アスペクト比固定ハック */
.aspect-ratio-16-9 {
  position: relative;
  width: 100%;
  padding-top: 56.25%; /* 9 / 16 = 56.25% */
}
.aspect-ratio-16-9 > .content {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
```

### 5.7 Viewport Units (100vh) バグの回避策
iOS 9 Safariでは、上下のツールバー（アドレスバー・フッター）の出し入れ時に `100vh` の高さ計算が崩れ、画面下部が隠れるバグが存在します。そのため、`height: 100vh;` の代わりに JavaScript でインナーハイトを取得するか、`height: 100%` と html/body への継承を使用します。

---

## 6. iOS 9 向け実践コード例 & フォールバック構成

### 6.1 完全クロスブラウザ対応 Flexbox コンポーネント

```css
/* カードリストコンポーネント (iOS 9 完全対応) */
.card-grid {
  display: -webkit-flex;
  display: flex;
  -webkit-flex-wrap: wrap;
  flex-wrap: wrap;
  margin-left: -10px;
  margin-right: -10px;
}

.card-item {
  -webkit-flex: 0 0 50%;
  flex: 0 0 50%;
  max-width: 50%;
  padding-left: 10px;
  padding-right: 10px;
  box-sizing: border-box;
}

@media screen and (max-width: 600px) {
  .card-item {
    -webkit-flex: 0 0 100%;
    flex: 0 0 100%;
    max-width: 100%;
  }
}
```

### 6.2 Modern CSS vs iOS 9 対応 CSS 対比表

| 目的 | 2026年現在のモダン記述 | iOS 9 対応（使わざるを得ない）記述 |
| :--- | :--- | :--- |
| **レイアウト** | `display: grid; grid-template-columns: repeat(3, 1fr);` | `display: -webkit-flex;` + 幅 `%` または `float: left` |
| **要素間隔** | `gap: 20px;` | マージン調整 (`margin-right`) やパディング内包 |
| **アスペクト比** | `aspect-ratio: 16 / 9;` | `padding-top: 56.25%;` ハック |
| **CSS変数** | `color: var(--primary-color);` | ハードコード（Sass/SCSS等のコンパイルで展開） |
| **ぼかし背景** | `backdrop-filter: blur(10px);` | 不透明度の高い背景色 (`background: rgba(...)`) |
| **両端揃え** | `justify-content: space-between;` | `-webkit-justify-content: space-between;` |
| **スクロール** | `overflow-y: auto;` | `overflow-y: scroll; -webkit-overflow-scrolling: touch;` |

---

## 7. iOS 9 特有のCSSバグとハマりどころ

1. **`position: fixed` のスクロール時・キーボード表示時の破綻**
   * 入力フォーム (`<input>`) にフォーカスが当たってソフトウェアキーボードが表示された際、`position: fixed` で固定したヘッダーやフッターの位置がずれる、あるいは固定が解除される重篤なバグがあります。
   * **対策**: モーダル表示時に `body` のスクロールを固定する、あるいはJSでレイアウトを調整する。

2. **Flexboxアイテム内の文字あふれ・縮小不具合 (`min-width: 0`)**
   * iOS 9のFlexboxでは、Flexアイテム内の長い英単語やテキストが要素からはみ出る現象が発生します。
   * **対策**: Flexアイテムに明示的に `min-width: 0;` または `width: 100%;` を指定する。

3. **Flexboxの `flex-shrink` のデフォルト値解釈**
   * 仕様上は `flex-shrink: 1` ですが、iOS 9のWebKit実装においてアイテムが意図せず崩れるケースがあります。
   * **対策**: 縮小させたくない要素には明示的に `-webkit-flex-shrink: 0; flex-shrink: 0;` を指定する。

4. **100vh スクロール領域のオーバーフロー**
   * Safariのアドレスバー縮小時に `vh` 単位が画面外まで突き抜ける問題。
   * **対策**: `height: 100%` + コンテナの縦幅固定手法を使用する。

---

## 8. まとめ・開発上のアドバイス

iOS 9向けのCSSを構築する際は、以下の原則を守ることが推奨されます。

1. **Autoprefixer の導入**: 手動で `-webkit-` プレフィックスを書くのはミスに繋がるため、PostCSS + Autoprefixer (`browserslist: "iOS >= 9"`) を利用してビルド時に自動付与する。
2. **CSS Custom Properties や CSS Grid の非使用**: Preprocessor（Sass / Less）で変数を展開し、レイアウトは Flexbox または Float を基準にする。
3. **実機・シミュレータでの検証**: 定義上サポートされていてもバグが存在することが多いため、特に Flexbox、`position: fixed`、`-webkit-overflow-scrolling` 周りは実際の描画を確認する。
