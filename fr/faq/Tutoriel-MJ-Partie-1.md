---
title: Tutoriel MJ - Partie 1
description: Une introduction de base à la configuration de Foundry VTT axée sur le MJ, fournissant toutes les étapes de départ nécessaires à la configuration d'un jeu pour vos joueurs. Traduit depuis https://foundryvtt.com/article/tutorial/
published: true
date: 2024-09-03T20:45:33.632Z
tags: tutorial, faq, foundryvtt, mj, tutoriel
editor: markdown
dateCreated: 2024-09-03T17:59:36.379Z
---

# Tutoriel - Maître du jeu, première partie

## Aperçu

Foundry Virtual Tabletop est une application puissante dotée de nombreuses fonctionnalités. Elle peut donc être difficile à utiliser pour un nouveau maître du jeu, même si vous avez déjà utilisé un logiciel de jeu de table virtuel. Cet article fournit une introduction de base à l'interface de Foundry VTT et vous donne toutes les étapes de démarrage nécessaires à la configuration d'un jeu pour vos joueurs. Cet article vous présentera :

-   **Le menu principal.** Tout le monde doit commencer quelque part, et c'est ici que chaque utilisateur commence avec Foundry VTT !
-   **Installation de systèmes de jeu.** Les systèmes de jeu contiennent toutes les règles et la tuyauterie nécessaire pour exécuter un jeu et stocker des informations à son sujet.
-   **Créer (ou installer) des mondes de jeu.** Les mondes de jeu utilisent un système de jeu pour disposer d'un endroit où toutes les données et informations peuvent vivre. Chaque monde de jeu est en fait une campagne autonome avec des scènes, des personnages, des objets et autres éléments similaires.
-   **Lancement des jeux.** Une fois qu'un système est installé et qu'un monde de jeu est créé, vous voudrez le démarrer pour voir ce qu'il contient.
-   **L'interface utilisateur de Foundry.** L'interface utilisateur de Foundry est assez simple, mais il est toujours utile de savoir de quoi on parle !

## Le menu principal

<table align="right" width="344">
      <tr>
        <td><img align="right" width="344" height="182" src="https://foundryvtt.wiki/dnd-modules/foundry-vtt-configuration-setup-2023-06-05.webp"></td>
    </tr>
    <tr>
        <td align="center"><em>Le menu de configuration et d'installation de Foundry Virtual Tabletop.</em>
        </td>
    </tr>
</table>
  <figure>

  <figcaption align="right">
  </figcaption>
</figure>

Le menu principal de Foundry VTT permet de gérer les mondes, les systèmes de jeu, les modules et divers paramètres de configuration du logiciel lui-même.

#### Mondes

Ce panneau est celui qui s'affiche par défaut lors du chargement de Foundry. Il contient tous les mondes de jeu que vous avez créés pour y exécuter des parties. Lorsque Foundry est lancé pour la première fois, cette liste sera vide, mais sera rapidement remplie de campagnes dans lesquelles vous et vos joueurs pourrez vous aventurer.

#### Systèmes

Ce panneau répertorie tous les systèmes de jeu actuellement installés et vous permet de les mettre à jour vers leurs versions les plus récentes si nécessaire. Les systèmes de jeu sont essentiels à la création de mondes de jeu et à l'exécution de sessions de jeu.

#### Modules

Ce panneau répertorie tous les modules actuellement installés et disponibles pour être utilisés dans un monde de jeu. Ce panneau vous permet également de mettre à jour les modules vers leur version la plus récente si nécessaire. Les modules ne font rien par eux-mêmes et doivent être chargés à partir d'un jeu en direct. Cette série de didacticiels ne passera pas en revue les modules car ils ne sont pas nécessaires à l'utilisation des différents ensembles d'outils de Foundry.

#### Configuration

Ce panneau répertorie différents paramètres de configuration que vous pouvez utiliser pour affiner le comportement de Foundry.

