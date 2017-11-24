---
title: VMDエクスポートスクリプト
date: 2016-05-20 00:00:00
edit: 2016-05-22 00:00:00
renderer_options:
  gfm: false
tags:
- blender
- vmd
category:
- blender
id: vmd_exporter
twitter_card: summary_large_image
twitter_img: /blender/vmd_exporter/twitter.png
twitter_script: true
tweet: true
---

# はじめに

Blenderのモーションデータをvmd形式で出力するスクリプトを作成したので、
コードと使用方法をまとめます。

また、MMDを触ったことがある方で「Blenderでのモーション付けってどんな感じ？」という方向けに、
[付録](#付録A・Blenderでのモーション付けについて)にBlenderのアニメーション機能の特徴を少しまとめました。

出力したvmdをMMDで読み込んだ様子は以下のツイートをご確認ください。

<div style="height:1000px;">
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr">BlenderからMMD用のモーションデータを出力。kemoさんのツイートがきっかけになりました。ありがたや。 <a href="https://t.co/6ofmm9jHNR">pic.twitter.com/6ofmm9jHNR</a></p>&mdash; Takosuke (@takosuke_tw) <a href="https://twitter.com/takosuke_tw/status/728547660630614016">2016年5月6日</a></blockquote>
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr">マイムアーティストRemy <a href="https://t.co/mlmLWOwqAi">pic.twitter.com/mlmLWOwqAi</a></p>&mdash; Takosuke (@takosuke_tw) <a href="https://twitter.com/takosuke_tw/status/729980969403305985">2016年5月10日</a></blockquote>
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr">Blender側でIKを設定。手首の接続を切り離して動かす。 <a href="https://t.co/pAfQjlBjnT">pic.twitter.com/pAfQjlBjnT</a></p>&mdash; Takosuke (@takosuke_tw) <a href="https://twitter.com/takosuke_tw/status/729982808312012800">2016年5月10日</a></blockquote>
<br clear="left">
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr">動作確認、ステップドレミー。<br>モデル製作者：桜餅様 <a href="https://t.co/90zTxqpwNy">pic.twitter.com/90zTxqpwNy</a></p>&mdash; Takosuke (@takosuke_tw) <a href="https://twitter.com/takosuke_tw/status/728950112265773057">2016年5月7日</a></blockquote>
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr">バグがあったので修正して完成。ふわふわRemy <a href="https://t.co/E4qJ7iNGci">pic.twitter.com/E4qJ7iNGci</a></p>&mdash; Takosuke (@takosuke_tw) <a href="https://twitter.com/takosuke_tw/status/731513283119702016">2016年5月14日</a></blockquote>
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr">Blenderの画面。センターボーンと腕の上下だけ設定して、すべての親ボーンはパスで移動&amp;回転。<br>(なんだかツイートがうまく繋がらないので再投稿) <a href="https://t.co/WnN6BK16ll">pic.twitter.com/WnN6BK16ll</a></p>&mdash; Takosuke (@takosuke_tw) <a href="https://twitter.com/takosuke_tw/status/731515760636350464">2016年5月14日</a></blockquote>
<br clear="left">
</div>

----
# 注意事項

{% alert warning %}
モーション作成のため、MMD用モデルをBlenderにインポートして使用しています。
MMD以外での使用を禁止しているモデルが多いので、
ご利用の際はモデルのREADMEをよくご確認ください。
{% endalert %}

{% alert warning %}
本ページの内容を実行する際は、すべて自己責任でお願いします。
{% endalert %}

----
# 前提条件

前提条件として、ここでの手順で使用しているソフト・ツールのバージョンを以下にまとめます。
といってもバージョンが多少前後しても問題無いと思います。
blender_mmd_toolsは
[2016/05/20時点のmasterブランチのもの](https://github.com/sugiany/blender_mmd_tools/tree/f5591f331aa1359bfe755451f2f28c45fdfe4732)
を使用しています。

|  ソフト・ツール名 | バージョン  |                                                 ダウンロードURL                                                 |
|-------------------|-------------|-----------------------------------------------------------------------------------------------------------------|
| Windows           | 7           | -                                                                                                                |
| Blender           | 2.77        | <a href="https://www.blender.org/download/" target="_blank">www.blender.org/download/</a>                       |
| blender_mmd_tools | 0.5.0開発版 | <a href="https://github.com/sugiany/blender_mmd_tools" target="_blank">github.com/sugiany/blender_mmd_tools</a> |
| MikuMikuDance     | 9.26        | <a href="http://www.geocities.jp/higuchuu4/" target="_blank">www.geocities.jp/higuchuu4/</a>                    |

----
# 準備 (4ステップ)

## 1. mmd toolsインストール

sugiany氏作成の
<a href="https://github.com/sugiany/blender_mmd_tools" target="_blank">mmd tools</a>
アドオンをインストールし、有効化します。  
アドオンをインストールする方法は
<a href="http://qiita.com/nutti/items/5e9ebe9c28751066b688" target="_blank">こちら</a>
の記事が詳しいです。

<img src="/blender/vmd_exporter/vmd_exporter.004.png" width="500px">

## 2. MMDモデルインポート

「ファイル > インポート > MikuMikuDance Model」メニューを選択し、
MMDモデルを読み込みます。

<img src="/blender/vmd_exporter/vmd_exporter.005.png" width="33%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.007.png" width="33%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.006.png" width="33%" align="left">
<br clear="left">

{% alert danger %}
MMD以外での使用を禁止しているモデルもあるので、
モデルをインポートする際は利用規約を必ず確認してください。
{% endalert %}

## 3. モーション作成

インポートしたモデルを右クリックで選択すると、
右側のプロパティパネルに「アーマチュア」のチェックボックスがあるので、
これをONにしてボーンを表示させます。
(モデルを選択しないとチェックボックスは表示されません)

表示されたボーンを使用して、アニメーションを設定します。  
アニメーションの設定は
<a href="http://krlab.info.kochi-tech.ac.jp/~kurihara/lecture/cg/BlenderWeb_Hayashi/html/animation.html" target="_blank">こちら</a>
や
<a href="http://nvtrlab.jp/column/2-5" target="_blank">こちら</a>
や
<a href="http://yugalab.sakura.ne.jp/archives/2570" target="_blank">こちら</a>
のサイトなどを参考にしてください。

<img src="/blender/vmd_exporter/vmd_exporter.008.png" width="50%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.009.png" width="50%" align="left">
<br clear="left">

{% alert warning %}
Blenderのフレームレートの初期値は24fpsです。
モーション作成前に、MMDに合わせたフレームレートに変更するようにしてください。
(大抵30fpsか60fps)
{% endalert %}

{% alert info %}
プロパティパネルは3Dビューの「ビュー > プロパティ」メニューで表示します。
ショートカットは「N」です。
{% endalert %}

{% alert info %}
「ポーズを付けて1フレームだけ出力する」場合は、
アクションを作成したりキーフレームを打つ必要はありません。
ボーンの位置や角度を変更するだけでOKです。
{% endalert %}

## 4. タイムライン範囲設定

vmdエクスポートスクリプトは、Blenderで設定した開始～終了(最終)フレーム間のデータを出力します。
出力したい範囲をBlenderで指定しておいてください。
フレームレートもここで再度確認しておきます。

<img src="/blender/vmd_exporter/vmd_exporter.010.png" width="50%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.011.png" width="50%" align="left">
<br clear="left">

{% alert info %}
開始フレームと終了(最終)フレームを同じ値に設定すると、1フレームだけデータを出力します。
{% endalert %}

これで準備は完了です。

----
# スクリプト実行手順 (7ステップ)

## 1. コンソール表示
Blenderメニューの「ウィンドウ > システムコンソール切り替え」をクリックし、
コンソールを表示させておきます。
スクリプトを実行したときのエラーメッセージはこちらに表示されるので、
コンソールの内容を確認しつつ作業します。

<img src="/blender/vmd_exporter/vmd_exporter.000.png" width="70%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.012.png" width="30%" align="left">
<br clear="left">

{% alert warning %}
コンソールを右上の「閉じる」ボタンで閉じてしまうと、
Blender全体が終了してしまうので注意してください。
コンソールを表示させた場合は操作ミスに備え、こまめに保存してください。
{% endalert %}

## 2. テキスト作成
スクリーンレイアウトをScriptingに変更し、
テキストエディタを二つ用意します。
そしてそれぞれのエディタの「新規」ボタンをクリックし、
テキストファイルを二つ作成します。  
画面の操作は
<a href="http://www.cgradproject.com/archives/1743" target="_blank">こちら</a>
や
<a href="http://www.blender3d.biz/basis_windowlayout_customizelayout.html" target="_blank">こちら</a>
を参照してください。

<img src="/blender/vmd_exporter/vmd_exporter.013.png" width="33%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.014.png" width="33%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.015.png" width="33%" align="left">
<br clear="left">

## 3. 設定ファイル作成
以下のソースコードを片方のテキストエディタにコピペし、
** テキストのファイル名を"config.ini"に変更します。**  

<script src="https://gist.github.com/TakosukeGH/5c59acccf646c6152a33383947c73083.js"></script>

<img src="/blender/vmd_exporter/vmd_exporter.016.png" width="500px">

## 4. スクリプトファイル作成

以下のソースコードを、もう一つのテキストエディタにコピペします。  
こちらはファイル名は変更しなくてOKです。  

<script src="https://gist.github.com/TakosukeGH/02e513a716f3d02f9ab55f727317389c.js"></script>

<img src="/blender/vmd_exporter/vmd_exporter.017.png" width="700px">

## 5. コンフィグ設定

config.iniの一番上の[config]セクションに、
出力先フォルダパスと出力ファイル名を記述します。  
その下の[bone]セクションには、
データを出力したいボーン名を羅列します。  
[bone_isolated]セクションは後程説明しますので、
ここでは気にしないでください。

![img](/blender/vmd_exporter/vmd_exporter.018.png)

{% alert success %}
最初は[bone]セクションに「センター」ボーンだけ記述して、
動作を確認してみることをおすすめします。
また、先頭に#を付けてコメントアウトできます。
{% endalert %}

{% alert warning %}
Cドライブ直下などを出力先に指定すると、
権限が無いためエラーとなる場合があります。
自分のユーザーフォルダ（私の場合C:\Users\takosuke）などを指定することをおすすめします。
{% endalert %}

{% alert warning %}
[bone]セクション内に同じボーン名が二つあると、
iniファイル読み込み時にエラーとなります。
{% endalert %}

## 6. スクリプト実行

まず3Dビューでボーンを選択した状態にします。
そして処理スクリプトを記述したテキストエディタ(ここでは下側のエディタ)で右クリックし、
「スクリプト実行」を選択します。
処理が成功すると指定の場所にvmdファイルが作成されます。

<img src="/blender/vmd_exporter/vmd_exporter.019.png" width="70%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.020.png" width="30%" align="left">
<br clear="left">

## 7. 確認

MMDを起動し、モデルとvmdファイルを読み込み動作確認します。

<img src="/blender/vmd_exporter/vmd_exporter.021.png" width="700px">

スクリプト実行手順は以上です。

----
# 問題と対策

本スクリプトで起こりやすい問題とその対策を以下に記述します。

{% alert danger %}
コンソールに"duplicate name exists"と表示される。
{% endalert %}

{% alert success %}
[bone]セクション内で同じボーン名を記述するとエラーとなる他、
[bone]セクションと[bone_isolated]セクションのキー値で重複があった場合もエラーとなります。
ボーン名を検索するなどして、ボーン名に重複が無いか確認してください。
{% endalert %}

{% alert danger %}
"***.vmd.log"というファイルが生成される。
{% endalert %}

{% alert success %}
これはエラーの内容や出力したデータの内容を記述したログファイルです。
中身は普通のテキストファイルなので、メモ帳などのテキストエディタで中身を確認できます。
{% endalert %}

{% alert danger %}
足の角度がBlenderとMMDで違う。
{% endalert %}

{% alert success %}
ikの挙動がBlenderとMMDで異なります。きっちりと合わせたい場合はMMD側のikをoffにしてください。
{% endalert %}

{% alert danger %}
コンソールが文字化けして、エラーの内容が分からない。
{% endalert %}

{% alert success %}
<p>
MMDのボーン名が日本語のため、Windows版Blenderコンソールの内容が文字化けする場合があります。
ログファイルの方を確認すると日本語エラーの内容を確認できますが、
ファイル生成前に発生したエラーはコンソールでしか見ることが出来ません。
</p>
<p>
解決のためにはBlenderコンソールの設定を変える必要があります。
まずコンソール左上のBlenderマークをクリックしてプロパティを選択します。
そしてフォントタブで「MSゴシック」フォントを選択してOKボタンをクリックします。
最後にBlenderで新しくテキストファイルを作成し、以下の三文をコピペ&実行してみてください。  
(ちょっと表示がおかしいですが、これのなおし方が分かりません…)  
</p>
元に戻す場合は2行目の<code>65001</code>を<code>932</code>に変更して実行してください。
{% endalert %}

```
import os
os.system("chcp 65001")
print("日本語表示確認")
```

<img src="/blender/vmd_exporter/vmd_exporter.022.png" width="30%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.024.png" width="40%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.023.png" width="30%" align="left">
<br clear="left">

----
# ボーン付け替え機能

BlenderとMMDで、異なる親子関係を持ったデータを出力することができます。  
俗にいう「腕切IK」や「手首キャンセル」状態でモーションを作成しつつ、
通常のボーン構造でデータを出力したい場合に使用します。

具体的には以下のようなモーションを作成する場合に使用します。

<video src="/blender/vmd_exporter/vmd_exporter.001.mp4" controls width="500px"></video>

この機能の使い方はちょっと特殊です。  
具体例として、ここでは手首ボーンを切り離し、腕ikを設定する方法を説明していきます。

## 1. ボーン設定変更

MMDボーンの腕周りには、腕捩れボーンや肩Pボーンなどさまざまなボーンが存在する場合があります。
まずはこの構造を変更し、足ボーンのようにシンプルな構造にします。

レミリアモデルの場合、親子関係が「腕 -> 腕捩 -> ひじ」となっているので、
これを「腕 -> ひじ」となるように
「ひじ」ボーンの親を「腕捩」から「腕」に変更します。

<img src="/blender/vmd_exporter/vmd_exporter.026.png" width="50%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.025.png" width="50%" align="left">
<br clear="left">

## 2. ik設定・手首の切り離し

「ひじ」ボーンにikコンストレイントを設定し、ターゲットを「手首」に設定します。  
チェーンの値は2にします。(ikの影響範囲が「ひじ」とその親の「腕」の二つになる)

そして「手首」ボーンの親を「手捩」から「全ての親」または親無しに設定します。

<img src="/blender/vmd_exporter/vmd_exporter.027.png" width="33%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.028.png" width="33%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.029.png" width="33%" align="left">
<br clear="left">

## 3. 手首のロック解除

mmd toolsでモデルをインポートした場合、
手首ボーンのトランスフォームにロックがかかっていて移動できないので、これを解除します。
解除後に手首ボーンを動かすと、腕ボーン・ひじボーンが手首ボーンの位置に合わせて回転するのが確認できます。

<img src="/blender/vmd_exporter/vmd_exporter.030.png" width="33%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.031.png" width="33%" align="left">
<img src="/blender/vmd_exporter/vmd_exporter.000.gif" width="33%" align="left">
<br clear="left">

## 4. コンフィグ設定

config.iniの[bone]セクションに腕.L、ひじ.Lボーンを記述し、  
[bone_isolated]セクションには「手首.L : 手捩.L」と記述します。  
(左側にデータ出力するボーン名、右側に接続したいボーン名を記述します)

<img src="/blender/vmd_exporter/vmd_exporter.032.png">

## 5. 確認

あとはスクリプトを実行し、MMDで動作確認します。
Blender側ではボーンを切り離してモーションを作成しましたが、
ボーンが接続されたMMD側でも同じ動きになります。

<img src="/blender/vmd_exporter/vmd_exporter.033.png" width="300px">

メモ: 指定したボーンの角度を、iniファイルで指定した親ボーンからの
相対角度として計算してデータ出力しています。
Blenderで改造したボーンのモーションを、無改造のMMDボーンに適用することが出来ます。

----
# その他・ツールの特徴

ここまでで挙げられていない本ツールの特徴を以下にまとめます。

> 1. コンストレイント適用後のボーンデータを出力
> 1. 出力後のモーションデータは、全てのフレームにキーが打たれた状態
> 1. モーション以外のデータ(モーフなど)は出力しない
> 1. ボーン名の変換は「.L/.R」→「左/右」

本ツールはコンストレイント適用後のデータを出力しているので、
ボーンをカーブに沿って移動させたモーションなども出力可能です。

2はモーションエクスポートツールによくある仕様ですが、
補間曲線やNLAなどの関係で「Blenderで打ったキーフレームだけ出力する」というのが難しいです。

表情モーフは、MMDでは一つのpmd(pmx)データに対して設定するのですが、
Blenderではメッシュに設定するシェイプキーがこれに当たります。
シェイプキーの値を取得するのは簡単ですが、
同じ名前のシェイプキーが複数あった場合に
MMD用データに変換することがちょっと難しい状態です。  
(mmdインポートツールに「素材ごとにメッシュを分割する」機能があり、
これを使用すると顔だけでなく、手や足のメッシュにも同じ名前の表情シェイプキーが残る)

また、vmdデータ構造に含まれる以下のデータも出力しません。

* カメラ
* 照明
* セルフ影
* モデル表示・IK on/off

ボーン名は、例えばBlenderで「手首.L」だった場合、
vmdでは「左手首」に変換して出力します。
この名前変換規則を変更したい場合は、
change_bone_name_to_mmdメソッドを修正して下さい。

----
# おわりに

本ツールは、開発のしやすさからアドオン形式ではなくスクリプトとして作成しました。
ボーンの取捨選択もGUIではなくiniファイルを使用しており、少々扱いづらいかと思います。

もともとはMMD動画作成が目的で作ったツールです。
今後はのんびりとモーショントレースしたり動画作成したり切り紙絵作ったりする予定です。
なので、今後このツールのアドオン化やツールの更新などはほとんどしないと思います。

本ページ内のコードは自由にお使いください。  
この記事がアドオン製作者の目にとまり、
よりよいツールが開発されることを願います。

何かありましたら[@takosuke_tw](https://twitter.com/takosuke_tw)まで連絡ください。
<a href="https://twitter.com/takosuke_tw" class="twitter-follow-button" data-show-count="false" data-show-screen-name="false">Follow @takosuke_tw</a>

----
# 謝辞

使用したレミリアモデルはフリック様配布のものを使用しています。  
いつもお世話になっています。

<iframe width="312" height="176" src="http://ext.seiga.nicovideo.jp/thumb/im2651527" scrolling="no" style="border:solid 1px #888;" frameborder="0"><a href="http://seiga.nicovideo.jp/seiga/im2651527">運命を貫く神槍</a></iframe>

<br />
<br />
そして今回、数人の方に動作確認をお願いしていました。  
このページもまだ存在しない時に、
適当なREADMEと適当な説明で動作確認をお願いしたにも関わらず、
動画まで作っていただき感謝の極みです。

<div style="height:1100px;">
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr"><a href="https://t.co/DfjAibDJdl">https://t.co/DfjAibDJdl</a><br><br>Blender→MMDにモーション持ってくるテストで作ったもの <a href="https://t.co/gPmOZ4BiVt">pic.twitter.com/gPmOZ4BiVt</a></p>&mdash; 桜餅 (@Sakura0323moti) <a href="https://twitter.com/Sakura0323moti/status/731180951053570048">2016年5月13日</a></blockquote>
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr">Blender＞VMDエクスポートテストのやつ（グルーブ感のないトップロック）<br>基本的な準標準エクスポート（腕捻れ、手捻じれ、上半身2、グルーブ、手持ちダミー、肩キャンセル） <a href="https://t.co/E2bEfuErEt">pic.twitter.com/E2bEfuErEt</a></p>&mdash; Gared (@ayakasikone) <a href="https://twitter.com/ayakasikone/status/731340688642605056">2016年5月14日</a></blockquote>
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr">Blender＞VMDエクスポートテストのやつ3<br>isolate機能てすと。<br>手のIKはもう必須だからな、MMDに持ってくときにつなぎかえる機能があるのは便利！ <a href="https://t.co/4psut0hde7">pic.twitter.com/4psut0hde7</a></p>&mdash; Gared (@ayakasikone) <a href="https://twitter.com/ayakasikone/status/731511294960926720">2016年5月14日</a></blockquote>
<br clear="left">
<blockquote class="twitter-tweet" data-lang="ja" width="32%" align="left"><p lang="ja" dir="ltr">Blender＞VMDエクスポートテストのやつ4<br>ちゃんと出力できてるから、もうテストじゃないな。<br>6歩2サイクル。<br>トレースすればもっときれいかもしれないけど、体動かしながらだとこんなもん。<br>5歩目、左足の引きが変 (´・ω・｀) <a href="https://t.co/jnHjdGS4AW">pic.twitter.com/jnHjdGS4AW</a></p>&mdash; Gared (@ayakasikone) <a href="https://twitter.com/ayakasikone/status/731750071591567362">2016年5月15日</a></blockquote>
<br clear="left">
</div>

----
# 付録

## 付録A・Blenderでのモーション付けについて

ここでは「Blenderってどんなことができるの？」と思ったMMDer向けに、
Blenderのアニメーション機能の特徴（MMDとの違い）をいくつか挙げてみます。

> 1. 位置xyz・回転xyzwについて、別々にキーフレームを打てる
> 1. <a href="https://wiki.blender.org/index.php/Doc:JA/2.6/Manual/Animation/Editors/NLA" target="_blank">NLAエディタ</a>
> を使用してモーションのレイヤー管理・リピート・ブレンドが可能
> 1. <a href="http://ch.nicovideo.jp/Arasen/blomaga/ar636768" target="_blank">キーフレーム補間</a>
> に関する機能が多い
> 1. <a href="https://wiki.blender.org/index.php/Doc:JA/2.6/Manual/Constraints" target="_blank">コンストレイント</a>
> でボーンにさまざまな制限/制御を追加できる  
> (ikは数あるコンストレイントの一つ)
> 1. ポーズモード(≒MMDでの操作)とエディットモード(≒PMXEでの編集)を瞬時に切り替え可能
> 1. スクリプトによるボーン制御
> 1. アニメーション作成を補助する
> <a href="http://ch.nicovideo.jp/tomb_saikaya/blomaga/ar943618" target="_blank">アドオン</a>
> あり

1と2で多段ボーンの主要機能をカバーできるかと思います。
4と5で「腕切」や「手首キャンセル」などの改造を施しつつ、そのままモーション作成に移れます。
(おまけ的な機能ではありますが、
本ツールはBlenderで改造したボーンのモーションを無改造のMMDボーンに適用することが出来ます)  
6を使用すれば複雑で幾何学的なモーションの作成も可能です。

<br/>
また、MMDと比べたときのデメリットとしては、以下が挙げられるかと思います。

> 1. ショートカットキーが多く、誤爆しやすい
> 1. MMDのような軽快な表示が難しい

1については、MMDからBlenderに移る方や、多機能ツールを複数使う人にとって頭が痛い問題かと思います。
私はショートカットキーは覚えないようにし、
GUIを使用したりメニューからたどるようにしています。
そのほうがショートカットより思い出しやすいので、
Blenderを触らない期間があってもあまり問題がなくなりました。

また、MMDと同じメッシュ・剛体・ボーンをBlenderで表示するとちょっと重いです。
これを解消しようとするとかなりの手間がかかるので、
私は簡単な素体でモーション付けするようにしています。

----
## 付録B・設定ファイルテンプレート

余計なものがないconfig.iniを置いておきます。

<script src="https://gist.github.com/TakosukeGH/e0eb2feacdc2eaa06456ec38cce5c179.js"></script>

----
## 付録C・ボーンレイヤー操作スクリプト

自分が使っている、ボーンを特定のレイヤーに移動させるスクリプトを置いておきます。

アーマチュアを選択した状態でスクリプトを実行すると、
正規表現にマッチするボーンを指定したボーンレイヤーに移動させます。
条件は<code>regex\_set</code>に記述していきます。
<code>r"^sk\_":7,</code>と記述した場合、
「先頭が<code>sk\_</code>で始まるボーンを<code>7</code>番目のボーンレイヤーに移動する」
という意味になります。
<code>r".*リボン":7,</code>と記述した場合は
「名前の中に<code>リボン</code>を含むボーンを<code>7</code>番目のボーンレイヤーに移動する」
という意味になります。

<script src="https://gist.github.com/TakosukeGH/bafb37dc45c0089252ba6d1b2bdaf2c4.js"></script>

{% alert info %}
正規表現について詳しい情報を知りたい方は、
<a href="http://docs.python.jp/3/library/re.html" target="_blank">Pythonの正規表現操作</a>
を確認してください。
regex_setのデータ構造を詳しく知りたい方は、
<a href="http://docs.python.jp/3.5/tutorial/datastructures.html#dictionaries" target="_blank">Pythonのデータ構造・辞書型</a>
を確認してください。
{% endalert %}

----
## 編集履歴

* 2016/05/22 : スクリプトコード修正
  - ルートボーンの初期値が&lt;0, 0, 0&gt;でない場合、その位置を移動値として出力してしまっていたので修正
* 2016/05/20 : 記事公開

<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
