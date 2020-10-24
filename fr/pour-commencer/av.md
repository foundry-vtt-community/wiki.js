---
title: 3. Audio, Vidéo, HTTPS
description: 
published: true
date: 2020-10-24T10:22:32.907Z
tags: 
editor: markdown
dateCreated: 2020-10-23T17:47:31.756Z
---

Lorsque nous parlons ici d'Audio/Vidéo dans Foundry VTT, nous entendons par la, un mode de visio conférence intégré à la VTT.
- Soyons clair dès le départ,pour les MJs, l'utilisation Audio/Vidéo, voir de Foundry VTT en général, est une ressource réservée aux MJs ayant une très bonne connexion comme la Fibre Optique voir la VDSL.
Pour ceux en ADSL/ADSL2+ cela devient plus compliqué et un serveur VPS ou service tierce, comme The Forge par exemple, sera conseillé.

- Dans tous les cas, pour activer ce mode, plusieurs paramètres doivent être pris en compte et malheureusement cette partie n'est pas la plus simple qu'il soit. 
**Pour cela il faudra :**
	- *Une Adresse IPv4 fixe,*
	- *Un nom de domaine,*
	- *Un accès à votre Box Internet,*
	- *Une connexion à votre serveur sécurisée (l’URL commence par « http**s** »),*
	- *Donner les droits aux joueurs,*
	- *Une Webcam.*
	- *Une licence Foundry VTT*

## Adresse IPv4 fixe
En ce qui concerne l'adressage IP, nous vous conseillons d'utiliser l'adressage IPv4 et de désactiver sur votre Box Internet ou sur votre Machine, l'adressage IPv6.
En fonction de votre Fournisseur d'Accès Internet (FAI) et de votre connexion, vous aurez le droit à un adressage IPv4 fixe ou dynamique. Je vous invite à consulter le site de [lafibre.info](https://lafibre.info/dyndns/les-ip-fixes-ou-dynamiques-des-operateurs-francais/) pour les opérateurs français.
- Si vous êtes dans le cas d'un adressage IPv4 en dynamique vous serez obligé de changer manuellement votre adresse IP sur le site de votre Nom de Domaine ou de passer par un service de DynDNS.

## Nom de domaine
- Une adresse Internet ou nom de domaine est l'équivalent de votre adresse postale sur Internet. C'est la manière dont vos contacts et clients vont trouver votre site Internet sur le web. Un nom de domaine est donc indispensable lors de la création de votre site web !

- Une adresse Internet se compose d'un préfixe "www" (world wide web) et d'un nom de domaine. Ce nom de domaine est lui-même composé d'une chaîne de caractères et d'une extension (TLD - Top Level Domain). Dans l'exemple ci-dessous, l'extension utilisée est relative à la France : le .fr.
![nom-de-domaine.jpg](/setup/winstall/nom-de-domaine.jpg)

- Dans Foundry VTT, votre nom de domaine vous permet d'avoir une connexion sécurisée et de rendre ainsi fonctionnel l'Audio/Vidéo.

- Il est possible d'avoir un **Nom de Domaine** gratuitement par l'intermédiaire du site [Freenom](https://www.freenom.com/fr/index.html?lang=fr). ( à voir si freenom a les réglages suffisants)
- Pour ma part, et cela reste un avis personnel, je préfère m'orienter vers [OVH](https://www.ovh.com/fr/domaines/), afin d'avoir mon propre Nom de domaine ([OVH, Extensions et Tarifs](https://www.ovh.com/fr/domaines/tarifs/)).

## Connexion sécurisée, HTTPS
### Parlons un peu boutique
L'abréviation HTTPS signifie littéralement **"HyperText Transfer Protocol Secure"**. Ce "protocole de transfert hypertexte sécurisé" combine le protocole de communication client-serveur, ou HTTP, avec un certificat d'authentification du site exploré.

- *À quoi sert le HTTPS ?*
Le protocole HTTPS est une garantie pour un internaute de naviguer sur un site Internet respectueux des règles de confidentialité. Le visiteur est alors assuré de la fiabilité d'un site sur lequel il peut être amené à communiquer des données personnelles. C'est dans le cadre des transactions financières que le HTTPS trouve sa plus grande application, même s'il n'est pas rare, aujourd'hui, de voir des réseaux sociaux ou des services de courriers électroniques s'en doter ; de nombreuses boutiques en ligne utilisent d'ailleurs ce système de sécurité.

- *Comment fonctionne le HTTPS ?*
Connectés par défaut au port TCP 443, les serveurs HTTPS associent le HTTP à un algorithme de chiffrement de type **Secure Sockets Layer (SSL)** ou **Transport Layer Security (TLS)**. Le SSL et le TLS constituent des protocoles qui permettent de sécuriser les échanges sur Internet. Développés par Netscape puis par l'Internet Engineering Task Force, ces protocoles permettent une vérification du site visité via la délivrance d'un certificat d'authentification attribué par une autorité indépendante. Parmi les objectifs de sécurité assurés par le HTTPS figurent notamment l'authentification du serveur, l'intégrité et la confidentialité des données échangées.

### Foundry VTT & le HTTPS
Pour avoir l'audio/vidéo fonctionnel sur Foundry VTT il faut un serveur sécurisé SSL.
Seul hic, sur les machines locales, avec le client Foundry classique, le process implique généralement un certificat SSL auto-signé qui provoque un warning pour les clients navigateurs et potentiellement le blocage des images et autres ressources par certains antivirus.

### Box Internet et Ouverture du Port 80.
Afin que vous puissiez utiliser l'audio/vidéo avec Foundry VTT, nous allons devoir utiliser la [redirection de port](https://fr.wikipedia.org/wiki/Redirection_de_port) (ou port forwarding) sur votre Box Internet.
Pour cela, il vous faudra vous connecter à votre Box Internet.
- **En IPv4**, il faudra dans un premier temps :
	- `rediriger le port 30000 vers le port 80 en TCP.`

### Générer un Certificat SSL
Pour générer un Certificat SSL, nous allons utiliser le Logiciel [Crypt-LE](https://github.com/do-know/Crypt-LE/releases) dans sa version 64bits.
- Le logiciel Crypt-Le qui va gérer les intéractions avec la plateforme [Let's Encrypt](https://letsencrypt.org/fr/). 
- Let's Encrypt est une autorité de certification à but non lucratif fournissant leurs certificats TLS à plusieurs millions de sites Web. Let's Encrypt permet d'avoir une certification valide qu'il faudra renouveler tous les 3 mois. Vous serez prévenu par mail quelques jours avant la fin de l'échéance afin de pouvoir faire votre renouvellement.

Afin de générer le Certificat SSL, nous allons placer l'exécutable Crypt-LE dans un répertoire spécifique avec le nom :
- `C:\Users\<NomDeVotreProfil>\AppData\Local\FoundryVTT\Data\.well-known\acme-challenge`
*(Le chemin d'accès ci-dessus correspond, au chemin d'accès aux ressources utilisateur par défaut. Si vous avez changé votre chemin d'accès aux ressources utilisateur, il faudra remplacer **C:\Users\<NomDeVotreProfil>\AppData\Local\FoundryVTT** par votre propre chemin d'accès)*

- Lancer foundry VTT. Lors des opérations suivantes, il faut absolument que Foundry VTT en fonction.