> Lorsque vous lancez Foundry Virtual Tabletop pour la première fois, il est recommandé d'accéder à l'onglet Configuration et de définir un **mot de passe administrateur**. Ce mot de passe est crypté et vous permet de sécuriser Foundry VTT pour empêcher l'accès au menu de configuration principal. Il est également nécessaire pour utiliser la fonction **Retour à l'accueil** à partir de la page de connexion de tout monde actuellement hébergé.
{.is-warning}

#### Mise à jour du logiciel

Ce panneau vous permet de voir la version actuelle de Foundry et si une version plus récente est disponible. Vous pouvez mettre à jour votre version de Foundry à partir de ce menu, et même basculer vers différentes branches de test, à condition qu'une soit actuellement disponible.

## Installation d'un système de jeu

!\[Le navigateur du système de jeu\](./Tutoriel - Maître du jeu, première partie \_ Foundry Virtual Tabletop\_files/the-game-system-browser-2023-06-05.webp)

Les systèmes de jeu peuvent être facilement installés à l'aide du navigateur.

Avant de pouvoir créer votre premier monde, vous devez d'abord installer un **système**. Les systèmes de jeu définissent les règles selon lesquelles votre monde fonctionne, qu'il s'agisse de l'un des ensembles de règles les plus courants d'un grand éditeur ou que vous préfériez utiliser quelque chose comme le système Simple WorldBuilding. Chaque monde est associé à un **système de jeu**. Sans système de jeu installé, il est impossible de créer un monde.

Foundry VTT fournit un système d'installation de packages disponible depuis l'écran de configuration pour l'installation des **mondes de jeu**, **des systèmes de jeu** et **des modules complémentaires**. Le bouton « Installer » en bas de chacun de ces onglets sur l'écran de navigation vous permettra d'installer le type de packages pour cet onglet.

#### Menu d'installation du système de jeu

1.  Depuis l' **écran de configuration,** accédez à l'onglet Systèmes de jeu
2.  Cliquez sur le bouton « **Installer un système de jeu**» en haut du menu
3.  Un navigateur pour l'installation de package apparaîtra vous permettant de voir tous les systèmes de jeu actuellement disponibles pour Foundry VTT
4.  Vous pouvez rechercher cette liste avec le champ de recherche ou filtrer la liste à l'aide des catégories de packages
5.  Une fois que vous avez choisi un système de jeu, cliquez sur le bouton d'installation à droite du nom du système et FVTT le téléchargera et l'installera pour vous

Les systèmes de jeu qui n'ont pas encore été officiellement publiés peuvent également être installés manuellement si vous disposez d'un lien vers le fichier `system.json`(appelé **URL du manifeste** ) pour ce système, en collant l'URL dans le champ URL du Manifeste et en cliquant sur le bouton Installer.

#### Mise à jour des systèmes de jeu

Pour rappel, il est conseillé de mettre à jour périodiquement les systèmes installés depuis l'onglet Système de Foundry VTT. Vous pouvez mettre à jour les systèmes de jeu individuellement en cliquant sur le bouton « Effectuer la mise à jour » associé dans l'entrée du système, ou utiliser le bouton « Tout mettre à jour » en haut de l'onglet pour vérifier si des mises à jour sont disponibles sur tous vos systèmes installés, qui seront ensuite automatiquement appliquées si une mise à jour est disponible.

#### À propos des modules

>Certains utilisateurs ont tendance à se précipiter pour installer des modules complémentaires sans avoir au préalable appris à utiliser les fonctionnalités de base de Foundry VTT. Bien que les modules offrent une grande variété de personnalisations et de modifications du fonctionnement des fonctionnalités de base de Foundry VTT, ils peuvent entraîner des problèmes de compatibilité et doivent être installés par petits incréments pour vous assurer d'être certain des fonctionnalités ajoutées par des modules et de celles qui font partie du logiciel de base.
{.is-warning}

N'oubliez jamais que la première étape pour résoudre tout problème que vous pourriez rencontrer dans Foundry VTT est de désactiver tous les modules.

## Créer un nouveau monde

!\[La feuille de configuration du monde\](./Tutoriel - Maître du jeu, première partie \_ Foundry Virtual Tabletop\_files/the-world-configuration-sheet-2023-06-05.webp)

