---
title: 3.1 LiveKit server chez Oracle Cloud
description: Création d'un serveur LiveKit chez Oracle Cloud
published: true
date: 2022-04-21T04:10:21.621Z
tags: vm, ubuntu, server, livekit, serveur
editor: markdown
dateCreated: 2022-04-21T01:40:46.576Z
---

# Création d'un serveur LiveKit chez Oracle Cloud

Cette procédure est faite sous un environnement Windows 10/11 64bits. 

Pré-requis à télécharger
- *La dernière version 64bits pour windows de PuTTY :* https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
- *MobaXterm version Home :* https://mobaxterm.mobatek.net 


> **Dans la dernière version de PuTTY, les clés sont créées dans le nouveau format (v3) que MobXterm ne prend pas en charge. Il faudra, soit utiliser un autre client SSH, soit enregistrer la clé dans un ancien format (v2).**
{.is-warning}


## Générer une clé SSH

Installer la dernière version de PuTTY puis lancer **PuTTYgen**

- Cliquer sur **Key** et cliquer sur **Parameters for saving key files**
<img src="https://puu.sh/IVWQh/b043db6e44.png">

- Cliquer sur cercle devant le **2** de la ligne **PPK file version**, puis sur **OK**
<img src="https://puu.sh/IVWQC/dff247fc71.png">

- Cliquer sur **Generate** et **Bouger la souris** jusqu'à que la clé soit générée
<img src="https://puu.sh/IVWTR/23dce73561.png">

- Sans fermer le logiciel, copier l'intégralité du texte se trouvant dans le cadre **Public key for pasting into Open SSH authorized_keys file** dans un fichier texte, sans faire d'espace avant ou après la copie de la clé.
- Enregistrer le fichier en le nommant **ma_cle_ssh.txt**.
- Renommer l'extension **.txt** en **.pub**, ignorer l'avertissement et cliquer sur **OK**. Désormais, votre fichier ressemble à **ma_cle_ssh.pub**
- Cliquer sur **Save public key**
<img src="https://puu.sh/IVWYu/c49952b0eb.png">

- Ignorer le message d'avertissement et sauvegarder cette clé en **ma_cle_ssh.ppk**.
- Vous devez disposer maintenant de deux clés comme ci-dessous, **ma_cle_ssh.pub** & **ma_cle_ssh.ppk**
<img src="https://puu.sh/IVX1s/7edda5d138.png">


## Créer un compte Oracle Cloud

- C'est dans Oracle Cloud que nous allons  créer et héberger l'instance correspondant à notre serveur.

> **Lors de la création du compte, le service Oracle nous demandera une Carte de Crédit afin d'effectuer une transation. Cette transaction d'une valeur de 0.85€ sert à vérifier l'authenticité des informations. Rien ne vous sera facturé tant que vous restez sur des services gratuits.**
{.is-warning}

- Créer un compte de service sur le site d'Oracle Cloud : http://oraclecloud.com/

> **Lorsque l'on vous demande l'emplacement des services, prenez en un qui possède tous les services, de préférence :**
> ***France South (Marseille)***
> ***Germany Central (Frankfurt)***
> ***UK South (London)***
{.is-warning}


## Créer une Instance
> Chaque location bénéficie des 3 000 premières heures d'OCPU et des 18 000 premières heures de Go gratuites par mois pour créer des instances Compute Ampere A1 à l'aide de la forme VM.Standard.A1.Flex (équivalente à 4 OCPU et 24 Go de mémoire). Chaque location dispose également de deux instances VM.Standard.E2.1.Micro gratuites.
{.is-info}

- Cliquer sur les trois traits en haut à gauche
- puis cliquer sur **Compute** dans la colonne de gauche
- puis **Instances** dans la colonne de droite
<img src="https://puu.sh/IVXfT/bb4e55bf6f.png">

- Vous pouvez aussi taper dans la barre de recherche le môt **instances**

### Option Recommandée : Serveur Ampère
> Dans le niveau gratuit, un total de 4 OCPU et 24 Go sont disponibles, qui peuvent être répartis entre deux instances (le maximum) ou attribués à une seule. C'est-à-dire deux instances avec 2 OCPU et 12 Go chacune, ou une avec 4 OCPU et 24 Go.
{.is-warning}

**Pour un seul serveur :**
- Processeur : ARM allant jusqu'à 4 coeurs OCPU
- RAM : 24Go
- Bande passante : 4Gbps.
- Image : Canonical Ubuntu
- Forme : Machine virtuelle - AMPERE - VM.Standard.A1.Flex

<img src="https://puu.sh/IVXtQ/1935e76855.png">

> Lorsque vous choisissez l'option Image & Forme, vous devez vous assurer de bien selectionner l'une des formes (Shape) qui sera **Admissible à Toujours gratuit / Admissible à Toujours gratuit**, afin de nous assurer de toujours rester dans le niveau de gratuité d'Oracle gratuit.
{.is-warning}

#### Image et Forme 
<img src="https://puu.sh/IVXk9/1e31da8a36.png">

#### Shape 
<img src="https://puu.sh/IVXpA/dc85446c0c.png">

### Ajouter des clés SSH
- Cliquer sur **Télécharger les fichiers de clés publiques (pub)**
- Dans la  case **clés publiques SSH**
<img src="https://puu.sh/IVXwy/4d728c23b7.png">

- Cliquer sur **Parcourir** et selectionner votre fichier **ma_cle_ssh.pub**
<img src="https://puu.sh/IVXzi/f5f31f5fad.png">

