---
title: "Web Vitalsとは何か"
emoji: "🌐"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["パフォーマンス","performance"]
published: true
---

※本記事は[web.dev](https://web.dev/vitals/)を学習用にまとめたものです。解釈に齟齬等あればコメントください。

# Overview
Googleで提唱されているWebページのUXを最適化するためのガイドライン。

# Core Web Vitals
UXに影響を与える重要なメトリクスとして以下３つを掲げている。
一方で、重要視されるメトリクスは日々変化するため注意が必要。
2020年執筆時点では、「**読み込み**」「**インタラクティブ性**」「**視覚的安定性**」に着目している。

### LCP (Largest Contentful Paint)
📝内容：表示領域における「最大の画像またはテキストブロック」のレンダリング時間を評価する
🎯目標値：**2.5秒以内** (75％ile)
💡目的：「ページが有効である」とユーザが知覚されるまでの読み込み速度を最適化し、ユーザを安心させるのに役立つ（あ、表示されないからこのページ見るのやめよ💢）

**最大コンテンツとされるもの**

- `<img>` 要素
- `<image>` 要素内の`<svg>`要素
- `<video>` 要素
- `url()関数`を介してロードされた背景画像を持つ要素
- テキストノードまたは他のインラインレベルのテキスト要素の子を含むブロックレベルの要素

### FIP (First Input Delay)
📝内容：ページに対してアクションを実行してから、実際にブラウザがイベントを受け付けるまでの時間を評価する
🎯目標値：**100ミリ秒以内** (75％ile)
💡目的：サイトとの応答性に対するユーザーの第一印象を測定するのに役立つ（見えてるのに操作できないとイライラする💢）

**最初の入力としてカウントされるもの、されないもの**

- ✔️ クリック
- ✔️ タップ
- ✔️ キーの押下
- ❌ スクロール
- ❌ ズーム


### CLS (Cumulative Layout Shift)
📝内容：ページの存続期間中に発生するすべてのレイアウトシフトについて、すべてのスコアの総和を評価する
🎯目標値：**0.1未満**
💡目的：いわゆる広告表示などでコンテンツの表示位置がガクッと変わる事象を評価する（間違って広告踏んで遷移するとイライラする💢）

**レイアウトシフトの計算方法**

```
layout shift score = impact fraction * distance fraction
```

- Impact fraction
	- ビューポート全体における何％くらいに影響を与えるか
- Distance fraction
	- 移動した距離がビューポートに対して何％くらいか

# Measure
改善のためには何より計測することが重要

### 本番環境での計測
RUM (RealUser Monitoring)を導入してるなら、それを使うべき
設定してないなら、無料のツールを使って計測可能

- [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)
- [Search Console](https://search.google.com/search-console/welcome)
- [CrUX dashboard](https://developers.google.com/web/updates/2018/08/chrome-ux-report-dashboard)

[web-vitals](https://github.com/GoogleChrome/web-vitals)というJavaScript libraryを使えば、クライアントサイドにおけるメトリクスを計測可能。
計測した内容をGoogle Analyticsなどに送信すると、データを収集して解析できる。

## 開発環境での計測
ラボデータ（合成データ）と呼ばれるものを開発時点で計測するには、以下のようなツールを活用

- [Web Vitals Chrome Extension](https://github.com/GoogleChrome/web-vitals-extension)
- [Lighthouse](https://github.com/GoogleChrome/lighthouse-ci)
- [WebPageTest](https://webpagetest.org/)

※ただし、合成データとRUMを比較すると、デバイス・ネットワーク・ロケーションなどに差があるため注意


# 感想
パフォーマンスを語っているだけのことはあって、参照した https://web.dev/ は表示速度がバリ速い
それぞれの改善方法については、また別の機会に