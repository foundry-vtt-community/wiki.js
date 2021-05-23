---
title: Débutant : traduction simple d'un compedium avec Babele
description: Mini guide pour traduire un compendium à l'aide du module Babele
published: true
date: 2021-05-23T10:45:10.729Z
tags: localization, development, tutorial, template, compendium, translations, modules, languages, translation, guide, code, module, localizations, babele, didacticiel
editor: markdown
dateCreated: 2021-05-11T14:37:27.438Z
---

# Débutant : traduction simple d'un compedium avec Babele
> Cette page explique comment mapper vos traductions de compendium grâce au module [Babele](https://foundryvtt.com/packages/babele/), leur permettant ainsi de rester compatibles avec un système de base, d'éviter de recréer tous les objets déjà présents et de casser les différents liens vers d'autres fiches de personnage ou objets de compendium. Attention, il n'explique pas comment traduire tout un système complet.
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
3. [Ajouter des attributs à traduire](#ajouter-des-attributs-à-traduire)
	3.1. [Définir les attributs](#définir-les-attributs)
  3.2. [Ajouter les attributs à un objet](#ajouter-les-attributs-à-un-objet)
4. [Diffuser son module](#diffuser-son-module)
  4.1. [Partage de fichier ZIP](#partage-de-fichier-zip)
  4.2. [Utilisation d'un dépôt GitHub](#utilisation-dun-dépôt-github)
  
## Création d'un module
### Arborescence du dossier d'un module
  
```
systemefoundry-fr-translation
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
  "name": "systemefoundry-fr-translation",
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
- `"name"` : nom court du module (lettres minuscule, sans caractère spécial ni accentué et sans espace).
- `"title"` : titre plus détaillé et libre du module, celui qui s'affichera dans l'explorateur de modules de Foundry VTT.
- `"description"` : description qui s'affichera dans les détails du module dans l'explorateur de modules de Foundry VTT.
- `"version"` : version de ce module.
- `"minimumCoreVersion"` : version minimale compatible de Foundry VTT.
- `"compatibleCoreVersion"` : version maximale compatible de Foundry VTT.
- `"author"` : auteur du module.
- `"dependencies"` : dépendances du module, à savoir les autres modules ou systèmes nécessaires au fonctionnement de ce module. Typiquement les dépendances sont le système traduit ainsi que , bien entendu le module Babele.
	1. `"name"` : nom du module ou du système requis par votre module.
  2. `"type"`: système ou module
  3. `"manifest"`: lien optionnel ciblant l'installation du module ou du système requis. Ce qui permet par exemple de proposer d'installer automatiquement Babele avec votre module.
  4. `"version"`: la version minimale requise du système ou module requis par votre module.
- `"esmodules"` : scripts à lancer au chargement du module. Typiquement le script register-babele.js, mais d'autres peuvent être ajouté.
- `"packs"` : optionnel dans notre cas.
- `"languages"` : langages du module.
- `"system"` : systèmes avec lesquels le module est compatible
- `"systemVersion"` : la version du système avec le module est compatible.
- `"url"` : url du site de documentation de ce module ou de son auteur.
- `"manifest"` : lien vers le manifest d'installation du module.
- `"download"` : fichier ZIP à télécharger contenant tous les fichiers du module.

### Fichier d'enregistrement de Babele
Votre module doit idéalement comporter un fichier javascript (par exemple : `register-babele.js`) permettant de déclarer automatiquement à Foundry VTT la traduction Babele lorsque le module est activé. Le fichier suivant est une déclaration typique :
```js
Hooks.on('init', () => {
	if(typeof Babele !== 'undefined') {
		Babele.get().register({
			module: 'systemefoundry-fr-translation',
			lang: 'fr',
			dir: 'compendium'
		});
	}
});
```
- l'attribut `module: 'systemefoundry-fr-translation'` spécifie le nom court du module de traduction à charger au lancement du monde.
- L'attribut `dir: 'compendium'` permet de préciser le dossier à l'intérieur duquel Babele trouvera les fichiers de traduction de compendia au format JSON.
  
Bien entendu tous ces attributs sont **sensibles à la casse**. Utilisez donc le nom exact en tenant compte des majuscules et minuscules. Par exemple, si le nom court du module est `wfrp4-wfrp4e-npc-generator`noter dans ce fichier `module: 'wfrp4-wfrp4e-npc-generator'` et non pas `module: '[WFRP4] NPC generator'`.
    
## Comment traduire

### Récupérer les fichiers JSON de Babele
> Le nom du fichier JSON de Babele doit toujours être du type ***systeme.compendium.json***, tel qu'exporté par Babele. **Ne le renomez pas**.
{.is-warning}
- ***systeme*** correspondant à la dénomination abrégée du système (ex: dnd5e, wfrp4, pf1, sfrpg, etc)
- ***compendium*** correpondant au nom exact du fichier de compendium sans l'extension .db (ex: skills pour skills.db, feats pour feats.db, etc)
 
*Exemple : `sfrpg.classes.json`*
  
Bien entendu la traduction implique que vous ayez installé et activé dans votre monde le module Babele.
Il est par ailleurs **fortement conseillé de créer et dédié un monde à la traduction**, afin d'éviter toute mauvaise manipulation sur une de vos parties en cours.

Voici une démarche possible pour la traduction de description en utilisant Foundry.
- Dupliquez le compendium que vous souhaitez traduire et le nommer avec une marque différente de l'original :
![dupliquer-compendium01.png](/fr/developpement/dupliquer-compendium01.png)![dupliquer_compendium01.webp](/setup/babele/dupliquer_compendium01.webp) ![dupliquer-compendium02.png](/fr/developpement/babele/dupliquer-compendium02.png)

- Ouvrez votre compendium, puis un objet. Pour modifier la description de l'objet, cliquez sur le bouton qui apparaît en haut à droite du texte de la description :
![dupliquer-compendium03.png](/fr/developpement/babele/dupliquer-compendium03.png) ![dupliquer-compendium04.png](/fr/developpement/babele/dupliquer-compendium04.png) ![dupliquer-compendium05.png](/fr/developpement/babele/dupliquer-compendium05.png)

- Une fois vos objets traduits, récupérez les fichiers de traduction Babele en cliquant sur le bouton "Traduction" en haut de votre compendium :
![dupliquer-compendium06.png](/fr/developpement/babele/dupliquer-compendium06.png)

- Choisissez un mode de fichier à récupérer (*il est souvent préférable d'opter pour le mode "Legacy"*) :
![dupliquer-compendium07.png](/fr/developpement/babele/dupliquer-compendium07.png)
  
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
	1. `"name"` est le nom traduit de lobjet
  2. `"description"` est la... Traduction de la description de l'objet

> Notez que chaque nom est systématiquement entouré de guillemets et terminé par une virgule (***à l'exeption du dernier élément d'une série***), format de d'écriture obligatoire pour les fichiers d'échange de données JSON. Par exemple : `"name": "Soldat",`.
{.is-info}

Plusieurs traductions d'objets seront placées à la suite les unes des autres dans la liste `"entries"` et comme dans l'exemple, chaque objet sera entre accolades `{ }` et les objets entre eux seront séparés par des `,`.  

### Outils pour éditer ces fichiers
Voici une liste de quelques outils qui vous aiderons à éditer les fichiers :
- Foundry VTT : tout simplement l'éditeur de description des objets à utiliser sur les objets du compendium dupliqué (voir section [Récupérer les fichiers JSON de Babele](#récupérer-les-fichiers-JSON-de-babele)).
- Le site [html5-editor.net](https://html5-editor.net/) qui permet d'éditer un texte au format HTML5, utilise pour les descriptions.
- Notepad, [Notepad++](https://notepad-plus-plus.org/downloads/) ou équivalent selon votre système d'exploitation pour éditer les les fichiers de script, JSON, etc.

## Ajouter des attributs à traduire
Si vous souhaitez ajouter des éléments de traduction en plus du nom de l'objet et de sa description, il faut définir quels champs de le fiche Foundry Babele devra mapper avec vos traductions.
Puis, objet par objet, ajouter les valeurs pour ces champs.
### Définir les attributs
Dans les premières lignes du fichier Babele JSON de traduction (du type *système.compedium.json*), ajouter vos mappings à la suite de l'attribut `"label"` et avant les `"entries"` comme dans l'exemple suivant :
```json
"label": "Compendium-traduit",
	"mapping": {
		"source": "data.source",
		"type": "data.type",
    "subtype": "data.subtype"
  },
```
### Ajouter les attributs à un objet
Pour chaque objet, ajouter les attributs ainsi que les traduction à associer au mapping :
```json
{
	...
  "description": "<p>Lorem ipsum dolor sit amet</p>",
  "source": "Règles p.42",
	"type": "arme",
	"subtype": "corps à corps"
},
```
## Diffuser son module
  
### Partage de fichier ZIP
Pour partager simplement votre module de main en main, sur un quelconque service Drive ou sur Discord par exemple, il vous suffit de compresser en ZIP le dossier de votre module Foundry et de le transmettre.
  
### Utilisation d'un dépôt GitHub
Pour avoir des informations claires sur l'utilisation de GitHub, conultez la [page dédiée à Git](/fr/developpement/guides/git).
  
  