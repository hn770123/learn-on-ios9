# iOS 9 (Mobile Safari 9) JavaScript 互換性・リファレンスガイド

iOS 9 (2015年リリース / Mobile Safari 9) のJavaScriptエンジン (JavaScriptCore) で動作するJavaScript構文・APIについてのまとめです。
iOS 9は **ECMAScript 5 (ES5) に完全対応** しており、当時の最新仕様であった **ECMAScript 2015 (ES6) の一部機能が先行実装** されています。しかし、ES2016以降の構文や多くのモダンWeb APIは利用できません。

---

## 1. iOS 9 で使える命令・構文

iOS 9では、ES5の全機能に加えて、ES6 (ES2015) の主要な構文・オブジェクトの多くがサポートされています。

### 構文・基本機能 (ES6先行対応分含む)
* **`let` / `const`**: ブロックスコープ変数宣言（※ strict mode以外での挙動やTDZの初期バグに留意が必要ですが、基本構文として利用可能です）
* **アロー関数 (`() => {}`)**: 簡略化された関数構文および lexical `this` のバインディング
* **クラス (`class`, `extends`, `constructor`)**: ES6クラス構文
* **テンプレートリテラル (`` `string ${var}` ``)**: 埋め込み文字列
* **デフォルト引数 (Default parameters)**: `function(a = 1) {}`
* **Rest Parameter**: `function(...args) {}`
* **`for...of` ループ**: イテラブルオブジェクトのループ処理
* **`Promise`**: 非同期処理の標準オブジェクト

### 組み込みオブジェクト・メソッド
* **`Map` / `Set` / `WeakMap` / `WeakSet`**: ES6コレクションオブジェクト
* **`Symbol`**: プリミティブ型 Symbol
* **`Object.assign()`**: オブジェクトのマージ・浅いコピー
* **`Object.keys()` / `Object.defineProperty()`** (ES5)
* **`Array.prototype.map()` / `filter()` / `reduce()` / `forEach()` / `indexOf()` / `slice()` / `concat()`** (ES5)
* **`JSON.parse()` / `JSON.stringify()`** (ES5)

---

## 2. iOS 9 で使えない命令・構文

iOS 9では動作せず、使用すると構文エラー (`SyntaxError`) や例外 (`TypeError`) になる主な機能です。

### 構文・演算子 (ES2016以降 / 未サポートのES6)
* **オプショナルチェイニング (`?.`)**: 例: `obj?.prop` (ES2020)
* **ヌリッシュ・コーレシング演算子 (`??`)**: 例: `a ?? b` (ES2020)
* **スプレッド構文 (`...`) の一部**:
  * **オブジェクトスプレッド (`{ ...obj }`)**: ES2018仕様のため不可
  * 配列スプレッドはES6仕様ですが、iOS 9では一部制限・不具合があるため注意が必要
* **`async` / `await`**: 非同期構文 (ES2017)
* **ES Modules (`import` / `export`)**: ブラウザ直のモジュール読み込み (iOS 10.3/11から対応)
* **べき乗演算子 (`**`)**: 例: `2 ** 3` (ES2017)
* **`BigInt`**: 長大整数 (ES2020)
* **プライベートフィールド・メソッド (`#field`)**: (ES2022)

### 組み込みメソッド
* **`Array.prototype.includes()`**: 配列の要素存在チェック (ES2016)
* **`String.prototype.includes()` / `startsWith()` / `endsWith()`** (一部ES6機能はiOS9で不完全)
* **`Object.values()` / `Object.entries()`**: オブジェクトの値・エントリー取得 (ES2017)
* **`Object.fromEntries()`**: (ES2019)
* **`Array.prototype.flat()` / `flatMap()`**: (ES2019)
* **`Promise.prototype.finally()`** (ES2018) / **`Promise.allSettled()`** (ES2020)

### Web API / ブラウザ機能
* **`fetch()` API**: (iOS 10.3から対応。iOS 9は未対応)
* **`URLSearchParams`**: クエリ文字列操作 API
* **Web Components (Custom Elements v1, Shadow DOM v1)**
* **`IntersectionObserver` / `ResizeObserver`**

---

## 3. 今でも現役の頻出命令・構文

2026年現在のモダンJavaScript開発でも一般的に広く使われ、かつ iOS 9 でも問題なく動作する構文・メソッドです。

