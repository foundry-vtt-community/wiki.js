---
title: ! Mise à jour v10
description: Procédure de mise à jour vers une monté de version
published: true
date: 2025-05-04T09:26:43.573Z
tags: 
editor: markdown
dateCreated: 2021-12-20T12:19:28.031Z
---

*À l’occasion de la publication d’une nouvelle version dite « majeure » de Foundry VTT — et plus généralement pour toute mise à jour — nous vous recommandons de ne pas vous précipiter pour effectuer la migration. Bien que cette version devienne aussitôt la nouvelle référence de la plateforme, il est préférable d’attendre quelques jours.*

*Ce délai permet aux modules et systèmes de se stabiliser, et réduit le risque pour les utilisateurs moins familiers avec les aspects techniques de se retrouver confrontés à une instance de Foundry VTT instable ou partiellement fonctionnelle.*

*Pour celles et ceux qui souhaitent néanmoins procéder immédiatement à la mise à jour, nous avons mis à disposition une procédure dédiée. Nous espérons qu’elle vous sera utile et qu’elle vous évitera des complications qui nécessiteraient ensuite une assistance en urgence.*

# Mise à jour d'une version majeure de Foundry VTT

Cette procédure permet de faire le passage, avec un risque minimal, d'une version Majeure à une autre, tout en conservant la possibilité d'un retour vers la version précédente.

> Cette procédure est faite sur un environnement spécifique d'installation sous Windows 10/11 mais s'applique parfait dans un environnement classique d'installation tel que vous pouvez retrouver dans la [Procédure d'installation Windows](https://foundryvtt.wiki/fr/pour-commencer/win). 
> La partie **Sauvegarde** peut s'appliquer à tous les systèmes d'exploitation.
> Avant de vous lancer dans la migration de votre VTT, il est fort conseillé de vérifier que les mises à jour de vos modules et systèmes soient disponibles pour vos différents Mondes. Vous pouvez trouver ces informations sur le site officiel de [Foundry VTT > System & Modules](https://foundryvtt.com/packages/) ou via le [Discord de la communauté francophone](https://discord.gg/pPSDNJk) ou encore via cette [Documentation](https://docs.google.com/spreadsheets/u/2/d/e/2PACX-1vRrcQL-r09Bi4MsbHUXlmOVx6DP4Ju143zRmk3HiUK2qU6gA3naxuSUcyv3EVhjMThXzJ_455jnyWfK/pubhtml?gid=0&single=true) {.is-warning} 



## 1. Téléchargement.

En premier lieu, nous allons devoir télécharger les différents exécutables nécessaires en fonction de votre système d'exploitation.

Vous trouverez sur le site [Foundry VTT](https://foundryvtt.com/), dans la Rubrique **"Pourchased Licenses"** de votre profil utilisateur, les différentes sources.

> - Télécharger la version dans laquelle votre VTT est : **Actuellement en version X.X.X (buildXXX)**,
> - Télécharger la version cible : **Actuellement en vXX (buildXXX)**

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

> - Installer la nouvelle version de foundry VTT
		*Vous pouvez suivre la [Procédure d'installation Windows](https://foundryvtt.wiki/fr/pour-commencer/win#installation-de-foundry-virtual-tabletop) si nécessaire.*
> - Lancer Foundry VTT et faire les différentes mises à jour des modules et systèmes.

## 4. Restoration d'une version précédente.

Il se peut que votre migration soit un échec en terme de mise à jour de Systèmes & Modules, ce qui est souvent le cas lors de la livraison d'une nouvelle version.

> Pour cela, il suffit de : 
> - Supprimmer ou désinstaller la version en cours,
> - Supprimmer ou renommer le répertoire Data,
> - Installer la version antérieure, que vous avez téléchargé au préalable,
> - Restorer vos anciennes Data depuis votre Sauvegarde.
		*Si, vous avez une installation custom, il faudra sans doute lancer une première fois foundry afin de modifier le path dans le **"Chemin des données utilisateur"** et lui indiquer l'emplacement de vos datas. 
    Redémarrer foundry afin de prendre en compte le changement et de pointer sur le bon répertoire Data.*

