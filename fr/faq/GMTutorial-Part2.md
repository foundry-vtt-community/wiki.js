---
title: Tutoriel MJ - Partie 2
description: Tutoriel - MJ Partie 2 · Présentation · Configuration des utilisateurs · Création d'objets · Création d'une scène · Inviter des joueurs · Rencontres de combat. Traduit depuis https://foundryvtt.com/article/tutorial-two/
published: true
date: 2024-09-08T07:49:35.518Z
tags: faq, foundryvtt, mj, tutoriel
editor: markdown
dateCreated: 2024-09-04T07:47:43.999Z
---

## Aperçu

Maintenant que votre monde est configuré, vous allez vouloir configurer des comptes utilisateurs pour vos joueurs, leurs personnages et les scènes nécessaires pour qu'ils puissent jouer ! Il est temps de vous fournir une introduction de base aux aspects fondamentaux du gameplay de Foundry VTT et de vous donner les prochaines étapes pour configurer vos joueurs. Cet article vous présentera :

-   **Utilisateurs.** Toute personne qui se connecte à une session Foundry Virtual Tabletop est un utilisateur et peut être un joueur ou un maître de jeu. Les joueurs de votre jeu auront le contrôle des acteurs, tandis que les utilisateurs qui sont des maîtres de jeu ou des assistants MH peuvent créer des scènes et gérer le jeu pour les joueurs. Tous les utilisateurs ont un rôle qui détermine ce qu'ils peuvent voir et faire dans Foundry. Vous trouverez plus d'informations à ce sujet dans l' article [Utilisateurs et autorisations .](https://foundryvtt.com/article/users/ "Utilisateurs et autorisations")
-   **Acteurs.** Les protagonistes, alliés, monstres, antagonistes et personnes au sein du monde que vous créez. Le système de jeu utilisé détermine les types exacts d'acteurs qui sont pertinents en focntion de ses règles, mais pour la plupart des systèmes de jeu de rôle sur table, ces acteurs représenteront les personnages que le joueur contrôle et rencontre.
-   **Objets.** Les objets ont également leurs propres statistiques et caractéristiques dépendantes du système de jeu, mais peuvent être associés ou utilisés par des acteurs dans le jeu. Les objets peuvent représenter n'importe quel objet physique ou métaphysique, des dagues, des cailloux ou la confiance accordée par un noble allié.
-   **Scènes.** Zones que les joueurs peuvent explorer et qui sont généralement composées d'images de cartes utilisées comme arrière-plans, ou de nombreux morceaux si elles sont construites à partir de [tuiles](https://foundryvtt.com/article/tiles/ "Tuiles") . Les scènes peuvent représenter n'importe quoi, des cartes du monde ou régionales aux petits bâtiments ou donjons. Les scènes peuvent contenir [des jetons](https://foundryvtt.com/article/tokens/ "Jetons") , [des éclairages](https://foundryvtt.com/article/lighting/ "Éclairage") , [des murs](https://foundryvtt.com/article/walls/ "Murs") et [des sons ambiants](https://foundryvtt.com/article/ambient-sound/ "Sons ambiants") . Pour en savoir plus sur ce type de document clé, visitez la page [Scènes](https://foundryvtt.com/article/scenes/ "Scènes") .
-   **Rencontres.** Tout combat, course, énigme ou autre événement où un groupe de personnes effectue à tour de rôle des actions dans le jeu peut être considéré comme une rencontre. Les rencontres sont liées à une scène spécifique, généralement la scène actuellement active et visualisée au moment de la création de la rencontre.

## Configuration des utilisateurs

<table align="right" width="345">
      <tr>
        <td><img align="right" width="345" height="181" src="https://foundryvtt.wiki/dnd-modules/getting-started-user-management-2021-01-13.jpg"></td>
    </tr>
    <tr>
        <td align="center"><em>L'écran de gestion des utilisateurs vous permet de créer, de modifier et de supprimer des comptes utilisateurs sur votre monde de jeu</em>
        </td>
    </tr>
</table>

Avant que les joueurs puissent rejoindre une session, vous devrez configurer des noms d'utilisateur et (éventuellement) des mots de passe pour eux. Pour ce faire, cliquez sur l'icône Paramètres du jeu (icône à trois engrenages) dans la barre latérale droite, puis cliquez sur Configurer les joueurs. À partir de là, vous devriez voir l'utilisateur Gamemaster et un bouton indiquant «Créer un utilisateur supplémentaire». En cliquant sur ce bouton, Foundry créera un nouvel emplacement d'utilisateur générique, généralement nommé «Joueur 2» ou similaire.

Vous pouvez ensuite renommer l'utilisateur, lui attribuer un mot de passe et définir son niveau d'autorisation. Dans le cadre de ce guide, il n'est pas nécessaire d'attribuer aux joueurs un rôle autre que celui de «Joueur». Gardez également à l'esprit que les mots de passe des utilisateurs ne sont pas sécurisés par cryptographie et ne doivent pas être réutilisés à partir d'autres comptes ou services importants que vous ou vos joueurs utilisez.

Une fois que tous les comptes de joueurs nécessaires ont été créés, cliquez sur «Enregistrer et revenir» et vous serez renvoyé vers votre jeu. Les joueurs peuvent désormais rejoindre et se connecter avec les identifiants que vous leur avez fournis. Il reste cependant un peu de configuration à faire avant de pouvoir jouer.

### Créer et affecter des acteurs

<table align="right" width="345">
      <tr>
        <td><img align="right" width="345" height="222" src="https://foundryvtt.wiki/dnd-modules/getting-started-actor-permissions-2021-01-13.jpg"></td>
    </tr>
    <tr>
        <td align="center"><em>La fenêtre Configuration des droits de l'acteur vous permet de déterminer qui a le contrôle d'un acteur et de ses données.</em>
        </td>
    </tr>
</table>

Les acteurs sont les créatures, les joueurs, les ennemis et les alliés que vous et vos joueurs utiliserez pour jouer au jeu. Ils contiennent des données sur les capacités, les attributs, les pouvoirs, etc. Pour créer un nouvel acteur, sélectionnez le répertoire des acteurs dans la barre latérale droite en cliquant sur l'icône qui ressemble à un groupe de personnes.

En haut de cet onglet de répertoire, vous pouvez créer un nouvel acteur. Selon le système que vous avez choisi, vous pouvez avoir plusieurs options ici. Cependant, pour la plupart des jeux, les joueurs seront étiquetés comme «Personnage joueur» et c'est ce que vous devez chercher.

Une fois votre acteur nommé et créé, sa fiche de personnage devrait s'ouvrir, vous permettant de le configurer selon vos besoins, et il apparaîtra également dans le répertoire des acteurs à droite. Une fois que vous avez terminé de modifier l'acteur, vous pouvez fermer la fenêtre et les modifications seront enregistrées. Vous pouvez toujours cliquer sur un acteur dans la barre latérale pour rouvrir la fiche et le modifier davantage si vous en avez besoin.

Une fois que vous avez créé un acteur, vous devez l'assigner à un joueur ! Faites un clic droit sur un acteur que vous avez créé et sélectionnez «Configurer les droits».

Dans le menu qui apparaît, vous pouvez choisir comment tous les joueurs interagissent avec l'acteur (par défaut, ils ne peuvent pas le faire) et sélectionner comment les utilisateurs individuels interagissent avec l'acteur.

Attribuer à un utilisateur le niveau d'autorisation «propriétaire» lui permet de modifier la fiche de personnage et de le contrôler entièrement dans les scènes. Il s'agit du niveau par défaut que vous souhaiterez pour la plupart des personnages joueurs. Tout niveau inférieur (comme limité ou observateur) leur fera perdre la possibilité de contrôler directement les acteurs qui leur sont assignés.

## Créer des objets

La création d'objets est très similaire à la création d'acteurs, et les deux sont étonnamment similaires : ils contiennent des informations sur un document, ce qu'il peut faire, des illustrations, etc. Les objets, cependant, sont destinés à être attachés à des acteurs et à être utilisés par eux.

Pour créer un objet, sélectionnez le **répertoire Objets** (il se trouve à droite du répertoire Acteurs) et en haut de la barre latérale, cliquez sur «**Créer Objet».** De la même manière que pour créer un acteur, vous devez fournir un nom. Selon le système de jeu que vous utilisez, vous pourrez également déterminer le type spécifique d'objet que vous créez. Par exemple, le système DnD5e vous permet de choisir entre des armes, des outils et des consommables parmi plusieurs autres types d'objets.

Une fois que vous avez nommé et créé votre objet, le menu de configuration de l'objet s'ouvre. À partir de là, vous rédigez une description de votre objet et vous lui appliquez des fonctionnalités en fonction de ce que votre système de jeu propose. Lorsque vous êtes satisfait de votre objet, vous pouvez fermer le menu de l'objet, car les modifications sont automatiquement enregistrées. Si vous avez besoin de modifier un objet, vous pouvez cliquer sur le nom de l'objet pour rouvrir la fiche de l'objet.

#### Attribuer des objets aux acteurs

Pour donner un objet à un acteur, ouvrez simplement la fiche de l'acteur et faites glisser l'objet choisi depuis le répertoire des objets vers la fiche. L'objet sera alors ajouté à l'inventaire de l'acteur. La plupart des systèmes de jeu ont un onglet d'équipement ou d'inventaire sur les fiches d'acteur, bien que cela puisse varier en fonction de la façon dont le système de jeu gère les objets. Par exemple, un objet classé comme un sort peut être ajouté à la liste des sorts d'un acteur au lieu de son inventaire.

## Créer une scène

<table align="right" width="345">
      <tr>
        <td><img align="right" width="345" height="165" src="https://foundryvtt.wiki/dnd-modules/getting-started-making-a-scene-2021-01-15.jpg"></td>
    </tr>
    <tr>
        <td align="center"><em>Ceci est un exemple du strict minimum nécessaire pour créer une scène dans Foundry VTT.</em>
        </td>
    </tr>
</table>

Pour créer une nouvelle scène :

1.  Ouvrez le répertoire des scènes (l'icône de la carte, 3ème à partir de la gauche) dans la barre latérale sur le côté droit de l'écran.
2.  En haut de la barre latérale, cliquez sur **Créer Scène** , vous serez invité à donner un nom à la scène, puis cliquer sur "Créer scène" ouvrira le menu de configuration de la scène.

Si vous n'avez pas d'image de carte prête, nous vous en avons fourni une ici sur [DeviantArt](https://www.deviantart.com/foundryatropos/art/Foundry-Tavern-at-Night-746759206) . Cette image a les dimensions suivantes : 3150x2800px.

Une fois que vous avez défini l'image d'arrière-plan et que vous vous êtes assuré que les dimensions correspondent à l'image que vous utilisez, vous pouvez enregistrer les modifications et Foundry VTT créera votre scène pour vous.

#### Naviguer dans les scènes

Une fois que vous avez créé une scène, vous devez être capable d'effectuer des opérations de navigation de base. Pour déplacer votre vue de la carte, faites un clic droit sur la scène et déplacez la souris pour déplacer la scène. Vous pouvez également utiliser `Ctrl (CMD for macOS) + Flèches de Direction`pour déplacer votre vue. Pour effectuer un zoom avant ou arrière, utilisez simplement la molette de votre souris ou les `page up/down`touches. Vous pouvez appuyer sur Tab pour déplacer votre vue vers le point de départ de la scène. Comme nous n'avons pas défini de coordonnées spécifiques pour cela, la valeur par défaut sera le coin supérieur gauche de l'image.

#### Organiser les scènes

Vous avez également la possibilité de créer un dossier pour organiser les scènes en groupes, ce que vous pouvez faire à tout moment en créant un dossier dans le répertoire de scène et en faisant glisser la scène vers le dossier souhaité.

#### Changer de Scène

Lorsque vous avez plusieurs scènes, vous pouvez soit cliquer sur le nom de la scène en haut de l'écran de Foundry VTT, ce qui déplacera votre vue vers la scène choisie sans l'activer, soit faire un clic droit sur la scène et l'activer, ce qui attirera tous les joueurs connectés vers la scène, y compris vous. Vous pouvez faire la même chose à partir du répertoire des scènes en faisant un clic droit sur n'importe quelle scène et en choisissant d'afficher (pour simplement regarder la scène par vous-même) ou de l'activer.

### Créer des Murs

<table align="right" width="345">
      <tr>
        <td><img align="right" width="345" height="204" src="https://foundryvtt.wiki/dnd-modules/tutorial-tavern-scene-2020-05-24.png"></td>
    </tr>
    <tr>
        <td align="center"><em>Ceci est un exemple de placement de murs et portes sur l'image de la taverne founrie en lien.</em>
        </td>
    </tr>
</table>

Maintenant que vous avez une scène, vous devrez placer des murs pour déterminer comment la lumière se répand, ainsi que les limites des pièces et du terrain. Par défaut, toutes les nouvelles scènes ont la vision activée, ce qui signifie que les acteurs avec vision ne pourront voir la carte que si elle est éclairée et que leur ligne de vue le permet. La plupart du temps, les acteurs personnages joueurs auront la vision activée. Les murs sont un moyen de déterminer la partie d'une zone qu'un acteur avec la vision peut voir.

Créer des murs est aussi simple : sélectionnez l'outil de contrôle des murs et tracez des lignes pour indiquer un mur qui bloque la vue. Bien qu'il existe plusieurs types de murs, qui ont tous des fonctions légèrement différentes, les deux murs les plus courants que vous utiliserez sont les murs et les portes standard.

Si vous utilisez le plan de la taverne fourni ci-dessus, vous pouvez utiliser l'exemple d'image comme guide pour savoir comment le bâtiment doit être entouré de murs.

Vous pouvez en apprendre davantage sur les murs dans l’ article [Murs](https://foundryvtt.com/article/walls/ "Murs").

### Créer des Lumières

Les lumières sont nécessaires pour que les acteurs voient les choses, à moins que la scène ne soit dotée de l'étiquette Illumination globale, auquel cas la scène entière sera fortement éclairée. Les lumières sont particulièrement importantes lorsque le niveau d'obscurité de la carte augmente et que la visibilité diminue. Vous pouvez ajuster le niveau d'éclairage et d'obscurité de la carte en cliquant avec le bouton droit de la souris sur la carte dans le répertoire de la scène et en choisissant Configurer, ou en allant dans le panneau de contrôle de l'éclairage à gauche et en utilisant les boutons de transition vers la lumière du jour ou l'obscurité.

Les lumières interagiront avec les murs, créant des ombres et des zones d'obscurité que la lumière ne peut pas atteindre. Créer une lumière est aussi simple : cliquez sur un point de la scène et faites  glisser le rayon de la lumière. Les deux anneaux visibles lors de la création de la lumière indiquent le rayon lumineux intérieur et le rayon sombre extérieur. Une fois que vous avez créé une lumière, vous pouvez double-cliquer dessus pour modifier ses différentes propriétés, notamment la couleur et les rayons spécifiques des anneaux lumineux et sombres. Vous pouvez également cliquer avec le bouton droit sur la lumière pour allumer ou éteindre rapidement une lumière sans la supprimer.

Vous pouvez en apprendre davantage sur les lumières dans l’ article [Eclairage](https://foundryvtt.com/article/lighting/ "Éclairage").


### Placer les acteurs dans une scène

<table align="right" width="345">
      <tr>
        <td><img align="right" width="345" height="204" src="https://foundryvtt.wiki/dnd-modules/getting-started-placing-tokens-webp-2021-01-19.webp"></td>
    </tr>
    <tr>
        <td align="center"><em>Les acteurs peuvent être facilement placés dans une scène en les faisant glisser depuis le répertoire des acteurs.</em>
        </td>
    </tr>
</table>


Une fois que vous avez créé et configuré une scène, que vous vous êtes habitué à déplacer votre vue et que vous avez posé des murs et des lumières, vous souhaiterez faire entrer un ou plusieurs acteurs dans la scène. Pour ce faire, sélectionnez l'icône Répertoire des acteurs à droite de l'icône de l'onglet Scènes, puis  glissez-déplacez un acteur du répertoire vers la scène active. Cela placera un jeton représentant l'acteur dans la scène, et son propriétaire pourra voir la scène grâce à la vision de ce jeton (s'il a une vision).

En tant que maître du jeu, vous pouvez voir ce que n'importe quel jeton peut voir en cliquant dessus.

Vous pouvez en apprendre davantage sur les acteurs dans l' article Acteurs et davantage sur les jetons dans l' article Jetons.

#### Façons de déplacer les jetons


Déplacer un jeton peut être effectué de plusieurs manières, en fonction de vos besoins et de ceux de vos joueurs, et de ce qui est le plus simple pour vous :

- **Déplacement par glissement** - Cliquez et faites glisser un jeton pour le déplacer en ligne droite le long du chemin vers un nouvel emplacement. Dès que vous relâchez le jeton, Foundry exécute le déplacement. Il convient de noter qu'en tant que maître de jeu, vous pouvez faire glisser des jetons à travers les murs et autres obstacles, mais les joueurs ne le peuvent pas.
- **Mouvement incrémental** - Les `touches fléchées` ou `ZQSD` peuvent être utilisées pour déplacer un acteur contrôlé dans une scène. Cela est particulièrement utile pour effectuer de petits mouvements incrémentaux dans des espaces restreints où les murs et le terrain peuvent bloquer le mouvement. Vous pouvez également maintenir la touche `Maj` enfoncée tout en utilisant ces touches de mouvement pour faire pivoter rapidement un acteur vers une orientation particulière sans changer sa position sur la carte.
- **Mouvement par mesure (point de cheminement)** - Les outils de mesure peuvent être utilisés pour déplacer des jetons. Cela se fait rapidement en maintenant la touche `Ctrl (CMD pour macOS)` enfoncée, puis en cliquant et en faisant glisser dans une direction à partir d'un jeton que vous contrôlez. Cela vous montrera un chemin et la distance jusqu'à l'emplacement final. Vous pouvez cliquer avec le bouton gauche de la souris sur des endroits supplémentaires le long de l'itinéraire pour placer des points de cheminement que le jeton suivra. Pour déplacer un jeton à l'aide de cette méthode, appuyez simplement sur la `barre d'espace` et Foundry déplacera le jeton le long de la ligne jusqu'au point final du chemin. Vous pouvez faire la même chose avec l'outil de mesure de distance si le début de la mesure se trouve sur un jeton que vous contrôlez.

Vous pouvez en savoir plus sur la configuration des commandes de Foundry VTT dans l' article [Commandes de Jeu](https://foundryvtt.com/article/controls/ "Commandes de jeu") article.

## Inviter des Joueurs

<table align="right" width="345">
      <tr>
        <td><img align="right" width="345" height="156" src="https://foundryvtt.wiki/dnd-modules/game-invitation-links-closed-2021-12-15.jpg"></td>
    </tr>
    <tr>
        <td align="center"><em>L'application "Liens d'invitation à la partie" vous permet d'inviter facilement vos joueurs à votre jeu.</em>
        </td>
    </tr>
</table>


Une fois vos utilisateurs, acteurs, objets et scènes créés, vous aurez besoin de joueurs. Inviter des joueurs à votre jeu est très simple : dans la barre latérale droite, cliquez sur l'onglet Paramètres du jeu (l'icône en forme d'engrenage), puis cliquez sur Liens d'invitation. Une fenêtre apparaîtra avec deux adresses IP. La première est destinée aux utilisateurs qui se trouvent sur le même réseau local que votre installation de Foundry, et la deuxième est votre adresse IP publique pour les connexions Internet externes. Dans les deux cas, les utilisateurs peuvent coller ces liens dans un navigateur Web et rejoindre votre partie.

Notez que si vous utilisez une solution d'hébergement à distance, il y a de fortes chances que vous ayez une adresse différente à donner aux joueurs, comme un nom de domaine de site Web approprié au lieu d'une adresse IP. Cela ne sera pas reflété dans le panneau d'invitation.

#### Test de connexion

Si vous hébergez via une connexion IPV4, la fenêtre de l'application "Liens d'invitation à la partie" affichera un indicateur simple pour savoir si votre serveur est accessible à partir d'une connexion à distance. Cela se produit par le biais du logiciel Foundry VTT qui déclenche une tentative de connexion à partir de l'un de nos propres hôtes distants sous la forme d'une confirmation de connectivité de base par « oui ou non », affichant une notification indiquant si la connexion semble être ouverte ou fermée.

#### Obfuscation des liens

Pour éviter que les utilisateurs qui diffusent des parties sur Foundry VTT ne révèlent accidentellement leurs liens d'invitation de jeu à leur public, le lien d'invitation à distance est masqué par défaut. Un bouton a été fourni pour permettre de basculer la visibilité du lien, mais cliquer sur le lien masqué copiera l'adresse dans votre presse-papiers pour un collage pratique.

## Rencontres de combat

<table align="right" width="345">
      <tr>
        <td><img align="right" width="345" height="243" src="https://foundryvtt.wiki/dnd-modules/getting-started-toggle-combat-state-2021-01-15.jpg"></td>
    </tr>
    <tr>
        <td align="center"><em>Un clic droit sur un jeton et un clic sur cette icône activeront ou désactiveront le placement de ce jeton dans le suivi de combat.</em>
        </td>
    </tr>
</table>

Parfois, les scènes sont vouées à la violence et aux combats, ou bien elles nécessitent que vous suiviez ce que font les autres à chaque instant. Pour cela, vous devez utiliser le suivi des combats. Pour utiliser le suivi des combats, il vous suffit de sélectionner un acteur dans une scène, de faire un clic droit dessus et de cliquer sur l'icône représentant une épée et un bouclier. Cela les place dans une nouvelle rencontre et vous permet de commencer à suivre les combats.

Une fois que vous avez tous vos combattants dans la rencontre, vous pouvez cliquer sur l'icône d20 à côté de leur nom dans le suivi de combat pour lancer leur initiative, puis cliquer sur le bouton Commencer le combat pour commencer à suivre les rounds et les tours.

Pour en savoir plus sur les rencontres de combat, reportez-vous à l'article sur les [Rencontres de Combat](https://foundryvtt.com/article/combat/ "Rencontres de combat").

### En savoir plus

Maintenant que vous avez terminé le guide de démarrage, vous devriez comprendre les bases de la configuration des mondes de jeu, des joueurs, des acteurs et des scènes. Mais il y a encore beaucoup à apprendre sur Foundry VTT ! Cet article contient plusieurs liens vers d'autres articles de la base de connaissances qui expliquent les concepts plus en détail, ce qui pourrait être un bon point de départ pour la lecture.

Il existe également une fantastique série de didacticiels vidéo sur le VTT Foundry réalisée par la communauté Encounter Library, que vous pouvez trouver ici : [Encounter Library Foundry Basics Part 1](https://www.youtube.com/watch?v=-aHlApa1nUA)

Vous pouvez également rejoindre le serveur Discord de la communauté Foundry pour interagir avec d'autres maîtres de jeu et joueurs, en savoir plus sur Foundry, ce qu'il peut faire et quelles ressources sont à votre disposition pour jouer en tant que nouvel utilisateur.

#### Conclusion de la Partie 2

**Ceci conclut la deuxième partie du didacticiel de démarrage !**

- Retour au [Tutoriel-MJ-Partie-1](/fr/faq/Tutoriel-MJ-Partie-1 "Tutoriel MJ - Partie 1")
- Référez vos joueurs au tutorial [Premier pas des Joueurs](/fr/faq/Tutoriel_Players1)


