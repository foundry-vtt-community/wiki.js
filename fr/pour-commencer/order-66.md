---
title: ! Mise à jour v9
description: Procédure de mise à jour vers la v9
published: true
date: 2021-12-20T15:39:11.946Z
tags: 
editor: markdown
dateCreated: 2021-12-20T12:19:28.031Z
---

*Lors de la livraison d'une nouvelle version dite "Majeure" de Foundry VTT, tel que le passage de la v7 à la v8, et maintenant à la v9, nous recommandons fortement, et à chaque fois d'ailleurs, de ne pas se precipiter vers la nouvelle version, même si nous comprenons qu'elle devient à sa publication, le nouveau standard pour la VTT.
Nous conseillons plus d'attendre quelques jours, histoire que tous les modules et systèmes gagnent en stabilité d'une part, mais aussi afin d'éviter aux moins experts en Informatique d'entre nous, de ne pas se retrouver plonger dans les méandres d'une VTT dysfonctionnelle.
Pour les fous, les inconscients, les courageux ou les plus téméraires d'entre vous, nous avons décidé de faire une petite procédure, qui nous l'espérons, pourra vous aider, avant que l'irréparable soit commis et que nous devions éponger tous les cris, les S.O.S.*

# Mise à jour d'une version majeure de Foundry VTT

Cette procédure permet de faire le passage, avec un risque minimal, d'une version Majeure à une autre, tout en conservant la possibilité d'un retour vers la version précédente.

> Cette procédure est faite sur un environnement spécifique d'installation sous Windows 10/11 mais s'applique parfait dans un environnement classique d'installation tel que vous pouvez retrouver dans la [Procédure d'installation Windows](https://foundryvtt.wiki/fr/pour-commencer/win). 
> La partie **Sauvegarde** peut s'appliquer à tous les systèmes d'exploitation.
> Avant de vous lancer dans la migration de votre VTT, il est fort conseillé de vérifier que les mises à jour de vos modules et systèmes soient disponibles pour vos différents Mondes. Vous pouvez trouver ces informations sur le site officiel de [Foundry VTT > System & Modules](https://foundryvtt.com/packages/) ou via le [Discord de la communauté francophone](https://discord.gg/pPSDNJk) ou encore via cette [Documentation](https://docs.google.com/spreadsheets/d/1ppPR348igxL75M_G7dWl3otzXYpPwrnj7NVSDP8GmVw/edit#gid=0) {.is-warning} 



## 1. Téléchargement.

En premier lieu, nous allons devoir télécharger les différents exécutables nécessaires en fonction de votre système d'exploitation.

Vous trouverez sur le site [Foundry VTT](https://foundryvtt.com/), dans la Rubrique **"Pourchased Licenses"** de votre profil utilisateur, les différentes sources.

> - Télécharger la version dans laquelle votre VTT est : **Actuellement en version 0.8.9 (build255)**,
> - Télécharger la version cible : **Actuellement en v9 (buildXXX)**
> - Télécharger la dernière version de Node.js : [NodeJS LTS](https://nodejs.org/en/download/) 
*Prendre la dernière version LTS (.msi) de Nodejs pour windows 64-bit.*

![1.software.png](/setup/winstall/1.software.png)

## 2. Sauvegarde.

En deuxième temps, nous allons faire la sauvegarde de l'environnement de travail afin de pouvoir procéder à un éventuel retour en arrière si nécessaire.


Afin de savoir ce que vous devez sauvegarder : 

> - Vérifier que Foundry VTT ne soit pas lancé. Si c'est le cas, fermez l'application proprement afin que toutes les bases de données soient fermées correctement.
> - Ouvrir l'onglet de configuration de Foundry VTT

![2.config.png](/setup/winstall/2.config.png)

> - Repérer la ligne **"Chemin des données utilisateur"**

![3.path.png](/setup/winstall/3.path.png)

> - Ouvrir un explorateur windows (Raccourci clavier : **Touche Windows + E**)
> - Ouvrir le répertoire du Path (ici en exemple **E:\FoundryVTT_Data**)
> - **Sauvegarder les trois répertoires suivants :**
		*1. Config (répertoire contenant les fichiers de configuration de votre VTT)*
    *2. Data ( sans aucun doute, le répertoire le plus important car il comprend en son sein toutes les données de votre VTT comme les répertoires des Modules, des systèmes et vos Worlds)*
    *3. Log (répertoire des journaux d'erreurs et debug)*

![4.windows_explorer.png](/setup/winstall/4.windows_explorer.png)

## 3. Migration.

Lorsque toutes les sauvegardes sont faites, il ne reste plus qu'à faire la mise à jour des logiciels.

> - Installer la dernière version LTS de NodeJS.
> - Installer la nouvelle version de foundry VTT
		*Vous pouvez suivre la [Procédure d'installation Windows](https://foundryvtt.wiki/fr/pour-commencer/win#installation-de-foundry-virtual-tabletop) si nécessaire.*
> - Lancer Foundry VTT et faire les différentes mises à jour des modules et systèmes.

## 4. Restoration d'une version précédente.

Il se peut que votre migration soit un échec en terme de mise à jour de Systèmes & Modules, ce qui est souvent le cas lors de la livraison d'une nouvelle version.

