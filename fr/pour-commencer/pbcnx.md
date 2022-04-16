---
title: 5.1 Mes joueurs ne se connectent pas
description: 
published: true
date: 2022-04-16T07:37:22.816Z
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
5. Sur certaines box, dans ces paramètres NAT, il vous est parfois demandé "Adresse IP Source" (ou un truc du du genre, avec "Source" dans le titre). Dans ce cas, il faut laisser "Toutes" ou "All", et ne surtout pas mettre une adresse IP.
6. Rebootez votre box (optionnel, mais conseillé)
7. Essayez de vous connectez avec l'adresse IP externe de votre box, suivie de ':30000'. Vous pouvez obtenir votre adresse IP publique en allant sur https://www.monippublique.com/ .

## Ca marche toujours pas

Ca peut venir de plusieurs choses. Vérifiez bien que vous avez une adresse IP fixe et que vous pouvez ouvrir le port 30000 auprès de votre opérateur. Ensuite, venez faire un tour sur le Discord, section #support-technique.

Vou pouvez aussi consulter la page https://foundryvtt.wiki/fr/pour-commencer/win, qui contient de détails supplémentaires et des exemples.

## Avez vous un VPN sur le PC ? 

Si oui, vérifiez bien qu'une tache lui appartenant ne s'execute pas en fond. Si oui, assurez de bien le/les stopper avant de ré-essayer. 

## Mon opérateur me dit que mes ports sont possibles que de ZZZZZ à UUUUUU

Bon fichtre, c'est pas très grave. Si par exemple votre opérateur vous dit que vos ports valides sont de 42000 à 50000, choisissez un port dans cette plage. Prenons 42100 par exemple.

Dans ce cas, votre port externe devient 42100, à mettre en port externe/WAN dans les réglages du NAT de la box, mais laissez 30000 partout ailleurs (ie port interne, port de Foundry, port du firewall du PC Foundry).

Votre adresse IP publique sera donc http://xxx.zzz.www.uuu:42000/

## Ben ca marche toujours pas (v2) ?

Il nous faut découvrir si le problème vient de votre box ou de votre PC/Mac Foundry. Pour se faire, ce petit process dichotomique peut vous aider : 

1 - Prenez un autre PC/tablette chez vous (ou taxez en un à votre voisine ou à votre grand-mère)
2 - Connectez vous à votre PC/Mac Foundry sur le réseau  local à partir de ce second PC/tablette. Il faut naturellement mettre l'@ IP locale de votre PC/Mac Foundry (ie genre http://192.168.1.www:30000/ )
3 - Je vois la page Foundry : Ca marche ! => le problème est sur la box. Vérifiez votre redirection de port et l'adresse IP de votre PC/Mac Foundry. Normalement, vous devriez voir 30000 -> 30000, TCP, @IP local de PC/Mac Foundry
4 -> Je vois pas la page Foundry: Ca marche pas :( => le problème est sur le PC/Mac Foundry. Vérifiez bien que le port 30000/TCP est ouvert dans le firewall. Essayez également en désactivant temporairement votre anti-virus (si vous en avez un), juste pour tester si ce n'est pas lui qui rajoute une couche de blocage.
5 - Ca permet déja de cibler le défaut, et en général, le problème est ensuite rapidement résolu 

Bon si y'a pas du tout de second matos dispo, c'est plus dur, c'est sûr.


