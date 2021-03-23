---
title: Starwars FFG
description: Support pour Starwars FFG
published: true
date: 2021-02-10T20:23:15.709Z
tags: 
editor: markdown
dateCreated: 2020-10-19T15:58:58.380Z
---

![logo SWFFG](https://gamersplane.com/images/logos/starwarsffg.jpg) 
Ceci est une implémentation communautaire du jeu de rôle Star Wars de Fantasy Flight Games pour le programme Foundry Virtual Tabletop.

## Installation

 -   Lancez Foundry VTT
 -   Allez dans l'onglet "Systèmes de Jeu"
 -   Cliquez sur le bouton "Installer un Système de jeu"
 -   Copiez Un des liens ci-dessous dans le champ "URL du Manifest"
 -   Cliquez sur Installation, après quelques secondes le système devrait être installé.

### Manifest

* Manifest Original : 
https://raw.githubusercontent.com/StarWarsFoundryVTT/StarWarsFFG/master/system.json
* Manifest Custom avec un meilleur rendu graphique (par Mandar) : 
https://raw.githubusercontent.com/Mandaar/StarWarsFFG/master/system.json

Merci à toute la team **StarWarsFoundryVTT**

## Créer un monde

Maintenant que le système est installé, vous devez créer un monde.
Retournez à la page Mondes de jeu, cliquez sur Créer un monde, et assurez-vous de sélectionner **Star Wars FFG** dans le menu déroulant Système de jeu.

![créer un monde](https://cdn.discordapp.com/attachments/722396272505389087/791333007659106324/screenNewWorld.jpg)

## Importer des données

Comme le contenu de FFG Star Wars est protégé par des droits d'auteur, vous devrez remplir le contenu vous-même. Pour vous aider dans cette tâche, les développeurs ont créé plusieurs outils qui vous permettent d'importer des données à partir d'autres outils plus courants.

###### Importer des données OggDude

Pour importer la grande majorité des données d'articles, vous pouvez utiliser l'importateur "OggDude Dataset Importer".

- Dans votre monde, sélectionnez Paramètres du jeu
- Cliquez sur Configurer les paramètres
- Sélectionnez les paramètres du système
- Cliquez sur OggDude Dataset Importer
- Agrandissez le dossier Data de votre générateur de personnages OggDude (photo ci-dessous)
- Sélectionnez votre fichier de données zippé à l'aide du sélecteur de fichiers "Choose File
- Cliquez sur Charger le fichier
- Sélectionnez les types de choses que vous souhaitez importer
- Cliquez sur Démarrer l'importation

Les données qui en résultent sont ajoutées aux Compendiums.

![Le fichier de données zippé](https://camo.githubusercontent.com/8ee0498bce3adcaf3abb4873645598d767481a5758ff38ccebaaf6100597e989/68747470733a2f2f692e696d6775722e636f6d2f726651504a73732e706e67)

**Remarque :** Si l'un des Compendiums est vide, cela signifie généralement qu'il y a un problème avec votre jeu de données, mais si, vous-êtes sur la branche de développement, il peut également s'agir d'un bogue.

###### Importer des données sur les adversaires

Vous pouvez également importer tous les PNJ figurant sur [Star Wars Adversaries](http://swa.stoogoff.com).
Pour ce faire, il suffit de :

- Naviguer vers la page Github
- Cliquez sur le bouton vert "Code" et sélectionnez "Télécharger ZIP
- Dans votre monde, sélectionnez Paramètres du jeu
- Cliquez sur Configurer les paramètres
- Sélectionnez les paramètres du système
- Sélectionner l'Importateur Star Wars Adversaries
- Sélectionnez le ZIP que vous avez téléchargé à l'étape 2
- Cliquez sur "Démarrer l'importation".

**Note :**
Comme ces données sur les adversaires ne sont pas configurées pour utiliser les données OggDude, les adversaires importés n'auront aucun élément de ces compendiums, donc s'il y a une faute de frappe dans les données sur les adversaires, le donné n'apparaîtra pas.

## Modules conseillés par la communauté

Voici une liste des modules conseillés par la communauté Discord FR pour profiter au maximum du système Star Wars FFG:
- [Chat Images](https://foundryvtt.com/packages/chat-images/) Permet d'importer rapidement via un cliquer glisser dans le Chat.
- [Easy Target](https://foundryvtt.com/packages/easy-target/) Simplifie le target des tokens
- [FXMaster](https://foundryvtt.com/packages/fxmaster/) Permet un excellent éventail d'effets divers.
- [JB2A - Jules&Ben's Animated Assets](https://foundryvtt.com/packages/JB2A_DnD5e/) Un module au départ pour DnD mais qui permet des effets d'arme et de sort utilisable dans Star Wars.
- [Maestro](https://foundryvtt.com/packages/maestro/) - Pour la musique et certains automatismes
- [Multilevel token](https://foundryvtt.com/packages/multilevel-tokens/) - Permet de déplacer les tokens entre map et bien d'autres choses encore
- [Parallaxia](https://foundryvtt.com/packages/parallaxia/) - Permet de faire de la parallaxe
- [Permission Viewer](https://foundryvtt.com/packages/permission_viewer/) - Petit mais puissant indique avec des couleurs les permissions de chaque item/table/objets
- [Pings](https://foundryvtt.com/packages/pings/) - Permet déplacer les gens sur un ping ou encore montrer des choses sur la map
- [PopOut](https://foundryvtt.com/packages/popout/) - Permet de sortir des fiches de ta fenêtre navigateur
- [The Furnace](https://foundryvtt.com/packages/furnace/) - Gros module visant à améliorer la vie des Joueurs et MJ
- [Token Action HUD](https://foundryvtt.com/packages/token-action-hud/) Ce module remplit un HUD flottant, montrant les actions communes pour un jeton contrôlable.
- [Universal Battlemap Importer](https://foundryvtt.com/packages/dd-import/) Permet d'importer des fichiers d'export Dungeondraft et DungeonFog dans FoundryVTT, avec murs et lumières.