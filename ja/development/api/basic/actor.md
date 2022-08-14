---
title: Actor
description: 
published: true
date: 2022-08-14T00:55:10.787Z
tags: 
editor: markdown
dateCreated: 2022-08-14T00:55:10.787Z
---

# キャラクター (Actor) について

この記事では Foundry VTT の基本的な概念の一つである Actor についての説明を試みます。

この記事は書きかけで、内容についての検証が行われていません。

## Official Resources

- https://foundryvtt.com/api/Actor.html
現行バージョン API Documentation の Actor ページ
- https://foundryvtt.com/api/v10/classes/client.Actor.html
v10 API Documentation の Actor ページ
- https://foundryvtt.wiki/en/development/guides/SD-tutorial/SD06-Extending-the-Actor-class
英語 Community Wiki の System Module Development Tutorial に書かれたアクター関係の記事|

## 対訳

キャラクター = Actor

Actor は直訳すると「俳優」ですが、ゲームプログラミングにおいて画面に登場するキャラクター全般を Actor と表現することがあります。日本語環境では、Actor はキャラクターと訳されています。

- https://github.com/BrotherSharper/foundryVTTja/blob/master/lang/ja.json#L441-L442

## Foundry VTT における Actor

Foundry VTT では、いわゆるプレイヤーキャラクター・ノンプレイヤーキャラクターをひとまとめに Actor というデータ構造で扱います。Actor の特徴は以下のとおりです。

- Actor はマップの駒（トークン/Token）になることができる。
- Actor はゲームシステムごとに異なる様々なデータを保持しておくことができる。
- Actor は Item を所持できる。
- Actor は ActiveEffect の効果の適用対象になることができる。