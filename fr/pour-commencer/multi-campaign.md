---
title: 4.1 Créer plusieurs parties à partir d'une seule et même préparation.
description: 
published: true
date: 2021-04-21T15:09:23.752Z
tags: 
editor: markdown
dateCreated: 2021-04-12T11:07:10.096Z
---

La première chose que nous voulons faire, c'est de créer notre partie afin de la faire jouer, voir rejouer avec nos amis.

**Cette page est largement incomplète  est sera mise à jour prochainement**

## Créer plusieurs parties à partir d'une seule et même préparation.

La création d'une partie est assez simple et demande une préparation au préalable afin de pouvoir faire les choses simples et le plus rapidement possible. La première des choses commence donc par une bonne préparation.

## Comment faire une bonne préparation

Comment faire une préparation de campagne ou scénario et la jouer sur plusieurs tables différentes.
Afin d'éviter à devoir tout refaire à chaque fois, je vais essayer de vous expliquer une petite astuce qui devrait réduire votre temps de travail.
Prenons les deux cas dès le départ, cela sera plus simple.
Pour cela il va falloir créer 3 Worlds, Un pour la préparation, 2 pour les campagnes. 

***On va les nommer :***
- Campagne Originale (La Préparation)
- Campagne 1
- Campagne 2

Nous allons faire **toutes les préparations** au fur et à mesure sur **Campagne Originale** et nous importerons à chaque fois les fichiers de **Campagne Originale** sur **Campagne 1 et 2**.
Lors du lancement des campagnes, il faudra Copier/coller, dans Campagne 1 et 2, les répertoires de Campagne Originale suivant :
- Le Répertoire Data
- Le Répertoire Scene
A partir de la vous avez votre base de travail.

Vous faites votre première séance dans les campagnes et c'est la qu'il faudra répéter à chaque fois le même schéma :
- Avant toutes choses, Exporter les personnages de chaque campagne
- Copier à partir du répertoire Data de la Campagne Originales les fichiers :
	- **actors.db,** 
  - **chat.db,** 
  - **combat.db,** 
  - **fog.db,** 
  - **folders.db,** 
  - **items.db,** 
  - **journal.db,** 
  - **macros.db,** 
  - **playlists.db,** 
  - **scenes.db,** 
  - **settings.db,** 
  - **tables.db,** 
  - ainsi que le répertoire scenes et coller les dans vos répertoires de **Campagne 1 & 2**
- **NE JAMAIS** copier le fichier **users.db**
- Revenir sur votre Campagne 1 & 2, Importer les personnages

Voila, vous avez fait la mise à jour de votre préparation sur chaque campagne.
Les points négatifs, c'est que cela remet à zero tout ce qui a pu etre crée et qui ne se trouve pas sur la feuille de personnage du joueur tel que les macros dans les barres des taches.