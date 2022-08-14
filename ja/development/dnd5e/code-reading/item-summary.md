---
title: Item Summary の開閉
description: 
published: true
date: 2022-08-14T03:24:25.272Z
tags: 
editor: markdown
dateCreated: 2022-08-14T03:22:52.432Z
---

![Up to date as of 1.6.3](https://img.shields.io/static/v1?label=dnd5e&message=1.6.3&color=informational)

# Item Summary の開閉

キャラクターシートなどで Item の行をクリックすると、詳細が開いたり閉じたりする機能にまつわる覚書です。

## base.js

- https://github.com/foundryvtt/dnd5e/blob/release-1.6.3/module/actor/sheets/base.js#L555

`activateListeners` 内部で jQuery を使って `.item .item-name.rollable h4` に対してクリックイベントをセットしています。

`_onItemSummary` は Item Summary を HTML に追加したり削除したりします。

- https://github.com/foundryvtt/dnd5e/blob/release-1.6.3/module/actor/sheets/base.js#L956

FvTT は内部的に jQuery が利用できるようになっていて、このコードも jQuery を利用したアニメーションになっています。

キャラクターシート（Actor Sheet）のアイテム一覧の HTML は、例えば呪文書なら `templates/actors/parts/actor-spellbook.html` にありますが、 `.item-summary` についてはこのテンプレートファイルを見てもなにも情報を得られません。