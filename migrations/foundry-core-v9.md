---
title: Migration Summary for v9
description: 
published: true
date: 2021-08-25T21:53:51.075Z
tags: 
editor: markdown
dateCreated: 2021-08-04T02:32:23.426Z
---

> Stub

## Deprecations from v8 deleted in v9

[Official Issue](https://gitlab.com/foundrynet/foundryvtt/-/issues/5783)

Anything marked Deprecated in v8 has been removed in v9. There's a full list of the removed methods as well as what should be used in their stead below, copied from the linked issue.

<details>
  <summary>List copied as of 08/25/2021</summary>
  
  ```
  Actor#_data -> Actor#data#_source
Actor#createOwnedItem -> Actor#createEmbeddedDocuments
Actor#deleteOwnedItem -> Actor#deleteEmbeddedDocuments
Actor#getOwnedItem -> Actor#items#get
Actor#updateOwnedItem -> Actor#updateEmbeddedDocuments
Actor.createTokenActor -> TokenDocument#getActor
Actor.fromToken -> TokenDocument#getActor
BaseEntitySheet -> DocumentSheet
Canvas#initializeSources -> Canvas#perception#initialize
CompendiumCollection#createEntity -> CompendiumCollection#documentClass.create or Document.createDocuments
CompendiumCollection#deleteEntity -> CompendiumCollection#getDocument#delete or Document.deleteDocuments
CompendiumCollection#entity -> CompendiumCollection#documentClass#documentName
CompendiumCollection#getContent -> CompendiumCollection#getDocuments
CompendiumCollection#getEntity -> CompendiumCollection#getDocument
CompendiumCollection#getEntry -> CompendiumCollection#getDocument#data
CompendiumCollection#importEntity -> CompendiumCollection#importDocument
CompendiumCollection#updateEntity -> CompendiumCollection#getDocument#update or Document.updateDocuments
CONFIG#[documentName]#entityClass -> CONFIG#[documentName]#documentClass
DicePool -> PoolTerm
DiceTerm.fromExpression -> DiceTerm.matchTerm or DiceTerm.fromMatch
DiceTerm.fromResults -> new DiceTerm
DiceTerm.getResultLabel -> DiceTerm#getResultLabel
Document#_id -> Document#id
Document#createEmbeddedEntity -> Document#createEmbeddedDocuments
Document#deleteEmbeddedEntity -> Document#deleteEmbeddedDocuments
Document#entity -> Document#documentName
Document#getEmbeddedEntity -> Document#getEmbeddedDocument
Document#hasPerm -> Document#testUserPermission
Document#owner -> Document#isOwner
Document#updateEmbeddedEntity -> Document#updateEmbeddedDocuments
Document.config -> Document.metadata
Document.delete -> Document.deleteDocuments
Document.update -> Document.updateDocuments
DocumentSheet#entity -> DocumentSheet#object
DocumentSheet#getData#entity -> DocumentSheet#getData#data
Drawing#author -> DrawingDocument#author
Drawing#owner -> DrawingDocument#isOwner
EasyRTCClient -> No longer used
Folder#entities -> Folder#contents
Item#_data -> Item#data#_source
Item.createOwned -> Item.create
Macros.canUseScripts -> User#can("MACRO_SCRIPT")
PlaceableObject#delete -> Document#delete
PlaceableObject#getFlag -> Document#getFlag
PlaceableObject#setFlag -> Document#setFlag
PlaceableObject#unsetFlag -> Document#unsetFlag
PlaceableObject#update -> Document#update
PlaceableObject#uuid -> Document#uuid
PlaceableObject.create -> Document.create
PlaceableObject.layer -> PlaceableObject#layer
PlaceablesLayer.createMany -> Document.createDocuments or Scene#createEmbeddedDocuments
PlaceablesLayer.dataArray -> PlaceablesLayer.documentClass.metadata.collection
PlaceablesLayer.deleteMany -> Document.deleteDocuments or Document#deleteEmbeddedDocuments
PlaceablesLayer.updateMany -> Document.updateDocuments or Scene#updateEmbeddedDocuments
Ray#normAngle -> No longer used
Roll#_rolled -> Roll#_evaluated
Roll.MATH_PROXY.safeEval -> Roll.safeEval
RollTable#getTableResult -> RollTable#results#get
SoundsLayer#update -> SoundsLayer#refresh
TabDirectory#entities -> TabDirectory#documents
Token#getBarAttribute -> TokenDocument#getBarAttribute
Token.fromActor -> Actor#getToken
User#isRole -> User#hasRole
User#setPermission -> User#assignPermission
window.FEATURES -> no longer used
WorldCollection#entities -> WorldCollection#contents
WorldCollection#importFromCollection -> WorldCollection#importFromCompendium
WorldCollection#insert -> WorldCollection#set
WorldCollection#object -> WorldCollection#documentClass
WorldCollection#remove -> WorldCollection#delete

  ```
</details>