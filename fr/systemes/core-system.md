---
title: Core-system
description: Support pour le Core-system
published: true
date: 2020-12-23T14:15:58.814Z
tags: 
editor: markdown
dateCreated: 2020-10-19T09:56:23.255Z
---

# Le core	

## Quelques généralités : 
Par "core" on parle du socle de base (neutre) de Foundry_VTT. 
Cela comprend une partie serveur (nodejs) et une partie cliente (celle que vous voyez dans votre navigateur)  

Ce socle permet 
- soit d'héberger l'application et partager sa connection : exemple sur windows, lors du lancement du .exe, cela lance un serveur et une fenêtre cliente (base chromium), mais vous pouvez tout à fait lancer la consultation 'cliente' dans un autre navigateur (et même avoir deux vues "Maitre du jeu" pour profiter de vos écrans... mais on y reviendra).

- Soit de déployer la partie serveur sur un .. serveur :) et donc de ne pas être pénalisé par une connection faible. 
	Ce déploiement à distance peut être 'manuel' parce que vous êtes un geek qui roxxe du ponay, ou sinon il existe des solutions -payantes- clé en main ( 'the forge' par ex).

Une petite note sur la charge demandée : la solution fait clairement porter la charge (en puissance machine) sur le client (rendu de l'éclairage dynamique, etc) qui nécessitera de fait une machine assez récente et surtout un prérequis de supporter WebGL (accélération graphique matérielle), la partie serveur est beaucoup plus 'légère' (une offre T2 micro d'aws suffit par ex. ou un raspberry pi, un viueux pc recyclé).  
 
## Avoir un beau core français 
beaucoup de sport, du vin modérement ... euh non je m'égare :
Il faut simplement installer le module de traduction : dans le menu `Add-on Modules` puis `Install Module` rechercher `Translation: French [core]` et installer le. 

Ensuite (à partir de la versions 0.7.5), il faut après avoir récupéré le module, toujours dans `Configuration and Setup`, aller dans le menu `Configuration` puis sur la ligne `Default Language` sélectionner le fichier `French (FRANCE) - fr-FR - Core Game` dans la liste et sauvegarder.
Et c'est tout !
Cela définit la langue par défaut du jeu pour tout le monde qui se connecte au serveur (au GM souvent). 
Il reste cependant possible pour chacun individuellement de modifier la langue utilisée : il faut se connecter en partie, puis dans `Configure Settings` (ou `Configuration des options` :) ), sélectionner la langue voulue (disponible) dans `Language preference`/`Choix de la langue` et ne pas oublier de sauvegarder.
*note : cette personnalisation est à manier avec précaution néanmoins, tous les systèmes de jeux ne supporte pas forcément bien le 'multi-langue'.*

## Ce que le core apporte et ce qu'il ne fourni pas  

![en cours](https://www.chorum.fr/wp-content/uploads/2015/09/work-in-progress.png)

FoundyVTT a été concu de manière à être très versatile (on le voit dans ses possibilités d'installation) mais également dans ses possibilités de personnalisation (qui fédère du coup ! ).

le jeu s'articule donc autour de 4 grandes fonctionnalités (qu'on retrouve quasi dans toutes les VTT) : 
1. - le core
2. - les modules 
3. - les sytèmes 
4. - les mondes

### 1 - Le core ###



### 2 - Les modules ###



### 3 - Les systèmes ###




### 4 - Les mondes ###
