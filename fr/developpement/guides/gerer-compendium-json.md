---
title: Gérer les compendium v11 et + en JSON
description: Comment gérer les compendiums en fichiers JSON ?
published: false
date: 2023-11-16T18:45:01.795Z
tags: compendium
editor: markdown
dateCreated: 2023-11-16T18:17:17.123Z
---

# Comment gérer les compendiums en fichiers JSON ?

À partir de la v11, le format des compendiums est passé du format NedDB au format LevelDB.

Un compendium au format NedDB était un simple fichier texte contenant une entrée de compendium par ligne au format JSON, ce qui rendait son exploitation relativement simple en utilisant n’importe quel éditeur de texte. Le format LevelDB est quant à lui un format binaire, la donnée n’est pas lisible directement sans passer par des étapes intermédiaires ; chaque compendium n’est plus un simple fichier texte, mais un répertoire contenant plusieurs fichiers qu’il ne faut absolument pas éditer manuellement.

Pour pouvoir éditer le contenu de ce nouveau format, nous allons passer par [foundryvtt-cli](https://github.com/foundryvtt/foundryvtt-cli), l’outil en ligne de commande officiel de Foundry qui permet, entre autres, d’obtenir des fichiers JSON ou YAML de nos compendiums.

## Installation de foundryvtt-cli

L’outil est écrit en JavaScript et nécessite que NodeJS soit installé afin de pouvoir être utilisé. Normalement, si Foundry est déjà installé sur votre machine, alors vous n’avez rien à faire.

Pour l’installer de manière globale sur votre ordinateur, il suffit d’exécuter la commande suivante : `npm install -g @foundryvtt/foundryvtt-cli`

Une fois fait, vous aurez alors accès à la commande `fvtt` depuis une fenêtre de terminal. Vous pouvez tester la bonne installation de l’outil en tapant `fvtt --version` : vous devriez voir en retour quelque chose comme 

`1.0.0-rc.4`.

*Ce guide ne se focalise par sur l’installation du CLI. Si vous rencontrez un problème lors de son installation, n’hésitez pas à venir en discuter sur le Discord !*

## Configuration de foundryvtt-cli

Avant de pouvoir travailler avec l’outil, il est nécessaire de le configurer pour lui indiquer où est installé Foundry et où se trouve le répertoire `Data/`.

Pour cela, il faut exécuter deux commandes :

-   Définir où est le répertoire `Data/` de Foundry : `fvtt configure set dataPath /path/to/Foundry/Data`

## Transformer les LevelDB en fichiers JSON/YAML

## Transformer les fichiers JSON/YAML en LevelDB