---
title: グローバル Game オブジェクト
description: Foundry VTT における Game クラスおよび game オブジェクトについて
published: true
date: 2022-08-14T02:08:26.609Z
tags: 
editor: markdown
dateCreated: 2022-08-14T01:33:57.604Z
---

# Game クラス

> この記事は簡単な覚書です。
{.is-warning}

https://foundryvtt.com/api/Game.html

## JavaScript Console　をつかった解析

ブラウザ版であっても、Electron 版（通常のアプリとして動作するバージョン）であっても、Foundry VTT は内部的には JavaScript で動作しています。そのため、JavaScript Console を開いてデータに直接アクセスすることができます。

Foundry VTT は JavaScript の Global 領域に game オブジェクトを作成し、すべてのデータがここに格納されています。そのため、例えば以下のようなコードを JavaScript Console で実行することで、例えば実際に動作している Foundry VTT の Actor データを確認することができます。

```javascript
game.actors
```

これにはすべてのデータが含まれます。そのため、例えば D&D System Module では PC と NPC でデータ構造が異なります。D&D System Module においてPCのデータだけを抜き出す場合、以下のようにフィルタリングします。

```javascript
game.actors.filter(actor => actor.type == 'character')
```

`filter` メソッドは JavaScript の一般的なメソッドです。

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter
