---
title: モデリングTips-1
renderer_options:
  gfm: false
tags:
  - blender
  - modeling
  - tips
category:
  - blender
id: modeling-tips-1
twitter_card: summary_large_image
twitter_img: /twitter.png
twitter_script: true
tweet: true
date: 2017-11-24 20:38:44
edit: 2017-11-24 20:38:44
---

第一回目は、楽器モデリングでよく使う円形メッシュについて。
円形メッシュの作り方と、注意点をいくつかまとめます。

# プリミティブメッシュ

<img src="/blender/modeling-tips-1/img_000.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_001.png" width="33%" align="left">
<br clear="left">

まずはBlenderで最初から用意されている、プリミティブメッシュを使う方法です。
「追加」メニューから追加することが出来て、
追加後は左側のツールパネルで頂点数や半径を変えることができます。

プリミティブメッシュはどれも、
追加するときにしかパラメーターを変えられません。
追加後に他の操作をすると、ツールパネルに表示されていたパラメーターが消え、
頂点数や半径などを指定出来なくなってしまいます。

これでは不便なので、円形メッシュを作成するときは
大抵他の方法を使います。

# スクリューモディファイア

<img src="/blender/modeling-tips-1/img_002.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_003.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_004.png" width="33%" align="left">
<br clear="left">
<img src="/blender/modeling-tips-1/img_005.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_006.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_007.png" width="33%" align="left">
<br clear="left">

スクリューモディファイアは、円形やらせん形状を生み出すモディファイアです。
まずは下準備として、頂点を1つだけ持つオブジェクトを作成します。

オブジェクトは何でも良いですが、
ここでは先ほどの円形プリミティブメッシュを使用します。

編集モードでどこか一つの頂点を選択し、
選択メニューの「反転」で選択状態を反転させます。
メッシュメニューの「削除 > 頂点」で頂点を削除します。

この状態でスクリューモディファイアを追加すると円形が出来上がります。
編集モードだと頂点だけ表示されますが、
オブジェクトモードにすると円形の辺が表示されるようになります。

欠点としては、頂点や辺だけだとウェイトを設定することが難しく、
ボーン変形を上手く設定できない場合があります。
実際ウェイトペイントモードに映り、ウェイトを設定しようとしても
なにも変化がありません。
一応いくつか回避方法があります。

<img src="/blender/modeling-tips-1/img_008.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_009.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_010.png" width="33%" align="left">
<br clear="left">

一つは自動ウェイトです。
オブジェクトを選択した状態で、Shiftを押しながらボーンを選択すると、
オブジェクトとボーンがどちらも選択され、
なおかつボーンがアクティブ状態になります。

この状態でオブジェクト(またはポーズ)メニューの
「親 > アーマチュア変形 > 自動のウェイトで」を選択すると、
頂点一つでもウェイト設定をすることが出来ます。

<img src="/blender/modeling-tips-1/img_011.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_012.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_013.png" width="33%" align="left">
<br clear="left">

他には、一度スクリューモディファイアを適用させて面を作り、
その状態でウェイトを塗る方法があります。

ウェイトを塗った後に頂点を削除し、
再度スクリューモディファイアを適用させると
ウェイト設定が残るようになります。

ウェイト設定が手間ですが、
スクリューモディファイアは編集する頂点数がぐっと減るので
とても便利です。


# べベルモディファイア

<img src="/blender/modeling-tips-1/img_014.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_015.png" width="33%" align="left">
<img src="/blender/modeling-tips-1/img_016.png" width="33%" align="left">
<br clear="left">

べベルモディファイアは面取りを行うモディファイアです。
これを使用して、四角柱のオブジェクトを円柱にします。

まずは追加メニューから円柱オブジェクトを追加します。
このとき頂点を4、ふたのフィルタイプを「なし」にすることで
断面が四角い筒が出来上がります。

このオブジェクトにべベルモディファイアを追加します。
モディファイアの「幅」プロパティの値を大きくし、
「セグメント」プロパティの値を増やしていくと
徐々に円柱に近づいていきます。

この方法だと頂点の重なりが出来てしまうので、
モディファイア適用後に重複頂点を削除しないといけないですが、
他と比べてウェイト設定がしやすいです。

# カーブ+べベル

(編集中)

# Wonder mesh アドオン

(編集中)
