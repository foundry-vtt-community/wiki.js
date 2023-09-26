---
title: 5.1 Mes joueurs ne se connectent pas
description: 
published: true
date: 2023-09-26T14:11:07.958Z
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

- D'une box Fibre (ou ADSL, mais attention au débit montant, souvent insuffisant). **Si vous avez un modem 4G/5G (ou une connexion 4G/5G) cela ne pourra jamais fonctionner, à cause de l'opérateur. Dans ce cas, il faudra vous tourner vers des hébergements tiers de type TheForge.**
- De l'adresse IP local de votre PC/Mac qui fait fonctionner Foundry. En général, c'est une adresse en 192.168.1.X ou 192.168.0.X.
- De vos mots login/mot de passe de configuration de votre box (vous devez les avoir quelque part)

## Comment ?

1. Lancez Foundry et vérifiez que le port externe est bien à 30000 et que la case à cocher UPnP est **décochée**
2. Sur votre PC/Mac, autorisez le port 30000/TCP entrant dans votre firewall. Si vous ne savez pas le faire, cherchez "Ouvrir un port TCP dans le firewall de Windows" (ou Mac) dans votre moteur de recherche favori.
3. Connectez vous à l'interface de configuration de votre box, et cherchez une section s'appelant NAT/PAT (ou redirection de ports, ou quelque chose d'équivalent). Si vous ne savez pas commen faire, cherchez "Comment ouvrir un port TCP dans ma box XXXXXX" dans votre moteur de recheche favori (avec XXXX étant la marque/modèle de votre box).
4. Une fois sur votre box, cherchez le menu d'ouverture de port (en général "Port Mapping", NAT, PAT "Redirection de ports", etc). Dans cette interface, vous pouvez saisir le port externe TCP (30000), le port interne TCP (30000) et l'adresse IP de votre PC/Mac (sur certaines box, vous pouvez choisir dans une liste déroulante).
5. Sur certaines box, dans ces paramètres NAT, il vous est parfois demandé "Adresse IP Source" (ou un truc du du genre, avec "Source" dans le titre). Dans ce cas, il faut laisser "Toutes" ou "All", et ne surtout pas mettre une adresse IP.
6. Rebootez votre box (optionnel, mais conseillé)
7. Essayez de vous connectez avec l'adresse IP externe de votre box, suivie de ':30000'. Vous pouvez obtenir votre adresse IP publique en allant sur https://www.monippublique.com/ .

**RAPPEL** : Chaque box internet a une interface différente, il nous est impossible de donner une recette magique pour toutes les interfaces et modèles de box. Cependant, une recherche sur le net permet en général de se dépanner : "Comment ouvrir un port TCP sur ma box XXXXX", avec XXX = le nom de votre fournisseur.

## Ca marche toujours pas

Ca peut venir de plusieurs choses. Vérifiez bien que vous avez une adresse IP fixe et que vous pouvez ouvrir le port 30000 auprès de votre opérateur. Essayez également la méthode dichomotique présentée dans la dernière section de cette FAQ (ie tout en bas de cette page). Ensuite, venez faire un tour sur le Discord, section #support-technique.

Vou pouvez aussi consulter la page https://foundryvtt.wiki/fr/pour-commencer/win, qui contient de détails supplémentaires et des exemples.

## Vérifiez votre adresse IP externe

Dans certains cas, l'adresse IP externe que vous obtenez dans FoundryVTT est erronée. Vérifiez la via un site extene, par exemple https://myexternalip.com/ 

## Avez vous un VPN sur le PC ? 

Si oui, vérifiez bien qu'une tache lui appartenant ne s'execute pas en fond. Si oui, assurez de bien le/les stopper avant de ré-essayer. 

## Avez vous un anti-virus intrusif ?

Certains antivirus (comme Avast par exemple) viennent par défaut bloquer les ports, en plus du firewall. Si vous avez ce type d'antivirus, faites un essai en le désactivant temporairement.

## Mon opérateur me dit que mes ports sont possibles que de ZZZZZ à UUUUUU

Bon fichtre, c'est pénible, mais pas très grave. Si par exemple votre opérateur vous dit que vos ports valides sont de 42000 à 50000, choisissez un port dans cette plage. Prenons 42100 par exemple.

Dans ce cas, votre port externe devient 42100, à mettre en port externe/WAN dans les réglages du NAT de la box, mais laissez 30000 partout ailleurs (ie port interne, port de Foundry, port du firewall du PC Foundry).

Votre adresse IP publique sera donc http://xxx.zzz.www.uuu:42000/

## Je vois pas de NAT (ou redirection de port) dans ma box, ou bien j'ai un message qui m'indique que la redirection n'est pas disponible

Cela veut dire que votre opérateur utilise un mode GCNAT, et qu'il interdit l'ouverture de port (pour faire simple). Votre seule option dans ce cas est d'appeler le service support de votre opérateur (oui, je sais...) et de demander une IP "full stack".

Dans le cas de Free, cette IP "full stack" peut-être obtenue directement via l'interface Web de votre compte en ligne sur free.fr.

Une fois cette IP "full stack" obtenue, vous pouvez reprendre les étapes de "Comment ?" ci-dessus.

## Ben ca marche toujours pas (v2) ?

C'est la méthode dichotomique qui va permettre de localiser le problème. Il nous faut découvrir si le problème vient de votre box ou de votre PC/Mac Foundry. Pour se faire, ce petit process dichotomique peut vous aider : 

1 - Prenez un autre PC/tablette chez vous (ou taxez en un à votre voisine ou à votre grand-mère)
2 - Connectez vous à votre PC/Mac Foundry sur le réseau  local à partir de ce second PC/tablette. Il faut naturellement mettre l'@ IP locale de votre PC/Mac Foundry (ie genre http://192.168.1.www:30000/ )
3 - Je vois la page Foundry : Ca marche ! => le problème est sur la box. Vérifiez votre redirection de port et l'adresse IP de votre PC/Mac Foundry. Normalement, vous devriez voir 30000 -> 30000, TCP, @IP local de PC/Mac Foundry
4 -> Je vois pas la page Foundry: Ca marche pas :( => le problème est sur le PC/Mac Foundry. Vérifiez bien que le port 30000/TCP est ouvert dans le firewall. Essayez également en désactivant temporairement votre anti-virus (si vous en avez un), juste pour tester si ce n'est pas lui qui rajoute une couche de blocage.
5 - Ca permet déja de cibler le défaut, et en général, le problème est ensuite rapidement résolu 

Bon si y'a pas du tout de second matos dispo, c'est plus dur, c'est sûr.


