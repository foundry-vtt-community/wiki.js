---
title: Core-system
description: Support pour le Core-system
published: true
date: 2020-10-19T20:42:37.189Z
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

Une petite note sur la charge demandée : la solution fait clairement porter la charge (en puissance machine) sur le client (rendu de l'éclairage dynamique, etc) donc un serveur peut être 'léger' (offre T2 micro d'aws par ex. ou même un raspberry pi), par contre cela nécessite de fait une machine assez récente et surtout un prérequis de supporter WebGL (accélération graphique matérielle).  
 
## Avoir un beau core français 
beaucoup de sport, du vin modérement ... euh non je m'égare :
Il faut simplement installer le module de traduction : dans le menu `Add-on Modules` puis `Install Module` rechercher `Translation: French [core]` et installer le. 

Ensuite : 
- pour les versions 0.6.x (et inférieur, bientôt obsolète) il faut que le GM **et** chaque Joueurs activent chacun la langue par défaut une fois en partie : `Configure Settings` puis sélectionner français dans `Language preference` et ne pas oublier de sauvegarder.

- pour les versions 0.7.x, il faut après avoir récupérer le module, toujours dans `Configuration and Setup`, aller dans le menu `Configuration` puis sur la ligne `Default Language` sélectionner le fichier `French (FRANCE) - fr-FR - Core Game` dans la liste et sauvegarder (cela sera valable pour tout le monde par défaut, mais chacun a possibilité de changer pour son environnement via la méthode ci-dessus).   


## Ce que le core apporte et ce qu'il ne fourni pas  

Todo
