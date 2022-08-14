---
title: ActiveEffect と Actor
description: 
published: true
date: 2022-08-14T02:05:35.247Z
tags: 
editor: markdown
dateCreated: 2022-08-14T02:05:35.247Z
---

# Resources

## Actor

対訳：キャラクター

`Actor` エンティティ。D&D 5e System Module では `Actor5e` に拡張されている。

- [Actor](/ja/development/api/basic/actor) on This Wiki
- [Code on GitHub](https://github.com/foundryvtt/dnd5e/blob/master/module/actor/entity.js)
- [Official API Documentation](https://foundryvtt.com/api/Actor.html)
- [Official API Documentation for v10](https://foundryvtt.com/api/v10/classes/client.Actor.html)

## ActiveEffect

対訳：アクティブ効果

Foundry VTTにおいてActorやItemに特定のデータを上書きする能力を持つオブジェクト。例えば、「このActiveEffectが有効なときにアーマークラスを+1する」ようなことができる。

# ActiveEffect の書き方

ActiveEffect の個々の要素は、3つのパラメータを持っています。つまり、キー、上書き方法、値です。

![screen_shot_activeeffect_changeeffect.png](/development/dnd/screen_shot_activeeffect_changeeffect.png)

このセクションは Voundry VTT 内部では ChangeData というクラスによって構造化されています。

- https://foundryvtt.com/api/data.EffectChangeData.html

それぞれ `key`, `mode`, そして値の部分はドキュメントが壊れていますが、`value` で参照されます。また、D&D 5e System Module から直接操作できませんが、`priority` もあります。

`mode` は Foundry VTT 本体に定義されているようですが、API Documentation からは確認できないようです。

- https://github.com/foundryvtt/dnd5e/blob/release-1.6.3/module/active-effect.js#L88

D&D 5e System Module 1.6.3 の `ActiveEffect5e` のコードを読んでみると、Foundry VTT に定義されている ActiveEffect の変更用シートを呼び出しているようです。つまり、`mode` (ChangeMode) の値について、System Module 側は意識する必要はないということのようです。

> 個人的な感想ですが、Foundry VTT の ActiveEffect Sheet を直接呼び出す場合、ユーザーが `key` (Attribute Key) を操作できてしまうので、あまりいい方法とは言えないんじゃないかと思います。

ActiveEffect を Actor に適用する処理は、Foundry VTT が行ってくれます。`createEmbeddedDocuments("ActiveEffect", ...` を使いましょう。

- https://foundryvtt.com/api/Actor.html#createEmbeddedDocuments