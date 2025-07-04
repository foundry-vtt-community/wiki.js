---
title: FAQ Foundry
description: 
published: true
date: 2025-06-07T06:59:45.053Z
tags: 
editor: markdown
dateCreated: 2020-10-20T11:11:27.841Z
---

# Questions sur Foundry en général

## FoundryVTT, c'est quoi ?
FoundryVTT est un logiciel permettant de jouer à des jeux de rôles sur table en mode connecté. La table physique devient ainsi "virtuelle", et le MJ et ses joueurs communiquent à distance via micro et webcam. [Un choix de cet outil expliqué ici](http://www.lahiette.com/leratierbretonnien/warhammer/table-virtuelles-de-jdr/)

## OK, ça ressemble vachement à Roll20
Oui, tout à fait, c'est dans la même catégorie de logiciels. Il existe à mon sens deux grandes catégories de logiciels de table virtuelles : les "clients lourds" et les applications webs. Les clients lourds sont des applications (des .exe quoi, pour les Windoziens) à installer sur<strong> chaque</strong> poste des joueurs. Les applications Web nécessitent un serveur (soit auto-hébergé, soit hébergé par un tiers) et le MJ et les joueurs utilisent simplement un navigateur (Chrome conseillé) pour y accéder. FoundryVTT  est dans la catégorie des applications web, comme Roll20 ou AstralTableTop.

## Du coup c'est quoi la différence avec Roll20 ?
FoundryVTT a démarré son développement notamment car l'auteur principal ne trouvait pas son compte avec Roll20. Il a donc conçu FoundryVTT comme une plateforme générique de gestion de règles de JDR, quelle qu'elles soient. Ainsi, tout système de JDR est vu comme un plugin, à installer en plus de Foundry. De plus, FoundryVTT offre une grande liberté pour son installation et sa gestion : soit en local, soit en serveur privé, soit encore en hébergement tiers. J'y reviendrais plus bas. Enfin, son modèle de licence est ultra-simple et appréciable : vous achetez le logiciel, et c'est à vous une fois pour toute. Pas d'abonnement, rien, nada. Vous achetez, et c'est à vous.

## Très bien, ~~mais je vois que le truc est en bêta~~, comment qu’on l’achète ?

A ce jour, le 26 Aout 2024, Foundry n'est plus en beta depuis plus de 3,5 ans. Il est actuellement en version 12, achetable en 1 fois pour la somme de 60$. Des promotions d'environ -15/-20% sont proposées régulièrement. Ca se passe ici :  https://foundryvtt.com/ 

## Putain, 60$ ?

C’est peut-être effectivement cher pour vous – et je peux le comprendre – mais pensez que ça correspond probablement à 3 ou 4 mois de votre abonnement de smartphone, et qu’une fois cette somme dépensée, le soft est à vous à vie. Comparez également avec un abonnement en ligne d’1 an chez Roll20 ou équivalents. Enfin, pensez que vous allez pouvoir héberger tous vos jeux sur une seule licence, et que vous n’aurez que vos propres limites en matière de stockage de cartes, dessins, images, etc. Quand à ceux qui, par principe, cherchent à ne jamais dépenser quoi que ce soit, pour rien, et qui veulent toujours grappiller le moindre bout de machin – et ce sont ceux qui curieusement sont également les plus exigeants – , et bien vous pouvez arrêter de lire cette FAQ, l’outil n’est probablement pas pour vous :) !

## 60 $ ? Et les joueurs doivent payer aussi ?

