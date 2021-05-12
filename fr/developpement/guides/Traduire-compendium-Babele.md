---
title: Débutant : traduction simple d'un compedium avec Babele
description: Mini guide pour traduire un compendium à l'aide du module Babele
published: true
date: 2021-05-12T09:21:42.610Z
tags: compendium, guide, code, module, babele
editor: markdown
dateCreated: 2021-05-11T14:37:27.438Z
---

# Débutant : traduction simple d'un compedium avec Babele
> Cet page explique comment mapper vos traductions de compendia grâce au module [Babele](https://foundryvtt.com/packages/babele/), leur permettant ainsi de rester compatibles avec un système de base et d'éviter de recréer tous les objets déjà présents. Attention, il n'explique pas comment traduire un système complet.
{.is-info}

## Index du document
1. [Création d'un module](#création-d'un-module)
2. 
3. 
4. 

Pour plus de détails sur la traduction d'un système complet ou le scripting avec Babele, voir sur la [page Gitlab du développeur](https://gitlab.com/riccisi/foundryvtt-babele).

## Création d'un module
### Arborescence du dossier d'un module

### Fichier module.json type pour Babele

  
### Fichier d'enregistrement de Babele
Votre module doit idéalement comporter un fichier javascript de déclaration de traduction Babele du type suivant :
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

### Structure des fichiers de traduction de Compendium
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
  
### Nom du fichier JSON de Babele
> Le nom du fichier JSON de Babele doit toujours être du type ***systeme.compendium.json***, tel qu'exporté par Babele (voir plus bas).
{.is-warning}
- ***systeme*** correspondant à la dénomination abrégée du système (ex: dnd5e, wfrp4, pf1, sfrpg, etc)
- ***compendium*** correpondant au nom exact du fichier de compendium sans l'extension .db (ex: skills pour skills.db, feats pour feats.db, etc)

  
  
## Comment traduire

### Récupérer les fichiers JSON de Babele

### Outils pour éditer ces fichiers
 
## Diffuser son module
  
  
### Partage de fichier ZIP
  
  
### Dépôt Git
  
  