La fiche de configuration du monde vous permet de spécifier les détails de votre monde de jeu et de les modifier ultérieurement.

Maintenant que vous disposez d'un **système de jeu**, accédez à l' onglet **Mondes**; à partir de là, vous créerez votre tout premier monde de jeu ! Cliquez sur le bouton « Créer un monde » de cet onglet pour afficher un menu de dialogue.

#### Modifier un monde existant

Le bouton **Editer le monde** peut être utilisé à tout moment après la création d'un monde, ce qui vous permet d'afficher à nouveau ce menu afin de modifier les détails de votre monde de jeu tels que la description, la prochaine session, l'image d'arrière-plan ou le titre ! Il peut également être utilisé pour **réinitialiser les modules actifs** ou **réinitialiser les clés d'accès utilisateur** sans avoir à lancer le monde au préalable. Si vous rencontrez des problèmes pour lancer votre monde ou vous connecter, modifiez simplement le monde et cochez la case appropriée pour réinitialiser les modules ou les clés d'accès utilisateur si nécessaire.

#### Le menu Créer un monde

**Nom du Monde**
Le nom que les joueurs verront lors de la connexion au jeu et la manière dont le monde sera désigné dans l'interface utilisateur.

**Chemin de données**
Le nom du dossier dans le dossier Worlds où vos données seront stockées. Comme il sera utilisé dans les URL Web, il **ne peut pas contenir d'espaces ni de caractères spéciaux**. Utilisez plutôt des tirets pour séparer plusieurs termes. (Exemple : **ma-premiere-campagne** est mieux que **« ma première campagne »** .)

**Système de jeu**
Les règles que le monde utilisera. Un système de jeu ne peut pas être modifié une fois que le monde est créé, alors assurez-vous d'utiliser le bon système.

**Image d'arrière-plan**
Une image que vous et vos joueurs verrez lorsque vous vous connecterez au Monde du jeu pour une session. Cette image sera étirée pour s'adapter à la fenêtre du navigateur, il est donc recommandé d'utiliser une image suffisamment grande pour fonctionner comme fond d'écran du bureau.

**Prochaine session**
Vous permet de définir la date et l'heure de la prochaine session de jeu. Ces informations seront visibles par tous les joueurs depuis l'écran de connexion de votre monde de jeu. Ceci est purement facultatif. La date et l'heure sont automatiquement localisées dans le fuseau horaire correct de l'utilisateur.

**Description du monde**
Fournit une description textuelle de votre monde, permettant des informations thématiques supplémentaires ou une brève description de votre environnement, des points de l'intrigue actuels ou d'autres informations que vos joueurs verront lors de la connexion.

Une fois que vous avez rempli ces champs à votre guise, cliquez sur Créer un monde pour finaliser votre monde de jeu !

## Installation d'un monde de jeu

Foundry VTT propose un certain nombre d'aventures et de campagnes déjà préparées avec tout ce dont vous avez besoin pour les exécuter avec vos joueurs. Similaires à l'installation des systèmes de jeu, ces mondes peuvent être installés à partir de l'onglet Mondes et installeront automatiquement tout le contenu, y compris le système de jeu pour vous.

#### Menu d'installation du monde du jeu

1.  Depuis l' **écran de configuration**, accédez à l'onglet Mondes
2.  Cliquez sur le bouton « **Installer un monde** » en haut du menu
3.  Un navigateur pour l'installation de package apparaîtra vous permettant de voir tous les mondes de jeu actuellement disponibles pour Foundry VTT
4.  Vous pouvez rechercher dans cette liste avec le champ de recherche ou filtrer la liste à l'aide des catégories de packages
5.  Une fois que vous avez choisi un monde de jeu, cliquez sur le bouton d'installation à droite du titre du monde et FVTT le téléchargera et l'installera pour vous.

Comme pour les systèmes de jeu, les mondes de jeu qui n'ont pas encore été officiellement publiés peuvent également être installés manuellement à l'aide du `world.json` à renseigner dans le champ URL du manifeste.

## Recherche des mondes, systèmes et modules installés

