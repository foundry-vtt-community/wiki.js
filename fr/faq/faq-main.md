---
title: FAQ Foundry
description: 
published: true
date: 2022-03-01T06:48:24.200Z
tags: 
editor: markdown
dateCreated: 2020-10-20T11:11:27.841Z
---

## FoundryVTT, c'est quoi ?
FoundryVTT est un logiciel permettant de jouer à des jeux de rôles sur table en mode connecté. La table physique devient ainsi "virtuelle", et le MJ et ses joueurs communiquent à distance via micro et webcam. [Un choix de cet outil expliqué ici](http://www.lahiette.com/leratierbretonnien/warhammer/table-virtuelles-de-jdr/)

## OK, ça ressemble vachement à Roll20
Oui, tout à fait, c'est dans la même catégorie de logiciels. Il existe à mon sens deux grandes catégories de logiciels de table virtuelles : les "clients lourds" et les applications webs. Les clients lourds sont des applications (des .exe quoi, pour les Windoziens) à installer sur<strong> chaque</strong> poste des joueurs. Les applications Web nécessitent un serveur (soit auto-hébergé, soit hébergé par un tiers) et le MJ et les joueurs utilisent simplement un navigateur (Chrome conseillé) pour y accéder. FoundryVTT  est dans la catégorie des applications web, comme Roll20 ou AstralTableTop.

## Du coup c'est quoi la différence avec Roll20 ?
FoundryVTT a démarré son développement notamment car l'auteur principal ne trouvait pas son compte avec Roll20. Il a donc conçu FoundryVTT comme une plateforme générique de gestion de règles de JDR, quelle qu'elles soient. Ainsi, tout système de JDR est vu comme un plugin, à installer en plus de Foundry. De plus, FoundryVTT offre une grande liberté pour son installation et sa gestion : soit en local, soit en serveur privé, soit encore en hébergement tiers. J'y reviendrais plus bas. Enfin, son modèle de licence est ultra-simple et appréciable : vous achetez le logiciel, est c'est à vous une fois pour toute. Pas d'abonnement, rien, nada. Vous achetez, et c'est à vous.

## Très bien, mais je vois que le truc est en bêta, comment qu’on l’achète ?

A ce jour (mai 2021), Foundry est en version 0.7.9, achetable en 1 fois pour la somme de 60$. Une réduction est appliquée pour les développeurs et les supporters de Patreon, basés sur la durée de support (le soft est gratuit au final pour les développeurs et ceux présents depuis plus d’1 an grosso-modo).

## Putain, 60$ ?

C’est peut-être effectivement cher pour vous – et je peux le comprendre – mais pensez que ça correspond probablement à 3 ou 4 mois de votre abonnement de smartphone, et qu’une fois cette somme dépensée, le soft est à vous à vie. Comparez également avec un abonnement en ligne d’1 an chez Roll20 ou AstralTableTop. Enfin, pensez que vous allez pouvoir héberger tous vos jeux sur une seule licence, et que vous n’aurez que vos propres limites en matière de stockage de cartes, dessins, images, etc. Quand à ceux qui, par principe, cherchent à ne jamais dépenser quoi que ce soit, pour rien, et qui veulent toujours grappiller le moindre bout de machin – et ce sont ceux qui curieusement sont également les plus exigeants – , et bien vous pouvez arrêter de lire cette FAQ, l’outil n’est pas pour vous. Ce blog non plus, d’ailleurs, pendant que j’y pense 🙂 !

## 60 $ ? Et les joueurs doivent payer aussi ?

Bon malgré le fait que la réponse à cette question se trouve tout simplement en gros et gras à plusieurs d’endroits du site de Foundry, c’est la question qui revient quasiment le plus souvent. Bref, je dirais RTFM. Donc, non bien sûr les joueurs ne payent pas. Une seule licence permet de faire jouer autant de joueurs que vous voulez à votre table. La seule contrainte est qu’il ne peut y a voir qu’1 seul jeu actif à un moment donné.

## Admettons que je l’achète. Est-ce que je dois re-payer pour les mises à jour ?

Non, les mises à jours de bugfixes et releases sont comprises dans le prix d’achat. Ce qui va peut-être venir en supplément (et je dis peut-être, on en sait rien), ce sont des éventuelles licences officielles, signées avec les éditeurs, et venant avec un module complet, et pour lesquels l’éditeur demande lui même une rétribution. Mais tant que que l’on joue avec les modules et systèmes disponibles, il n’y a rien d’autre à payer que le prix d’achat initial.

## J’y connais rien en informatique, je peux m’en servir quand même ?

Si vous savez ouvrir des ports dans votre routeur ADSL/Fibre (tapez ‘ouvrir des ports dans son modem ADSL‘ dans votre moteur de recherche…) et que vous savez installer un exécutable, il est probable que vous saurez vous en servir. Si tout ceci est déjà trop compliqué ou chiant (:)) pour vous, il faudra attendre les offres d’hébergement tiers qui vont arriver, comme The Forge. Sinon, si un de vos joueurs est un peu informaticien, il peut probablement vous aider dans cette tâche. Il faut compter 5-6 Mbs d’upload montant pour héberger une partie chez vous, donc il est possible que vous ayez besoin d’un serveur tiers si votre connexion est lente. Il existe de l’aide à ce sujet sur le Wiki de Foundry. Un certains nombres d’utilisateurs présents sur le Discord FR de Foundy sont également capables d’apporter du support sur ces sujets.

