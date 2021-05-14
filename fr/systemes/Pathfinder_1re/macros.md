---
title: 1.0. Les Macros
description: 
published: true
date: 2021-04-21T16:46:38.076Z
tags: 
editor: markdown
dateCreated: 2021-02-27T10:38:48.179Z
---

# Macros

Retrouvez sur cette page la documentation pour toutes les macros incluses dans le module `pf1-fr` *(compendium `Macros PF1`)*.

## Activer Défense Totale

Cette macro active/désactive l'effet `Défense Totale` sur le personnage dont le jeton (token) est sélectionné. Cela permet d'appliquer rapidement les effets de cette position de combat.

**Étapes**

* **Prérequis** : assurez-vous d'avoir importé la macro utilitaire `effet` ![effet.png](/community/french/pf1/macros/effet.png =18x)
1. Sélectionnez un acteur (que vous possédez) sur la scène active
1. Exécutez la macro
1. Une icône apparaît sur le jeton pour indiquer que l'effet est désormais actif

**Comment ça fonctionne ?**

* La macro utilise `effet` pour rechercher l'effet `Défense Totale` dans le compendium `Effets` et l'ajouter sur le personnage sélectionné
* Si l'effet existe déjà sur le personnage, la macro inverse son état (désactive si activé, active si désactivé)


## Annoncer l'utilisation d'une aptitude/sort

Cette macro affiche un texte dans les messages pour annoncer l'utilisation d'une aptitude/sort récurrent et réduit le nombre d'utilisation pour la journée. Cette macro a été conçue pour la représentation bardique, permettant ainsi à un barde, à chaque tour, de réduire le nombre de rounds restant et permettre aux autres joueurs d'appliquer les effets de l'`inspiration vaillante`. La macro peut être réutilisée pour d'autres contextes (ex: rage du barbare).

![annoncer-utlisation.png](/community/french/pf1/macros/annoncer-utlisation.png)

**Étapes**

* **Prérequis** : assurez-vous que le personnage dispose d'un sort (ex: spell-like) avec un nombre d'utilisations restantes supérieur à `0`.
* **Configurations** :
  * configurer la macro en spécifiant le nom de l'aptitude/sort (`ABNAME`)
  * ajuster le message à afficher (`MESSAGE`) en fonctionne de vos besoins. `##` sera remplacé par le nombre d'utilisations restantes.
1. Sélectionnez un acteur (que vous possédez) sur la scène active
1. Exécutez la macro
1. Le texte s'affiche désormais dans les messages et le nombre d'utilisation est réduit de `1`.

**Comment ça fonctionne ?**

* La macro recherche dans le personnage sélectionné un élément dont le nom correspond à celui spécifié (configuration)
* La macro vérifie le nombre d'utilisations restantes (`preparedAmount`) puis le réduit de `1`
* La macro affiche le texte demandé

## Aperçu des PJs

Cette macro est dédiée au maître du jeu (MJ). Elle présente une vue d'ensemble de tous les attributs et compétences des personnages joueurs. Elle permet également de rapidement faire un jet pour un pj ou pour l'ensemble du groupe.

![pc-overview.png](/community/french/pf1/macros/pc-overview.png =500x)

**Étapes**

* **Prérequis** : assurez-vous d'avoir assigné un personnage à chaque joueur
1. Exécutez la macro
1. Passez votre souris sur la fenêtre pour mettre en évidence une ligne
1. Cliquer sur le nom d'un attribut ou d'une compétence pour lancer les dés pour tout le groupe
1. Cliquer sur la valeur d'un attribut ou d'une compétence pour lancer les dés pour le pj correspondant

**Comment ça fonctionne ?**

* La macro récupère le personnage associé à chacun des joueurs, puis affiche toutes les valeurs pour ceux-ci sous la forme d'une table
> Si un des joueurs contrôle plusieurs personnages, seul celui qui lui est associé dans la configuration des joueurs apparaitra dans le tableau. Pour remédier à ce comportement, il est possible de modifier la macro afin de changer la sélection des `actors` par défaut et sélectionner par exemple tous les `actors` de type `character`(PJ) et dont le droit par défaut est positionné à `Observateur`. Pour cela, situez la ligne suivante en fin de macro :
> {.is-info}

> `game.users.forEach( function(u) { if( !u.isGM ) { actorIds.push(u.data.character) } } )`

> Une fois la ligne trouvée, remplacez-la par la ligne suivante  :
> {.is-info}

> `game.actors.forEach( function(a) { if (a.data.type == "character" && a.data.permission.default == "2") { actorIds.push(a.id) } } )`

* Pour les jets supportés, la macro ajoute un événement pour réagir au clics. Le curseur de la souris change de forme lorsque c'est le cas.
* Il est possible de configurer les compétences à afficher en modifiant la constante `SKILLS`
* Il est possible de configurer les éléments à afficher en modifiant la constante `DISPLAY`
* Il est possible de modifier le format d'affichage de chaque type de valeur en modifiant les fonctions de la macro
