---
title: 1.1. Installation Windows (mode serveur)
description: 
published: true
date: 2021-11-20T17:06:43.550Z
tags: 
editor: markdown
dateCreated: 2021-11-20T17:06:43.550Z
---

Il existe différentes manières de faire l'installation d'un logiciel, comme nous avons pu le voir dans [l'Installation Windows](https://foundryvtt.wiki/fr/pour-commencer/win) de l'application FoundryVTT.
Cette installation de FoundryVTT est une installation en mode serveur, c'est à dire que le logiciel n'utilisera pas d'interface graphique tel que nous avons pu le voir dans l'installation précédente.
L'application ne sera accessible que via un navigateur, de préférence sous Chrome/Chromium, à partir une adresse IP locale ou distante en fonction de votre type d'installation (machine locale ou serveur distant dédié par exemple).

## Nous allons vous présenter ici, d'installation de FoundryVTT en mode serveur.
>- Cette installation fonctionne aussi bien sur un poste de travail Windows 10/11 qu'un serveur Windows.
>- Nous ne rentrerons pas dans les détails pour certaines parties de l'installation, comme les diverses installations de logiciels dont vous pouvez trouver la liste ci-dessous.
Nous partons du principe que vous êtes "Expert" en informatique et que vous possédez des connaissances supérieures à celle d'un utilisateur final.
{.is-warning}

Contrairement à la procédure précédente, nous aurons besoin d'installation et de télécharger en amont quelques logiciels :
- [PowerShell LTS](https://docs.microsoft.com/fr-fr/powershell/scripting/install/installing-powershell-on-windows) : Prendre la dernière version LTS de Powershell. 
- [FoundryVTT version Linux/Nodejs](https://foundryvtt.com/) : Prendre la dernière version Stable de FoundryVTT pour Linux/Nodejs.
- [NodeJS LTS](https://nodejs.org/en/download/) : Prendre la dernière version LTS (.msi) de Nodejs pour windows 64-bit.

Une fois que vous avez téléchargé les différents exécutables pour Windows, faites en l'installation. 
>Pour tous les exécutables, Clic droit sur l'exécutable, "Exécuter en tant qu'Administrateur".
{.is-warning}


### Pour l'explication de la procédure, nous allons vous donner un exemple de configuration foundryVTT: 
- Environnement : Windows 11 professional 64-bit
- Foundry VTT (Système), Partition dédiée, emplacement : e:/foundryvtt
- Foundry VTT (Data), Partition dédiée, emplacement : e:/foundryvtt_data