## Je n'ai pas de serveur, je peux le mettre sur mon PC ?

Oui, bien sûr, c’est l’un des 3 modes d’installation. Dans ce cas, vous avez un .EXE qui va transformer votre PC en serveur Foundry. Vous aurez à ouvrir le port TCP 30000 (par défaut), et à l’autoriser dans votre firewall. Plus d’infos ici.

## Et il faut de la bande passante ?

Oui, un peu, surtout en montant. Si vous jouez avec 4-5 joueurs, il vous faudra au moins 4 Mbs montant (je dis bien montant) pour jouer confortablement. Donc si vous avez la fibtre ou un "gros" ADSL, ca doit marcher. Sinon, il faut vous orienter vers un hébergement tiers.

## J’ai déjà un serveur, comment je fais ?

Bon si vous avez ça, l’installation ne devrait vous poser de problèmes, je vous conseille les liens suivants : 
- https://foundryvtt.com/article/hosting/ 
- https://foundryvtt.com/article/audio-video/
- https://foundry-vtt-community.github.io/wiki/Ubuntu-VM/

## Et il n'y a pas de services en lignes déjà disponibles ?

Si, il y en a, notamment avec avec TheForge (https://forge-vtt.com/, le plus populaire à l'heure actuelle) et FoundryServer (https://www.foundryserver.com/ ).

## Y’a quoi comme jeu dessus ?

Dans Foundry, les « jeux » sont appelés « systems ». Au 01/02/2022, on recense 180+ « systems » : 
- 13th age,
- Appel de Cthulhu, DeltGreen
- Cthlulhu Hack
- Chroniques Oubliées
- Aliens
- D&D5,
- Tales From The Loop
- Anneau Unique
- Fate, 
- Pathfinder 1/2, 
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

## C’est tout en anglais ce machin !

Oui, c’est vrai, les auteurs étant majoritairement américains ou anglo-saxons. Mais des traducteurs traduisent petit à petit les contenus : ainsi l’interface générale du jeu est traduite bar « Baktov », ainsi que les systèmes D&D5 (Baktov également), Shadowrun, Starfinder, Savage Worlds (ça c’est bibi) et Warhammer 4 (ça c’est bibi aussi). Cependant, pour l’instant, il faut reconnaître qu’une partie importante du support et des documentations sont en anglais : si vraiment cette langue est pour vous hors de portée, je crains qu’il ne vous faille attendre… ou utiliser un autre outil, plus francisé.

## J’ai vu qu’il y avait des modules, c’est quoi ?

Les modules sont des extensions, comme des plugins, qui viennent ajouter des fonctionnalités au logiciel. Ces modules viennent donc ajouter des compléments aux « systèmes », en offrant des aides pour les MJ et les joueurs : 
- effets spéciaux, 
- navigation dans les fichiers images, 
- communication, etc, etc. 

A chacun de venir piocher ce qu’il lui plaît !

## Bon OK, mais pourquoi s’intéresser autant à cet outil ? Et pas aux autres ?

Mon choix – partial s’il en est – est expliqué sur [cette page](http://www.lahiette.com/leratierbretonnien/warhammer/table-virtuelles-de-jdr/). De plus, sur le fond, je pense que l’on est un peu tous responsable des hégémonies technologiques que l’on observe : Facebook serait-il devenu Facebook si on avait pas cédé à la « masse » et donner sa chance à un autre outil ? Idem pour Windows, Google et autres. On se plaint de l’hégémonie de tel ou telle société/outil/méthode, mais nous sommes – je m’inclus là dedans – souvent les premiers à l’entretenir. Donc à un tout petit niveau, à l’échelle excessivement modeste des « tables virtuelles », j’essaye également très modestement de faire connaître un outil auquel je trouve des qualités techniques et de jeu assez abouties pour éviter que Roll20 ne devienne le choix par défaut, fautes d’autres combattants. L’objectif est bien entretenir le choix, pas de rentrer dans des batailles trollesques de qualité/défaut de chaque outil ! 🙂

## Est-ce qu’il a des coupures audio/video ou autres ?

Tout n’est pas parfait, le soft est en fin de bêta et a encore quelques bugs, il faut parfois faire un « refresh » de son navigateur. Pour l’audio et la vidéo intégré, je vous conseille fortement d’ouvrir une session [Jitsi](http://www.jitsi.org ) dans un onglet à côté de celui de Foundry, et de vous connecter via ce canal. Cela permet de faire fonctionner l’audio en permanence, indépendamment du rendu graphique des pages de Foundry.
Beaucoup utilisent également l'audio via un serveur Discord.

## Y'a plein de versions, j'y comprends rien.

Foundry est basé sur des paliers et des canaux de "releases" : alpha, beta et release.
Un palier, c'est la première partie des numéros de versions. Par exemple, dans 0.5.3, le palier c'est "0.5". Dans un palier donné, il y a un ensemble de nouvelles fonctionnalités qui sont implémentées.  Dans un même palier, les derniers numéros indiquent les versions au sein du palier. Par exemple 0.7.0 est la première version du palier "0.7", puis 0.7.1 la seconde, etc..
Tant que le palier n'a pas atteint un niveau release (stable), les versions sont soient en alpha, soit en beta.

Au sein d'un même palier, les modules et systèmes indiquant le support du palier sont tous compatibles. Autrement dit, si votre système/module marche en 0.7.5, il marchera en 0.7.7.

**Règle 1** : Ne jamais mettre à jour juste avant une partie
**Règle 2** : Si vous n'êtes pas développeur, n'installez pas les releases alpha ni les premières beta. Attendez les betas "tardives" ou les premières "releases".
**Règle 3**  : Au sein d'un même palier, placez vous toujours sur la dernière version du palier.



## Mon JDR « Borglur of Daemon of Death Doomesque » est pas dispo ! Que faire ?

Plusieurs options sont possibles. La première est d’utiliser le module générique. La seconde, qui n’empêche d’ailleurs pas la première, est d’aller sur le Discord de Foundry et de demander si quelqu’un est pas en train de bosser dessus. Et ensuite, si vous avez des connaissances Javascript (et éventuellement CSS), vous pouvez vous lancer dans le développement du système. Nul doute qu’en parlant sur le Discord, vous trouverez des personnes prêtes à vous aider. Bon peut être pas pour Borglur of Daemon of Death Doomesque, mais pour les autres, probablement.

## Je veux développer mon système ou module, ou en traduire un existant. Je fais comment ?

Tout d'abord, il faut s’inscrire sur le Discord de FoundryVTT. Je sais, je me répète, mais c’est vraiment indispensable pour l’entraide. Ensuite, je dirais qu’il faut pour l’instant avoir des connaissances (ou les apprendre) dans les 3 domaines suivants :

- Lire/écrire en anglais à peu près (à peu près, hein, pas d’inquiétude non plus) correctement, principalement pour comprendre la documentation, les principes et les messages échangés sur Discord.
- Connaître les bases de la programmation en Javascript (ou avoir des bases de programmation dans un autre langage)
- Connaître un peu le principe des CSS. Ce point n’est pas obligatoire tant que vous ne voulez pas changer l’apparence graphique par défaut.

Autrement dit, c’est certain, si vous avez déjà une expérience en programmation (et encore mieux en programmation Web), ça sera beaucoup plus facile 🙂 ! Ensuite, le bon point d’entrée, c’est de regarder le code d’un module/système proche de celui que vous voulez réalisez sous Gitlab ou Github, puis de partir de là comme base.

Je suis – raisonnablement – disponible sur le Discord de Foundry (@LeRatierBretonnien), vous pouvez me contacter sur ce type de sujet en message privé.

## Je suis paumé ... ça ne fonctionne pas !

Dans ce cas, vous aurez un support rapide et fiable en allant sur le Discord de Foundry. Vous n’avez rien à installer, tout se passe via un navigateur. Il existe plusieurs canaux de discussion, donc essayez de choisir le bon. Si votre problème est lié à l’installation, ben… #installation. Si c’est lié à un JDR (ie un système), et bien #systeme-discussion. Si c’est un #module-discussion. Et si c’est une question générale, plutôt dans #vtt-discussions ou #testing-question. Vous pouvez me contacter sur ces canaux en préfixant votre message avec @LeRatierBretonnien.

## Bah finalement, on peut faire la même chose avec un de meeting vidéo et partage d’écran

Oui, tout à fait, et ça marche assez bien aussi. Les outils intégrés comme Roll20, Astral, FantasyGrounds, Foundry etc offrent toutefois des capacités des gestion des scènes, des actions, des PNJs qui sont difficilement atteignables avec un simple outil de meeting. Mais l’important c’est de jouer de toute manière.

## J'aime pas jouer en ligne, c'est de la m..., alors ça me sert à rien

Cete attitude connue et un brin rétrograde est compréhensible, mais à titre perso je maitrise plus sur table réelle que virtuelle, et j'utilise Foundry dans tout les cas. C'est devenu un précieux outil de gestion et d'animation des parties dont je ne saurais plus me passer sur table désormais. Plus de détails ici : http://www.lahiette.com/leratierbretonnien/foundryvtt-pour-table-reelle/ 

## Bon je veux bien regarder, il n'y a pas une vidéo de démonstration ?

La modernité veut systématiquement des vidéos pour tout et n’importe quoi, donc OK, allons-y :
[cette vidéo est fréquemment citée comme une excellente introduction](https://www.youtube.com/watch?v=kEQlhdF1568&list=PLGgCMB0gYnLFWxyrCkUYwHY4vvA_yME7m)

## J'ai le soft, mais mes joueurs ne peuvent pas se connecter

La règle pour déterminer les origines des soucis c'est ça : 
1 - Tout les joueurs ont **tous** des problèmes de connexions -> Le problème est côté serveur (donc chez celui héberge typiquement).
2 - Soit **1 ou 2 joueurs** (toujours les mêmes) ont des problèmes de connexions -> le problème est chez eux. Cela peut venir de leur connexion à eux, de leur PC ou de toute autre merdouille informatique. 

Si le problème est côté serveur, il faut vérifier votre bande passante et probablement faire des essais sans UPnP.
Si le problème est côté joueurs, il est souvent du soit à leur mauvaise connexion internet (et là on y peut pas grand chose), soit à un PC trop ancien/faiblard. Dans ce dernier cas, faites leur baisser le framerate à 15fps dans les réglages de Foundry.