---
title: Gérer les compendium v11 et + en JSON
description: Comment gérer les compendiums en fichiers JSON ?
published: true
date: 2023-11-16T21:06:41.044Z
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

Une fois fait, vous aurez alors accès à la commande `fvtt` depuis une fenêtre de terminal. Vous pouvez tester la bonne installation de l’outil en tapant `fvtt --version` . Vous devriez voir en retour quelque chose comme : `1.0.0-rc.4`.

> Ce guide ne se focalise par sur l’installation du CLI. Si vous rencontrez un problème lors de son installation, n’hésitez pas à venir en discuter sur le Discord de [La Fonderie](https://discord.gg/pPSDNJk) !
{.is-info}

## Configuration de foundryvtt-cli

Avant de pouvoir travailler avec l’outil, il est nécessaire de le configurer pour lui indiquer où se trouve le répertoire de vos données (le répertoire qui contient par défaut les sous-répertoires `Config/`, `Data/` et `Logs`).

Pour cela, il faut exécuter la commande suivante : `fvtt configure set dataPath /path/to/Foundry`

> Il convient d’adapter le chemin vers le bon répertoire. Référez-vous [à la documentation officielle](https://foundryvtt.com/article/configuration/#where-user-data) si vous ne le connaissez pas.
{.is-warning}

## Pré-requis

> Avant de pouvoir travailler avec les compendiums, il convient de s’assurer qu’aucun monde n’est lancé.
{.is-info}

Il faut indiquer à l’outil quel est le système / module / monde sur lequel on souhaite travailler.

Pour se faire, on va utiliser la commande suivante : `fvtt package workon "xxx" --type "yyy"`.
- `xxx` est à remplacer par l’ID du système/module/monde concerné (le nom du répertoire contenant les données)
- `yyy` est à remplacer par `System` si on travaille sur un système ; `Module` si on travaille sur un module ; `World` si on travaille sur un monde)

Voici un exemple qui montre la commande, suivi de la réponse fournie par l’outil :

```bash
$ fvtt package workon "heist" --type "System"
Swapped to System heist
```

## Transformer les LevelDB en fichiers JSON/YAML

Maintenant que l’outil est prêt, il ne reste plus qu’à extraire le compendium voulu via la commande suivante : `fvtt package unpack xxx`
- `xxx` est à remplacer par l’ID du compendium (le nom du répertoire contenant le fichiers binaires)

Voici un exemple qui montre la commande, suivi de la réponse fournie par l’outil :

```bash
$ fvtt package unpack skills
[classic-level] Unpacking "/home/djlechuck/.local/share/FoundryVTT-v11/Data/systems/heist/packs/skills" to "/home/djlechuck/.local/share/FoundryVTT-v11/Data/systems/heist/packs/skills/_source"
Wrote Discr_tion_ISd3uey0EJHCXEJa.json
Wrote Piratage_eGeDbV6vjWQpcvUk.json
Wrote Crochetage_xSmO93zWpQ1Xv4u4.json
```

> Il est possible de choisir le répertoire de destination des fichiers via l’option `--out /path/to/files`
> Il est également possible d’obtenir des fichiers YAML au lieu des JSON via l’option `--yaml`
{.is-info}

On obtient alors un fichier par élément du compendium et il n’y a plus qu’à effectuer les modifications souhaitées directement dans ces derniers.

Une fois que l’on a terminé il faut effectuer l’opération inverse, à savoir transformer les fichier JSON / YAML en fichiers binaires pour LevelDB.

## Transformer les fichiers JSON/YAML en LevelDB

La commande à utiliser est similaire à celle permettant d’extraire les données : `fvtt package pack xxx`
- `xxx` est à remplacer par l’ID du compendium (le nom du répertoire contenant le fichiers binaires)

Voici un exemple qui montre la commande, suivi de la réponse fournie par l’outil :

```bash
$ fvtt package pack skills
[classic-level] Packing "/home/djlechuck/.local/share/FoundryVTT-v11/Data/systems/heist/packs/skills/_source" into "/home/djlechuck/.local/share/FoundryVTT-v11/Data/systems/heist/packs/skills"
Packed xSmO93zWpQ1Xv4u4 (Crochetage)
Packed ISd3uey0EJHCXEJa (Discrétion)
Packed eGeDbV6vjWQpcvUk (Piratage)
```

> Il est possible de choisir le répertoire source des fichiers via l’option `--in /path/to/files`
> **Attention :** Il est nécessaire d’ajouter l’option `--yaml` si vous l’avez utilisée pendant l’extraction des données.
{.is-info}

## Nettoyage de l’espace de travail

Une fois que tout est terminé, il convient d’indiquer à l’outil que nous avons terminé grâce à la commande suivante : `fvtt package clear`.

On peut alors relancer Foundry et vérifier que nos modifications ont bien été prise en compte.
