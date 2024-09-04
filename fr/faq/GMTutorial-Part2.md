---
title: Tutoriel MJ - Partie 2
description: Tutoriel - MJ Partie 2 · Présentation · Configuration des utilisateurs · Création d'objets · Création d'une scène · Inviter des joueurs · Rencontres de combat.
published: true
date: 2024-09-04T07:48:08.856Z
tags: faq, foundryvtt, mj, tutoriel
editor: markdown
dateCreated: 2024-09-04T07:47:43.999Z
---

## Aperçu

Maintenant que votre monde est configuré, vous allez vouloir configurer des comptes utilisateurs pour vos joueurs, leurs personnages et les scènes nécessaires pour qu'ils puissent jouer ! Il est temps de vous fournir une introduction de base aux aspects fondamentaux du gameplay de Foundry VTT et de vous donner les prochaines étapes pour configurer vos joueurs. Cet article vous présentera :

-   **Utilisateurs.** Toute personne qui se connecte à une session Foundry Virtual Tabletop est un utilisateur et peut être un joueur ou un maître de jeu. Les joueurs de votre jeu auront le contrôle des acteurs, tandis que les utilisateurs qui sont des maîtres de jeu ou des assistants peuvent passer leur temps à créer des scènes et à gérer le jeu pour les joueurs. Tous les utilisateurs ont un rôle qui détermine ce qu'ils peuvent voir et faire dans Foundry. Vous trouverez plus d'informations à ce sujet dans l' article [Utilisateurs et autorisations .](https://foundryvtt.com/article/users/ "Utilisateurs et autorisations")
-   **Acteurs.** Les protagonistes, alliés, monstres, antagonistes et personnes au sein du monde que vous créez. Le système de jeu utilisé a le contrôle pour définir les types exacts d'acteurs qui sont pertinents pour son gameplay, mais pour la plupart des systèmes de jeu de rôle sur table, ces acteurs représenteront les personnages que le joueur contrôle et rencontre.
-   **Objets.** Similaires aux acteurs, dans le sens où ils ont leurs propres statistiques et caractéristiques, mais peuvent être associés ou utilisés par des acteurs dans le jeu. Les objets peuvent représenter n'importe quel objet physique ou métaphysique, des dagues et des rochers à la confiance d'un noble allié.
-   **Scènes.** Zones que les joueurs peuvent explorer et qui sont généralement composées d'images de cartes utilisées comme arrière-plans, ou de nombreuses parties si elles sont construites à partir de [tuiles](https://foundryvtt.com/article/tiles/ "Carrelage") . Les scènes peuvent représenter n'importe quoi, des cartes du monde ou régionales aux petits bâtiments ou donjons. Les scènes peuvent contenir [des jetons](https://foundryvtt.com/article/tokens/ "Jetons") , [des éclairages](https://foundryvtt.com/article/lighting/ "Éclairage") , [des murs](https://foundryvtt.com/article/walls/ "Murs") et [des sons ambiants](https://foundryvtt.com/article/ambient-sound/ "Sons ambiants") . Pour en savoir plus sur ce type de document clé, visitez la page [Scènes](https://foundryvtt.com/article/scenes/ "Scènes") .
-   **Rencontres.** Tout combat, course, puzzle ou autre événement où un groupe de personnes effectue à tour de rôle des actions dans le jeu peut être considéré comme une rencontre. Les rencontres sont liées à une scène spécifique, généralement la scène actuellement active et visualisée au moment de la création de la rencontre.

## Configuration des utilisateurs

[![Mise en route – Gestion des utilisateurs](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-user-management-2021-01-13.jpg)](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-user-management-2021-01-13.jpg)

L'écran de gestion des utilisateurs vous permet de créer, de modifier et de supprimer des comptes d'utilisateurs de votre monde de jeu.

Avant que les joueurs puissent rejoindre une session, vous devrez configurer des noms d'utilisateur et (éventuellement) des clés d'accès pour eux. Pour ce faire, cliquez sur l'icône Paramètres du jeu (icône à trois engrenages) dans la barre latérale droite, puis cliquez sur Configurer les joueurs. À partir de là, vous devriez voir l'utilisateur maître du jeu et un bouton indiquant « Créer un utilisateur supplémentaire ». En cliquant sur ce bouton, Foundry créera un nouvel emplacement d'utilisateur générique, généralement nommé « Joueur 2 » ou similaire.

Vous pouvez ensuite renommer l'utilisateur, lui attribuer un mot de passe et définir son niveau d'autorisation. Dans le cadre de ce guide, il n'est pas nécessaire d'attribuer aux joueurs un rôle autre que celui de « Joueur ». Gardez également à l'esprit que les mots de passe des utilisateurs ne sont pas sécurisés par cryptographie et ne doivent pas être réutilisés à partir d'autres comptes ou services importants que vous ou vos joueurs utilisez.

Une fois que tous les comptes de joueurs nécessaires ont été créés, cliquez sur « Enregistrer et revenir » et vous serez renvoyé vers votre jeu. Les joueurs peuvent désormais rejoindre et se connecter avec les identifiants que vous leur avez fournis. Il reste cependant un peu de configuration à faire avant de pouvoir jouer.

### Créer et affecter des acteurs

[![Premiers pas – Autorisations des acteurs](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-actor-permissions-2021-01-13.jpg)](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-actor-permissions-2021-01-13.jpg "Voir une image plus grande")

La fenêtre Autorisations de l'acteur vous permet de déterminer qui a le contrôle d'un acteur et de ses données.

Les acteurs sont les créatures, les joueurs, les ennemis et les alliés que vous et vos joueurs utiliserez pour jouer au jeu. Ils contiennent des données sur les capacités, les attributs, les pouvoirs, etc. Pour créer un nouvel acteur, visitez le répertoire des acteurs dans la barre latérale en cliquant sur l'icône qui ressemble à un groupe de personnes.

En haut de cet onglet de répertoire, vous pouvez créer un nouvel acteur. Selon le système que vous avez choisi, vous pouvez avoir plusieurs options ici. Cependant, pour la plupart des jeux, les joueurs seront étiquetés comme « personnage joueur » et c'est ce que vous voulez.

Une fois votre acteur nommé et créé, sa fiche de personnage devrait s'ouvrir, vous permettant de le configurer selon vos besoins, et il apparaîtra également dans le répertoire des acteurs à droite. Une fois que vous avez terminé de modifier l'acteur, vous pouvez fermer la fenêtre et les modifications seront enregistrées. Vous pouvez toujours double-cliquer sur un acteur dans la barre latérale pour rouvrir la fiche et le modifier davantage si vous en avez besoin.

Une fois que vous avez créé un acteur, vous devez l'assigner à un joueur ! Faites un clic droit sur un acteur que vous avez créé et sélectionnez « Configurer les autorisations ».

Dans le menu qui apparaît, vous pouvez choisir comment tous les joueurs interagissent avec l'acteur (par défaut, ils ne peuvent pas le faire) et sélectionner comment les utilisateurs individuels interagissent avec l'acteur.

Attribuer à un utilisateur le niveau d'autorisation « propriétaire » lui permet de modifier la fiche de personnage et de le contrôler entièrement dans les scènes. Il s'agit du niveau par défaut que vous souhaiterez pour la plupart des personnages joueurs. Tout niveau inférieur (comme limité ou observateur) leur fera perdre la possibilité de contrôler directement les acteurs qui leur sont assignés.

## Créer des éléments

La création d'éléments est très similaire à la création d'acteurs, et les deux sont étonnamment similaires : ils contiennent des informations sur un document, ce qu'il peut faire, des illustrations, etc. Les éléments, cependant, sont destinés à être attachés à des acteurs et à être utilisés par eux.

Pour créer un objet, sélectionnez le **répertoire Objets** (il se trouve à droite du répertoire Acteurs) et en haut de la barre latérale, cliquez sur « **Créer un objet ».** De la même manière que pour créer un personnage, vous devez fournir un nom. Selon le système de jeu que vous utilisez, vous pourrez également déterminer le type spécifique d'objet que vous créez. Par exemple, le système DnD5e vous permet de choisir entre des armes, des outils et des consommables parmi plusieurs autres types d'objets.

Une fois que vous avez nommé et créé votre objet, le menu de configuration de l'objet s'ouvre. À partir de là, vous rédigez une description de votre objet et vous lui appliquez des fonctionnalités en fonction de ce que votre système de jeu propose. Lorsque vous êtes satisfait de votre objet, vous pouvez fermer le menu de l'objet, car les modifications sont automatiquement enregistrées. Si vous avez besoin de modifier un objet, vous pouvez cliquer sur le nom de l'objet pour rouvrir la fiche de l'objet.

#### Offrir des objets aux acteurs

Pour donner un objet à un acteur, ouvrez simplement la fiche de l'acteur et faites glisser l'objet choisi depuis le répertoire des objets vers la fiche. L'objet sera alors ajouté à l'inventaire de l'acteur. La plupart des systèmes de jeu ont un onglet d'équipement ou d'inventaire sur les fiches d'acteur, bien que cela puisse varier en fonction de la façon dont le système de jeu gère les objets. Par exemple, un objet classé comme un sort peut être ajouté à la liste des sorts d'un acteur au lieu de son inventaire.

## Créer une scène

[![Premiers pas – Créer une scène](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-making-a-scene-2021-01-15.jpg)](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-making-a-scene-2021-01-15.jpg "Voir une image plus grande")

Ceci est un exemple du strict minimum nécessaire pour créer une scène dans Foundry VTT.

Pour créer une nouvelle scène :

1.  Ouvrez le répertoire des scènes (l'icône de la carte, 3ème à partir de la gauche) dans la barre latérale sur le côté droit de l'écran.
2.  En haut de la barre latérale, cliquez sur **Créer une nouvelle scène** , vous serez invité à donner un nom à la scène, puis cliquer sur Créer une scène ouvrira le menu de configuration de la scène.

Si vous n'avez pas d'image de carte prête, nous vous en avons fourni une ici sur [DeviantArt](https://www.deviantart.com/foundryatropos/art/Foundry-Tavern-at-Night-746759206) . Cette image a les dimensions suivantes : 3150x2800px.

Une fois que vous avez défini l'image d'arrière-plan et que vous vous êtes assuré que les dimensions correspondent à l'image que vous utilisez, vous pouvez enregistrer les modifications et Foundry VTT créera votre scène pour vous.

#### Naviguer dans les scènes

Une fois que vous avez créé une scène, vous devez être capable d'effectuer des opérations de navigation de base. Pour déplacer votre vue de la carte, cliquez et faites glisser le bouton droit de la souris pour déplacer la scène. Vous pouvez également utiliser `Control (CMD for macOS) + Arrow Keys`pour déplacer votre vue. Pour effectuer un zoom avant ou arrière, utilisez simplement la molette de votre souris ou les `page up/down`touches. Vous pouvez appuyer sur Tab pour déplacer votre vue vers le point de départ de la scène. Comme nous n'avons pas défini de coordonnées spécifiques pour cela, la valeur par défaut sera le coin supérieur gauche de l'image.

#### Organiser les scènes

You also have the option to create a folder for organizing scenes into groups, which you can do at any time by creating a folder in the scene directory and dragging the scene to the desired folder.

#### Changing Scenes

When you have multiple scenes, you can either click on the scene's name on the top of the Foundry VTT screen, which will shift your view to the chosen scene without activating it, or you can right click on the scene and activate it, which will pull all connected players to the scene, including you. You can do the same from the scene directory by right clicking any scene there and choosing view (to simply look at the scene by yourself) or to activate it.

### Creating Walls

[![Tutoriel Scène de taverne](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/tutorial-tavern-scene-2020-05-24.png)](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/tutorial-tavern-scene-2020-05-24.png "Voir une image plus grande")

This is an example of the provided tavern map with walls and doors placed.

Now that you have a scene, you will want to place walls to determine how light spreads, and the confines of rooms and terrain. By default, all new scenes have vision enabled, meaning that actors with vision will only be able to see the map if it is lit, and their line of sight allows it. Most of the time player character actors will have vision enabled. Walls are a way to determine how much of an area an actor with vision can see.

Creating walls is as easy as selecting the wall control tool and dragging out lines to denote a wall that blocks sight. While there are multiple types of walls, which all do slightly different things, the two most common walls you'll use are standard walls and doors.

If you're using the tavern map provided above, you can use the image example as a guide for how the building should be walled in.

You can learn more about walls in the [Walls](https://foundryvtt.com/article/walls/ "Murs") article.

### Creating Lights

Lights are necessary for actors to see things, unless the scene has the Global Illumination tag set, in which case the entire scene will be brightly lit. Lights are especially important when the map's darkness level increases, and visibility drops. You can adjust the map's light and darkness level by right clicking on the map in the scene directory and choosing configure, or by going to the lighting controls panel on the left and using the transition to daylight or darkness buttons.

Lights will interact with walls, creating shadows and sections of darkness where the light can't reach. Creating a light is as simple as clicking on a point in the scene and dragging out the light's radius. The two rings visible as the light's being created shows the inner bright radius, and the outer dim radius. Once you have created a light you can double click on it to change its various properties including the color, and specific radii of the bright and dim rings. You can also right click on the light to quickly turn a light on or off without removing it.

You can learn more about lights in the [Lighting](https://foundryvtt.com/article/lighting/ "Éclairage") article.

### Placing Actors in a Scene

[![Mise en route – Placement de jetons (webp)](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-placing-tokens-webp-2021-01-19.webp)](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-placing-tokens-webp-2021-01-19.webp "Voir une image plus grande")

Actors can be easily placed into a scene by dragging them from the Actors Directory. (Click to enlarge)

Once you have created and configured a Scene, gotten used to moving your view, and have put down walls and lights, you'll want to bring one or more actors into the scene. To do this, select the Actors Directory icon to the right of the scenes tab icon, then click and drag an actor from the directory to the active scene. This will place a token representing the actor into the scene, and its owner will be able to see the scene through that token's vision (if it has vision).

As a Gamemaster you can view what any token can see by clicking on it.

You can learn more about actors in the [Actors](https://foundryvtt.com/article/actors/ "Acteurs") article, and more about tokens in the [Tokens](https://foundryvtt.com/article/tokens/ "Jetons") article.

#### Ways to move tokens

Moving a token can be done in several ways, depending on what you and your players need, and what is easiest for you:

-   **Drag Movement** \- Click and drag a token to move it in a straight line along the path to a new location. As soon as you release the token Foundry will execute the move. It should be noted that as a gamemaster you can drag tokens through walls and other obstructions, but players cannot.
-   **Incremental Movement** - The arrow or WASD keys can be used to move a controlled actor around a scene. This is especially useful for doing small, incremental moves in tight spaces where walls and terrain might block movement. You can also hold down Shift while using these movement keys to quickly rotate an actor to a particular facing without changing their position on the map.
-   **Measured (Waypoint) Movement** \- The measurement tools can be used to to move tokens. This is done quickly holding down the control (CMD for macOS) key, then clicking and dragging in a direction from a token you control. This will show you a path, and distance to the final location. You can left click in additional places along the route to place waypoints the token will follow. To move a token using this method, simply press the spacebar, and Foundry will move the token along the line to the final measured point. You can do the same thing with the measure distance tool if the start of the measurement is on a token you control.

You can learn more about Foundry VTT's control setup in the [Game Controls](https://foundryvtt.com/article/controls/ "Commandes de jeu") article.

## Inviting Players

![Liens d'invitation au jeu - Fermé](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/game-invitation-links-closed-2021-12-15.jpg)

The Game Invitation Links Application allows you to conveniently invite your players to your game.

Once your users, actors, items and scenes have been created, you're going to need players. Inviting players to your game is very simple: from the right sidebar click on the Game Settings tab (the gear icon) and then click on Invitation Links. This will pop up a window with two IP Addresses. The first one is for users that are on the same local network as your copy of Foundry, and the second ip is your public IP for outside internet connections. In both cases, users can paste these links into a web browser and join your game.

Note that if you are using a remote hosting solution, there is a good chance you will have a different address to give to players, such as a proper website domain name instead of an IP. This will not be reflected in the invitation panel.

#### Connection Testing

If you are hosting via an IPV4 connection, the Game Invitation Links application window will display a simple indicator for whether your server is reachable from a remote connection. This occurs by way of the Foundry VTT software triggering an attempt to connect from one of our own remote hosts as a basic 'yes or no' confirmation of connectivity, displaying a notification for whether or not the connection appears to be open or closed.

#### Link Obscuration

To help prevent cases of users who stream Foundry VTT games accidentally revealing their game invitation links to their audience, the remote invite link is obscured by default. A button has been provided to allow toggling visibility of the link, but clicking the obscured link will copy the address to your clipboard for convenient pasting.

## Combat Encounters

[![Mise en route – Activer/désactiver l'état de combat](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-toggle-combat-state-2021-01-15.jpg)](./Tutoriel - Maître du jeu, deuxième partie _ Foundry Virtual Tabletop_files/getting-started-toggle-combat-state-2021-01-15.jpg "Voir une image plus grande")

Right-clicking a token and clicking this icon will toggle that token's placement in the combat tracker on or off.

Sometimes scenes are destined to result in violence and combat, or times they need you to track what everyone's doing from moment to moment. For this, you need to use the combat tracker. To make use of the combat tracker you simply need to select an actor in a scene, right click them and click the sword and shield icon. This places them into a new encounter, and sets you up to begin tracking combat.

Once you have all your combatants in the encounter, you can click the d20 icon next to their name in the combat tracker to roll their initiative, and then click the begin combat button to start tracking rounds and turns.

For more on combat encounters, refer to the article on [Combat Encounters](https://foundryvtt.com/article/combat/ "Rencontres de combat").

### Learning More

Now that you've completed the getting started guide, you should understand the basics of setting up game worlds, players, actors, and scenes. But there's plenty more to learn about Foundry VTT! This article has several links to other articles in the knowledge base which explain concepts in greater detail, which could be good places to start reading.

There is also a fantastic community made Foundry VTT Tutorial video series from Encounter Library which can be found here: [Encounter Library Foundry Basics Part 1](https://www.youtube.com/watch?v=-aHlApa1nUA)

You can also join the [Foundry Community Discord server](https://discord.gg/foundryvtt) to interact with fellow gamemasters and players, learn about Foundry, what it can do, and what resources are available for you to play with as a new user.

#### Conclusion of Part 2

**This concludes part two of the Getting Started Tutorial!**

-   Return to [Tutorial - Gamemaster Part One](https://foundryvtt.com/article/tutorial/ "Tutoriel - Maître du jeu, première partie") part one.
-   Refer your players to the [Tutorial - Player Orientation](https://foundryvtt.com/article/player-orientation/ "Tutoriel - Orientation du joueur").