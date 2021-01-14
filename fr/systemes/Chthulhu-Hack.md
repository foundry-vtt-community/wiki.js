---
title: Cthulhu Hack
description: Cthulhu Hack VF © 2018 est un supplément édité par les XII Singes © 2006-2018 authorized translation of Cthulhu Hack © 2017 Paul Baldowski.
published: true
date: 2020-12-28T23:58:38.869Z
tags: 
editor: undefined
dateCreated: 2020-12-05T05:14:37.025Z
---

![ctulhuhack.png](/logos/ctulhuhack.png)
Le système est actuellement en cours de développement et dans une version pas assez avancée pour être rendu publique.
Par contre, des testeurs sont toujours les bienvenus, alors n'hésitez pas à rejoindre le canal dédié à Cthulhu Hack sur Discord pour vous faire connaitre.
## **Qu’est-ce que Cthulhu Hack ?**
*Cthulhu Hack* est un jeu de rôle traditionnel.
Des amis s’assoient autour d’une table (ou se réunissent via internet) pour raconter des histoires, soit en tant que **joueur** qui interprète un personnage de l’histoire, soit en tant que **Meneur de Jeu (MJ)** qui met en place le contexte, met en scène l’histoire, incarne les personnages non-joueurs (PNJ) et s’assure du bon respect des règles.

Tout semble reposer sur les épaules du MJ. En réalité, l’ensemble des participants collabore au développement d’une histoire attrayante pour tous.
Les règles de ***Cthulhu Hack*** constituent un ensemble de mécanismes simples qui permettent de jouer des investigations au cours desquelles les personnages des joueurs se confrontent aux créatures inventées par **H.P. Lovecraft.** Dans ce jeu, les joueurs lancent toujours les dés et gèrent des Ressources, tandis que le MJ se concentre sur l’histoire et sa mise en scène.

Les **joueurs** sont des investigateurs de l’Inconnu : des personnes ordinaires qui, par leur curiosité, leur héritage, ou à la faveur de circonstances particulières, se retrouvent embourbées dans un récit horrifique de dimension cosmique. Les joueurs imaginent le monde vu par leur investigateur.
Lorsqu’un investigateur tente de découvrir un indice, ou se bat pour sa propre survie, le joueur lance un dé pour savoir si son personnage a réussi ou s’il subit les conséquences de l’échec de ses initiatives.

Le **MJ** prépare et raconte l’histoire. Il décrit le monde, les PNJ, les péripéties et fournit des détails lorsque les investigateurs utilisent leurs sens ou demandent des précisions sur le contexte, la temporalité, les PNJ, le décor… tout ce qui ne relève pas de leurs personnages.
Le **MJ** écoute les joueurs et collabore avec eux pour créer une histoire palpitante, souvent en s’appuyant sur l’imagination fertile des joueurs et parfois même en retournant contre eux les idées et les craintes verbalisées pour leurs personnages.

### installer le soft et le système
- avoir foundryVTT d'installé
- avoir le système Cthulhu Hack d'installé

voir la page https://foundryvtt.wiki/fr/pour-commencer/setup pour ces étapes


## Ce que propose le système de Cthulhu Hack VF
Système créé par **Kristov**

### Actor

- Personnage
  - Ajout/modification/suppression des objets et des capacités spéciales
  - Jet de sauvegarde avec avantage/désavantage/bonus/malus, affichage de l'avantage éventuel donné par une capacité
  - Jet de ressource avec avantage/désavantage/bonus/malus et gestion de la diminution

- Opposant
	- Les attaques sont sous forme d'item


### Item

- Ability : capacité spéciale
- Item : gestion du dé de matériel avec possibilité de faire un jet (limitation connue : en cas de perte de ressource, il faut diminuer le dé à la main)
- Weapon : pour les armes, à faire glisser sur la fiche de personnage
- Attack : les attaques, à faire glisser sur la fiche d'opposant

### Options

- **Fortune** : active/désactive l'affichage sur la fiche
Le MJ active l'option et met le nombre de jetons disponibles pour les joueurs
Affichage du nombre de jetons sous le portrait et possibilité de dépenser pour un joueur (limitation connue : après la dépense, il faut fermer et ouvrir la fiche pour voir le nombre restant mis à jour)

- **Adrenaline** : active/désactive l'affichage sur la fiche
Sous le portrait : éclair jeton pour le joueur, tête de mort jeton pour le MJ

- **Dé de vie en tant que ressource** : active/désactive l'affichage sur la fiche
- **Richesse en tant que ressource** : active/désactive l'affichage sur la fiche

### Compendiums

- **Archétypes standards** (avec les variations pour le savant) : Drag and drop sur la fiche de perso qui remplace les valeurs de la fiche par celles de l'archetype
- **Capacités spéciales** : contient toutes les capacités standards

- Drag and drop pour ajouter à la fiche (avec prise en compte des capacités multiples)
- Gestion du nombre d'utilisations et affichage de la date de dernier usage lors du clic sur l'utilisation
- Clic sur le nom pour déplier la description

## Notes de Mise à Jour

**Le projet** : https://gitlab.com/foundryvtt2/FoundryVTT-Cthulhu-Hack

**La dernière version sur gitlab :** 29 Décembre 2020
* Compendium des armes spéciales
* Champ description sur l'item attack

