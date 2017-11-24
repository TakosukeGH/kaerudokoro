---
title: MMDモデルへのRigify適用
renderer_options:
  gfm: false
tags:
  - blender
  - rigify
category:
  - blender
id: mmd_rigify
date: 2015-12-20 00:00:00
edit: 2015-12-20 00:00:00
---

# はじめに

ありがたいことにRigifyの記事が[増えており](http://ch.nicovideo.jp/tomb_saikaya/blomaga/ar919388)、Rigifyでもっと遊んでみたい。
MMD向けのモデルが大好きなので、Rigifyを適用してBlenderで使いやすくしてみる。

# 注意事項

レミリアモデルでしかテストしていないので、
他のモデルに使用するにはコード修正が必要と思われます。

MMD以外での使用を禁止しているモデルが多いので、
ご利用の際はモデルのREADMEをよくご確認ください。

# MMDモデルのインポート

ここではフリック氏配布のレミリアモデルを使用する。

URL : <http://seiga.nicovideo.jp/seiga/im2651527>

MMDモデルのインポーターは、sugiany氏作成のものを使用する。

URL : <https://github.com/sugiany/blender_mmd_tools>
※2015/12/20時点での最新のものを使用。

インポート直後の画像はこちら。

![img](/blender/mmd_rigify/rigify_002.png)

インポート後の設定は好みだが、ここでは以下の5つの設定を行う。

1. マテリアル設定
ツールシェルフ > mmd_toolsタブ > 「Convert Materials For Cycles」ボタンをクリックし、Cycles用マテリアルの設定をする。

2. 剛体設定
ツールシェルフ > mmd_toolsタブ > Rigidbody「Build」ボタンをクリックし、剛体設定を完了させる。

3. 白目修正
目に影が出来てしまうので、"目玉"のマテリアルを変更する。
MMDBasicShaderを切断し、放射シェーダーを接続する。

4. オブジェクト分離
以下のマテリアルを選択し、オブジェクトを分離する。
(ショートカット : P)
  * 青ざめ
  * 魅了の眼
  * 口2
  * 頬紅
  * 照れ
  * 瞳2

5. オブジェクト配置
まずはAlt+Hですべてのオブジェクトを表示させ、
各オブジェクトのレイヤーを変更する。
必須の設定ではないが、ボーン、メッシュ、それ以外でレイヤーを分けると楽。

<img src="/blender/mmd_rigify/rigify_004.png" width="24%" align="left">
<img src="/blender/mmd_rigify/rigify_005.png" width="24%" align="left">
<img src="/blender/mmd_rigify/rigify_007.png" width="24%" align="left">
<img src="/blender/mmd_rigify/rigify_006.png" width="24%" align="left">
<br clear="left">

ここまではインポーターの機能やモデル特有の設定なので特に自動化はしない。

# Rigify適用

ここからはスクリプトを使用してRigifyを適用していく。
エラーに備え、Blenderのシステムコンソールを表示させておく。
(Window > Toggle System Console)

# 基本データ

まずはMMDとRigifyのボーン名の対応情報を持つスクリプトファイルをBlenderに作成する。
スクリーンレイアウトをScriptingに変更し、テキストを新規作成する。
ファイル名は"commons.py"とする。
※この名前を使用してモジュールインポートしているので、ファイル名は固定で。

![img](/blender/mmd_rigify/rigify_008.png)

以下のコードをテキストエディタに貼り付ける。

<script src="https://gist.github.com/TakosukeGH/3d4099103296b2661af2.js"></script>

<br>
このスクリプトは他のスクリプトから呼ばれるものなので、
ここではまだスクリプト実行を行わない。
貼り付け後の画像は以下。

![img](/blender/mmd_rigify/rigify_011.png)

# metarig生成

テキストを新規作成し、以下のコードを張り付ける。
※ファイル名は何でもよい。

<script src="https://gist.github.com/TakosukeGH/633962a83e5c179eb8aa.js"></script>

<br>
MMDアーマチュアを選択した状態で、スクリプトを実行する。

![img](/blender/mmd_rigify/rigify_009.png)

スクリプト実行後、metarigが生成されていることを確認する。

![img](/blender/mmd_rigify/rigify_010.png)

# Rigify生成

メタリグのアーマチュアタブの一番下、「生成」(Generate)ボタンをクリックし、
Rigifyアーマチュアを生成する。

![img](/blender/mmd_rigify/rigify_012.png)

必須ではないが、Rigifyアーマチュア生成後はmetarigをどこか適当なレイヤーに移動させておく。
ここではボーンシェイプオブジェクトが生成される19番レイヤーに移動しておく。

![img](/blender/mmd_rigify/rigify_013.png)

# Rigifyボーン設定

以下のコードをテキストエディタに張り付ける。
※metarig生成時のコードを上書きでOK

<script src="https://gist.github.com/TakosukeGH/ecc9a784f5b1c5c83892.js"></script>

<br>
MMDアーマチュアと、生成されたRigifyアーマチュアを選択してスクリプトを実行する。
※アクティブオブジェクトはスクリプト内で設定しているので、選択する順番は適当でOK

![img](/blender/mmd_rigify/rigify_014.png)

スクリプトを実行すると、MMDアーマチュアとRigifyアーマチュアが統合される。

![img](/blender/mmd_rigify/rigify_015.png)

ここでは大まかに以下の処理を行っている。

* MMDボーンを24,25番レイヤーに移動
* MMDアーマチュアとRigifyアーマチュアを統合
* MMDボーンの親をRigifyボーンに変更
* MMDボーンに付与されているコンストレイントのターゲット先をRigifyボーンに変更

# 頂点グループ名変更

以下のコードをテキストエディタに張り付ける。
※Rigifyボーン設定時のコードを上書きでOK

<script src="https://gist.github.com/TakosukeGH/1705e92c922d17450c84.js"></script>

<br>
モデルのメッシュを選択した状態でスクリプトを実行する。

![img](/blender/mmd_rigify/rigify_016.png)

スクリプト実行後、モデルの頂点グループ名がRigifyボーンの名前に置き換わる。

![img](/blender/mmd_rigify/rigify_017.png)

# 剛体設定

以下のコードをテキストエディタに張り付ける。
※頂点グループ名変更時のコードを上書きでOK

<script src="https://gist.github.com/TakosukeGH/3aa6808a3c625727e5c4.js"></script>

<br>
剛体を選択した状態でスクリプトを実行する。
※結構時間がかかるので、コンソールを眺めながらのんびり待つ。

![img](/blender/mmd_rigify/rigify_018.png)

# 確認

再生ボタンをクリックしてRigifyボーンを動かすと、
メッシュと剛体がボーンに追従して動くことが確認できる。

![img](/blender/mmd_rigify/rigify_001.gif)

# おまけ

Deformボーンの表示・非表示ボタンの追加は以下の記事を参考に。  
URL : <http://ch.nicovideo.jp/takosuke/blomaga/ar710878>

# おわりに

私自身は、今のところレミリアモデルしか使用する予定がないので、
他のモデルへの対応は未定。
また、Rigify Typeに応じた処理なども実装していないですが、
標準Rigifyボーンで満足してしまっているのでこちらも優先度が低いです。

各スクリプトファイルのpitchipoyプロパティをすべてFalseにすると、
pitchipoyでない方のRigifyアーマチュアの作成が可能です。

このページのソースコードはすべてライセンスCC0とします。
MMD x Rigifyのアドオンが増えることを願います。
(Rigify適用モデルを配布なんかしていただけたら・・・凄くありがたい！)

何かありましたら、[@takosuke_tw](https://twitter.com/takosuke_tw)まで連絡下さい。


