---
title: Installation sous Raspberry Pi
description: 
published: true
date: 2020-11-28T21:12:59.337Z
tags: 
editor: markdown
dateCreated: 2020-11-28T19:43:36.702Z
---

# Raspberry Pi et Serveur Foundry VTT
# À DESTINATION DE QUI?

Ceux qui n'ont pas envie de payer un hébergement de serveur et qui ne disposent pas d'une connexion suffisante pour héberger depuis l'application (4 à 5 Mbs, soit 32 à 40 Mbps en débit montant donc VDSL puissante, VDSL2 ou fibre).

Pensez à éteindre la RasPi quand vous ne l'utilisez pas, sans quoi votre connexion pourrait être prise par le serveur.

# HÉBÉGER UN SERVEUR FOUNDRY VTT À MOINDRE COÛT

### 1ère ÉTAPE : ACHETER UNE RASPBERRY PI

Rendez vous sur un revendeur de Raspberry Pi (Kubii, Materiel.net, Amazon, ...) et acheter une Rapsberry répondant à la configuration minimale : Raspberry Pi 3 B et supérieur, une alimentation correspondant à la version de la RasPi, une carte microSD et un boitier si vous le souhaitez.

[IMAGE]

Le plus simple : prendre un starter kit Raspberry 3 ou Raspberry 4

### 2ème ÉTAPE : PARAMÈTRER LA RASPBERRY PI

Installer sur microSD l'OS Raspbian et le mettre à jour ou avoir une microSD avec une préinstallation et le mettre à jour. Il y a plein de tuto en ligne pour débuter et paramétrer.

Connecter la WiFi ou se brancher à l'Ethernet.

Pour mettre à jour correctement une RasPi, ouvrez la console, copier ci dessous et dans le terminal faire clic droit, coller et Entrée.

`sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get dist-upgrade -y && sudo rpi-update`

Activer VNC (partage de l'écran) ou SSH (terminal) comme ci dessous si vous souhaitez un contrôle à distance pour vous passer de brancher en hdmi/microhdmi votre RasPi.

[IMAGE]

### 3ème ÉTAPE : INSTALLER Node.js SUR LA RASPBERRY PI

Issu de la doc sur Foundry VTT

Entrer successivement les commandes suivantes dans le terminal pour installer Node.js.
Copier puis clic droit et coller dans le terminal, faire entrée et attendre de pouvoir entrer une nouvelle commande, si vous obtenez en fin de terminal [O/n], taper O puis Entrée.

`sudo apt install -y libssl-dev`
`curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -`
`sudo apt install -y nodejs`

Créer ensuite les répertoires qui vont accueillir les dossiers et fichiers du serveur Foundry VTT

Installer le serveur, remplacer dans la 2ème commande ci dessous <token-de-téléchargement-du-serveur> par le lien copier sur son compte de Foundry VTT dans Purchases Licenses. Garder les guillements

`cd Foundryvtt`
`sudo wget -O foundryvtt.zip "<token-de-téléchargement-du-serveur>"`
`sudo unzip foundryvtt.zip`













