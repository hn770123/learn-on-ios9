# 今更聞けない！JSとCSS！ 〜iOS 9で壊して学ぶWeb技術〜

ようこそ！本リポジトリは、あえてレガシー環境である **iOS 9 (2015年〜2016年リリース / Mobile Safari 9)** を舞台に、JavaScriptとCSSの現代的仕様と旧仕様の違い、壊れる原因、およびフォールバック戦略を「壊して学ぶ」ための解説・ドキュメント集です。

現代のモダンWeb（2026年）では当たり前のように使われている機能（CSS Grid、CSS変数、`gap`、`async/await`、`fetch`、オプショナルチェイニングなど）が、なぜiOS 9では動かないのか？どう書けば動くのか？を読み物として体系的に学べる目次となっています。

---

## 📚 ドキュメント構成・総合目次

### 🎨 [1. iOS 9 対応 CSS 開発・完全リファレンスガイド](./ios9-css-guide.md)

iOS 9環境におけるCSSの互換性、ベンダープレフィックス、非対応プロパティ、よくある表示バグと回避策のまとめです。

- **[1. iOS 9 CSS環境の概要](./ios9-css-guide.md#1-ios-9-css環境の概要)**
  - 主な特徴と注意点（`-webkit-` プレフィックス、Flexbox仕様の過渡期、未実装機能）
- **[2. iOS 9 で使える CSS 命令・構文まとめ](./ios9-css-guide.md#2-ios-9-で使える-css-命令構文まとめ)**
  - 2.1 セレクタ・擬似クラス・擬似要素
  - 2.2 ボックスモデル・レイアウト
  - 2.3 フレックスボックス (Flexbox) ※`-webkit-` プレフィックス必須
  - 2.4 変形・アニメーション・グラフィック
  - 2.5 テキスト・フォント・メディアクエリ
- **[3. iOS 9 で使えない (非対応・危険な) CSS 命令・構文まとめ](./ios9-css-guide.md#3-ios-9-で使えない-非対応危険な-css-命令構文まとめ)**
  - 3.1 非対応のレイアウト・モジュール (CSS Grid, `gap`, ロジカルプロパティ, `aspect-ratio`, コンテナクエリ)
  - 3.2 非対応のカスタムプロパティ・関数・セレクタ (CSS変数, `@supports`, `:has()`, `:is()`, `:where()`, モダンカラー記法)
  - 3.3 非対応または制限のある装飾・フィルター (`backdrop-filter`, `clip-path`, `contain`, `will-change`)
- **[4. 今でも (2026年現在も) 現役の頻出命令・構文](./ios9-css-guide.md#4-今でも-2026年現在も-現役の頻出命令構文)**
  - モダン開発と共通して使える基礎プロパティ一覧
- **[5. 2026年現在では使われなくなった (iOS 9では使わざるを得ない) 命令・構文](./ios9-css-guide.md#5-2026年現在では使われなくなった-ios-9では使わざるを得ない-命令構文)**
  - 5.1 大量の `-webkit-` ベンダープレフィックス
  - 5.2 Flexbox `gap` 代替テクニック（マージン＆ネガティブマージン）
  - 5.3 iOS独自属性 `-webkit-overflow-scrolling: touch`
  - 5.4 Floatレイアウトと Clearfix (`.clearfix`)
  - 5.5 Table-cell レイアウト (`display: table-cell`)
  - 5.6 アスペクト比固定の `padding-top` ハック
  - 5.7 Viewport Units (100vh) バグの回避策
- **[6. iOS 9 向け実践コード例 & フォールバック構成](./ios9-css-guide.md#6-ios-9-向け実践コード例--フォールバック構成)**
  - 6.1 完全クロスブラウザ対応 Flexbox コンポーネント
  - 6.2 Modern CSS vs iOS 9 対応 CSS 対比表
- **[7. iOS 9 特有のCSSバグとハマりどころ](./ios9-css-guide.md#7-ios-9-特有のcssバグとハマりどころ)**
  - `position: fixed` のスクロール・キーボードバグ
  - Flexbox内の文字あふれ (`min-width: 0`)
  - `flex-shrink` のデフォルト値問題
  - `100vh` スクロールオーバーフロー
- **[8. まとめ・開発上のアドバイス](./ios9-css-guide.md#8-まとめ開発上のアドバイス)**

---

### ⚡ [2. iOS 9 JavaScript 互換性・リファレンスガイド](./ios9_javascript_guide.md)

iOS 9 (Mobile Safari 9) のJavaScriptエンジン (JavaScriptCore) における構文対応状況、ES5/ES6/ES2016+ の境界線、非同期処理やAPIの書き換え手法のまとめです。

- **[1. iOS 9 で使える命令・構文](./ios9_javascript_guide.md#1-ios-9-で使える命令構文)**
  - 構文・基本機能 (`let`/`const`, アロー関数, クラス, テンプレートリテラル, デフォルト引数, Rest Parameter, `for...of`, `Promise`)
  - 組み込みオブジェクト・メソッド (`Map`/`Set`, `Symbol`, `Object.assign()`, 配列高階関数など)
- **[2. iOS 9 で使えない命令・構文](./ios9_javascript_guide.md#2-ios-9-で使えない命令構文)**
  - 構文・演算子 (`?.`, `??`, オブジェクトスプレッド, `async`/`await`, ES Modules, べき乗演算子, `BigInt`, プライベートフィールド)
  - 組み込みメソッド (`Array.prototype.includes()`, `Object.values()`, `flat()`, `Promise.allSettled()` など)
  - Web API / ブラウザ機能 (`fetch()`, `URLSearchParams`, Web Components, `IntersectionObserver` など)
- **[3. 今でも現役の頻出命令・構文](./ios9_javascript_guide.md#3-今でも現役の頻出命令構文)**
  - ES6先行対応機能を中心としたモダン・レガシー共通構文
- **[4. 2026年現在では使われなくなった（iOS 9では使わざるを得ない）命令・構文・パターン](./ios9_javascript_guide.md#4-2026年現在では使われなくなったios-9では使わざるを得ない命令構文パターン)**
  - 1. `XMLHttpRequest` (XHR)
  - 2. 即時実行関数 (IIFE)
  - 3. `indexOf()` による存在チェック
  - 4. `var` 変数宣言および `var self = this;`
  - 5. 手動のヌルチェック (Guard Check)
  - 6. `arguments` オブジェクト
- **[5. コード書き換え・対比サンプル](./ios9_javascript_guide.md#5-コード書き換え対比サンプル)**
  - HTTPリクエスト (`fetch` vs `XMLHttpRequest` + `Promise`)
  - 要素・文字列の存在チェック (`includes` vs `indexOf`)
  - ネストされたプロパティの安全な参照 (`?.` vs AND条件ガード)
- **[6. まとめ](./ios9_javascript_guide.md#6-まとめ)**

---

## 🛠️ このリポジトリの楽しみ方・学習方法

1. **壊してみる**
   - Modernな記述（例: `gap`, `fetch()`, `?.`）を使って、iOS 9でどのように動作不全を起こすか（画面崩れやエラー）を確認します。
2. **原因を知る**
   - 本ガイドを参照し、どのバージョンでその仕様が導入されたのか、iOS 9のブラウザ（Mobile Safari 9）でなぜエラーになるのかを理解します。
3. **直してみる (フォールバック)**
   - レガシーな代替記述（`-webkit-` プレフィックス、ネガティブマージン、`XMLHttpRequest`、`indexOf` など）に書き換えて正常動作させます。
