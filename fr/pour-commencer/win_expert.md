---
title: 1.1. Windows (mode serveur)
description: 
published: true
date: 2022-11-07T14:43:11.029Z
tags: 
editor: markdown
dateCreated: 2021-11-20T17:06:43.550Z
---

Il existe différentes manières de faire l'installation d'un logiciel, comme nous avons pu le voir dans [l'Installation Windows](https://foundryvtt.wiki/fr/pour-commencer/win) de l'application FoundryVTT.
Cette installation de FoundryVTT est une installation en mode serveur, c'est à dire que le logiciel n'utilisera pas d'interface graphique tel que nous avons pu le voir dans l'installation précédente.
L'application ne sera accessible que via un navigateur, de préférence sous Chrome/Chromium, à partir une adresse IP locale ou distante en fonction de votre type d'installation (machine locale ou serveur distant dédié par exemple).

# Installation de FoundryVTT en mode serveur.
>- Cette installation fonctionne aussi bien sur un poste de travail Windows 10/11 qu'un serveur Windows.
>- Nous ne rentrerons pas dans les détails pour certaines parties de l'installation, comme les diverses installations de logiciels dont vous pouvez trouver la liste ci-dessous.
Nous partons du principe que vous êtes "Expert" en informatique et que vous possédez des connaissances supérieures à celle d'un utilisateur final.
{.is-warning}

Contrairement à la procédure précédente, nous aurons besoin d'installation et de télécharger en amont quelques logiciels :
- [PowerShell LTS](https://docs.microsoft.com/fr-fr/powershell/scripting/install/installing-powershell-on-windows) : Prendre la dernière version LTS de Powershell. 
- [FoundryVTT version Linux/Nodejs](https://foundryvtt.com/) : Prendre la dernière version Stable de FoundryVTT pour Linux/Nodejs.
- [NodeJS LTS](https://nodejs.org/en/download/) : Prendre la dernière version LTS (.msi) de Nodejs pour windows 64-bit.


Une fois que vous avez téléchargé les différents exécutables pour Windows, faites en l'installation.
Décompressez l'archive .zip de FoundryVTT, à l'endroit que vous désirez, que cela soit dans une partition dédiée ou dans program files.
>Pour tous les exécutables, merci de faire un Clic droit sur l'exécutable et de les "Exécuter en tant qu'Administrateur".


### Pour l'explication de la procédure, nous allons vous donner un exemple de configuration foundryVTT: 
- Environnement : Windows 11 professional 64-bit
- Foundry VTT (Système), Partition dédiée, emplacement : e:/foundryvtt
- Foundry VTT (Data), Partition dédiée, emplacement : e:/foundryvtt_data

## Héberger Node.js sur Windows avec PM2
Le PM2 est un gestionnaire de processus hautes performances et facile à utiliser pour les noeuds. Il est gratuit, multiplateforme, open source et dispose d'un équilibreur de charge intégré.

### Installation de PM2
Vous devez d'abord installer PM2. Pour que l'installation fonctionne, il faut impérativement avoir installé au préalable NodeJS sur votre machine.
- Ouvrer PowerShell ou le terminal de votre choix en mode Administrateur puis exécutez la commande ci-dessous dans n'importe quel répertoire :
> npm install pm2 -g

#### Installation de PM2 sous Windows 10
Sous Windows 10, vous devez lancer les deux commandes suivantes dans PowerShell en mode administrateur :

> npm install pm2 -g
> Set-ExecutionPolicy RemoteSigned

> **Remarque :** Il préférable de ne pas installer node-windows en même temps de pm2 car des conflits de codes existent entre les deux modules et empêche le lancement de la commande pm2.
{.is-warning}

## Héberger votre application Node.js
Normalement, héberger votre application Node.js avec PM2 est aussi simple que ABC.
- Ouvrez n'importe quel terminal, accédez au répertoire racine de votre projet
- Naviguer dans le répertoire système FoundryVTT jusqu'au fichier **"main.js"** se trouvant dans **".\foundryvtt\resources\app\"**
- Exécutez la commande selon la syntaxe ci-dessous :

>pm2 start .\main.js --namespace foundry

![pm2_start.png](/setup/winstall/pm2_start.png)
- Puis enregistrer la liste actuelle des processus
- exécutez la commande ci-dessous :

>pm2 save

![pm2_save.png](/setup/winstall/pm2_save.png)
Désormais, à chaque redémarrage de la fenêtre, votre liste de processus enregistrée sera à nouveau exécutée. Assurez-vous d'exécuter la commande pm2 save après avoir ajouté de nouveaux processus.

Voila, c'est tout. Votre application est désormais hébergée et accessible sur le port spécifié.

### Autres commandes PM2 utiles
On peut afficher la liste des processus enregistrés par PM2 avec la commande suivante :

>pm2 list

on aura un résultat ressemblant à ceci :
![pm2_list.png](/setup/winstall/pm2_list.png)

- La commande ci-dessous arrêtera le processus spécifié. Vous pouvez spécifier le processus par l'identificateur de processus ou le nom du processus. 
- Pour arrêter le processus par son nom de processus dans le répertoire racine :

>pm2 stop .\main.js

- Pour arrêter le processus par son nom de processus n'importe ou :

>pm2 stop foundry

- Pour arrêter le processus par son identificateur de processus n'importe ou :

>pm2 stop 0

- Pour supprimer un processus, on indique pm2 delete :

>pm2 delete 0

- Pour redémarrer un processus, on indique pm2 restart :

>pm2 restart 0 pm2 --update-env

- Pour lancer le moniteur de processus de PM2 :

>pm2 monit

on aura un résultat ressemblant à ceci :

![pm2_monit.png](/setup/winstall/pm2_monit.png)

## Exécution de PM2 au démarrage de Windows
On peut exécuter PM2 au démarrage de Windows en installant le paquet ci-dessous :

>npm install pm2-windows-startup -g
>pm2-startup install

## Vérifier que FoundryVTT fonctionne
- Lancer un navigateur
- Entrer l'url suivante :
>http://127.0.0.1:30000

## Paramétrage de Foundry VTT
En ce qui concerne le paramétrage sous windows, je vous invite à vous référer à la procédure suivante et de la dérouler à partir de [Pare-feu et redirection de Port sur votre Box Internet.](https://foundryvtt.wiki/fr/pour-commencer/win#pare-feu-et-redirection-de-port-sur-votre-box-internet) et jusqu'à la fin.



