# 今さら聞けない！0から始めるjs/css

このリポジトリはweb系の実装経験がほぼないITエンジニア向けに、jsとcssを0から解説することを目的とします。読み物では身につかないので、出先で気軽に変更とプレビューができるios9のiPadで動かすTextasticを使って、何を変えたら、どう変わるのか、実験しながら会得していくロードマップです。

---

## 目次

- [概要](#概要)
- [ドキュメント・ガイド一覧](#ドキュメントガイド一覧)
  - [iOS 9 対応 CSS 開発・完全リファレンスガイド](#ios-9-対応-css-開発完全リファレンスガイド)
  - [iOS 9 JavaScript 互換性・リファレンスガイド](#ios-9-javascript-互換性リファレンスガイド)

---

## 概要

本リポジトリでは、iOS 9 (Mobile Safari 9) の環境を基準とし、JavaScript および CSS の基本的な概念から実効的な互換性・フォールバック対応までを学べるコンテンツを提供しています。

---

## ドキュメント・ガイド一覧

### iOS 9 対応 CSS 開発・完全リファレンスガイド
* **ファイル**: [ios9-css-guide.md](./ios9-css-guide.md)
* **概要**: iOS 9 (Safari 9) におけるCSSの互換性、利用可能な構文、非対応機能、Modern CSSとの比較やフォールバック戦略（Flexboxプレフィックス、`gap` 代替手法、`position: fixed` の挙動等）についてまとめています。

### iOS 9 JavaScript 互換性・リファレンスガイド
* **ファイル**: [ios9_javascript_guide.md](./ios9_javascript_guide.md)
* **概要**: iOS 9 (JavaScriptCore) で利用可能な ES5/ES6 構文や非対応な ES2016+ 構文（`async/await`, `fetch`, オプショナルチェイニング等）、および代替記法（`XMLHttpRequest`, `indexOf` 等）についてまとめています。
