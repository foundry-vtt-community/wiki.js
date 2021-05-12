---
title: Débutant : traduction simple d'un compedium avec Babele
description: Mini guide pour traduire un compendium à l'aide du module Babele
published: true
date: 2021-05-12T11:35:16.428Z
tags: compendium, guide, code, module, babele
editor: markdown
dateCreated: 2021-05-11T14:37:27.438Z
---

# Débutant : traduction simple d'un compedium avec Babele
> Cet page explique comment mapper vos traductions de compendia grâce au module [Babele](https://foundryvtt.com/packages/babele/), leur permettant ainsi de rester compatibles avec un système de base et d'éviter de recréer tous les objets déjà présents. Attention, il n'explique pas comment traduire un système complet.
{.is-info}
  
Pour plus de détails sur la traduction d'un système complet ou le scripting avec Babele, voir sur la [page Gitlab du développeur](https://gitlab.com/riccisi/foundryvtt-babele).
  
### Table des matières
1. [Création d'un module](#création-dun-module)
	1.1. [Arborescence du dossier d'un module](#arborescence-du-dossier-dun-module)
	1.2. [Fichier module.json type pour Babele](#fichier-modulejson-type-pour-babele)
	1.2. [Fichier d'enregistrement de Babele](#fichier-denregistrement-de-babele)
2. [Comment traduire](#comment-traduire)
	2.1. [Récupérer les fichiers JSON de Babele](#récupérer-les-fichiers-json-de-babele)
  2.2. [Structure des fichiers de traduction JSON](#structure-des-fichiers-de-traduction-json)
  2.3. [Outils pour éditer ces fichiers](#outils-pour-éditer-ces-fichiers)
3. [Diffuser son module](#diffuser-son-module)
	3.1. [Diffuser son module](#diffuser-son-module)
  3.2. [Partage de fichier ZIP](#partage-de-fichier-zip)
  3.3. [Exemple de dépôt GitHub](#exemple-de-dépôt-github)
  
## Création d'un module
### Arborescence du dossier d'un module
  
![Compendium Folder.png](/fr/developpement/babele/dir-compendium.png "Compendium Folder")
  
  
```
monmodule-fr-translation
│   module.json
│   register-babele.js
│
└───compendium
        systeme.compendium.json
```

### Fichier module.json type pour Babele
Exemple type d'un fichier de module.json :
```json
{
  "name": "monmodule-fr-translation",
  "title": "Système Foundry - Traduction Française",
  "description": "Module de traduction du système Système Foundry en Français, basé sur Babele.",
  "version": "0.2.37",
  "minimumCoreVersion" : "0.7.1",
  "compatibleCoreVersion": "7.9.0",
  "author": "Auteur du module",
  "dependencies": [
	{
      "name": "system-de-base",
	  "type": "system",
	  "version": "1.3.22"
    },
    {
      "name": "babele",
	  "type": "module",
	  "manifest": "https://gitlab.com/riccisi/foundryvtt-babele/raw/master/module/module.json",
      "version": "1.28"
    }
  ],
  "esmodules": [
      "babele-register.js"
    ],
  "scripts": [ ],
  "styles": [],
  "packs": [],
  "languages": [{
  	"lang": "fr",
  	"name": "Français",
  	"path": "lang/fr.json"  	
  }],
  "system": "system-de-base",
  "systemVersion": "1.3.22",
  "url": "https://github.com/mon-compte/mon-repository",
  "manifest": "https://github.com/mon-compte/mon-repository/raw/main/module.json",
  "download": "https://github.com/mon-compte/mon-repository/archive/refs/heads/main.zip"
}
```
**Attributs notables :**
- `"name"` : 
- `"title"` : 
- `"description"` : 
- `"version"` : 
- `"minimumCoreVersion"` : 
- `"compatibleCoreVersion"` : 
- `"author"` : 
- `"dependencies"` : 
	1. `"name"` : 
  2. `"type"`: 
  3. `"manifest"`: 
  4. `"version"`: 
- `"esmodules"` : 
- `"packs"` : 
- `"languages"` : 
- `"system"` : 
- `"systemVersion"` : 
- `"url"` : 
- `"manifest"` : 
- `"download"` : 

### Fichier d'enregistrement de Babele
Votre module doit idéalement comporter un fichier javascript (par exemple : `register-babele.js`) permettant de déclarer la traduction Babele automatiquement à Foundry VTT lorsque le module est activé. Le fichier suivant est une déclaration typique :
```js
Hooks.on('init', () => {
	if(typeof Babele !== 'undefined') {
		Babele.get().register({
			module: 'sfrpg-fr-translation',
			lang: 'fr',
			dir: 'compendium'
		});
	}
});
```
- l'attribut `module: 'sfrpg-fr-translation'` spécifie le module de traduction.
- L'attribut `dir: 'compendium'` permet de préciser le dossier à l'interieur duquel Babele trouveras les fichiers de traduction de compendia au format JSON.
  
Bien entendu tous ces attributs sont **sensibles à la casse**. Utilisez donc le nom exact en tenant compte des majuscules et minuscules : si le nom court du module est `wfrp4-wfrp4e-npc-generator`noter dans ce fichier `module: 'wfrp4-wfrp4e-npc-generator'` et non pas `module: '[WFRP4] NPC generator'`.
    
## Comment traduire

### Récupérer les fichiers JSON de Babele
> Le nom du fichier JSON de Babele doit toujours être du type ***systeme.compendium.json***, tel qu'exporté par Babele. **Ne le renomez pas**.
{.is-warning}
- ***systeme*** correspondant à la dénomination abrégée du système (ex: dnd5e, wfrp4, pf1, sfrpg, etc)
- ***compendium*** correpondant au nom exact du fichier de compendium sans l'extension .db (ex: skills pour skills.db, feats pour feats.db, etc)
 
*Exemple : `sfrpg.classes.json`*
  
### Structure des fichiers de traduction JSON
La structure basique d'un fichier JSON de traduction Babele se présente ainsi :
```json
{
	"label": "Classes (fr)",
	"entries":
	{
   	"Soldier": { 
			"name": "Soldat",  
			"description": "<p>Que vous ayez pris les armes pour protéger les autres ..." 
	},
  {
		"Operative": { 
			"name": "Agent", 
			"description": "<p>Vous êtes une ombre. Vous vous déplacez rapidement ..." 
		}
	}
}

```
Le format de ce fichier est dit "***compatible***" dans Babele par opposition à l'ancien format "*legacy*".
- l'attribut `"label"` correspond au nom long qui remplacera le nom du compendium dans Foundry VTT.
- la liste `"entries"`correspond à la liste des objets du compendium
- la liste `"Soldier"` correpond au nom **original** de l'objet à traduire ainsi qu'à l'objet lui même et ses détails :
	1. `"name"` au nom traduit de lobjet
  2. `"description"` à la... Traduction de la description de l'objet
> Notez que chaque nom est systématiquement entouré de guillemets et terminé par une virgule (***à l'exeption du dernier élément d'une série***), obligatoire avec le format d'échange de données JSON : `"name": "Soldat",`.
{.is-info}

Plusieurs traductions d'objets seront placés à la suite les une des autres dans la liste `"entries"` comme dans l'exemple, chaque objet sera entre accolades `{ }` et les objets entre eux seront séparés par des `,`.  

### Outils pour éditer ces fichiers
 
## Diffuser son module
  
  
### Partage de fichier ZIP
  
  
### Exemple de dépôt GitHub
  
  