---
title: ルービックキューブ生成スクリプト
renderer_options:
  gfm: false
tags:
  - blender
category:
  - blender
id: rubix_cube
twitter_card: summary_large_image
twitter_img: /blender/rubix_cube/twitter.001.png
twitter_script: true
tweet: true
date: 2016-07-10 00:00:00
edit: 2016-07-10 00:00:00
---

# はじめに

ルービックキューブを生成するスクリプトを作成したので、
コードと使用方法をまとめます。

スクリプトを実行すると、キューブオブジェクトやボーン生成から、
マテリアル設定、ベベルモディファイア(角の丸め)適用、アニメーション設定まで一括で行います。

動作の様子は以下の動画を確認してください。

<video src="/blender/rubix_cube/rubix_cube.003.mp4" controls width="800px"></video>

(BlenderはVer.2.77を使用しています)

----
# 注意事項

{% alert warning %}
本ページの内容を実行する際は、すべて自己責任でお願いします。
{% endalert %}

----
# スクリプト実行手順

まずはBlenderを起動し、メニューの「オブジェクト > 削除」をクリックし、
中央のキューブを削除します。

<img src="/blender/rubix_cube/rubix_cube.000.png" width="33%" align="left">
<br clear="left">

スクリーンレイアウトをScriptingに変更し、
<a href="/blender/blender_scripts" target="_blank">この記事</a>にあるGistローダーで
ユーザーID「TakosukeGH」、キーワード「#rubix_cube_script」と入力してスクリプトを取得するか、
[こちら](https://gist.github.com/TakosukeGH/03a08e231ced1e387adb270105016936)から直接スクリプトをコピーしてBlenderにペーストします。  
(ファイル名は何でもOKです)

<img src="/blender/rubix_cube/rubix_cube.001.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.002.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.003.png" width="33%" align="left">
<br clear="left">

テキストエディタを右クリックし、「スクリプト実行」を選択します。
処理が成功するとルービックキューブが生成されます。

スクリーンレイアウトをDefaultに戻し、
再生ボタンをクリックするとスクリプトで生成したアニメーションを確認できます。

<img src="/blender/rubix_cube/rubix_cube.004.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.005.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.000.gif" width="33%" align="left">
<br clear="left">

以上でスクリプトの実行手順はおしまいです。

----
# 回転の設定

今回はスクリプトファイルに日本語コメントを多く記述したので、
設定についてはそちらをご確認ください。
ここではコメントで説明しきれていない「回転データ」について補足します。

ルービックキューブは、回転パターンを解説するために回転記号というものが使われています。
[Wikipediaのルービックキューブの項目](https://ja.wikipedia.org/wiki/ルービックキューブ#.E3.82.B9.E3.83.94.E3.83.BC.E3.83.89.E3.82.AD.E3.83.A5.E3.83.BC.E3.83.93.E3.83.B3.E3.82.B0)に
詳細が記述されていますが、
回転させる層に合わせてF・B・R・L・U・D・S・M・Eの記号が用いられます。

単に「F」と書いた場合は時計回りへ90°回転させることを意味し、
反時計回りに90°回転させる場合は「F'」と記述します。
本スクリプトでも同じ記号を使用しますが、
回転については「n」と「r」という記号を使用します。
「n」はnormalの頭文字で、時計回りの回転と対応します。
「r」はreverseの頭文字で、反時計回りの回転と対応します。

文字だけだと分かり辛いので、
設定値とアニメーションの具体例をいくつか挙げます。

## 時計回りの回転

まずは時計回りの全ての回転を確認します。

```
move_data = [
    (mn.F, rd.n),
    (mn.B, rd.n),
    (mn.R, rd.n),
    (mn.L, rd.n),
    (mn.U, rd.n),
    (mn.D, rd.n),
    (mn.S, rd.n),
    (mn.M, rd.n),
    (mn.E, rd.n),
]
```

スクリプトの43行目を、上記のように記述します。
回転記号を記述する際は<code>mn.F</code>のように、
先頭に<code>mn.</code>を付けて記述します。
同様に回転方向は<code>rd.</code>を付けます。

上記の設定の場合、キューブの回転は以下の様になります。

<img src="/blender/rubix_cube/rubix_cube.001.gif">

## 反時計回りの回転

回転方向をnからrにすると、反時計回りの回転になります。

```
move_data = [
    (mn.F, rd.r),
    (mn.B, rd.r),
    (mn.R, rd.r),
    (mn.L, rd.r),
    (mn.U, rd.r),
    (mn.D, rd.r),
    (mn.S, rd.r),
    (mn.M, rd.r),
    (mn.E, rd.r),
]
```

<img src="/blender/rubix_cube/rubix_cube.002.gif">

## ランダム回転

適当に動かしたい場合は、以下のコードを使用することができます。
<code>range(30)</code>が回転回数を表し、指定した回数だけランダムに回転します。

```
self.move_data = []
for i in range(30):
    self.move_data.append((random.randint(1,9), random.randint(0,1)))
```

上記のコードは、スクリプトの80行目にコメントアウトした状態で記述しています。
コードのインデントに気を付けながら、先頭の<code>#</code>を削除して実行してください。

実行結果は以下です。  
(ランダムなので毎回動作が変わります)

<img src="/blender/rubix_cube/rubix_cube.003.gif">

{% alert warning %}
回転のパターンによっては捩じれたような回転をしてしまう場合があります。
その場合は手動で修正してください。
{% endalert %}

----
# MMDでの使用

本スクリプトで生成したルービックキューブを、
pmxエクスポーターとvmdエクスポーターを使用してMMDで使用することが出来ます。

実際にMMDで使用した動画はこちらです。

<video src="/blender/rubix_cube/rubix_cube.001.mp4" controls width="500px"></video>

ここでは、ルービックキューブの生成からMMDで使用するまでの手順をまとめます。

## ルービックキューブの生成

上記のスクリプト実行手順と同じ手順でキューブを生成しますが、
ここではスクリプト38行目を<code>scale = 0.1</code>に設定し、
小さめのキューブを生成します。
また、回転はランダム回転を使用します。


<img src="/blender/rubix_cube/rubix_cube.020.png" width="700px">

## pmxエクスポーターのインストール

ここではpmxエクスポーターとして、かがやすさんのBlender2Pmxeを使用します。
[こちらのサイト](http://kagayas.com/blender_blender2pmxe_addon/)から
Ver.1.0.6をダウンロードし、Blenderのアドオンフォルダに配置、
そしてBlenderのユーザー設定でアドオンを有効化します。

アドオンをインストールする方法は
<a href="http://qiita.com/nutti/items/5e9ebe9c28751066b688" target="_blank">こちら</a>
の記事が詳しいです。

<img src="/blender/rubix_cube/rubix_cube.006.png" width="33%" align="left">
<br clear="left">

## pmxファイルのエクスポート

アドオンを有効化したら、ルービックキューブのアーマチュアを選択し、
メニューの「エクスポート > PMXファイル for MMD（機能拡張版）(.pmx)」を選択します。
出力ファイルを指定する画面で、
**左下にある「モディファイアーを適用」にチェックを入れて**エクスポートします。

<img src="/blender/rubix_cube/rubix_cube.008.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.011.png" width="33%" align="left">
<br clear="left">

## pmxの編集

生成されたpmxをPmxEditorで開き、
Indexのコピー&ペーストを利用して表示枠を修正します。
表示枠の設定は自由ですが、
ここではBone以外の全てのボーン（Bone.001～Bone.026）を「その他」表示枠に設定します。

<img src="/blender/rubix_cube/rubix_cube.009.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.010.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.012.png" width="33%" align="left">
<br clear="left">

これでルービックキューブモデルがMMDで使用出来るようになります。
つづいてモーションデータ(vmd)をエクスポートします。

## vmdエクスポーターのインストール

Gistローダーを使用する場合は、
ユーザーID「TakosukeGH」、キーワード「#vmd_export_script」と入力して、
「vmd_export_script.py」「config.ini」を読み込みます。
もしくは[こちらの記事](/blender/vmd_exporter)からスクリプトと設定ファイルをコピーし、
Blenderのテキストエディタにペーストしてください。

config.iniは以下のように出力パスとボーン名記述します。
出力パスは環境に合わせて適宜変更してください。

```
[config]
folder : D:\MMD\tmp
file : sample.vmd

[bone]

Bone.001
Bone.002
Bone.003
Bone.004
Bone.005
Bone.006
Bone.007
Bone.008
Bone.009
Bone.010
Bone.011
Bone.012
Bone.013
Bone.014
Bone.015
Bone.016
Bone.017
Bone.018
Bone.019
Bone.020
Bone.021
Bone.022
Bone.023
Bone.024
Bone.025
Bone.026

[bone_isolated]

```

<img src="/blender/rubix_cube/rubix_cube.007.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.013.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.014.png" width="33%" align="left">
<br clear="left">

## vmdエクスポート

先にBlenderの開始フレーム、終了フレームを調整し、
キューブのアニメーションがすべて再生範囲内に収まるようにします。

キューブのアーマチュアを選択し、vmd_export_script.pyを実行します。
エクスポートが成功すると、指定したパスにvmdファイルが生成されます。

<img src="/blender/rubix_cube/rubix_cube.015.png" width="33%" align="left">
<br clear="left">

## 動作確認

エクスポートしたpmxファイルとvmdファイルをMMDに順に読みこみ、
再生ボタンをクリックするとBlenderと同じような動きをすることが確認できます。

<video src="/blender/rubix_cube/rubix_cube.002.mp4" controls width="500px"></video>

以上でMMDで使用するための手順はおしまいです。

----
# おわりに

今回のスクリプトはやっていることは一つ一つシンプルです。
面倒な処理を自動でやるときの一つの例として見ていただければ幸いです。

「その機能、Blenderでも出来る」と言うだけでなく、
「Blenderの方がやり易い・早い・かっこいい」と言えるようなものがどんどん増えると嬉しいですね。

何かありましたら[@takosuke_tw](https://twitter.com/takosuke_tw)まで連絡ください。
<a href="https://twitter.com/takosuke_tw" class="twitter-follow-button" data-show-count="false" data-show-screen-name="false">Follow @takosuke_tw</a>

----
# 付録・アニメーションの逆再生

本スクリプトで作成したキューブは、
初期状態は（大抵は）面がすべてそろった状態になります。
アニメーションを再生すると、
そろった状態からシャッフルされた状態に移行します。

しかし動画で使う場合は、シャッフルされた状態から元に戻すのが普通です。
そこで、ここでは生成したアニメーションを逆再生させる手順をまとめます。  
(NLAエディタを使用するので、詳しくは[こちら](https://wiki.blender.org/index.php/Doc:JA/2.6/Manual/Animation/Editors/NLA)を
参照してください)

## アクションストリップの作成

スクリーンレイアウトをAnimationに変更し、
どこか適当なエリアをNLAエディターに変更します。
NLAエディタ上に「RubixCubeAction」が表示されているので、
その隣のボタンをクリックします。

<img src="/blender/rubix_cube/rubix_cube.016.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.017.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.021.png" width="33%" align="left">
<br clear="left">

すると一段下に黄色い帯が生成されます。これがアクションストリップです。
Blenderではストリップを使用し、モーションの再利用やリピート、逆再生をすることが出来ます。

<img src="/blender/rubix_cube/rubix_cube.024.png" width="33%" align="left">
<br clear="left">

## 反転再生

NLAエディタのメニューの「ビュー > プロパティ」をクリックすると、
右側にプロパティが表示されるので、
そのなかの「反転再生」にチェックを入れます。

この状態で再生すると、シャッフルされた状態から元に戻すアニメーションになります。

<img src="/blender/rubix_cube/rubix_cube.022.png" width="33%" align="left">
<img src="/blender/rubix_cube/rubix_cube.023.png" width="33%" align="left">
<br clear="left">

## その他の方法

上記以外にも、キューブ生成時に「シャッフル状態→元に戻す」アニメーションを生成することができます。
スクリプトの40行目にあるプロパティを、以下の様に設定すると、
通常とは逆の動きをするようになります。

```
frame_start = 500
frame_rotate = -5
frame_interval = -5
```

本スクリプトは、<code>frame_start</code>からキーフレームを追加し始め、
<code>frame_rotate</code>分フレームを進めた後にキューブを回転させ、
またキーフレームを追加します。
さらに<code>frame_interval</code>分フレームを進め、
そこでキーフレームを追加して1サイクルとなります。

そのため<code>frame_rotate</code>と<code>frame_interval</code>に
負の値を設定すると、
フレームを進めるのではなく**戻す**ようになり、
通常とは逆のアニメーションを生成することができます。  
(ですので負の値を設定する場合は、<code>frame_start</code>に大き目の値を設定してください)

----
## 編集履歴

* 2016/07/10 : 記事公開

<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


