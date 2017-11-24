---
title: 追加入力ソケット付きMappingノード
renderer_options:
  gfm: false
tags:
  - blender
  - node
category:
  - blender
id: mapping_node
date: 2016-06-09 00:00:00
edit: 2016-06-09 00:00:00
---

# はじめに

{% youtube e4BhdrFDHkQ %}

この動画の5:30あたりで、マッピングノードのRotationを個別に操作できないため、
グループ化しないままノードをコピーして使用している。
これを回避するためにLocation, Rotation, Scaleに対する入力ソケットを持つMappingノードがほしい。

# マテリアルノードを使用

Blender標準のマテリアルノードを使用する。
回転行列に対応するノードが無いため、Mathノードなどを多用する。

![img](/blender/mapping_node/imn_000.png)

外観  

![img](/blender/mapping_node/imn_001.png)

位置、回転、拡大縮小を別グループに  

![img](/blender/mapping_node/imn_002.png)

位置の変更は加算  

![img](/blender/mapping_node/imn_003.png)

拡大縮小は乗算  

![img](/blender/mapping_node/imn_004.png)

回転はそれぞれの軸に沿った回転でグループ分け  

![img](/blender/mapping_node/imn_005.png)

x軸周りの回転  

![img](/blender/mapping_node/imn_006.png)

Radian→Degree変換  

サンプルプロジェクト : [ideal_mapping_node.blend](ideal_mapping_node.blend)

# Python Nodesを使用

Pythonスクリプトでカスタムノードを追加する。
しかし、テクスチャ座標のデータが取得できず、望むノードを作ることができなかった。

![img](/blender/mapping_node/imn_008.png)

図のようにつないで接続を変更しても、default_valueとして常に[0.0, 0.0, 0.0]のベクトルを取得してしまう。
接続元の名前は正しく取得できるが、ベクトルデータがうまく取得できない。
以下は調査時に作成したスクリプト。

サンプルコード : [https://gist.github.com/TakosukeGH/47520085854e360f0b83](https://gist.github.com/TakosukeGH/47520085854e360f0b83)

# Animation Nodesを使用

![img](/blender/mapping_node/imn_009.png)

Animation Nodesには行列計算ノードがあるのでこれをうまく使いたい。
しかしこちらもうまくいかない。

![img](/blender/mapping_node/imn_010.png)

マテリアルに設定したノードに対し、入力ソケットが接続されていない場合はAnimation Nodesで操作可能。
上の図のように接続された入力ソケットに対してはその入力が優先され、Animation Nodesで操作ができない。

Animation Nodes側にテクスチャ座標ノードがあればうまくいきそう。
現状ないので作成する必要があるが、テクスチャ座標の取得方法が分からない。

# OSLを使用

![img](/blender/mapping_node/imn_011.png)

レンダリングがCPUに限定されてしまうが、OSLを使って機能を実現することができる。

<script src="https://gist.github.com/TakosukeGH/00bb881ac95514203770.js"></script>

コードはとてもシンプル。

# おわりに

スクリプトでいろいろやるのが難しい。
力技だがマテリアルノードで組めてはいるので当面はそれを使用する。
