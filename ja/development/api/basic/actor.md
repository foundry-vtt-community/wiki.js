---
title: キャラクター - Actor
description: 
published: true
date: 2022-08-14T02:09:19.323Z
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

## 覚書

実際のデータを確認することで、そのシステムの Actor データの構造を見てみることもできます。[グローバル Game オブジェクト](/ja/development/api/global-game-object)記事を参考にしてください。

### D&D

D&D System Module では、`Actor` は `Actor5e` サブクラスに拡張され、`"character"` / `"npc"` / `"vehicle"` の3種類の `type` のうちいずれかを設定されています。それぞれ `"プレイヤーキャラクター"` / `"ノンプレイヤーキャラクター"` / `"乗り物"` に対応します。

- https://github.com/foundryvtt/dnd5e/blob/master/module/actor/entity.js