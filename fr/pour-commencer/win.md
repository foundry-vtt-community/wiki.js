---
title: 1.0. Installation Windows
description: 
published: true
date: 2024-02-28T15:09:33.883Z
tags: 
editor: markdown
dateCreated: 2020-10-19T10:40:52.879Z
---

# Installation sur Windows

Il existe différentes manières de faire l'installation d'un logiciel vous allez me dire, mais je vais essayer de vous présenter ici, les deux façons de faire en fonction de votre machine.

Une fois que vous avez téléchargé l'exécutable pour Windows, il va falloir faire des choix afin d'optimiser votre temps pour la suite de l'aventure en compagnie de Foundry VTT.

>La première question importante, est de savoir si vous disposez ou non d'au moins un des éléments suivants :
>Plusieurs Partitions ?
>Un ou plusieurs HDD/SSD/M2 Internes ?
>Un ou plusieurs HDD/SSD Externes ?
>Un Nas etc ...
{.is-warning}

Cette question est importante car elle va déterminer le choix d'installation que vous allez faire pour les données utilisateurs. 
La chose fondamentale qu'il faut savoir avec Foundry VTT, c'est que l'application sépare le coeur du logiciel, des données utilisateurs. 
Cette règle est vraiment importante à garder en tête, car il est littéralement **impossible de démarrer** Foundry VTT, si vous tentez de mettre les données utilisateurs dans le même répertoire que le coeur du logiciel.

## Installation de Foundry Virtual Tabletop
1. En premier, nous allons faire un petit paramétrage de votre Explorateur Windows afin de faciliter la navigation dans les fichiers. Ouvrez l'Explorateur Windows (raccourci Win+E), cliquez sur ***Affichage*** en haut de la fenêtre puis cochez :
- Extensions de noms de fichiers
- Eléments masqués
![4_affichage.png](/setup/winstall/4_affichage.png)

2. Une fois que vous avez téléchargé l'exécutable pour Windows, faites **SHIFT + clic droit** dessus et sélectionnez ***Exécuter en tant qu'administrateur***
![0_runas.png](/setup/winstall/0_runas.png)

3. Si vous avez raté votre **SHIFT + clic droit** sur l'exécutable, une fenêtre Windows de protection va s'ouvrir en vous indiquant que cette application peut mettre votre ordinateur en danger.
![1_win1.png](/setup/winstall/1_win1.png)

- Pas d'inquiétude, cliquez sur ***Informations complémentaires***, puis ***Exécuter quand même***
![1_win2.png](/setup/winstall/1_win2.png)

4. Début de l'installation :
- cliquez sur ***J'accepte***
![3_licence.png](/setup/winstall/3_licence.png)

- Choisissez le dossier d'installation, ***laissez le chemin par défaut***, puis cliquez sur ***Installer***
![3_install.png](/setup/winstall/3_install.png)

- Lorsque l'installation est terminée, cliquez sur ***Fermer*** afin de lancer Foundry VTT
![3_launch.png](/setup/winstall/3_launch.png)

