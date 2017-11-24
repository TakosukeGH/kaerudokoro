---
title: Blenderスクリプト
renderer_options:
  gfm: false
tags:
  - blender
  - script
category:
  - blender
id: blender_scripts
twitter_card: summary_large_image
twitter_img: /blender/blender_scripts/twitter.png
twitter_script: true
tweet: true
date: 2016-06-12 00:00:00
edit: 2016-06-12 00:00:00
---

ここには、Blenderスクリプトや、
スクリプトを読み込むアドオンなどについてまとめていきます。

----
# Gist Loader アドオン

最初にスクリプトを共有しやすくする自作アドオンを紹介します。  
Gistに登録したスクリプト等をBlenderに読み込むアドオンです。

URL : <https://github.com/TakosukeGH/gist_loader>

インストール方法については
[GitHubのwiki](https://github.com/TakosukeGH/gist_loader/wiki/ユーザーガイド)
にまとめてありますので、
ここでは自分がやっているインストール方法を書いていきます。
(他の基本的なインストール方法については
[こちら](http://qiita.com/nutti/items/5e9ebe9c28751066b688)
の記事に詳しくまとまっています。とてもありがたいです)

## インストール方法

アドオンをインストールするというよりは、
アドオンを特定のフォルダにまとめて、
Blenderにそのフォルダをアドオン置き場として認識させるといった感じです。

具体的には
[Blenderのマニュアル](https://www.blender.org/manual/preferences/file.html)
にある
[Scriptsパス](https://www.blender.org/manual/preferences/file.html#scripts-path)
を使用していきます。

まずはBlenderを起動し、
「ユーザー設定 > ファイルタブ」でスクリプトのパスを指定します。
ここでは<code>D:\Blender\Library\</code>とします。
パスを入力後は**ユーザー設定を保存**し、Blenderをいったん終了します。

![img](/blender/blender_scripts/000.png)

Blenderで指定したパスに移動し、<code>addons</code>という名前のフォルダを作成します。  
(<code>add-ons</code>だと認識してくれませんでした)

![img](/blender/blender_scripts/001.png)

そしてこの中にダウンロードしたGist Loader アドオンを入れておきます。  
gist_loader-x.x.xフォルダではなく、その中のgist_loaderフォルダをここに設置します。  
( <code>addons\gist_loader\\\__init__.py</code>となるように設置します)

![img](/blender/blender_scripts/002.png)

Blenderを起動して「ユーザー設定 > アドオンタブ」を見ると、
しっかりアドオンが認識されていることが確認できます。

![img](/blender/blender_scripts/005.png)

## 使用方法

テキストエディタを表示し、
「ビュー > プロパティ」メニューをクリックしてプロパティパネルを表示させます。
パネルの最下部にGist Loaderアドオンで追加したパネルがあります。

私が作成したスクリプトをGist Loaderを使ってダウンロードしたい場合は、
ユーザーIDに「TakosukeGH」と入力して「Gist 情報を取得」ボタンをクリックください。
ページの範囲は「開始：1」「終了：1」でOKです。  
(今後スクリプトが増えてきたら「終了：2」になるかもしれません)

![img](/blender/blender_scripts/009.png)

リストの左下にちいさな「+」ボタンがあり、
それをクリックするとフィルタ入力フィールドが表示されます。
ここにキーワードを入力するとファイルを絞り込めるので、
スクリプトを紹介する場合はこちらのキーワードと合わせて紹介します。

ファイルの右側のチェックボックスをONにして
「選択したファイルを読み込む」ボタンをクリックすると、
Gistに保存してあるスクリプトをBlender内部テキストとして保存します。

![img](/blender/blender_scripts/010.png)

以上でGist Loaderのインストール手順・使用手順はおしまいです。

----
# モジュールの追加

スクリプトを使用するときに、
Blenderにないpythonモジュールを使用したいときがあります。
私は設定ファイルにYAML形式を使用したりするので、
YAMLデータを取り扱うためのモジュールを追加しています。

ここではPyYAMLを例にとって、
サードパーティー製モジュールのインストール方法をまとめます。

## モジュールのダウンロード

PyYAMLのサイトは[こちら](http://pyyaml.org/)です。
[ダウンロードページ](http://pyyaml.org/wiki/PyYAML)
からZIP packageをダウンロードし、適当な場所に展開しておきます。

![img](/blender/blender_scripts/006.png)

## インストール方法

モジュールのインストール方法は上記のアドオンのインストール方法と一緒です。  
モジュールの場合は<code>modules</code>というフォルダを作成します。

![img](/blender/blender_scripts/004.png)

ダウンロードしたPyYAMLのZIPを展開すると、
<code>lib3\yaml</code>というフォルダがあるかと思います。  
この<code>yaml</code>フォルダを上記の<code>modules</code>フォルダの中に設置します。  
( <code>modules\yaml\\\__init__.py</code>となるように設置します)

![img](/blender/blender_scripts/003.png)

Blenderを起動し、テキストエディタで以下のコードを実行すると、
問題なく動作するかと思います。


```
import yaml

print("test")
```


![img](/blender/blender_scripts/008.png)

成功しているかどうか分かり辛いですが、
モジュールをインストールしていない状態だと
最初の<code>import yaml</code>の処理でyamlモジュールを読み込めずにエラーとなります。

![img](/blender/blender_scripts/007.png)

以上でモジュールの追加手順はおしまいです。

----
# 自作スクリプト

## VMDエクスポートスクリプト

ユーザーID : <code>TakosukeGH</code>  
キーワード : <code>#vmd_export_script</code>

BlenderからMMD用のモーションデータ(vmd形式)を出力するスクリプトです。  
詳しい使用方法は[こちら](/blender/vmd_exporter/)。

## ルービックキューブ生成スクリプト

ユーザーID : <code>TakosukeGH</code>  
キーワード : <code>#rubix_cube_script</code>

ルービックキューブを生成するスクリプトです。  
詳しい使用方法は[こちら](/blender/rubix_cube/)。

{% alert info %}
こんな感じで、何かスクリプトを作成したらキーワードと概要を追加していきます。
{% endalert %}



