---
title: 3.1 LiveKit server chez Oracle Cloud
description: Création d'un serveur LiveKit chez Oracle Cloud
published: true
date: 2022-04-21T01:40:46.576Z
tags: vm, ubuntu, server, livekit, serveur
editor: markdown
dateCreated: 2022-04-21T01:40:46.576Z
---

# Création d'un serveur LiveKit chez Oracle Cloud

Cette procédure est faite sous un environnement Windows 10/11 64bits. 

Pré-requis à télécharger
- La dernière version 64bits pour windows de PuTTY : https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
- MobaXterm version Home : https://mobaxterm.mobatek.net 


> Dans la dernière version de PuTTY, les clés sont créées dans le nouveau format (v3) que MobXterm ne prend pas en charge. Il faudra, soit utiliser un autre client SSH, soit enregistrer la clé dans un ancien format (v2).
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



