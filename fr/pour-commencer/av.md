---
title: 3. Audio, Vidéo, HTTPS
description: 
published: true
date: 2020-10-24T12:04:31.109Z
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

### Box Internet, Ouverture du Port 80.
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

- ***Lancer foundry VTT. Lors des opérations suivantes, il faut absolument que Foundry VTT soit en fonction.***

- Lancer l'invite de commande dans le répertoire contenant l'exécutable Crypt-LE :
	- Sélectionner le répertoire **'acme-challenge'** dans votre Explorateur Windows
  - Cliquez sur **'Fichier'** puis sélectionner **'Ouvrir Windows PowerShell'** et cliquez sur **'Ouvrir Windows PowerShell en tant qu'administrateur'**
  ![powershell.png](/setup/winstall/powershell.png)
  - Dans l'invite de commande, tapez la ligne suivante :
`  .\le64.exe --key account.key --email "mon@email.fr" --csr domain.csr --csr-key domain.key --crt domain.crt --generate-missing --domains "mondomaine.fr" --live`
***ATTENTION - N'appuyez pas sur entrée de suite après avoir lancé la commande.***

- Après quelques instants la commande va se mettre en pause, vous allez devoir de créer un fichier avec un nom particulier contenant une clé :
	- Créer un fichier.txt, éditer son contenu en mettant la clé indiquée dans le message,
  - Renommer le fichier.txt avec le nom particulier indiqué dans le message,
  - Supprimer l'extension ".txt" du fichier, valider le warning windows,
  - Placez ce fichier rempli dans le dossier :
  	- `C:\Users\<NomDeVotreProfil>\AppData\Local\FoundryVTT\Data\.well-known\acme-challenge`


- Lorsque le fichier spécifique est dans le bon répertoire, nous allons vérifier qu'il est bien accessible via votre navigateur :
	- Lancer votre navigateur,
  - taper dans votre navigateur URL suivante :
  	`http://LeNomDeMonDomaine.fr/.well-known/acme-challenge/LeNomDeVotreFichier`
  - Si votre navigateur nous demande de télécharger le fichier, c'est que votre paramétrage de Nom de Domaine et/ou Redirection de port sont valides.
  - Une fois que la vérification de l'accessibilité est valide, revenez sur l'invite de commande et appuyez sur Entrée.
Après quelques instants votre certificat SSL va être validé.

### Box Internet, Redirection du Port 80 vers le Port 443.
Afin que vous puissiez utiliser l'audio/vidéo avec Foundry VTT, nous allons devoir utiliser la [redirection de port](https://fr.wikipedia.org/wiki/Redirection_de_port) (ou port forwarding) sur votre Box Internet.
Pour cela, il vous faudra vous connecter à votre Box Internet.
- **En IPv4** :
	- il faudra supprimer la `redirection du port 30000 vers le port 80 en TCP.`
  - il faudra `rediriger désormais le port 30000 vers le port 443 en TCP.`

### Activation du SSL dans Foundry VTT
Maintenant nous allons devoir activer le SSL dans la VTT.
- Selectionner Foundry VTT qui doit encore tourner en fond de tache sur votre machine
- Cliquez sur l'onglet Configuration et remplisser les champs suivants par :
	- Certificat SSL (Secure Socket Layer) 
  `../Data/.well-known/acme-challenge/domain.crt`
  - Clé privée SSL (Secure Socket Layer)
  `../Data/.well-known/acme-challenge/domain.key`
  ![ssl.png](/setup/winstall/ssl.png)
- Cliquez sur ***Save Changes*** puis ***YES***
- Redémmarrez Foundry VTT, lancer votre navigateur puis rentrez l'URL de votre Nom de Domaine en commançant par HTTPS.

## Paramétrage de l'Audio/Vidéo dans Foundry VTT
Maintenant que le plus gros du travail est fait, nous allon activer l'Audio/Video pour les joueurs.
- Pour que l'Audio/Video fonctionne à travers votre navigateur, vos joueurs devront ***OBLIGATOIREMENT*** posséder une Webcam. Si ce n'est pas le cas, il est toujours possible de se servir de votre smartphone comme Webcam pour le biais d'application telle que [IVCam](https://www.e2esoft.com/ivcam/) ou [Iriun Webcam](https://iriun.com/).
- Dans tous les cas et même si vos joueurs ne veulent pas que l'on voit leur tête, il faudra une Webcam pour que le navigateur vous propose l'utilisation de l'Audio/Vidéo.

Dans Foundry VTT, il vous faudra :
1. Télécharger le module ***'jitsi WebRTC client'***

2. Ouvrir une partie :
- Cliquez sur les engrenages en haut à droite, dernier Onglet, puis **'Gestion des modules'**
	- Cocher Jitsi ***'WebRTC client'***
  - Cliquer sur ***'Sauvegarder les paramètres de modules'***
- Cliquez sur les engrenages en haut à droite, dernier Onglet, puis **'Configuration des Options'**
- Dans la fenêtre **'Configuration générale'**, cliquez sur ***'Ouvrir la Configuration des droits'***
	- Cocher pour Joueur (Player) :
  	1.Autoriser la diffusion Audio
    2.Autoriser la diffusion Vidéo
    3.Sauvegarder
- Dans la fenêtre **'Configuration générale'**, cliquez sur ***'Configurer Audio/Vidéo'***
	- Dans l'onglet **'Général'**
  **'Mode de Conférence audio/vidéo'**, activer ***'Audio/Video Enabled'***
  **'Mode de capture de la voix'**, activer ***'Voice Activation'***
  Cliquez sur **'Save Changes'**
  ![av_config_general.png](/setup/winstall/av_config_general.png)
- Cliquez sur les engrenages en haut à droite, dernier Onglet, puis **'Configuration des Options'**
- Dans la fenêtre **'Configuration générale'**, cliquez sur ***'Configurer Audio/Vidéo'***
  - Dans l'onglet **'Périphériques'**
  Laissez par défaut si vous n'avez pas plusieurs sources de captures vidéos ou audios.
  
La configuration Audio/Vidéo devra être faite pour chaque nouveau World, par chaque utilisateur et à chaque fois qu'il y aura un changement de navigateur ou de machine.  
  


