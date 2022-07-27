---
title: Appel de Cthulhu 7e
description: Support pour l'Appel de Cthulhu
published: true
date: 2022-05-19T13:20:38.071Z
tags: 
editor: markdown
dateCreated: 2020-10-16T19:08:28.749Z
---

![logo_v7_(4).png](/images/home/logo_v7_(4).png)

## **Qu’est-ce que le système CoC7 ?**
CoC7 est une implémentation **non officielle** du jeu de rôle l’Appel de Cthulhu pour Foundry VTT.
L’Appel de Cthulhu est un grand ancien du jeu de rôle. La première version anglophone date de 1981, la première version francophone de 1984.

Ce jeu de rôle est basé sur l’univers d’HP Lovecraft. Il propose aux joueurs (investigateur) de se confronter au surnaturel et aux horreurs cosmiques qui forment le mythe de Cthulhu. Au cours de leurs aventure leur santé mentale serra mise à rude épreuve.

L’Appel de Cthulhu est un JDR d’ambiance plus qu’un jeu de simulation. Si les règles sont nécessaires, sinon il ne s’agit plus d’un jeu de rôle, elles peuvent parfois gêner la narration.
Le système CoC7 essaie (*modestement*) de décharger le gardien (maître de jeu) et les investigateurs de l’aspect gestion du jeu pour pouvoir se concentrer sur la narration et l’ambiance.

Historiquement les aventures pour l’appel de Cthulhu se déroulent dans les années 1920, mais peuvent être facilement transposées dans d’autres époques.

Le système CoC7 propose aussi une implémentation succincte des règles de Pulp Cthulhu.

## Installer le système
- [Installer l'application foundry VTT](https://foundryvtt.wiki/fr/pour-commencer/setup)
- Dans l'onglet Game System de l'application foundry chercher et installer "[Call of Cthulhu 7th edition](https://foundryvtt.com/packages/CoC7/)"

## Module recommandé
- [[Call of Cthulhu 7th French (Unofficial)]](https://foundryvtt.com/packages/coc7-module-fr-toc) : Integre la traduction française du système ADC ainsi que tous les éléments utiles (fiches, compétences, sortilèges, créatures...) SANS description.     
- [Dice so Nice](https://foundryvtt.com/packages/dice-so-nice/): Ajoute des dés en 3D.

## Tutoriel sur Youtube
- [Tutoriel AdC sur Foundry VTT (Kyl Game)](https://youtu.be/XkpVSr75A8k)

## Scénario clé en main
- [[Panique à San Francisco]](https://www.tentacules.net/index.php?id=5146) (issu d'un vieux Casus Belli; synopsis: Jean Christophe Carbonel; adaptation: J. Demesse; adaptation FoundryVTT : Carter)

## FAQ
**Pourquoi je n'ai qu'un contenu limité et en anglais livré avec le système ?**
>Le jeu de rôle l’AdC est propriété de Chaosium. Tout le texte des livre tombe sous le coup du de la propriété intellectuelle. Pour cette raison aucun contenu ne peut être distribué et chaque gardien devra rentrer son contenu pour pouvoir utiliser le système.
Quelques exemples sont livrés avec le système, ils ne contiennent aucun texte issu des livres. Jouant essentiellement en anglais j’ai fourni quelques exemples en anglais.

**Pourquoi les personnages que je créé n'ont aucune compétences ?**
>Encore une fois pour des raisons de propriété intellectuelle aucun contenu (compétence ou autre n'est livré avec le système)
De plus l'AdC est un système très ouvert et il est courant que chaque gardien créé ses propres compétences.

**J'ai trouvé un bug ou j'ai une question que faire ?**
>Vous pouvez contacter *Vétérini#8953* sur le discord de foundry ou sur le discord de la communauté française.
N’hésitez pas à rapporter vos bugs/suggestion sur la page [github du projet](https://github.com/HavlockV/CoC7-FoundryVTT/issues).

## Fonctionnalités principales
- Fiches de personnage. Les attributs dérivés sont calculés automatiquement. Les champs des fiches sont cliquables et vont déclencher le jet correspondant.

- Jets de compétences, avec les système de redoublement, et dépense de chance.

- Automatisation de la création de personnages. 3 types d'objet existent pour se faire.
  - Les setup, qui permettent de définir une ensemble de compétences/caractéristiques. Une fois l'objet créé il suffit de le glisser sur la fiche de personnage pour créer un personnage 'vierge'.
  - Les occupations, qui permettent de définir une profession ainsi que de calculer les points a distribuer parmi les différentes compétences. Comme pour les setup, il suffit de glisser une occupation sur un PJ. Les points peuvent ensuite être distribués dans l'onglet 'développement' de la fiche de personnage. Pour se faire votre gardien doit activer le mode création de personnage.
  - Les archétypes, fonctionnent comme les occupations et sont spécifiques a Pulp Cthulhu

- Implémentation du système d'expérience.

- Fiches de créatures. Fonctionnent de la même manière que les fiches de PJ. Elle permettent en plus d'avoir des jet de dès pour les caractéristiques.

- Jets de SAN.

- Automatisation des combats. Calcul des différents jets de dés, distance, balles tirées, difficultés etc...

- TBC...