- Puis cliquer tout en bas à gauche sur **Créer**
<img src="https://puu.sh/IVXA0/c381eb0a23.png">

## Adresse IP du serveur et Paramétrage Réseau.
Une fois que vous avez crée l'instance, vous allez vous retrouver sur une interface de ce genre
<img src="https://puu.sh/IVXF2/4d3f0ad87c.png">

### Accès à l'instance
- Veuiller noter **l'Adresse IP Publique**, elle vous servira afin de vous connecter sur votre instance 
<img src="https://puu.sh/IVXGM/30fe3b6030.png">

### Certe d'interface réseau virtuelle principale
- Afin de faire le paramétrage et l'ouverture des ports nécessaire, il vous faudra cliquer sur le lien en face de **Sous-réseau**
<img src="https://puu.sh/IVXHK/1ff91a8c7c.png">

- Dans **Subnet-xxxxxxxx-xxxx**
- Puis **Listes de sécurité**
- Cliquer sur **Default Security List for vcn-xxxxxxxx-xxxx**
<img src="https://puu.sh/IVXIV/7a3431acdd.png">

- Dans **Default Security List for vcn-xxxxxxxx-xxxx**
- Puis dans **Règles Entrantes**
- Cliquer sur **Ajouter des règles entrantes**
<img src="https://puu.sh/IVXMf/5d4c243829.png">

- Pour les ports en **TCP** il faudra créer une règle entrante 
<img src="https://puu.sh/IVXPb/904b396ca1.png">
<img src="https://puu.sh/IVXPT/69e2f8a9e3.png">
<img src="https://puu.sh/IVXQp/cec9a67db6.png">

- Pour les ports en **UDP** il faudra créer une règle entrante 
<img src="https://puu.sh/IVXRi/451c59500f.png">
<img src="https://puu.sh/IVXRH/c08ef0af10.png">

- **Toutes les Règles Entrantes TCP/UDP**
<img src="https://puu.sh/IVXSd/d819500f56.png">

## Les Sous-domaines
> Deux sous-domaines sont obligatoires lors de la création du serveur LiveKit afin de pouvoir utiliser l'Audio/vidéo.
{.is-warning}

> Le site [Duck DNS](https://www.duckdns.org) vous permettra de créer un nom de domaine gratuit, si vous ne possédez pas de nom de domaine.
{.is-info}

- Deux sous-domaines sont nécessaires pour le bon fonctionnement de LiveKit, **livekit** et **livekit-turn**
- **Pour les utilisateurs de serveur dns de type duckdns, il faudra créer un sous-domaine :**
1. Par exemple pour le premier sous-domaine : monserveurlivekit.duckdns.org
2. Par exemple pour le deuxième sous-domaine : monserveurlivekit-turn.duckdns.org
<img src="https://puu.sh/IVXWH/ec9ecc00a2.png">

- **Pour les utilisateurs de serveur dns de type OVH, il faudra créer une entrée pour chaque sous-domaine :**
1. Pour la première entrée de sous-domaine : livekit.monnomdedomaine.xxx
2. Pour le deuxième entrée de sous-domaine : livekit-turn.monnomdedomaine.xxx

- Pour créer un sous-domaine :
1. Ajouter une entrée
2. Champs de pointage : **A**
<img src="https://puu.sh/IVY3Q/504f5325bc.png">

3. Remplir le case pour le **sous-domaine**
4. Remplir la case de la **Cible** par l'adresse IP de l'Instance du serveur Oracle Cloud.
5. Valider et attendre la propagation DNS, qui peut prendre de quelques minutes à plus ou moins 24h.


## Connexion à l'instance via SSH
- Télécharger et installer MobaXterm ou Utiliser la version portable de ce dernier.
- Ouvrir MobaXterm, puis cliquer sur le premier icone **Session**
<img src="https://puu.sh/IVY8E/c2b5bac74b.png">

- Dans la case **Remote host**, mettre **ubuntu@<adresse_IP_de_mon_Instance>** (exemple: ubuntu@132.145.31.35) 
- Cliquer sur l'onglet **Advanced SSH settings**
- Cocher la case **Use private key**
- Cliquer sur l'image du fichier dans le champ de la case **Use private key**
- Selectionner le fichier **ma_cle_ssh.ppk**
- Cliquer sur **OK** cela va vous connecter au serveur
<img src="https://puu.sh/IVYbP/ffd9896a05.png">

## Installation et configuration du serveur LiveKit
> Tout se passe maintenant en ligne de commande en SSH via MobaXterm.
{.is-info}

> Faites des copier/coller des lignes suivantes en vous assurant de ne pas avoir d'erreurs
{.is-warning}

#### Mise à jour d'Ubuntu
> sudo apt-get update
{.is-success}

> sudo apt-get upgrade -y
{.is-success}

> sudo apt-get dist-upgrade -y
{.is-success}

>sudo apt-get install ca-certificates curl gnupg lsb-release
{.is-success}

#### Accès aux ports requis
> Permet au système d'accepter les connexions entrantes
{.is-info}

> Faites des copier/coller des lignes suivantes en vous assurant de ne pas avoir d'erreurs
{.is-warning}

> sudo iptables -I INPUT 6 -m state --state NEW -p tcp --match multiport --dports 80,443,7881 -j ACCEPT
{.is-success}

> sudo iptables -I INPUT 7 -m state --state NEW -p udp --match multiport --dports 443,50000:60000 -j ACCEPT
{.is-success}

> sudo netfilter-persistent save
{.is-success}