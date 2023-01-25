---
title: Faire un module de contenu
description: 
published: true
date: 2023-01-25T16:26:58.509Z
tags: 
editor: markdown
dateCreated: 2023-01-02T09:05:01.700Z
---

# Module de contenu

## Pré-requis

Créez vos compendiums dans votre monde, puis arrêtez votre monde et allez récupérer les fichiers .db correspondants.

Choisissez un nom pour votre module de contenu. Si votre module concerne les aventures du terrible pirate Roberts, nommez le "*fvtt-pirate-roberts*" par exemple.

## Structure

Dans un espace de travail, créez un répertoire avec un nom **strictement identique** au nom du module. Dans le cas de l'exemple en cours, on créé un répertoire *fvtt-pirate-roberts*.

Créez ensuite les répertoires et fichiers suivants : 
![module_content_01.jpg](/development/module_content_01.jpg)

1. Répertoire "packs" : contient les fichiers .db de vos compendiums. Vous devez les recopier ici.
2. Répertoire "styles": contient votre éventuel fichier CSS
3. Fichier module.json : c'est le plus important, celui qui va servir à déclarer le module sous Foundry.

## Fichier module.json

Il doit avoir la structure suivante :

```json
    "id": "fvtt-pirate-roberts",
    "title": "Pirate Roberts",
    "description": "Contenu des aventures du Pirate Roberts",
    "version": "1.0.0",
    "compatibility": {
        "minimum": 10,
        "verified": 10,
        "maximum": 10
    },
    "esmodules": [],
    "styles": [
      "styles/styles.css"
    ],
    "languages": [],
    "packs": [
        {
            "name": "cartes-roberts",
            "label": "Cartes du Pirate Roberts",
            "path": "packs/cartes-roberts.db",
            "type": "Adventure"
        }
    ],
    "url": "",
    "manifest": "",
    "download": ""
}
```

- id : très important. Doite être **strictement identique** au nom de votre module (et donc à votre répertoire)
- title : Ce que vous voulez
- description : Idem
- styles : Si votre module comporte des styles, placez le fichier CSS ici. Sino, supprimez la ligne, tout simplement.
- packs : c'est le second élément important. Vous devez venir déclarer vos compendiums ici. Le champe "name" doit être identique au nom du fichier sans le .db. Le champ Type est important, il doit avoir une des valeurs suivantes: 
    - RollTable : des roll tables
		- Scene : des maps et images
    - Item : des items (selon le système : sorts, équipement, etc, etc)
    - Adventure : un compendium "Aventure"
    - Actor : des PJs/PNJs
    - JournalNote : des notes de journaux

## Cas particulier des images
Si vous utilisez des images dans un de vos compendiums, que ce soit l'image d'un acteur ou une image intégrée dans un journal que vous avez mis dans une aventure, il y a un traitement particulier. En effet le chemin de l'image sera sauvegardé dans le compendium. Comme vous travaillez dans un monde, ce sera donc le chemin lié au monde (dans data/worlds/nom-du-monde-original). Si quelqu'un d'autre installe votre module, celui-ci ira chercher les images dans data/worlds/nom-du-monde-original et bien sûr ne trouvera rien.

Ce qui est conseillé : 
- Créez dans votre monde un répertoire assets dans lequel vous mettez toutes les images, vous pouvez organiser l'intérieur comme vous voulez.
- Copiez ce répertoire assets puis coller le dans votre module : à la racine, donc au même niveau que packs
- Après avoir récupéré les fichiers .db et les avoir copiée dans votre module : ouvrez les avec un éditeur de texte (Notepad++ ou VSCode), puis faites un rechercher/remplacer de /worlds/nom-du-monde-original vers /modules/nom-du-module. Cela permet de changer le chemin pour accéder aux images qui sont dans le module.

## Installation

Uen fois votre module.json prêt, recopiez l'intégralité du répertoire dans votre répertoires *modules* de Foundry et relancez Foundry. Si tout s'est bien passé, il devrait apparaître dans la liste des modules disponibles.

Si ce n'est pas le cas, consultez la console (F12) pour voir quellles erreurs se sont produites.

