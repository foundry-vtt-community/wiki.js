---
title: Rêve de Dragon pour FoundryVTT
description: 
published: true
date: 2021-01-29T00:09:19.133Z
tags: 
editor: markdown
dateCreated: 2020-11-26T16:29:39.001Z
---

# Rêve de Dragon
Présentation du système Rêve de Dragon pour FoundryVTT.

## Installation
Installer le système de jeu depuis:

https://foundryvtt.com/packages/foundryvtt-reve-de-dragon/


## Créer un personnage
Dans l'onglet des  acteurs, créer un Acteur, lui donner un nom, et choisir le type personnage . La feuille de personnage s'ouvre. Vopus auriez aussi pu choisir de créer une créature, ou une entité. Pour les races humanoïdes, nous utilisons la feuille de personnage classique.
![feuille_personnage.png](/images/reve-de-dragon/feuille_personnage.png)

Petit guide rapide:
1. Portrait du personnage. Cliquez dessus pour sélectionner l'image à utiliser.
1. Nom du personnage
1. Compteurs de vie, endurance, fatigue, case sonné, et compteur de rêve actuel. Passer la souris au dessus de la fatigue permetr de voir combien de cases de fatigue sont cochées.
1. Boutons d'actions pour le personnage. passez la souris au dessus pour afficher leur fonction.
	- Encaiser des dommages: utile pour encaisser des dommages de chute, brûlure, etc. Pour les dommages de combat, comme vous verrez plus tard, c'est intégré à la gestion des combats.
	- Remise à neuf: rétablit la santé du personnage à son maximum. Ce bouton n'est accessible qu'au Gardien des rêves.
	- Dormir une heure: permet de récupérer la fatigue, et d'effectuer le jet de récupération de rêve si besoin.
	- Chateau Dormant: permet de gérer l'heure de Chateau Dormant. Ce qui est fait: jets de constitution pour les blessures et la vie, transformation de stress, jet de moral neutre, récupération de chance actuelle, le rêve redescend vers le seuil si besoin.
	- Montée normale dans les TMR: ouvre la fenêtre des Terres Médianes du Rêve, et permet au joueur de se déplacer à vitesse normale. Un point de rêve est dépensé.
	- Montée accélérée dans les TMR: ouvre la fenêtre des Terres Médianes du Rêve, et permet au joueur de se déplacer à vitesse normale. Deux points de rêve sont dépensé.
	- Regarder les TMR: permet de voir où se trouve le demi-rêve, les sorts en réserves, et autres marqueurs des terres médianes.
1. Indicateurs informatifs rappelant l'état général, le malus de fatigue, un résumé des blessures, l'état d'encombrement et effets actifs.
1. Onglet des caractéristiques
1. Onglet des compétences et de l'archétype
1. Onglet du combat et des blessures
1. Onglet des savoirs et des tâches, qui regroupe par exemple les recettes de cuisines, alchimiques, chants, etc.
1. Onglet du Haut-rêve, où l'on trouve le seuil de rêve, les sorts, et ce qui est lié au haut-rêve, ainsi que les queues, têtes, et souffles de dragon.
1. Onglet de l'équipement qui permet d'organiser son sac, gérer l'argent, les armes et armures équipées.
1. Onglet description du personnage pour les traits physiques, notes du joueur et du Gardien.

### Quelques détails sur la santé et les effets actifs

* Les effets actifs correspondent à ce qui affecte le personnage (positionné sur le token), et qui peuvent causer une demi-surprise ou une surprise totale.
* Etre sonné ou renversé place un effet actif (et demi surprise), être dans les TMR cause aussi un effet actif de demi-surprise.
* Etre inconscient donne directement une surprise totale. Deux demi-surprises causent une surprise totale. Le passage de l'endurance à 0 ajoute l'effet "inconscient", remonter au dessus de 0 l'enlève.

### Quelques détails sur les récupérations
* Les récupérations de blessures, de vie, d'endurance, et de fatigue sont liées. Une blessure grave limite l'endurance maximale, un point de vie manquant aussi, l'endurance manquante limite la récupération de fatigue.
* les jets de constitutions sont faits si le nombre de jours requis sont écoulés. Les ajustements notés sur la blessure sont pris en compte: si une herbe de soin est appliquée qu qu'un bonus de soins complets est obtenu, notez le bonus à cet endroit.
* L'âge des blessures est augmenté lors du clic sur chateau dormant, pas en fonction du calendrier. Il faudra donc passer deux heures de Chateau Dormant pour commencer à tenter le jet de consitution pour récupérer.
* Un jet de constitution réussi rétrograde la blessure correspondante d'un stade.
* La transformation de stress diminue le compteur de stress et incrémente l'expérience disponible en conséquence, dans le commpteur juste en dessous. Le joueur pourra manuellement diminuer cette expérience pour augmenter la compétence de son choix.


## Au fil du jeu


### Jets de caractéristique
Dans Il suffit de cliquer sur le nom de la car

### Jets de compétences

### Tâches

### Autres actions

## Combats


