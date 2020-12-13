---
title: Macros
description: Les macros du module `pf1-fr`
published: true
date: 2020-12-13T21:55:05.095Z
tags: pf1, macros
editor: markdown
dateCreated: 2020-12-13T20:46:13.112Z
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

* La macro récupère l'acteur de chacun des joueurs, puis affiche toutes les valeurs sous la forme d'une table
* Pour les jets supportés, la macro ajoute un événement pour réagir au clics. Le curseur de la souris change de forme lorsque c'est le cas.
* Il est possible de configurer les compétences à afficher en modifiant la constante `SKILLS`
* Il est possible de configurer les éléments à afficher en modifiant la constante `DISPLAY`
* Il est possible de modifier le format d'affichage de chaque type de valeur en modifiant les fonctions de la macro