!\[Démo des packages de filtrage\](./Tutoriel - Maître du jeu, première partie \_ Foundry Virtual Tabletop\_files/filter-packages-demo-2023-06-05.webp)

Une vue filtrée de l'onglet des mondes du jeu.

Lorsque vous avez créé plusieurs mondes ou installé plusieurs systèmes et modules, il peut être difficile d'en trouver un en particulier. Pour accéder rapidement à un package spécifique, vous pouvez utiliser l'entrée Filtre en haut à gauche de chaque onglet. Commencez simplement à saisir le nom du package que vous recherchez et la liste se filtrera pour afficher uniquement les éléments qui correspondent. S'il n'y a aucun package correspondant dans cet onglet, vous verrez un message « Aucun package ne correspond à votre recherche.» et vous souhaiterez peut-être lancer l'installateur de package pour le télécharger.

## Lancez votre nouveau monde

!\[Mondes de jeu - Écran de connexion utilisateur\](./Tutoriel - Maître du jeu, première partie \_ Foundry Virtual Tabletop\_files/game-worlds-user-login-screen-2023-06-05.webp)

L'écran de connexion au monde vous permet de vous connecter à un monde de jeu actif ou de revenir à la configuration.

Une fois votre monde de jeu créé, cliquez simplement sur le bouton « Lancer » (bouton "Play" sur la vignette du monde) pour lancer le monde et vous amener à l'écran de connexion du jeu. À partir de là, vous pouvez sélectionner le nom d'utilisateur pour vous connecter (par défaut, tous les mondes commencent avec un seul compte Gamemaster sans mot de passe) ou revenir au menu de configuration.

> **Retour à la configuration :** si vous souhaitez revenir aux menus de configuration à partir de la page de connexion, vous pouvez cliquer sur le bouton **Retour à l'accueil** pour fermer le monde et revenir aux menu principal de configuration de Foundry VTT. Si vous avez défini un **mot de passe administrateur** pour votre logiciel, vous devrez le fournir pour utiliser cette option.
{.is-info}

**Sélectionnez le joueur Gamemaster et connectez-vous.**

## Introduction à l'interface utilisateur

