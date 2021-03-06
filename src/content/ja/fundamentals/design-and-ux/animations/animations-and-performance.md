project_path: /web/_project.yaml
book_path: /web/fundamentals/_book.yaml
description: アニメーションの動きがスムーズでないと、ユーザー エクスペリエンスは悪化します。

{# wf_updated_on: 2016-08-23 #}
{# wf_published_on: 2014-08-08 #}

# アニメーションとパフォーマンス {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}
{% include "web/_shared/contributors/samthorogood.html" %}

アニメーションを使用する際は、必ず 60 fps を維持してください。アニメーションがスムーズに動かなかったり途中で止まったりすると、ユーザーの目に留まりやすく、ネガティブな印象を与えます。

### TL;DR {: .hide-from-toc }
* 任意の CSS プロパティをアニメーション化するときの影響を把握し、アニメーションによってパフォーマンスが下がらないように注意してください。
* ページ（レイアウト）の形状変更や描画処理を伴うプロパティのアニメーション化は、リソースを多く消費します。
* 可能な限り、形状と不透明度を変更するようにしてください。
*  <code>will-change</code> を使用して、ブラウザにアニメーション化する対象を通知します。


アニメーション プロパティはリソースを消費し、その程度はプロパティによって異なります。たとえば、要素の `width` および `height` をアニメーション化すると、要素の形状を変化させることになるため、ページ上の他の要素の移動やサイズ変更が必要になる場合があります。この処理は「レイアウト」（Firefox など Gecko ベースのブラウザの場合は「リフロー」）と呼ばれ、ページに多くの要素が含まれていると、リソースを多く消費する可能性があります。レイアウトがトリガーされるたびに、通常はページまたはその一部をペイントする必要があります。この処理は一般的に、レイアウト操作そのものよりもリソースを消費します。

可能な限りレイアウトやペイントをトリガーするプロパティのアニメーション化は避けてください。つまり、大半の最新のブラウザでは、`opacity` または `transform` にのみアニメーションを適用するようにしてください。どちらのプロパティもブラウザによって十分に最適化されており、JavaScript と CSS のどちらでアニメーションの処理を行うかは関係ありません。

個々の CSS プロパティによってトリガーされる処理の一覧は、[CSS Triggers](http://csstriggers.com) でご確認いただけます。さらに [High Performance Animations on HTML5 Rocks](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/) の包括的な作成ガイドも用意されています。

###  will-change プロパティの使用

[`will-change`](https://dev.w3.org/csswg/css-will-change/) を使用すると、要素のプロパティを変更する予定があることをブラウザに通知できます。これによりブラウザは、実際に変更を行う前に、適切な最適化を行うことができます。ただし、`will-change` は使いすぎないように注意してください。多用するとブラウザがリソースを浪費し、パフォーマンスの低下を招くおそれがあります。

一般的な目安としては、ユーザー操作またはアプリケーションの状態変化によってアニメーションが 200 ミリ秒以内で開始される場合は、`will-change` を使用すべきだと考えられています。ほとんどの場合、変更するプロパティの種類によらず、アプリの現在のビュー内にある要素でアニメーション化する予定ものはすべて `will-change` を有効化します。前のガイドで使用したボックス サンプルにおいて、形状と不透明度の変更のために `will-change` を追加すると次のようになります。


    .box {
      will-change: transform, opacity;
    }
    

この機能に対応しているブラウザは、[現時点では Chrome、Firefox、Opera](http://caniuse.com/#feat=will-change) です。これらのプロパティの変更やアニメーションをサポートするために、内部では適切な最適化が行われます。

##  CSS と JavaScript のパフォーマンス比較

パフォーマンスの観点から、CSS や JavaScript のアニメーションの優劣を議論する ウェブ ページやコメント スレッドは数多くあります。ここで留意すべき点がいくつかあります。

* CSS ベースのアニメーション、およびネイティブでサポートされているウェブ アニメーションは、一般的に「コンポジタ スレッド」と呼ばれるスレッドで処理されます。これは、スタイリングやレイアウト、描画処理、JavaScript が実行されるブラウザの「メイン スレッド」とは異なります。つまり、ブラウザがメインスレッド上で負荷の高いタスクをいくつか実行している場合でも、これらのアニメーションは中断されることなく続行できます。

* 形状と不透明度に対するその他の変更も、多くの場合はコンポジタ スレッドで処理されます。

* アニメーションがペイント、レイアウト、またはその両方をトリガーする場合は、「メイン スレッド」で処理する必要があります。これは、CSS ベースと JavaScript ベースの両方のアニメーションに当てはまります。その結果、レイアウトやペイントのオーバーヘッドにより、CSS や JavaScript の実行に関連するあらゆる作業が妨げられるという問題が発生する場合があります。

特定のプロパティのアニメーション化によってトリガーされる動作の詳細については、[CSS トリガー](http://csstriggers.com)を参照してください。




{# wf_devsite_translation #}