* **`let` / `const`**: 変数・定数宣言の標準
* **アロー関数 (`() => {}`)**: コールバック関数や無名関数
* **テンプレートリテラル (`` `Hello ${name}` ``)**: 文字列結合
* **クラス (`class`)**: オブジェクト指向記法
* **`Promise`**: 非同期処理 (`then` / `catch` チェーン)
* **`Array` 高階関数**: `map()`, `filter()`, `reduce()`, `forEach()`
* **`Object.keys()` / `Object.assign()`**: オブジェクト操作
* **`JSON.parse()` / `JSON.stringify()`**: JSONデータの相互変換
* **DOM操作 API**: `document.querySelector()`, `document.querySelectorAll()`, `addEventListener()`
* **タイマー関数**: `setTimeout()`, `setInterval()`

---

## 4. 2026年現在では使われなくなった（iOS 9では使わざるを得ない）命令・構文・パターン

モダンWeb開発（2026年時点）ではより安全で簡潔な代替構文（`async/await`, `fetch`, `?.`, `includes` 等）があるため非推奨・旧式とされていますが、**iOS 9環境で直接動作させるには使わざるを得ない記述パターン** です。

### 1. `XMLHttpRequest` (XHR)
* **モダン**: `fetch()` または `async/await` + `fetch`
* **iOS 9での理由**: `fetch()` API が存在しないため、AJAX通信には `XMLHttpRequest` を使用（またはPromiseでラップ）する必要があります。

### 2. 即時実行関数 (IIFE: `(function() { ... })()`)
* **モダン**: ES Modules (`import`/`export`) やファイル単位のスコープ
* **iOS 9での理由**: `<script type="module">` が使えないため、グローバル汚染を防ぐカプセル化にIIFEが必須となります。

### 3. `indexOf()` による存在チェック
* **モダン**: `Array.prototype.includes()` / `String.prototype.includes()`
* **iOS 9での理由**: `includes()` が存在しないため、`arr.indexOf(item) !== -1` や `str.indexOf(sub) !== -1` を使用します。

### 4. `var` 変数宣言および `var self = this;` / `var that = this;`
* **モダン**: `const`/`let` および アロー関数による `this` 保持
* **iOS 9での理由**: アロー関数が使えない古いコードとの互換性確保や、iOS 9独自のレキシカルスコープのバグを回避するため、あえて `var` や `self = this` パターンが使用されることがあります。

### 5. 手動のヌルチェック (Guard Check)
* **モダン**: オプショナルチェイニング (`data?.user?.name`)
* **iOS 9での理由**: `?.` が構文エラーになるため、`data && data.user && data.user.name` のように段階的なAND条件チェックが必要です。

### 6. `arguments` オブジェクト
* **モダン**: Rest Parameter (`...args`)
* **iOS 9での理由**: iOS 9のRest Parameterの挙動不安定を避けるため、可変長引数の取得に `Array.prototype.slice.call(arguments)` が使用される場合があります。

---

## 5. コード書き換え・対比サンプル

iOS 9対応コードを作成・メンテナンスする際の対比例です。

### ① HTTPリクエスト
```javascript
// 【モダン (iOS 9不可)】
async function getData() {
  const res = await fetch('/api/data');
  const data = await res.json();
  return data;
}

// 【iOS 9互換】
function getDataiOS9() {
  return new Promise(function(resolve, reject) {
    var xhr = new XMLHttpRequest();
    xhr.open('GET', '/api/data');
    xhr.onload = function() {
      if (xhr.status >= 200 && xhr.status < 300) {
        resolve(JSON.parse(xhr.responseText));
      } else {
        reject(new Error(xhr.statusText));
      }
    };
    xhr.onerror = function() {
      reject(new Error('Network error'));
    };
    xhr.send();
  });
}
```

### ② 要素・文字列の存在チェック
```javascript
// 【モダン (iOS 9不可)】
if (items.includes('target')) { ... }

// 【iOS 9互換】
if (items.indexOf('target') !== -1) { ... }
```

### ③ ネストされたプロパティの安全な参照
```javascript
// 【モダン (iOS 9不可)】
const name = user?.profile?.name;

// 【iOS 9互換】
const name = (user && user.profile) ? user.profile.name : undefined;
```

---

## 6. まとめ
iOS 9向けにJavaScriptを書く場合、Babelなどのトランスパイラを通してES5互換コードにビルドするのが現代の最も安全なアプローチです。手書きで対応する場合は、`fetch` や `includes` などの未実装APIに注意し、`XMLHttpRequest` や `indexOf` などの旧来の構文・APIを活用してください。