!\[Carte de l'interface utilisateur GM\](./Tutoriel - Maître du jeu, première partie \_ Foundry Virtual Tabletop\_files/gm-ui-map-2021-01-27.jpg)

Cette image de l'interface utilisateur décrit les principaux éléments d'interface que vous rencontrerez pour la première fois dans un monde nouvellement créé.

Une fois que vous avez rejoint la session de jeu, vous verrez l'interface utilisateur principale que vous et vos joueurs utiliserez pour planifier et jouer à des jeux dans FVTT. L'image ci-dessous détaille les principaux éléments.

**Barre de navigation de la scène**
Utilisée pour basculer entre les scènes actuellement disponibles.

**Onglets de la barre latérale**
Utilisés pour accéder aux données des différents documents stockés dans votre monde.

<details><summary>Détails des barres latérales des répertoires</summary>

**Messages du tchat** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/comments.svg" width=16>)  
Cet onglet affiche [les messages de discussion](https://foundryvtt.com/article/chat/) et les résultats des lancers de dés et permet aux utilisateurs d'envoyer leurs propres messages.

**Rencontres de combat** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/swords.svg" width=16>)  
Cet onglet affiche toutes [les rencontres de combat](https://foundryvtt.com/article/combat/) actuellement actives , indiquant l'initiative et l'ordre du combat.

**Acteurs** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/users.svg" width=16>)  
Cet onglet contient [les acteurs](https://foundryvtt.com/article/actors/) que les joueurs utiliseront pour suivre leurs personnages.

**Objets** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/suitcase.svg" width=16>)  
Cet onglet stocke les fiches d'informations sur [les objets](https://foundryvtt.com/article/items/), qui peuvent être n'importe quoi, des armes et armures aux sorts et capacités.

**Journaux** (<img src = "https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/book.svg" width=16>)  
Cet onglet stocke [les entrées de journal](https://foundryvtt.com/article/journal/) et les pages de journal qui contiennent des informations et des traditions que les joueurs peuvent lire et modifier.

**Tables aléatoires** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/table-list.svg" width=16>)  
Cet onglet contient [les tables aléatoires](https://foundryvtt.com/article/roll-tables/) qui peuvent être utilisés pour déterminer des résultats aléatoires à partir d'une liste de résultats.

**Playlists** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/music.svg" width=16>)  
Cet onglet donne accès aux paramètres de volume globaux et au contrôleur des [Playlists](https://foundryvtt.com/article/playlists/) qui affiche l'audio en cours de lecture.

**Ensemble de cartes** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/cards.svg" width=16>)  
Cet onglet donne accès à la barre latérale des [Cartes](https://foundryvtt.com/article/cards/) , qui stocke les jeux de cartes, les mains et les pioches dont les utilisateurs auront besoin pour utiliser les cartes.

**Compendiums** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/book-atlas.svg" width=16>)  
Donne accès à la barre latérale des [Compendiums](https://foundryvtt.com/article/compendium/) , qui stocke les documents qui ne sont pas réellement nécessaires. La plupart des joueurs n'auront pas besoin d'accéder à cet onglet.

**Paramètres** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/gears.svg" width=16>)  
Ouvre la barre latérale [Paramètres du jeu](https://foundryvtt.com/article/settings/) , qui permet la configuration ou la personnalisation de votre expérience Foundry VTT.

**Exporter le contenu du tchat** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/floppy-disk.svg" width=16>)  
Ce bouton est utilisé pour enregistrer une copie de tous les messages qui apparaissent actuellement dans votre journal de tchat dans un fichier texte brut.

**Effacer le tchat** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/trash.svg" width=16>)
Ce bouton supprime tous les messages du journal de tchat.
</details>


**Indicateur de pause de jeu**
Lorsque le jeu est en pause, une horloge tournante apparaît. En pause, les joueurs ne peuvent pas déplacer leurs jetons ni manipuler les portes.

**Outils de contrôles**
Utilisés pour changer d'outil sur vos scènes afin de contrôler les différents objets placés sur le canevas. Chacun des icônes de contrôle fournit différents outils. Les outils de contrôle disponibles et que les joueurs peuvent utiliser incluent :


<details><summary>Détail des outils de contrôle</summary> 

  **Outils de token** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/user.svg" width=16>)
Contient tous les outils nécessaires pour sélectionner et contrôler [les tokens](https://foundryvtt.com/article/actors/) .

**Outils de gabarit** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/ruler-combined.svg" width=16>)
Contient les outils nécessaires à l'utilisation [des mesures et des gabarits](https://foundryvtt.com/article/measurement/) .

**Outils de tuile** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/cubes.svg" width=16>)
Outils de création, d'édition et de gestion [des tuiles](https://foundryvtt.com/article/tiles/) .

**Outils de dessin** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/pencil.svg" width=16>)
Outils nécessaires à la création, à l'édition et à la gestion [des dessins](https://foundryvtt.com/article/drawings/) .

**Outils de mur** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/landmark.svg" width=16>)
Outils nécessaires à la création, à la modification et à la gestion [des murs](https://foundryvtt.com/article/walls/) .

**Outils de lumière** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/lightbulb.svg" width=16>)
Outils nécessaires à la création, à l'édition et à la gestion [de l'éclairage](https://foundryvtt.com/article/lighting/) .

**Outils du son d'ambiance** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/volume-low.svg" width=16>)
Outils nécessaires à la création, l'édition et la gestion [des sons d'ambiance](https://foundryvtt.com/article/ambient-sound/) .

**Notes** (<img src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/bookmark.svg" width=16>)
Outils nécessaires à la création, à la modification et à la gestion [des entrées de journaux](https://foundryvtt.com/article/journal/) .
</details>


#### Conclusion de la partie 1

Ceci conclut la première partie du didacticiel de démarrage !

-   Continuer vers [le didacticiel - Maître du jeu, deuxième partie](https://foundryvtt.com/article/tutorial-two/) .
-   Référez vos joueurs au [Tutoriel - Orientation du joueur](https://foundryvtt.com/article/player-orientation/) .