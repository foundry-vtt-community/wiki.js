---
title: 5.1 Mes joueurs ne se connectent pas
description: 
published: true
date: 2021-07-25T10:44:47.179Z
tags: 
editor: markdown
dateCreated: 2021-05-14T18:39:22.832Z
---

# Impossible de se connecter à distance

Lorsque vous hébergez Foundry chez vous, si vous n'arrivez pas à vous connecter à distance, c'est probablement parce que le port externe n'est pas ouvert dans votre box ADSL/Fibre.

## Pourquoi ?
Lorsque vous hébergez Foundry chez vous, vous hébergez un serveur, comme si vous hébergiez Roll20 chez vous typiquement. Cela veut donc dire que les joueurs (ie externes) se connectent à votre réseau (interne) : par défaut, ce fonctionnemment est bloqué par les box.
Il faut donc autoriser la connexion à votre serveur Foundry depuis "l'extérieur", et cela veut dire 'ouvrir un port'.

## De quoi j'ai besoin ?

- De l'adresse IP local de votre PC/Mac qui fait fonctionner Foundry. En général, c'est une adresse en 192.168.1.X ou 192.168.0.X.
- De vos mots login/mot de passe de configuration de votre box (vous devez les avoir quelque part)

## Comment ?

1. Lancez Foundry et vérifiez que le port externe est bien à 30000 et que la case à cocher UPnP est **décochée**
2. Sur votre PC/Mac, autorisez le port 30000/TCP en entrant dans votre firewall
3. Connectez vous à l'interface de configuration de votre box, et cherchez une section s'appelant NAT/PAT (ou redirection de ports, ou quelque chose d'équivalent)
4. Dans cette interface, vous pouvez saisir le port externe TCP (30000), le port interne TCP (30000) et l'adresse IP de votre PC/Mac (sur certaines box, vous pouvez choisir dans une liste déroulante).
5. Rebootez votre box (optionnel, mais conseillé)
6. Essayez de vous connectez avec l'adresse IP externe de votre box, suivie de ':30000'. Vous pouvez obtenir votre adresse IP publique en allant sur https://www.monippublique.com/ .

## Ca marche toujours pas

Ca peut venir de plusieurs choses. Vérifiez bien que vous avez une adresse IP fixe et que vous pouvez ouvrir le port 30000 auprès de votre opérateur. Ensuite, venez faire un tour sur le Discord, section #support-technique.

Vou pouvez aussi consulter la page https://foundryvtt.wiki/fr/pour-commencer/win, qui contient de détails supplémentaires et des exemples.



