---
title: ! Mise à jour v9
description: Procédure de mise à jour vers la v9
published: true
date: 2021-12-20T12:19:28.031Z
tags: 
editor: markdown
dateCreated: 2021-12-20T12:19:28.031Z
---

# Mise à jour d'une version majeure de Foundry VTT

Cette procédure permet de faire le passage, avec un risque minimal, d'une version Majeure à une autre, tout en conservant la possibilité d'un retour vers la version précédente.



## 1. Téléchargement

En premier lieu, nous allons devoir télécharger les différents exécutables nécessaires en fonction de votre système d'exploitation.

Vous trouverez sur le site [Foundry VTT](https://foundryvtt.com/), dans la Rubrique **"Pourchased Licenses"** de votre profil utilisateur, les différentes sources.

> - Télécharger la version dans laquelle votre VTT est : **Actuellement en version 0.8.9 (build255)**,
> - Télécharger la version cible : **Actuellement en v9 (buildXXX)**
> - Télécharger la dernière version de Node.js : [NodeJS LTS](https://nodejs.org/en/download/) 
*Prendre la dernière version LTS (.msi) de Nodejs pour windows 64-bit.*

![1.software.png](/setup/winstall/1.software.png)

## 2. Sauvegarde

En deuxième temps, nous allons faire la sauvegarde de l'environnement de travail afin de pouvoir procéder à un éventuel retour en arrière si nécessaire.

Afin de savoir ce que vous devez sauvegarder : 

> - Ouvrir l'onglet de configuration de Foundry VTT

![2.config.png](/setup/winstall/2.config.png)

> - Repérer la ligne **"Chemin des données utilisateur"**

![3.path.png](/setup/winstall/3.path.png)

> - Ouvrir un explorateur windows (Raccourci clavier : Touche Windows + E)
> - Ouvrir le répertoire du Path (ici en exemple **E:\FoundryVTT_Data**)