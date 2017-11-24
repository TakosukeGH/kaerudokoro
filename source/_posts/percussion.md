---
title: 打楽器モデル解説
date: 2016-06-09 05:22:13
edit: 2016-06-09 05:22:13
renderer_options:
  gfm: false
tags:
- mmd
- percussion
category:
- mmd
id: percussion
twitter_card: summary_large_image
twitter_img: /mmd/percussion/twitter.png
twitter_script: true
tweet: true
---

自作打楽器モデルの解説ページです。  
基本的な使い方は↓を参照してください。  
ブロマガURL : <http://ch.nicovideo.jp/takosuke/blomaga/ar980544>

# モデル一覧

|                            画像                            |                            モデル名                           | モデルファイル名(バージョン番号省略) |   MMD表示名   |
|------------------------------------------------------------|---------------------------------------------------------------|--------------------------------------|---------------|
| <img src="/mmd/percussion/mb_bass.000.png" width="50">     | [バスドラ用接続パーツ](#バスドラ用接続パーツ)                 | mounting_bracket_bass_drum.pmx       | mb_bass       |
| <img src="/mmd/percussion/mb_snare.000.png" width="50">    | [スネア用（タム用）接続パーツ](#スネア用（タム用）接続パーツ) | mounting_bracket_snare_drum.pmx      | mb_snare      |
| <img src="/mmd/percussion/mb_stand.000.png" width="50">    | [スタンド用接続パーツ](#スタンド用接続パーツ)                 | mounting_bracket_stand.pmx           | mb_stand      |
| <img src="/mmd/percussion/rack_2.000.png" width="50">      | [ラック（2つ穴）](#ラック)                                    | percussion_rack_2.pmx                | pr_2          |
| <img src="/mmd/percussion/rack_4.000.png" width="50">      | [ラック（4つ穴）](#ラック)                                    | percussion_rack_4.pmx                | pr_4          |
| <img src="/mmd/percussion/rack_6.000.png" width="50">      | [ラック（6つ穴）](#ラック)                                    | percussion_rack_6.pmx                | pr_6          |
| <img src="/mmd/percussion/rod_I.000.png" width="50">       | [I型ロッド](#I型ロッド)                                       | rod_I.pmx                            | rod_I         |
| <img src="/mmd/percussion/rod_L.000.png" width="50">       | [L型ロッド](#L型ロッド)                                       | rod_L.pmx                            | rod_L         |
| <img src="/mmd/percussion/rod_R.000.png" width="50">       | [R型ロッド](#R型ロッド)                                       | rod_R.pmx                            | rod_R         |
| <img src="/mmd/percussion/rod_Z.000.png" width="50">       | [Z型ロッド](#Z型ロッド)                                       | rod_Z.pmx                            | rod_Z         |
| <img src="/mmd/percussion/pb.000.png" width="50">          | [プラスチックブロック](#プラスチックブロック)                 | plastic_block.pmx                    | plastic_block |
| <img src="/mmd/percussion/wb.000.png" width="50">          | [ウッドブロック](#ウッドブロック)                             | wood_block.pmx                       | wood_block    |
| <img src="/mmd/percussion/head.000.png" width="50">        | [汎用ドラムヘッド](#汎用ドラムヘッド)                         | head.pmx                             | head          |
| <img src="/mmd/percussion/mute_felt.000.png" width="50">   | [フェルトミュート](#フェルトミュート)                         | mute_felt.pmx                        | mute_felt     |
| <img src="/mmd/percussion/mute_gel.000.png" width="50">    | [ジェルミュート](#ジェルミュート)                             | mute_gel.pmx                         | mute_gel      |
| <img src="/mmd/percussion/mute_simple.000.png" width="50"> | [シンプルミュート](#シンプルミュート)                         | mute_simple.pmx                      | mute_simple   |
| <img src="/mmd/percussion/mute_square.000.png" width="50"> | [四角ミュート](#四角ミュート)                                 | mute_square.pmx                      | mute_square   |


----
# バスドラ用接続パーツ

<img src="/mmd/percussion/mb_bass.000.png" width="33%" align="left">
<img src="/mmd/percussion/mb_bass.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/mb_bass.002.jpg" width="33%" align="left">
<br clear="left">

バスドラに取り付けるパーツです。
図のようにバスドラのリムを挟む形で設置します。

この接続パーツを使用する場合、たいていはL型ロッドを使用します。

太鼓の中心にボーンがある場合は、そのボーンを外部親に設定するのがおすすめです。
rootボーンは太鼓の中心に置き、centerボーンでリムまで持っていくと、
後々の調整が楽かと思います。

上の二つ目の画像のように、リムの上側に設置した後、
rootボーンを回転させて三つ目の画像の位置にもっていくことができます。

# スネア用（タム用）接続パーツ

<img src="/mmd/percussion/mb_snare.000.png" width="33%" align="left">
<img src="/mmd/percussion/mb_snare.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/mb_snare.002.jpg" width="33%" align="left">
<br clear="left">

スネアやタムのリムに取り付けるパーツです。
太鼓の中心にボーンがある場合は、そのボーンを外部親に設定するのがおすすめです。
rootボーンは太鼓の中心に置き、centerボーンでリムまで持っていくと、
後々の調整が楽かと思います。

咥え部分の幅はモーフで変更可能です。


# スタンド用接続パーツ

<img src="/mmd/percussion/mb_stand.000.png" width="33%" align="left">
<img src="/mmd/percussion/mb_stand.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/mb_stand.002.jpg" width="33%" align="left">
<br clear="left">

スタンド等の棒状のものに取り付けるパーツです。
咥え部分の幅を調整可能なので、二番目の図のような使い方も可能です。
咥え幅はボーンではなく、モーフで変更してください。

頂点数を削減するため、rodを差し込む場所には穴を空けていません。

魚圭糸工様のドラムセットに取り付ける場合は、
「○○主軸」系のボーンに取り付けるのがおすすめです。


# ラック

<img src="/mmd/percussion/rack_2.000.png" width="33%" align="left">
<img src="/mmd/percussion/rack_2.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/rack_6.001.jpg" width="33%" align="left">
<br clear="left">

ラックはスタンド等に取り付けて、複数のロッドをさすことができるパーツです。
モデル名が穴の数と対応していて、rack_2は二つの穴、rack_6は六つの穴を持ちます。

複数のロッドをさすことができるので、leafボーンも複数あります。
奏者側からみて一番左がleaf.001です。
右に行くほど数字が大きくなります。

穴の間隔を変えることはできませんが、
tipボーンを動かすとモデルを左右に広げることができます。


----
# ロッド

## I型ロッド

<img src="/mmd/percussion/rod_I.000.png" width="33%" align="left">
<img src="/mmd/percussion/rod_I.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/rod_I.002.jpg" width="33%" align="left">
<br clear="left">

シンプルな直線型のロッドです。
曲げたりは出来ませんが、伸ばすことはできます。
シンプルなので使いやすいです。

## L型ロッド

<img src="/mmd/percussion/rod_L.000.png" width="33%" align="left">
<img src="/mmd/percussion/rod_L.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/rod_L.002.jpg" width="33%" align="left">
<br clear="left">

直角に曲がったロッドです。
曲げ角度は変更できませんが、辺の長さを変更できます。

基本的にはバスドラ用接続パーツと一緒に使います。
現状他の使い道は少ないと思います。
三枚目の画像はかなり無理やりタムに設置したパターンです。


## R型ロッド

<img src="/mmd/percussion/rod_R.000.png" width="33%" align="left">
<img src="/mmd/percussion/rod_R.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/rod_R.002.jpg" width="33%" align="left">
<br clear="left">

先端が少し曲がったロッドです。
角度は30°曲げました。

角度はある程度変更可能ですが、あまりきちんと設定していないので大きく曲げることはできません。
曲げると三つ目の画像の様にメッシュが変形します。

## Z型ロッド

<img src="/mmd/percussion/rod_Z.000.png" width="33%" align="left">
<img src="/mmd/percussion/rod_Z.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/rod_Z.002.jpg" width="33%" align="left">
<br clear="left">

二つの直角を持つロッドです。
角度は基本的に変えずに使用します。
角度を変える場合は[スタンド用接続パーツ](#スタンド用接続パーツ)等を使用して下さい。

三つの辺があるので、形状の自由度が高いです。
他の楽器の間を縫うように設置できるので便利です。


----
# プラスチックブロック

<img src="/mmd/percussion/pb.000.png" width="33%" align="left">
<img src="/mmd/percussion/pb.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/pb.002.jpg" width="33%" align="left">
<br clear="left">

ポコポコと音がなるプラスチック製のブロックです。
モーフでブロック部分のRGBが変更可能です。

beamボーンを動かすと付け根の部分が伸びます。
モーフでブロック全体を大きくすることもできます。

effectフォルダの中にノーマルマップが入っているので、
エフェクトと一緒に使うと表面に凸凹が付くようになります。
一応エフェクトファイルも一緒に入れました。

MMEを導入し、「MMEffect > エフェクト割り当て」メニューを開き、
モデルを右クリックしてサブセット展開します。
「plastic_block」にeffectフォルダのAlternativeFull.fxを割り当てると、
エフェクトが適用されます。


# ウッドブロック

<img src="/mmd/percussion/wb.000.png" width="33%" align="left">
<img src="/mmd/percussion/wb.001.jpg" width="33%" align="left">
<img src="/mmd/percussion/wb.002.jpg" width="33%" align="left">
<br clear="left">

ポコポコと音がなる木製のブロックです。
beamボーンを動かすと付け根の部分が伸びます。
モーフでブロック全体を大きくすることもできます。

extra_texturesの中に代えのテクスチャが入っています。
通常のものよりキズが多く、汚れたテクスチャになっています。
wood.pngに名前を変更し、texturesの中の同じ名前のものと取り換えてください。

大き目のサイズのテクスチャを使用しているので、
重い場合は適宜テクスチャをリサイズして使用して下さい。


----
# 汎用ドラムヘッド

<img src="/mmd/percussion/head.000.png" width="33%" align="left">
<img src="/mmd/percussion/head.001.png" width="33%" align="left">
<img src="/mmd/percussion/head.002.png" width="33%" align="left">
<br clear="left">

「皮」と「ロゴ」だけのモデルです。
いろいろなモデルに取り付けて楽器にすることが出来ます。

皮のテクスチャはhead.jpg、ロゴのテクスチャはlogo.pngです。
extra_texturesフォルダの中に、いくつか代えのテクスチャがあります。
head.jpgに名前を変更し、texturesフォルダの中のものと交換して下さい。

head.UV.pngが皮のUVマップなので、テクスチャ作成の際は参考にして下さい。

上の画像ではお椀に取り付けてティンパニー風にしたり、
ドラム缶に取り付けて変わり種ドラムのようにしています。


----
# ミュート

## フェルトミュート

<img src="/mmd/percussion/mute_felt.000.png" width="33%" align="left">
<img src="/mmd/percussion/mute_felt.001.png" width="33%" align="left">
<img src="/mmd/percussion/mute_felt.002.png" width="33%" align="left">
<br clear="left">

四角いフェルトにガムテープを付けたミュートです。
タムの上に（ロゴの位置あたり）置いて使用します。

ガムテープの部分は数種類のテクスチャを用意しています。
extra_texturesフォルダの中に、茶色、緑、白のテクスチャが入っているので、
名前をtape.jpgに変えて、texturesフォルダの同じ名前のファイルと取り換えると
テクスチャを変えることができます。
MMDの「ヘルプ > テクスチャ読み直し」メニューを活用して下さい。
モーフで粘着面を透明にしたり白色にしたりできます。

また、extra_texturesフォルダのfelt.jpgを
texturesフォルダ内にコピーした状態でモデルを読み込むと、
feltテクスチャが適用された状態でモデルを使用することができます。

ただしテクスチャを適用すると、モデルの変形にあわせて
テクスチャも伸び縮みしてしまいますので気を付けて下さい。
(UVモーフ設定はバージョンアップの時に頑張ってみます)


## ジェルミュート

<img src="/mmd/percussion/mute_gel.000.png" width="33%" align="left">
<img src="/mmd/percussion/mute_gel.001.png" width="33%" align="left">
<img src="/mmd/percussion/mute_gel.002.png" width="33%" align="left">
<br clear="left">

ジェル状のミュートです。
タムの上にそのまま置いたりします。

読み込み時は黒色で、モーフでRGBを調整可能です。
初期状態だとおかしな影が出てしまっていますが、
カメラモードにしたりボーンの選択ボタンをOFFにするときちんと透明になります。

縦横高さをモーフで変更可能で、すべて1.0に設定するとかなり大きくなります。


## シンプルミュート

<img src="/mmd/percussion/mute_simple.000.png" width="33%" align="left">
<img src="/mmd/percussion/mute_simple.001.png" width="33%" align="left">
<img src="/mmd/percussion/mute_simple.002.png" width="33%" align="left">
<br clear="left">

ただのガムテープです。
太鼓の打面に張ったり足元のバミリに使えたりします。
音楽用途以外のことには使用しないでください。

長さと太さを、ある程度モーフで変えることが出来ます。
テクスチャの関係で読み込み時より長くはならないので、
大きいものが必要な場合はPMXEで拡大して下さい。

extra_texturesフォルダの中に、茶色、緑、白のテクスチャが入っているので、
名前をtape.jpgに変えて、texturesフォルダの同じ名前のファイルと取り換えると
テクスチャを変えることができます。
MMDの「ヘルプ > テクスチャ読み直し」メニューを活用して下さい。

モーフで粘着面を透明にしたり白色にしたりできます。
MMD上でテクスチャも変えられたら一番いいのですが・・・。
バージョンアップするときに考えてみます。


## 四角ミュート

<img src="/mmd/percussion/mute_square.000.png" width="33%" align="left">
<img src="/mmd/percussion/mute_square.001.png" width="33%" align="left">
<img src="/mmd/percussion/mute_square.002.png" width="33%" align="left">
<br clear="left">

シンプルミュートと同じく、ただのガムテープです。

パンデイロの裏面にミュートを張るときにこの形で貼る人がいたりします。
ドラムにこの形で貼る人は見たことありません。

機能はシンプルミュートとほぼ同じですが、幅を変えることはできません。
大きさやテクスチャの変更は可能です。

----
おしまい。
