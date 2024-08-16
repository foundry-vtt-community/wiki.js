---
title: Starwars FFG
description: Support pour Starwars FFG
published: true
date: 2024-08-16T16:28:38.047Z
tags: 
editor: markdown
dateCreated: 2020-10-19T15:58:58.380Z
---

![logo SWFFG](https://gamersplane.com/images/logos/starwarsffg.png) 
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

Merci à toute la team **StarWarsFoundryVTT**

## Créer un monde

Maintenant que le système est installé, vous devez créer un monde.
Retournez à la page Mondes de jeu, cliquez sur Créer un monde, et assurez-vous de sélectionner **Star Wars FFG** dans le menu déroulant Système de jeu.

![créer un monde](https://camo.githubusercontent.com/b57d56ea3c99e9f5238e5c301313e3dbfa65905690ef1cb4c9148670232984b9/68747470733a2f2f692e696d6775722e636f6d2f63566561506f732e706e67)

## Importer des données

Comme le contenu de FFG Star Wars est protégé par des droits d'auteur, vous devrez remplir le contenu vous-même. Pour vous aider dans cette tâche, les développeurs ont créé plusieurs outils qui vous permettent d'importer des données à partir d'autres outils plus courants.

###### Importer des données OggDude

Pour importer la grande majorité des données d'articles, vous pouvez utiliser l'importateur "OggDude Dataset Importer".

- **Sur votre ordinateur**; Créez un zip du dossier Data de votre générateur de personnages OggDude (photo ci-dessous)
- **Dans votre monde**; Sélectionnez Paramètres du jeu
- Cliquez sur Configurer les paramètres
- Sélectionnez les paramètres du système
- Cliquez sur OggDude Dataset Importer
- Sélectionnez votre fichier de données zippé à l'aide du sélecteur de fichiers "Choisir un fichier"
- Cliquez sur Charger le fichier (important cela retire la bloquage des checkbox)
- Sélectionnez les types de données que vous souhaitez importer (cliquez sur les checkbox)
- Cliquez sur Démarrer l'importation en bas de la fenêtre

Les données qui en résultent sont ajoutées aux Compendiums.

![Le fichier de données zippé](https://camo.githubusercontent.com/92de8c3650a611d5848347b43ea6e7322f045ad1e49c5147a33b39b8039d3896/68747470733a2f2f692e696d6775722e636f6d2f726651504a73732e706e67)

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
- [Dice So Nice](https://foundryvtt.com/packages/dice-so-nice/) - Permet d'ajouter des animations 3D lors des jets de dés.
- [FXMaster](https://foundryvtt.com/packages/fxmaster/) Permet un excellent éventail d'effets divers.
- [JB2A - Jules&Ben's Animated Assets](https://foundryvtt.com/packages/JB2A_DnD5e/) Un module au départ pour DnD mais qui permet des effets d'arme et de sort utilisable dans Star Wars.
- [Multilevel token](https://foundryvtt.com/packages/multilevel-tokens/) - Ce module est intégré à la V12 dès à présent (https://foundryvtt.com/article/scene-regions/).
-- [Easy Regions](https://foundryvtt.com/article/scene-regions/) - Permet de faciliter l'utilisation de Scene Region (V12).
-- [Region Enchantment](https://foundryvtt.com/packages/regionenchantment) - Quelques aides supplémentaires pour l'utilisation de Scene Region (V12)
- [Tile Scroll](https://foundryvtt.com/packages/tile-scroll/) - Permet de faire de la parallaxe (*l'ancien module parallaxe n'est plus compatible FoundryVTT depuis la version 10*).
- [Permission Viewer](https://foundryvtt.com/packages/permission_viewer/) - Petit mais puissant indique avec des couleurs les permissions de chaque item/table/objets
- [Pings](https://foundryvtt.com/packages/pings/) - Permet déplacer les gens sur un ping ou encore montrer des choses sur la map
- [PopOut](https://foundryvtt.com/packages/popout/) - Permet de sortir des fiches de ta fenêtre navigateur
- [The Furnace](https://foundryvtt.com/packages/furnace/) - Gros module visant à améliorer la vie des Joueurs et MJ
- [Token Action HUD](https://foundryvtt.com/packages/token-action-hud/) - Ce module remplit un HUD flottant, montrant les actions communes pour un jeton contrôlable.
- [Universal Battlemap Importer](https://foundryvtt.com/packages/dd-import/) - Permet d'importer des fichiers d'export Dungeondraft et DungeonFog dans FoundryVTT, avec murs et lumières.
- [Smart Target](https://foundryvtt.com/packages/smarttarget/) ou [Easy Target](https://foundryvtt.com/packages/easy-target) - Permettent de cibler des PJ/PNJ très facilement. (raccourcis).
- [FFG Star Wars Enhancements](https://foundryvtt.com/packages/ffg-star-wars-enhancements/) - Excellent complément au système Starwars FFG (Intro, Datapad, Transitions en hyper-espace, animations des attaques, etc.)
- [Star Wars - Silhouette](https://foundryvtt.com/packages/starwars-silhouette) - Permet de personnaliser la silhouette de vos vaiseaux (options d'import/conversion en webp des images oggDude en prime).

-- ***Du côté des modules  "Interfaces utilisateurs":***
- [FFG Star Wars Space Opera Ui](https://foundryvtt.com/packages/space-op-ui/) - Une interface Star Wars basé sur (LCARS UI de Startrek Adventures)
- [Star Wars - User Interface Creative Common](https://foundryvtt.com/packages/swffgUI-cc/) - Un module d'interface avec de multiples options (Police de caractère Star Wars, Animations dans les fiches, différents thèmes pour vos campagnes, etc.)