5. Lors du premier démarrage de Foundry VTT, le logiciel vous demandera la clé de licence.
Vous pouvez récupérer facilement sur le [site officiel de Foundry VTT](https://foundryvtt.com) :
- Cliquez sur Login, entrain vos identifiants, puis cliquez sur **Log In**
- Lorsque vous êtes sur votre Profil Utilisateur, cliquez sur Purchased Licenses
- Dans la fenêtre de droite, copier la licence se trouvant en dessous du texte **Purchased Software Licenses**
>*Si vous faites un copier/coller de votre licence, merci de bien faire attention à une pas sélectionner d'espaces à gauche ou/et à droite du numéro de licence*

## Bugs connus
### Validation de l'Accord de Licence à l'utilisateur final.
Lors du premier démarrage de Foundry VTT, le logiciel vous demandera
- La clé de licence
- L'acceptation de l'Accord de Licence à l'utilisateur final.

![11_bug_validation_licensing.webp](/setup/winstall/11_bug_validation_licensing.webp)

>Dans certains cas rares, mais existants, il est possible que vous soyez bloqué sur cette fenêtre vous empêchant d'accéder à Foundry VTT.
{.is-warning}

**Actuellement pour ce genre de problèmes nous avons ces solutions :**
- Si vous avez mal renseigné votre numéro le licence Foundry VTT, vous devrez donc supprimer le fichier ***license.json*** et relancer l'application.
- Si vous utilisez un **VPN**, merci de couper ce dernier et de relancer le logiel afin de relancer la procédure d'acceptation de l'Accord de Licence à l'utilisateur final.
>Ce cas est très particulier est mérite une attention particulière.
En effet il est possible que les deux solutions ci-dessus ne fonctionne pas et que vous soyez toujours dans l'incapacité d'accepter l'Accord de Licence à l'utilisateur final.
Afin de résoudre ce problème vous devez vérifier si le nom de votre Ordinateur ne comporte pas un ACCENT.
{.is-warning}

Si c'est le cas, vous devez absolument changer le nom de machine de PC :
- Clique droit sur l'icône windows dans la barre des tâches et sélectionner ***Système***
- Dans la fenêtre ***Sytème***, cliquez sur ***Renommer ce PC***
![12_computer_name.webp](/setup/winstall/12_computer_name.webp)
- Dans la ***Renommer votre PC***, changez le nom de la machine en prenant en compte qu'en informatique : 
>Les noms de fichiers ne doivent contenir de signes diacritiques : pas d’accent ni de tréma (é à ï ), pas de cédille (ç).
>Ne pas utiliser de caractères spéciaux (sauf underscore « _ » ) : % $ ! & / \ : ; « » % & # @ etc ou d'espace.
  {.is-warning}
  
![13_change_computer_name.webp](/setup/winstall/13_change_computer_name.webp)

- Redémarrer la machine, relancer Foundry VTT et valider l'Accord de Licence à l'utilisateur final.

## Pare-feu et redirection de Port sur votre Box Internet.
### Pare-feu
Une fois que Foundry VTT sera installé en fonction de votre choix, Windows devrait vous demander une autorisation d'ouverture d'accès à internet, qu'il faudra accepter afin que vous puissiez avoir accès Foundry VTT via Internet.
Il peut arriver que Windows ne vous le propose pas par défaut.
- Ouvrez **Paramètres**
- Dans *Rechercher un paramètre*, 
	- tapez **Pare-feu**
  - Sélectionnez **Autoriser une application via Pare-feu Windows**
- Dans la fnêtre *Applications Autorisées*,
	- Cliquez sur **Modifier les paramètres**
- Une fois la modification des paramètres autirisée,
	- Cliquez sur **Autoriser une autre application ...**
- Dans la fenêtre *Ajouter une application*,
	- Cliquez sur **Parcourir...**
  - Allez chercher l'executable du foundry dans *c:\program files\foundryVTT*,
  - Cliquez sur **Ouvrir**,
  - Cliquez sur **OK**,
- Dans la fenêtre *Ajouter une application*,
	- Cliquez sur **OK**.

### Fixer l'adresse IP locale
Lorsque Foundry VTT sera installé et afin que vous puissiez jouer avec vos joueurs.

- **Nous travaillerons ici qu'en IPv4.** 

il vous faudra en premier lieu fixer votre adresse IP locale afin de ne pas avoir à faire cette manipulation à chaque fois que vous allumez votre ordinateur. 
Les façons de faire sont en fonction de la box internet que vous possédez, mais le principe est le même pour chaque.

- Aller dans la partie **DHCP** de votre Box Internet
	- Dans les **Adresses Statiques**, reservez une adresse locale de type **192.168.x.x** à l'**Adresse MAC** de votre ordinateur. [Obtenir l'adresse Mac sous Windows](https://www.commentcamarche.net/faq/10935-trouver-son-adresse-mac#obtenir-l-adresse-mac-sous-windows)

## Box Internet, Ouverture du Port 30000.
>**<u>ATTENTION:</u>** Lorsque vous allez ouvrir des ports sur votre Box, il faudra **ABSOLUMENT** décocher **UPnP** dans l'onglet 'Administration' de Foundry.
>*Cette option peut entrainer des dysfonctionnements et la perte de connexion sur vos parties.
La désactivation de cette dernière est OBLIGATOIRE pour le bon fonctionnement de la VTT*.
{.is-warning}

Par défaut, le port utilisé par Foundry VTT est le **Port 30000** et il faudra donc ouvrir ce dernier sur le NAT de votre Box Internet.
Afin que vous puissiez utiliser Foundry VTT, nous allons devoir utiliser la [redirection de port](https://fr.wikipedia.org/wiki/Redirection_de_port) (ou port forwarding) sur votre Box Internet.
Pour cela, il vous faudra vous connecter à votre Box Internet.
- **En IPv4**, il faudra dans un premier temps :
	- `rediriger le port externe 30000 vers le port de destination 30000 en TCP.`


# Utilisateur Freebox et IPv4 Full-Stack
><u>**ATTENTION:**</u> Les utilisateurs **Freebox** (**hors Freebox Delta**) devront choisir un port entre <u>**49152 et 65535**</u>. N'utilisez pas le port 49152, nous vous conseillons l'utilisation un port supérieur tel que le port **50000** ou **50100** pour Foundry VTT, fonctionnant parfaitement sur une freebox.
{.is-warning}

Face à la pénurie des IPv4 (et à leur prix), Free a mis en place un système de partage des adresses IP entre 4 abonnés, comme donc une adresse d’immeuble partagée avec 4 logements. Ce système fait que chaque abonné se voit dédié une plage de ports (1-16363, puis 16384-32767 etc). On a donc une IP publique fixe mais partagée.

Free change depuis quelques temps déjà les IP dites “Full Stack” de ses clients (fixes & dédiées) en IP partagées.

Cela ne pose pas de problème pour la plupart des utilisateurs mais peut se révéler gênant pour ceux qui utilisent – par exemple – leur Freebox en mode “bridge” et y branchent un autre routeur qu’ils veulent pouvoir gérer entièrement, et qui utilisaient des ports .. qui sont désormais dédiés à d’autres clients (les 3 autres avec qui ils partagent une IP si vous avez bien suivi).

Et Free change aussi – bizarrement – les IP publiques dédiées “fixes” de certains clients.. pour d’autres IP. Pourquoi, je l’ignore mais ça crée évidemment des surprises et des appels.

Si vous avez besoin de demander une IPv4 publique “full stack” chez Free, ça se fait dans votre compte client, dans la section **“Ma Freebox”** :

![ipv4-full-stack-demande-free-1024x759.webp](/setup/winstall/ipv4-full-stack-demande-free-1024x759.webp)

Vous aurez ensuite un avertissement :

![free-ipv4-full-stack-avertissement-1024x361.webp](/setup/winstall/free-ipv4-full-stack-avertissement-1024x361.webp)

Validez, puis redémarrez votre box 30 minutes après (ou moins parfois) et vous bénéficieriez à nouveau d’une IP fixe (à peu près) v4 full-stack.


# Utilisateur Bouygues BBOX et IP dédiée.
><u>**ATTENTION:**</u> Utilisateurs **BBOX** si sur la page de d'administration de votre box vous avez une plage définit de ports, c'est que votre adresse IP est une adresse Partagée avec d'autres utilisateurs. 
>Pour la plupart des gens, cela n'a aucune répercution, mais pour notre cas il va falloir faire une demande d'adresse IP dédiée. Sachez que cette demande peut prendre un certain temps, entre 15min et 12h, généralement dans la nuit afin que les scripts s'executent.
>Cela ne servira à rien de s'acharner sur un paramétrage tant que vous ne serez pas en **<u>IP Dédiée</u>** car aucun paramêtre n'est pris en compte sur une adresse IP Partagée.
>Pour savoir si vous êtes bien en adresse **<u>IP Dédiée</u>**, le champ **<u>Plage de ports</u>** est inexistant.
>Dernier point, il vous faudra changer le port d'utilisation de l'Accès à Distance de votre BBOX.
{.is-warning}

***Exemple d'adresse IP Partagée***
![bbox_ip_partagee.webp](/setup/winstall/bbox_ip_partagee.webp)


## Faire une demande dIP Dédiée
- La demande est directement possible depuis l'espace client BBOX
- Il faut aller sur : Mon offre > Ajouter des options > Onglet Pratique > Adressage IP Dédiée.
- Il suffit de souscrire pour l'activer, comme mentionné.
- Un délais autours de 15min (voir plus pour certaines personnes) entrainant un redémarrage de la BBOX automatique.

- ![bbox_ip_dediee.webp](/setup/winstall/bbox_ip_dediee.webp)

## Accès Distance à la BBOX
- Avant de faire le paramétrage pour Foundry VTT et l'ouverture du port 30000, il vous faudra changer le port de l'Accès à Distance de votre BBOX car ce dernier utilise lui aussi le port **<u>30000</u>** comme le FoundryVTT.
- Vous pouvez par exemple changer le port d'Accès à Distance **30000** par le port **40000**.

![bbox_ip_bbox_acces_distant](/setup/winstall/bbox_acces_distant.webp)



# Vous disposez d'un seul disque dur avec une seule partition.
- Fermer l'application Foundry VTT
- Grace au paramétrage de l'Explorateur Windows que nous avons fait au début de l'installation, nous avons désormais la visibilité sur les répertoires cachés.
- Cela va nous permettre d'accéder avec simplicité au répertoire utilisateur de Foundry VTT. C'est à cet endroit que vous trouverez toutes les installations dans des répertoires portant le même nom, de vos **Modules**, de vos **Systems** et de vos **Worlds** ***(Worlds étant le répertoire ou vous retrouverez toutes les parties/campagnes que vous allez créer)***.
- Le répertoire utilisateur par défaut se trouve à l'endroit suivant : 
***c:\ > utilisateurs > NomDeVotreProfil > AppData > FoundryVTT***
![5_profil.png](/setup/winstall/5_profil.png)

- Pour une question d'accessibilité rapide, je vous invite à créer un raccourci sur votre bureau à partir du répertoire FoundryVTT.
	- Réduisez la fenêtre de l'Explorateur Windows
  - Clic droit sur le répertoire ***FoundryVTT***
  - Sans relacher le clic droit déplacer le répertoire sur le Bureau
  - Lorsque vous êtes sur le Bureau, lachez le clic droit et sélectionner ***Créer les raccourcis ici*** afin de pouvoir accéder plus rapidement au répertoire dont vous aurez besoin par la suite.

## Vous disposez d'un ou plusieurs disques durs ou de plusieurs partitions etc ... 
Comme je vous l'indiquais au début du tutoriel, c'est ici que la question va se poser afin de savoir ce que voulez faire exactement de vos données utilisateurs.
- Les mettre sur un disque Extérieur, parce que vous avez plusieurs PC
- Changer simplement d'endroit, car vous avez plus de place, etc ... 

Le tout étant bien sur d'avoir un accès rapide aux divers répertoires utilisateurs.
Prenons donc le cas d'une autre partition ou d'un autre disque avec plus d'espace de stockage ou réservé aux Jeux de Rôle.

En premier lieu, nous allons devoir **COPIER** le répertoire se trouvant dans le profil utilisateur 
- Pour rappel, le répertoire utilisateur par défaut se trouve à l'endroit suivant : 
***c:\ > utilisateurs > NomDeVotreProfil > AppData > FoundryVTT***
![5_profil.png](/setup/winstall/5_profil.png)
- **Copier le répertoire FoundryVTT** (raccourci CTRL+C)
- **Coller le répertoire FoundryVTT** (raccourci CTRL+V) dans la nouvelle destination. Je prendrais ici **la partition E:\ comme exemple**
- Je vous invite à renommer le répertoire ***FoundryVTT*** par ***FoundryVTT_Data***,afin de ne pas faire la confusion avec le répertoire du coeur du logiciel.
- Exécuter Foundry VTT, puis sélectionnez l'onglet Configuration
- Dans le champ ***User Data Path***, remplacer le Path actuel par celui que vous venez de choisir en mettant des **"/"** comme sur l'exemple ci-dessous.

>**ATTENTION:** Avant de valider tous changements, merci de bien vérifier, comme sur la capture d'écran ci-dessous **Que "Enable UPnP ? (ou Activer UPnP ? en français)" soit bien <u>DÉCOCHÉ</u>.**
>**Pour rappel,** cette option est vraiment importante et n'est pas à négliger car elle peut entrainer **<u>des dysfonctionnements et des déconnexions intempestives lors vos parties</u>**.
La désactivation de cette dernière est **OBLIGATOIRE** pour le bon fonctionnement de la VTT.
{.is-warning}

- Lorsque vous avez fait la/les modification(s), cliquez sur ***Save Changes***, puis ***YES***
![6_configuration.png](/setup/winstall/6_configuration.png)

- Relancer Foundry VTT
- *Il est possible qu'il vous redemande le numéro de votre licence*
- Retourner dans l'onglet ***Configuration*** et vérifier que le changement a bien été pris en compte. Si tout est bon, pas besoin de refaire une Sauvegarde, il vous suffira de naviguer dans les divers onglets afin de commencer l'exploration en détails de Foundry.

## Passage de l'interface de Foundry VTT en français
Le premier module à télécharger est le pack de langue **fr-FR - Core Game** afin de passer Foundry VTT en français.
- Sélectionner l'onglet ***Add-on Module***, puis cliquez sur ***Install Module*** en bas à gauche de la fenêtre
![7_coregame.png](/setup/winstall/7_coregame.png)

- Dans le champ de recherche ***Filter Package***, tapez ***Translation***
![8_module.png](/setup/winstall/8_module.png)

- en face de ***Translation : French [Core]***, cliquez sur la case ***Install***
- Attendre la fin de l'installation. La case ***Install*** sera tag ***Installed***
- Fermer la fenêtre d'install Module, en cliquant en haut à droite sur ***Close***
- Le module apparaitra désormais dans la liste des modules installés
![8_module_installed.png](/setup/winstall/8_module_installed.png)

- Sélectionner l'onglet ***Configuration***, puis cliquez sur le menu déroulant en face de ***Default Language*** et sélectionnez ***Français fr-FR - Core Game***
![9_langage.png](/setup/winstall/9_langage.png)

- Cliquez sur ***Save Changes***, ***YES*** et redémarrer Foundry VTT

Vous avez désormais l'interface de Foundry Virtual Tabletop en français
![10_frfr.png](/setup/winstall/10_frfr.png)