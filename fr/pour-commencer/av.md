---
title: 3. Audio, Vidéo, HTTPS
description: 
published: true
date: 2020-10-24T07:33:13.624Z
tags: 
editor: markdown
dateCreated: 2020-10-23T17:47:31.756Z
---

Lorsque nous parlons ici d'audio et vidéo dans Foundry VTT, nous entendons par la, un mode de visio conférence intégré à la VTT.
Pour activer ce mode, plusieurs paramètres doivent être activés et malheureusement cette partie n'est pas la plus simple qu'il soit. Pour cela il faudra :
- Un nom de domaine,
- Une connexion à votre serveur sécurisée (l’URL commence par « http**s** »),
- Donner les droits aux joueurs,
- Une Webcam.

## Nom de domaine


## Connexion sécurisée, HTTPS
### Parlons un peu boutique
L'abréviation HTTPS signifie littéralement **"HyperText Transfer Protocol Secure"**. Ce "protocole de transfert hypertexte sécurisé" combine le protocole de communication client-serveur, ou HTTP, avec un certificat d'authentification du site exploré.

- *À quoi sert le HTTPS ?*
Le protocole HTTPS est une garantie pour un internaute de naviguer sur un site Internet respectueux des règles de confidentialité. Le visiteur est alors assuré de la fiabilité d'un site sur lequel il peut être amené à communiquer des données personnelles. C'est dans le cadre des transactions financières que le HTTPS trouve sa plus grande application, même s'il n'est pas rare, aujourd'hui, de voir des réseaux sociaux ou des services de courriers électroniques s'en doter ; de nombreuses boutiques en ligne utilisent d'ailleurs ce système de sécurité.

- *Comment fonctionne le HTTPS ?*
Connectés par défaut au port TCP 443, les serveurs HTTPS associent le HTTP à un algorithme de chiffrement de type **Secure Sockets Layer (SSL)** ou **Transport Layer Security (TLS)**. Le SSL et le TLS constituent des protocoles qui permettent de sécuriser les échanges sur Internet. Développés par Netscape puis par l'Internet Engineering Task Force, ces protocoles permettent une vérification du site visité via la délivrance d'un certificat d'authentification attribué par une autorité indépendante. Parmi les objectifs de sécurité assurés par le HTTPS figurent notamment l'authentification du serveur, l'intégrité et la confidentialité des données échangées.

