---
title: キャラクター / Actor
description: 
published: true
date: 2022-08-14T01:26:45.335Z
tags: 
editor: markdown
dateCreated: 2022-08-14T00:55:10.787Z
---

# キャラクター (Actor) について

![Up to date as of v9](https://img.shields.io/static/v1?label=FoundryVTT&message=v9&color=informational)

> Foundry VTT のオフィシャルドキュメントも参考にしてください [API Documentation for v9](https://foundryvtt.com/api/Actor.html)
{.is-info}

> この記事は書きかけで、内容についての検証が行われていません。また、v10 での検証を行っていません。
{.is-warning}

- https://foundryvtt.com/api/Actor.html
現行バージョン API Documentation の Actor ページ
- https://foundryvtt.com/api/v10/classes/client.Actor.html
v10 API Documentation の Actor ページ
- https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD06-Extending-the-Actor-class
英語 Community Wiki の System Module Development Tutorial に書かれたアクター関係の記事|

## 対訳

キャラクター = Actor

Actor は直訳すると「俳優」ですが、ゲームプログラミングにおいて画面に登場するキャラクター全般を Actor と表現することがあります。日本語環境では、Actor はキャラクターと訳されています。

- https://github.com/BrotherSharper/foundryVTTja/blob/d1aedbad5f67fb0cb57891c38acdc50578487bbf/lang/ja.json#L441-L442

## Foundry VTT における Actor

Foundry VTT では、いわゆるプレイヤーキャラクター・ノンプレイヤーキャラクターをひとまとめに Actor というデータ構造で扱います。Actor の特徴は以下のとおりです。

- Actor はマップの駒（トークン/Token）になることができる。
- Actor はゲームシステムごとに異なる様々なデータを保持しておくことができる。
- Actor は Item を所持できる。
- Actor は ActiveEffect の効果の適用対象になることができる。

Foundry VTT の画面の右側にあるサイドバーから、キャラクター一覧（Actors Directory）を選択することで、そのワールド上に存在するキャラクターの一覧が確認できます。GMユーザーならすべてのキャラクターが確認できるはずですが、プレイヤーユーザーの場合は自分に閲覧権限があるものだけが見えます。

![screen_shot_of_actordirectory.png](/images/japanese-community/development/screen_shot_of_actordirectory.png)

## Actor データの確認方法

各ゲームシステムモジュールで準備されているキャラクターシートを開いてデータを見るのが通常の使い方です。しかし、開発時はもっと詳細に確認したい場合もありますので、ここではいくつか方法を紹介します。

### JavaScript Console を使う

ブラウザ版であっても、Electron 版（通常のアプリとして動作するバージョン）であっても、Foundry VTT は内部的には JavaScript で動作しています。そのため、JavaScript Console を開いてデータに直接アクセスすることができます。

Foundry VTT は JavaScript の Global 領域に game オブジェクトを作成し、すべてのデータがここに格納されています。そのため、例えば以下のようなコードを JavaScript Console で実行することで、実際に動作している Foundry VTT の Actor データを確認することができます。

```javascript
game.actors
```

これにはすべてのデータが含まれます。そのため、例えば D&D System Module では PC と NPC でデータ構造が異なります。D&D System Module においてPCのデータだけを抜き出す場合、以下のようにフィルタリングします。

```javascript
game.actors.filter(actor => actor.type == 'character')
```

`filter` メソッドは JavaScript の一般的なメソッドです。

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter

## 覚書

### D&D

D&D System Module では、`Actor` は `Actor5e` サブクラスに拡張され、`"character"` / `"npc"` / `"vehicle"` の3種類の `type` のうちいずれかを設定されています。それぞれ `"プレイヤーキャラクター"` / `"ノンプレイヤーキャラクター"` / `"乗り物"` に対応します。

- https://github.com/foundryvtt/dnd5e/blob/master/module/actor/entity.js