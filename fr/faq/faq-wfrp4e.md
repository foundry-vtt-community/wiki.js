---
title: FAQ Warhammer 4 sous FoundryVTT
description: 
published: true
date: 2020-10-23T07:34:03.142Z
tags: 
editor: markdown
dateCreated: 2020-10-20T16:18:43.314Z
---

## Qu’est ce que FoundryVTT?

Bon ben dans ce cas, c’est la mauvaise page. [Va là](/fr/faq/faq-main).

## J’ai installé Foundry. Que faut-il de plus pour avoir Warhammer 4 en Français ?

Il faut installer :
- La traduction française du « core » de Foundry (https://gitlab.com/baktov.sugar/foundryvtt-lang-fr-fr/raw/master/fr-FR/module.json )
- Le système Warhammer v4 (https://raw.githubusercontent.com/CatoThe1stElder/WFRP-4th-Edition-FoundryVTT/stable/system.json)
- Le module Babele (https://gitlab.com/riccisi/foundryvtt-babele/raw/master/module/module.json)
- Mon module de traduction ( https://gitlab.com/LeRatierBretonnien/foundryvtt-wh4-lang-fr-fr )

Ces modules sont officiels, il suffit de les chercher dans l’interface de configuration de Foundry.

## Est-ce qu’il y a d’autres modules utiles ?

A titre personnel, voici ce que j’utilise :

- AboutTime + Calendar/Weather : cela permet d’avoir le calendrier impérial. A saisir à la main pour l’instant, car pas de possibilités d’import/export.
- Pings : Pour se « pinguer » sur les lieux et les cartes (ie indiquer une position aux joueurs ou au MJ).
- Furnace : Je l’utilise pas spécialement, en fait, mais le module « GM Toolkit » ci-dessous en a besoin.
- GM Toolkit (https://github.com/Jagusti/fvtt-wfrp4e-gmtoolkit) : Etend et améliore l’affichage des tokens et fournis des macros très utiles.
- DungeonDraft Importer : Je suis un grand fan de l’outil DungeonDraft, et ce module permet d’importer directement les plans, en gardant les métadatas, c’est à dire les murs, portes, éclairages, etc. Génial !
- FXMaster : Pour ajouter quelques effets sympathiques sur les cartes (nuages, pluie, neige, etc). Pas indispensable, honnêtement, mais fait son petit effet.

## Qu’est-ce qu’apporte le système Warhammer 4?

Outre la gestion des feuilles de personnage et tout les mécanismes de base de FoundryVTT, le système développé par MooMan offre des automatisations très confortables :

- commande `/table <nom_de_table>` pour effectuer un tirage sur une des tables pré-enregistrées (critiques, maladies, destinée, etc, etc. Tapez tout simplement /table pou en avoir la liste,
- commande `/cond <conditions>` pour avoir le détail d’un état (assomé, en flammes, …),
- commande  `/pay <somme>` pour réclamer de l’argent aux joueurs (/pay pour avoir la syntaxe),
- commande `/avail <disponibilité>` pour tirer aléatoirement la disponibilité d’un objet (/avail pour avoir la syntaxe),
- commande `/help` pour obtenir l’aide intégrée des commandes disponibles,
- commande `/auberge` pour générer des menus d'auberges,
- gestion des points de fortune et des sombres pactes : un clique droit sur la fiche d’un d’un joueur lui permet de dérouler un sous-menu affichant les options possibles,
- gestion de l’initiative selon les 3 modes proposés par le livre de règle,
- gestion intégrale de tout les atouts et défauts d’armes,
- gestion semi-automatique des avantages en combat,
- gestion automatique des dommages, blessures, points d’armure et localisation,
- gestion complète des tests opposés en combat !,
- gestion automatique des capacités de certains talents,
- compendium complet des carrières, traits, objets, sorts, prières, compétences, talents, blessures et critiques, avec naturellement la possibilité de créer les vôtres,
- génération automatique de PNJ/Créatures,
- assistance à la création de personnages,
- gestion des munitions des armes de tir,
- gestion automatique des nuits de repos,
- et j’en oublie probablement !

## J'ai l'impression qu'il ny a plus de données/compendiums ?

Oui, depuis la mise en légalité du système, il n'y a plus de distribution des compendiums dans le système à partir des versions supérieures à 2.X. Il est possible de rester en 1.6.X, avec les compendiums en cherchant sur le github de MooMan. 
Cependant, un module complet et produit par C7 avec tout le contenu verrra le jour d'ici quelques temps, mettant à nouveau toutes les données/compendiums dans le système.
 
## OK, super, mais comment j’apprends à me servir de tout ça ?

Je vous conseille fortement de prendre 20 minutes de votre temps pour regarder les 3 vidéos de MooMan concernant le système : https://www.youtube.com/watch?v=-CthIoE9o2E , https://www.youtube.com/watch?v=XMEJt5OB4Bc et https://youtu.be/HMjXCLDDfWE

Citons également cette vidéo : https://www.youtube.com/watch?v=LnzWaDKNUPE

Oui, je sais, ces vidéos sont en Anglais, mais l’auteur fait un effort pour parler très lentement et distinctement. Je vous recommande d’y jeter un oeil 🙂

La lecture du Wiki du système peut également donner des précisions bien utiles : https://github.com/CatoThe1stElder/WFRP-4th-Edition-FoundryVTT/wiki et https://github.com/CatoThe1stElder/WFRP-4th-Edition-FoundryVTT/wiki/FAQ

Si vous avez un souci ensuite, rendez-vous sur le channel #wfrp du Discord de Foundry. De nombreux utilisateurs – y compris francophones – sauront probablement vous répondre.

## Comment je mets une carrière/objet/compétences/talents/… à un personnage ?

La quasi-totalité des opérations fonctionnent par glisser-déplacer : si vous voulez par exemple placer une carrière sur un personnage, ouvre le compendium « Carrières », sélectionnez la carrière/niveau, puis faites la glisser sur la fiche. Hop ! Idem pour les objets, sorts, prières, etc.

## Comment obtenir le calendrier impérial ?

Il faut installer les modules AboutTime et Calendar/Weather : ces sont des modules officiels, il suffit de les chercher dans l’interface d’installation des modules. Une fois installé, activez les dans votre monde, puis allez éditer les paramètres des modules. Dans la section AboutTime, sélctionnez « Warhammer » dans l’item « Active calendar ». Ensuite, dans le widget du calendrier, cliquez sur la date pour ouvrir la fenêtre de configuration. Cliquez ensuite sur le bouton « Charger le calendrier par défaut » afin de charger le calendrier impérial !

## Comment faire un combat ?

Les vidéos de démonstration listées plus-haut peuvent vous être utiles, mais voici quelques étapes :

- Placez les tokens sur la carte, sélectionnez les puis placez les tous en « combat » (clique sur le « bouclier » des icones à côté des tokens)
- l’ensemble des acteurs apparaissent alors dans le « combat tracker » dans la barre de contrôle à droite. Cliquez sur « Démarrez le combat »
- les joueurs peuvent venir alors tirer leur initiative en cliquant sur le dé affiché. Faites de même pour les PNJs. Sélectionnez le premier à agir, si ce n’est pas fait automatiquement.
- Pour réaliser une action d’attaque, le joueur doit cibler un token : clique droit sur un token adverse, puis selection de l’icône « cible ». Le joueur peut alors faire attaquer son personnage, en général en cliquant sur l’arme choisie, dans l’onglet « Combat (et pas sur la compétence). Je fois le jet réalisé, le MJ peut faire le test opposé du défenseur en cliquant par exemple soit sur sont arme (parade), soit sur esquive. Comme la cible a été selectionnée, le test opposé est automatiquement résolu. Si des dommages sont à appliquer, il suffit de cliquer avec le bouton droit sur le résultat du test opposé pour faire apparaître le menu des dommages.
- Dans le cas d’un PNJ attaquant un PJ, il suffit d’appliquer le principe ci-dessus, en inversant les rôles.