C’est une question qui revient très souvent : donc, non bien sûr les joueurs ne payent pas. Une seule licence permet de faire jouer autant de joueurs que vous voulez à votre table. La seule contrainte est qu’il ne peut y a voir qu’1 seul jeu (ie qu'une seule partie de JDR) actif à un moment donné.

## Pourquoi c'est payant et pas gratuit ?

Le modèle de Foundry est un achat one-shot : vous disposez du soft, il est à vous à vie, et si jamais la société Foundry disparait, le code source sera disponible et pourra être repris par d'autres. Tout à l'opposé des modèles hébergés basés sur des abonnements ou des micro-transactions (Roll20, Lets'Role, etc).
En payant 60 EUR (ou 50 EUR lors des promos), vous rémunèrez une équipe de rolistes développeurs de manière décente pour le travail fourni. Précisons que l'équipe de Foundry est internationale, et qu'il y a même un Français dans la team officielle (SecretFire).
Naturellement, c'est une affaire de goût et de choix, mais Foundry est un peu plus "qu'une licence à payer", c'est un modèle idéologique derrière également. On adhère ou pas, mais il faut en être conscient 🙂 

## Admettons que je l’achète. Est-ce que je dois re-payer pour les mises à jour ?

Non, les mises à jours de bugfixes et releases sont comprises dans le prix d’achat. Ce qui peut venir en supplément, ce sont certaines licences officielles de contenus, signées avec les éditeurs, et venant avec un module complet, et pour lesquels l’éditeur demande lui même une rétribution. Mais tant que que l’on joue avec les modules et systèmes disponibles gratuitement, il n’y a rien d’autre à payer que le prix d’achat initial.

## Y'a plein de vocabulaire que je comprends pas, comment faire ?

Globalement, il a y trois termes/notions très importants sous FoundryVTT : les systèmes (systems), les modules (modules) et les mondes (worlds).

Un système (ie systems en angliche), c'est un système de jeu : un JDR quoi. Par exemple, tu veux jouer à l'Appel de Cthulhu, et bien c'est le système Appel de Cthulhu. Tu veux jouer à Savage Worlds, et bien c'est le système Savage Worlds, etc, etc.

Un module, ce sont des extensions (ie des plugins, en quelque sorte) qui viennent améliorer ou ajouter des choses à ta partie. Il en existe de plusieurs catégories : 
 - des modules qui apportent du contenu (ie des musiques, des illustrations, des cartes, etc...)
 - des modules qui modifient ou changent le comportement de Foundry (ie gestion des combats, affichage différent, outils de dessin supplémentaires, etc etc)
  - des modules de scénarios/campagnes complet, qui permettent d'avoir tout prêt des cartes, des images et des PNJs pour jouer.
  
 Un monde (ie world en angliche), et bien c'est ta partie. Si par exemple tu maitrises 1 table à DD5 et une autre table à Hawkmoon, et bien tu auras 2 mondes. Et si tu maitrises 3 parties différentes à DD5, et bien tu auras 3 mondes. Le nombre de mondes possibles est infini, mais tu ne peux faire fonctionner qu'1 seul monde (ie 1 seule partie) à un instant donné.
 
 Installer des systèmes, des modules et créer son monde est expliqué rapidement plus bas dans cette FAQ.
 
Pour aller plus loin, tu peux rapidement lire la Knowledge Base de Foundry, disponible ici : https://foundryvtt.com/kb 

Ensuite, selon ton système de jeu, regarde dans les émissions des Chroniques de la Fonderie : https://foundryvtt.wiki/fr/pages/choniquesfonderie et également sur la chaine YT de Carter : https://www.youtube.com/@carterfoundryvtt

D'autres vidéos sont souvent disponibles sur YT également, à chercher selon ton système de jeu.

Ensuite, naturellement, les canaux du Discord FR sont là pour répondre à toutes les questions : https://discord.gg/pPSDNJk

## Je joue pas en ligne, ça peut me servir quand même ?

Oui, tout à fait. Etant donné que Foundry s'installe en local, on peut parfaitement utiliser Foundry pour gérer sa campagne, le suivi des PJ et des PNJs, etc, etc.

Il peut de plus se transformer en assistant sur table, comme décrit ici par exemple : https://www.lahiette.com/leratierbretonnien/foundryvtt-pour-table-reelle/

## C’est tout en anglais ce machin !

Oui, c’est vrai, les auteurs étant majoritairement américains ou anglo-saxons. Mais des traducteurs traduisent petit à petit les contenus : ainsi l’interface générale du jeu est traduite par la communauté FR de Foundry, ainsi que les systèmes D&D5, Shadowrun, Starfinder, Savage Worlds, Pathfinder 1 & 2, Warhammer 4, Forbidden Lands, Alien, etc.

Cependant, il faut reconnaître qu’une partie du support et des documentations sont en anglais : si vraiment cette langue est pour vous hors de portée, je crains qu’il ne vous faille attendre… ou utiliser un autre outil, plus francisé.

## Bon OK, mais pourquoi s’intéresser autant à cet outil ? Et pas aux autres ?

Voici un choix et une explication étayée - qui vaut ce qu'elle vaut : [cette page](http://www.lahiette.com/leratierbretonnien/warhammer/table-virtuelles-de-jdr/). 

De plus, le modèle de Foundry est un achat one-shot : tu disposes du soft, il est à toi à vie, et si jamais la société Foundry disparait, le code source sera disponible et pourra être repris par d'autres. Tout à l'opposé des modèles hébergés basés sur des abonnements ou des micro-transactions (Roll20, Lets'Role, etc).

En payant 60 EUR (ou 50 EUR si promo), tu rémunères une équipe de rolistes développeurs de manière décente pour le travail fourni. Je précise d'ailleurs que l'équipe de Foundry est internationale, et qu'il y a même un Français dans la team officielle (SecretFire).

Naturellement, c'est une affaire de goût et de choix, mais Foundry est un peu plus "qu'une licence à payer", c'est un modèle idéologique derrière également. On adhère ou pas, mais il faut - je pense - en être conscient"

## Bah finalement, on peut faire la même chose avec un outil de meeting vidéo et partage d’écran

Oui, tout à fait, et ça marche assez bien aussi. Les outils intégrés comme Roll20, Alchemy, FantasyGrounds, Foundry etc offrent toutefois des capacités des gestion des scènes, des actions, des PNJs qui sont difficilement atteignables avec un simple outil de meeting. Mais l’important c’est de jouer de toute manière.

## J'aime pas jouer en ligne, c'est de la m..., alors ça me sert à rien

Cete attitude connue est compréhensible, mais à titre perso je maitrise plus sur table réelle que virtuelle, et j'utilise Foundry dans tout les cas. C'est devenu un précieux outil de gestion et d'animation des parties dont je ne saurais plus me passer sur table désormais. Plus de détails ici : http://www.lahiette.com/leratierbretonnien/foundryvtt-pour-table-reelle/ 

## Bon je veux bien regarder, il n'y a pas une vidéo de démonstration ?

Il ya de nombreuses vidéos sur YT, mais celle-ci est régulièrement citée en référnce  :
[cette vidéo est fréquemment citée comme une excellente introduction](https://www.youtube.com/watch?v=kEQlhdF1568&list=PLGgCMB0gYnLFWxyrCkUYwHY4vvA_yME7m)

La chaine YT de Carter (https://www.youtube.com/@carterfoundryvtt) contient plein de tutos à jour, et les Chroniques de la Fonderie (https://www.youtube.com/results?search_query=chroniques+de+la+fonderie) peuvent aussi aider, comme indiqué plus haut.

## Je suis pas trop vidéo, y'a pas un document explicatif ?

Ce document (https://www.lahiette.com/leratierbretonnien/wp-content/uploads/2024/11/guide_fvtt.pdf) explique comment acheter et installer FoundryVTT sur son PC. La fin du document est dédiée au système Rêve de Dragon, mais la première partie est valable pour n'importe quel système de jeu.

# Foundry et son installation 

## J'ai lu ou entendu que Foundry, c'était un peu compliqué à installer. Du coup j'hésite...

FoundryVTT, c'est comme disposer d'une sorte de super "Roll20" chez soi, en complète autonomie, et sans abonnement. Du coup, cette liberté et ce "pouvoir" implique effectivement un étape technique qui peut être ressentie comme plus ou moins difficile, selon ton affinité avec l'informatique. 

Cependant, il y a plusieurs milliers d'utilisateurs en France (et plusieurs dizaines de milliers dans le monde) qui ont franchi ce pas, et tous ne sont pas des informaticiens, loin de là. De grands pouvoirs impliquent de grandes responsabilités :) (et encore, faut voir)

Le Discord FR de Foundry est justement là - entre autre - pour aider sur ce point lorsque nécessaire :)

## J’y connais rien en informatique, je peux m’en servir quand même ?

Si vous savez ouvrir des ports dans votre routeur ADSL/Fibre (tapez ‘ouvrir des ports dans son modem ADSL‘ dans votre moteur de recherche…) et que vous savez installer un programme sur votre ordinateur, il est probable que vous saurez vous en servir. 

Si tout ceci est déjà trop compliqué ou chiant (:)) pour vous, il faudra vous tourner ves les offres d’hébergement tiers, comme The Forge. 

Sinon, si un de vos joueurs est un peu informaticien, il peut probablement vous aider dans cette tâche. 

Il faut compter 3-4 Mbs d’upload montant pour héberger une partie chez vous, donc il est possible que vous ayez besoin d’un serveur tiers si votre connexion est lente. Il existe de l’aide à ce sujet sur le Wiki de Foundry. Un certains nombres d’utilisateurs présents sur le Discord FR de Foundy sont également capables d’apporter du support sur ces sujets.

## Je n'ai pas de serveur, je peux le mettre sur mon PC ?

Oui, bien sûr, c’est l’un des 3 modes d’installation. Dans ce cas, vous avez un .EXE/.DMG qui va transformer votre PC/Mac en serveur Foundry. Vous aurez à ouvrir le port TCP 30000 (par défaut), et à l’autoriser dans votre firewall. Plus d’infos ici https://foundryvtt.wiki/fr/pour-commencer/pbcnx.

## Je veux que tout soit accessible en permanence, comment je fais ?

Cela revient à faire tourner Foundry tout le temps, et donc à laisser allumer un PC/serveur quelque part. Ce "quelque part" peut-être chez vous, ou encore sur un service d'hébergement en ligne (Oracle, AWS, TheForge). A noter que chez vous, une petit Raspberry Pi peut parfaitement se transformer un serveur Foundry pour un coût énergétique réduit.

## Et il faut de la bande passante ?

Oui, un peu, surtout en montant. Si vous jouez avec 4-5 joueurs, il vous faudra au moins 4 Mbs montant (je dis bien montant) pour jouer confortablement. Donc si vous avez la fibre ou un "gros" ADSL, ca marchera sans soucis. Sinon, il faut vous orienter vers un hébergement tiers.

## Ca marche si j'héberge avec la 4G/5G ?

Non, impossible. Les operateurs 4G ne permettent pas d'ouvrir des ports ou d'ouvrir sa connexion. Donc à moins de passer par des mécaniques de VPN avancées, la 4G/5G est inutilisable pour partager son Foundry local. 

On peut par contre l'utiliser pour se connecter à un serveur Foundry en tant que joueur.

## J’ai déjà un serveur, comment je fais ?

Bon si vous avez ça, l’installation ne devrait vous poser de problèmes, je vous conseille les liens suivants : 
- https://foundryvtt.com/article/hosting/ 
- https://foundryvtt.com/article/audio-video/
- https://foundry-vtt-community.github.io/wiki/Ubuntu-VM/

## Et il n'y a pas de services en lignes déjà disponibles ?

Si, il y en a, notamment avec avec TheForge (https://forge-vtt.com/, le plus populaire à l'heure actuelle) et FoundryServer (https://www.foundryserver.com/ ).

## Je peux l'installer plusieurs fois ?

Oui, tout à fait. Vous pouvez par exemple l'installer sur un serveur en ligne pour jouer, et sur votre PC en local pour préparer vos parties. Ou encore l'installer sur un PC fixe et un PC portable, etc. La licence Foundry l'autorise, du moment que vous n'avez qu'une seule instance en fonctionnement à un instant T.

## Y'a plein de trucs et j'y comprends pas grand chose. Par ou commencer ?

Tout d'abord, venir s'inscrire sur le Discord FR Foundry ( https://discord.gg/pPSDNJk ), puis lire cette FAQ :) 

Ensuite, parcourir la "Knowledge Base" de Foundry : https://foundryvtt.com/kb

Enfin, selon le système de jeu qui t'interesse, aller regarder 1 ou 2 vidéos sur YT. Tu peux déja regarder si ton jeu est traité par les Chroniques de la Fonderie, une émission en français que nous essayons de réaliser mensuellement. Chaque émission présente en général un système de jeu français ou traduit : https://www.youtube.com/results?search_query=chroniques+de+la+fonderie 

Liste ici : https://foundryvtt.wiki/fr/pages/choniquesfonderie 

# Comment jouer et questions associées 

## Y’a quoi comme jeux dessus ?

Dans Foundry, les « jeux » sont appelés « systems ». En Juin 2025, on recense 370+ « systems », qu'on peut parcourir ici  : https://foundryvtt.com/packages/systems. **La totalité des systèmes sont gratuits.** 
Une liste succincte d'exemples : 
- 13th age,
- Appel de Cthulhu, DeltGreen
- Cthlulhu Hack
- Chroniques Oubliées
- Aliens
- D&D5,
- Tales From The Loop
- Knight
- Donjons & Chatons
- Mournblade, Hawkmoon
- Anneau Unique
- Fate, 
- Pathfinder 1,
- Pathfinder 2,
- Metal Adventures,
- Mutants &Masterminds,
- Ecryme,
- Brigandyne,
- Barbarians of Lemuria,
- Les Héritiers,
- Wasteland,
- Shadowrun 5/6/Anarchy, 
- Starfinder, 
- Savage Worlds, 
- Warhammer 4
- Degenesis,
- Dungeon Crawl Classics,
- Monster of the Week,
- Numenera,
- Runequest…. 
- ....

Un système générique permet de plus de rapidement démarrer une table autour d’un jeu non supporté.

## Je vois qu'il y a des "systèmes". C'est quoi ?

Les systèmes, ce sont les systèmes de jeu. Donc 1 système = 1 JDR. Pour pouvoir jouer à un jeu Y, il faut donc installer le système Y correspondant. 

Exemple : je veux jouer à Warhammer, j'installe le système de jeu "Warhammer" en le cherchant dans l'interface "Systèmes".

## J’ai vu qu’il y avait aussi des modules, c’est quoi ?

Les modules sont des extensions, comme des plugins, qui viennent ajouter des fonctionnalités au logiciel. Ces modules viennent donc ajouter des compléments aux « systèmes », en offrant des aides pour les MJ et les joueurs : 

- effets spéciaux, 
- des contenus (images, musiques, cartes, ...)
- navigation dans les fichiers images, 
- communication, etc, etc. 

A chacun de venir piocher ce qu’il lui plaît !

## Et les mondes, c'est quoi ?

Un monde, c'est ta partie de JDR, ta table de jeu, quoi. On créée un monde à partir d'un système (cf plus haut), et éventuellement de quelques modules. Si on fait le parallèle avec une table de  jeu réelle, on a :

* Le bouquin de règles = le système Foundry
* La table et le scénario = le monde Foundry
* Les accessoires (musique, dessins, dés) = les modules Foundry

## C'est quoi qui est gratuit ? Et qui est payant ?

La totalité des systèmes de jeu sont gratuits. Et parmi les modules, il en existe des gratuits (la plupart) et des payants (qu'on appelle aussi modules Premium, cf ci-dessous).

Ces modules payants (ie Premium) peuvent être des campagnes complètes (ie par exemple Pax Elfica chez les 12 Singes), des petits scénarios, des recueils de maps/cartes, des modules de contenus (par exemple le module "core" pour Warhammer contient l'intégralité du livre de base), etc. Ces modules payants sont toujours optionnels, vous n'êtes jamais obligé de les acheter.


## J'ai vu qu'il y a avait des modules "premium". C'est quoi ?

La grande majorité des modules sont gratuits, mais certains modules de contenus (règles, scénarios, campagnes, etc) sont produits par des éditeurs et sont payants. En général, il faut les acheter sur le site de l'éditeur, qui fourni une clé de licence. Cette clé est à saisir dans l'interface d'administration de ton compte Foundry (ie sur https://foundryvtt.com ). 

Une fois cette clée saisie, le module apparait comme installable dans les modules "premiums", via l'interface "modules" de Foundry.  

## Comment je fais pour installer un système de jeu ?

Dans l'interface de Foundry, cliquez sur le bouton d'installation : ![install_system.webp](/setup/winstall/install_system.webp)

La fenêtre de recherche des systèmes s'ouvre. Saissisez alors le/les mots-clés du système souhaité : 
![install_system_02.webp](/setup/winstall/install_system_02.webp)

Exemple : Pour 'Warhammer", entrez "War"

## Comment je fais pour installer un module ?

La méthode est identique à l'installation des systèmes, sauf que tout se passe dans l'onglet des modules.

## On m'a passé un lien vers un system.json ou un module.json. J'en fais quoi ?

Ces liens sont en fait une ancienne méthode d'installation des systèmes et modules, obsolète depuis plus de 3 ans désormais. Donc, en priorité, vérifiez bien que votre système ou module n'est pas déja présent dans le "Marketplace" de Foundry (cf les 2 questions juste au-dessus sur l'installation des systèmes/modules). De plus, ces liens JSON pointent assez souvent sur des systèmes/modules plus mis à jour depuis longtemps, et qui ne fonctionneront donc pas sur votre installation de Foundry. Dans le doute, venez en discutez sur le Discord FR de Foundry. 

**Priviligiez toujours l'installation via le Markeplace de Foundry plutot que par des liens de .json.**

Cependant, il peut arriver parfois que des système/modules ne soient pas présents dans le Marketplace pour diverses raisons : l'auteur ne veut pas le publier, le contenu est "privé", etc, etc. Dans ce cas, effectivement, vous pouvez l'installer dans Foundry.

Pour ce faire, ouvrez la fenêtre de recherche de systèmes (ou modules) : 
![install_system.webp](/setup/winstall/install_system.webp)

Puis copier le lien dans le champ en bas de la fenêtre : 
![install_system_03.webp](/setup/winstall/install_system_03.webp)

Puis cliquez sur "Installation"

## On m'a passé un fichier .zip, j'en fais quoi ?

Il peut arriver que l'on vous fournisse un .zip contenant par exemple un module de contenu fait par un membre du Discord. Pour l'installer, il faut le dezipper dans le répertoire `Data/modules` de votre installation, comme indiquer dans "*Comment sont stockées les données  ?*" plus bas.

Une fois décompressé dans ce répertoire, redemarrez Foundry, et activez le module dans le monde souhaité. Et voilà !

## J'utilise l'appli Foundry et c'est très lent ou bien j'ai des messages d'erreurs

C'est assez fréquent : il est conseillé d'utiliser un navigateur externe plutot que celui intégré à l'application Foundry, car celui-ci peut devenir incompatible après des mises à jour de votre OS (Mac/Windows) ou de votre driver graphique. Le principe : 

1 - lancer l'application Foundry
2 - lancer un navigateur  (Chrome, Chromium, Firefox, Edge, Opera, ....)
3 - Se connecter en http://127.0.0.1:30000 ou https://127.0.0.1:30000 (si vous avez laissé le port 30000 par défaut bien sur, sinon à remplacer par le port utilisé)


## Comment mes joueurs peuvent-il se connecter ?

Si vous hébergez chez vous, il faut donner accès à votre PC depuis Internet. Pour cela, la manip est toujours la même : 

1 - Désactiver UpNP dans Foundry (dans les réglages)
2 - Sur votre box Fibre, ouvrir le port TCP 30000. 
3 - Dans le firewall du PC/Mac Foundry, ouvrir le port 30000 TCP
4 - Récupérer votre adresse IP publique (via https://www.monippublique.com/ par exemple)
5 - Votre URL de connexion pour vos joueurse sera : http://<ip_publique_obtenue>:30000 

C'est un exemple résumé, il existe des FAQ plus spécialisées ici https://foundryvtt.wiki/fr/pour-commencer/win et ici https://foundryvtt.wiki/fr/pour-commencer/pbcnx .

Si ça ne fonctionne toujours pas, venez poster un message dans le channel #support-technique du Discord Foundry FR.


## Est-ce qu’il a des coupures audio/video ou autres ?

Tout n’est pas parfait, il faut parfois faire un « refresh » de son navigateur. Pour l’audio et la vidéo intégré, je vous conseille fortement d’ouvrir une session [Jitsi](http://www.jitsi.org ) dans un onglet à côté de celui de Foundry, et de vous connecter via ce canal, ou bien d'utiliser un canal audio sous Discord. Cela permet de faire fonctionner l’audio en permanence, indépendamment du rendu graphique des pages de Foundry.

## Y'a plein de versions, j'y comprends rien.

Foundry est basé sur des paliers et des canaux de "releases" : stable, test, beta et prototype.
Un palier, c'est la première partie des numéros de versions. Par exemple, dans 0.5.3, le palier c'est "0.5". Ou dans 11.0.376, le palier, c'est 11. Dans un palier donné, il y a un ensemble de nouvelles fonctionnalités qui sont implémentées.  Dans un même palier, les derniers numéros indiquent les versions au sein du palier. Par exemple 0.7.0 est la première version du palier "0.7", puis 0.7.1 la seconde, etc.. Actuellement, en janvier 2024, nous sommes au palier 11.

Tant que le palier n'a pas atteint un niveau stable, les versions sont des versions potentiellement instables : si vous n'êtes pas développeur, il n'est pas conseillé de les installer.

Au sein d'un même palier, les modules et systèmes indiquant le support du palier sont tous compatibles. Autrement dit, si votre système/module marche en 9.239, il marchera en 9.245.

**Règle 1** : Ne jamais mettre à jour juste avant une partie
**Règle 2** : Si vous n'êtes pas développeur, n'installez pas les releases prototype, beta et test. Attendez les tests "tardives" ou les premières "stables".
**Règle 3**  : Au sein d'un même palier, placez vous toujours sur la dernière version du palier.

De plus, le versionnage des systèmes et modules est complètement libre pour leurs auteurs, et donc indépendant de la numérotation de Foundy. On peut donc avoir Foundry en version 11, avec un système en v7 (le cas de Warhammer au 28/01/24, par exemple), et un module modules affichant une version 10. C'est totalement indépendant, et c'est normal.

Pour faire un parallèle, c'est comme sous Windows avec ses applications : c'est pas parce que vous avez "Windows 11" que tout les softs affichent une version "11".

## Mon JDR « Borglur of Daemon of Death Doomesque » est pas dispo ! Que faire ?

Plusieurs options sont possibles. La première est d’utiliser le module générique. La seconde, qui n’empêche d’ailleurs pas la première, est d’aller sur le Discord de Foundry et de demander si quelqu’un est pas en train de bosser dessus. Et ensuite, si vous avez des connaissances Javascript (et éventuellement CSS), vous pouvez vous lancer dans le développement du système. 
Efin, il existe un système dédié à la création de JDR sans développer : Custom System Builder. Son auteur est français, et un canal dédié est disponible sur le Discord de Foundry FR, avec pas mal de membres capables de vous aider.
Donc au final, nul doute qu’en parlant sur le Discord, vous trouverez des personnes prêtes à vous aider. Bon peut être pas pour Borglur of Daemon of Death Doomesque, mais pour les autres, probablement.

## Je veux développer mon système ou module, ou en traduire un existant. Je fais comment ?

Tout d'abord, il faut s’inscrire sur le Discord de FoundryVTT. Je sais, je me répète, mais c’est vraiment indispensable pour l’entraide. Ensuite, je dirais qu’il faut pour l’instant avoir des connaissances (ou les apprendre) dans les 3 domaines suivants :

- Lire/écrire en anglais à peu près (à peu près, hein, pas d’inquiétude non plus) correctement, principalement pour comprendre la documentation, les principes et les messages échangés sur Discord.
- Connaître les bases de la programmation en Javascript (ou avoir des bases de programmation dans un autre langage)
- Connaître un peu le principe des CSS. Ce point n’est pas obligatoire tant que vous ne voulez pas changer l’apparence graphique par défaut.

Autrement dit, c’est certain, si vous avez déjà une expérience en programmation (et encore mieux en programmation Web), ça sera beaucoup plus facile 🙂 ! Ensuite, le bon point d’entrée, c’est de regarder le code d’un module/système proche de celui que vous voulez réalisez sous Gitlab ou Github, puis de partir de là comme base.

Je suis – raisonnablement – disponible sur le Discord de Foundry (@LeRatierBretonnien), vous pouvez me contacter sur ce type de sujet en message privé.
 
## J'ai ajouté des données à ma partie, et je veux les partager. j'ai le droit ?

Si c’est bien vous qui avez librement créé TOUT le contenu qui se voit et se lit (le texte, les images, les vidéos, le code source des modules…) celui qui s’entend (les sons/musiques) et que ces contenus portent l’empreinte de votre personnalité (on dit alors qu’ils sont originaux) alors vous êtes dans la meilleure des configurations : vous pourrez montrer, y faire jouer et même diffuser/distribuer votre contenu puisque vous disposez a priori de droits d’auteur sur celui-ci.

Si vous vous trouvez dans une autre configuration, par exemple :

- vous avez crée le contenu en travaillant avec plusieurs autres personnes et/ou
- vous avez à la fois créé du contenu original ET repris du contenu créé par d’autres personnes et/ou
- vous n’étiez pas complètement libre de votre création : par exemple parce que vous avez réalisé une traduction quasi littérale de contenus réalisés par d’autres, sans réelle part créative personnelle, ou que vous avez travaillé sous la dictée de quelqu’un sans prendre de réelles initiatives personnelles ou encore lorsque vous étiez contraint par d’autres paramètres assez exigeant qui ont bridé votre créativité et/ou
- ce que vous avez réalisé ne porte pas vraiment l’empreinte de votre personnalité mais se borne plutôt à un travail répétitif, procédurier, un travail au kilomètre etc.

Tout n’est pas perdu pour autant mais vos possibilités de montrer le contenu en question, d’y faire jouer et bien sûr de le diffuser/distribuer sans prendre de risques juridiques commencent à devenir incertaines.
Dans ce cas si vous êtes encore motivé(e), il vaudrait mieux vous abstenir de rendre votre travail public avant d’avoir consulté un juriste spécialisé (les avocats spécialisés en propriété intellectuelle et les conseils en propriété industrielle sont les experts de référence).

Si vous dépendez du droit français, et que vous craignez d’être sanctionnés pour avoir montré/diffusé la propriété intellectuelle de quelqu’un d’autre, alors il reste deux méthodes pour vous rassurer :

1. soit vous passez un pacte de non-agression avec ceux qui pourraient exercer des représailles juridiques contre vous (autrement dit un **contrat de licence d’utilisation** ou éventuellement une **autorisation de représentation** avec les ayant-droits du scénario/module/système).  
C’est ce qui se passe de manière transparente (et épouvantablement mal fait d’un point de vue de la forme) lorsque vous vous payez un contenu auprès d’un éditeur. 
C’est aussi une étape fastidieuse mais importante que vous feriez mieux d’entreprendre avant de diffuser un module publiquement (même à titre gratuit) - n’hésitez pas à vous faire conseiller par un juriste pro pour ce tout dernier exercice d’équilibriste.

1. soit vous ne passez pas de contrat mais profitez d’une particularité du droit d’auteur qui existe en France : **l’exception de représentation dans le cercle de famille**.  
Il s’agit d’une situation dans laquelle vous pouvez effrontément vous passer de l’avis d’un auteur (ou d’un ayant droit ou encore d’un co-auteur du travail sur lequel vous avez oeuvré) pour MONTRER ce qu’elle/il a créé à d’autres gens, en l’occurence votre cercle familial avec qui vous allez jouer à votre partie de jdr en ligne.

Veillez quand même à connaitre et bien respecter les paramètres de cette exception légale car si vous en franchissez les limites, vous vous retrouverez sous la grêle froide du délit de contrefaçon :


- a) vous ne faites que **montrer** ce que d’autres auteurs ont fait pendant la partie (hors partie, vous ne remettez pas de copie nouvelles de leurs créations à vos joueurs, vous ne vous remettez pas de copies de modules intégrant des oeuvres de tiers). Bien évidemment vous pouvez montrer vos créations 100% perso à qui vous le souhaitez sans avoir besoin de cette exception.

- b) ne proposez de partie **qu’à votre cercle rapproché**. Les juges considèrent généralement qu’il s’agit uniquement : « des personnes parentes ou amies proches unies de façon habituelle par des liens familiaux ou d'intimité ». On sent que les « vagues potes » ou les « clients du MJ payant en ligne » ne sont pas couverts. Quel magnifique plaidoyer pour nouer de belles amitiés et échanger des anecdotes intimes via Discord !

- c) la partie doit être **privée** : pas de stream switch, de spectateurs n’appartenant pas à votre cercle proches, pas d’actual play ou de redif sinon vous perdez le bénéfice de l’exception.

- d) la partie dois être **gratuite**. Si vous faites payer vous n’êtes plus couvert par l’exception : débrouillez-vous autrement.

- e) ce n’est pas explicite mais des gens ont déjà eu des surprises avec ce dernier critère : **la source** de l’oeuvre** d’un tiers que vous montrez à vos joueurs **doit être licite**. Pas de module tombé du camion (illicite car recel), de copie de fond de garage (illicite car contrefaites) etc.

## Je suis paumé ... ça ne fonctionne pas !

Dans ce cas, vous aurez un support rapide et fiable en allant sur le Discord de Foundry. Vous n’avez rien à installer, tout se passe via un navigateur. Il existe plusieurs canaux de discussion, donc essayez de choisir le bon. Si votre problème est lié à l’installation, ben… #installation. Si c’est lié à un JDR (ie un système), et bien #systeme-discussion. Si c’est un #module-discussion. Et si c’est une question générale, plutôt dans #vtt-discussions ou #testing-question. Vous pouvez me contacter sur ces canaux en préfixant votre message avec @LeRatierBretonnien.

## J'ai le soft, mais mes joueurs ne peuvent pas se connecter

La règle pour déterminer les origines des soucis c'est ça : 

1 - Tout les joueurs ont **tous** des problèmes de connexions -> Le problème est côté serveur (donc chez celui héberge typiquement).
2 - Soit **1 ou 2 joueurs** (toujours les mêmes) ont des problèmes de connexions -> le problème est chez eux. Cela peut venir de leur connexion à eux, de leur PC ou de toute autre merdouille informatique. 

Si le problème est côté serveur, il faut vérifier votre bande passante et probablement faire des essais sans UPnP.

Si le problème est côté joueurs, il est souvent du soit à leur mauvaise connexion internet (et là on y peut pas grand chose), soit à un PC trop ancien/faiblard. Dans ce dernier cas, faites leur baisser le framerate à 15fps dans les réglages de Foundry.

Une FAQ dédiée à ces problèmes est dispo ici https://foundryvtt.wiki/fr/pour-commencer/pbcnx

## Comment sont stockées les données  ?

FoundryVTT utilise 2 répertoires : un répertoire d'installation et un répertoire de données. Ces deux répertoires sont bien séparés, ce qui fait que lorsque vous mettez à jour Foundry, **aucune de vos données de mondes/systèmes/modules ne sont effacées**.

Par défaut, sous Windows, le répertoire des données est situé là : `C:\Users\ton_nom_d_utilisateur\AppData\Local\FoundryVTT\Data.`. Il contient : 
- config : La config de votre installation (notamment la clé de licence, etc),
- modules : Les modules que vous avez installé,
- systems : Les systèmes de jeu que vous avez installé,
- worlds : Vos mondes.

C'est le contenu par défaut. Il est possible que d'autres répertoire soient présents, souvent ajoutés par des modules ou par vous-mêmes : ce n'est pas grave !

Ce répertoire Data est donc précieux, c'est lui que vous pouvez sauvegarder par exemple pour conserver une image complète de vos données. 

## Le répertoire Data mais il est où celui là ?

Ce répertoire est utilisé par Foundry pour sauvegarder ce qui concerne les systèmes, les modules et les mondes (cf plus haut).

L'emplacement de ce répertoire peut être modifiée dans l'interface de Foundry : 

![fvtt_set_path.webp](/setup/winstall/fvtt_set_path.webp)

Sous Windows, si vous installez la version Windows de Foundry, par défaut c'est : `C:\Users\ton_nom_d_utilisateur\AppData\Local\FoundryVTT\Data`.

Vous pouvez aussi ouvir une fenêtre de l'explorateur Windows et taper `%localAppdata%\FoundryVTT\Data` pour ouvrir directement ce répertoire.

## Je change de PC : je peux récupérer tout mon contenu ?

Oui, bien sûr. Les données de Foundry sont toutes placées au même endroit, dans un répertoire `worlds`, sous le répertoire `Data`.
Donc changer de PC équivaut à recopier votre répertoire `worlds`, tout simplement.
Cela sauvegardera toutes vos parties.
Si vous voulez en plus récupérez vos systèmes et modules installés, il suffit de faire la même chose avec les répertoires `systems` et `modules`.
Autrement dit, si vous voulez êtrecertain de tout tout tout sauvegarder, faire une sauvegarde du répertoire `Data` en entier (et qui contiendra donc systems, modules, worlds, backups, etc)
Pour remettre les données sur votre nouveau PC, recopier le contenu de ce `Data` ou de `worlds` à la place des répertoires `Data` ou `worlds` (respectivement) de votre nouveau PC. Relancez Foundry, et vous aurez tout récupéré.
Notez que vous pouvez parfaitement avoir une installation de Foundry sur votre PC/Mac local (pour préparer vos parties par exemple) et une installation en ligne sur un serveur (TheForge par exemple), tout ça avec la même licence.

## Je veux faire des sauvegardes de mes parties, ça marche comment ?

Comme dit au paragraphe précédent, les données de vos parties sont toutes placées au même endroit, dans un répertoire `worlds`, sous le répertoire `Data`.
Donc faire une sauvegarde équivaut à recopier votre répertoire `worlds` et à l'archiver quelque part, tout simplement.
A partir de la version 11, il y a une fonctionnalité de sauvegarde intégrée à Foundry : elle permet de sauvegarder, des mondes, des systèmes, des modules etc...

## On m'a donné un lien vers un fichier "system.json" ou "module.json". J'en fais quoi ?

La méthode nominale pour installer un système ou un module, c'est de passer par l'interface de Foundry. Mais il se peut que certains systèmes/modules ne soient pas déclarés officiellement dans Foundry. Dans ce cas, il faut prendre le lien URL vers ce fichier JSON (appelé 'Manifest) et le copier dans la zone suivante : 
![install_module_json.webp](/setup/winstall/install_module_json.webp)

1 - Allez dans cette section en bas de la fenêtre d'installation d'un module/systeme
2 - Copier le lien vers le fichier JSON
3 - Cliquez sur le bouton

Votre système ou module est alors prêt à être utilisé.

## Une de mes joueuses/joueurs ne voit pas les scènes (ie maps), tout est noir. C'est grave ?

En général, c'est le signe que le navigateur a un conflit avec le driver du GPU (ie la carte graphique). Essayez les étape suivantes : 

- Mettre à jour le navigateur utilisé
- Mettre à jour le driver de la carte graphiqu
- Vérifier qu'il reste 4 Go de disponible sur le disque C: (si vous êtes sous Windoze)
- Ré-essayer

Si le problème persiste, essayez avec un autre navigateur : Chromium, Firefox, Chrome, Edge, Opera

Si le problème est toujours présent, vérifiez la résolution de votre image de la scène : si la carte graphique de votre joueuse/joueur est ancienne ou faiblarde, certaines résolutions au-dessus de 8000x4000 ne s'affichent plus. Si c'est le cas, vous n'avez pas d'autres choix que de réduire les résolutions des scènes concernées (ou acheter une autre carte graphique à votre joueuse/joueur :) )

## J'ai un message qui m'indique que mon navigateur ne supporte pas WebGL. C'est grave ?

FoundryVTT fait une utilisation importante de la technologie WebGL qui permet un affichage graphique évolué dans les navigateurs Web. Si vous avez ce message, c'est que votre navigateur ou votre driver de carte graphique n'est pas à jour. Commencez à mettre à jour ces 2 éléments.
Si le probème persiste, essayez avec un autre navigateur jusqu'à ce que ce message ne soit plus affiché.

## Le temps de chargement pour arriver à la première scène est très long, c'est normal ?

En général, non. Cela peut venir de plusieurs facteurs : 
- Votre monde contient énormément de PNJs, scènes, journaux, etc. Il vaut mieux ne conserver que ce dont vous avez besoin pour vos 2-3 sessions à venir, et pas plus. Si vous voulez conserver malgré tout des données plus anciennes, créez un compendium dans le monde, copiez  vos PNJs/scènes/journaux dedans et supprimez les du monde.
- Nettoyez régulièrement votre historique de tchat. Avec certains systèmes, cet historique peut avoir une taille de plusieurs dizaines de mega-octets.
- Evitez les images PNG pour vos tokens, portraits et scènes. Préferrez des images JPEG ou (encore mieux) des images WEBP.

## J'ai un Raspberry Pi, est-ce que je peux installer Foundry dessus ?

Vous pouvez parfaitemet installer la partie serveur de Foundry dessus, mais pas la partie cliente (ie depuis un navigateur).

Un tuto en anglais est disponible ici : https://foundryvtt.wiki/en/setup/hosting/raspberry-pi

En cas de soucis, plusieurs membres du Discord FR ont installé Foundry sur leur Pi, donc allez faire un tour dans le canal #support-technique.

## Accéder à une feuille de personnage + l'affecter à un joueur

Après que le Maitre de Jeu a créé les comptes des utilisateurs pour la partie dans Foundry, et après que les personnages aient été créés et donc que les feuilles des Personnages Joueurs sont prêtes, il faut que les joueurs aient les droits d'y accéder et les affecter nominativement. L'affectation des personnages à un joueur n'est pas indispensable pour que le joueur puisse accéder et modifier la feuille de personnage mais plusieurs fonctionnalités de Foundry ou de certains systèmes de jeu ne fonctionneront pas correctement sans cela. 
2 étapes :
1. Donner les droits d'accès des personnages aux joueurs
Dans l'onglet des Acteurs, faire un clic-droit sur la feuille de personnage et choisir l'option "Configurer les droits". Dans la fenêtre qui apparait, il faut mettre le niveau de visibilité "Propriétaire" pour le joueur concerné. Ensuite sauvegarder les modifications. Maintenant le joueur peut consulter et modifier cette feuille de personnage. Mais elle ne lui est pas encore affectée. Répéter cette action pour chaque Personnage Joueur.
2. Affecter chaque personnage à un joueur
En bas à gauche de Foundry, ouvrir la liste des joueurs (elle n'affiche, en mode réduit, que les joueurs connectés) afin de voir tous les comptes des joueurs. Faire un clic droit sur le compte d'un joueur et choisir l'option "Configuration du joueur". Dans la fenêtre qui apparait, sélectionner le personnage et enregistrer la configuration. Répéter cette action pour chaque Personnage Joueur.
Maintenant chaque joueur a une feuille de personnage qui lui est affectée. 