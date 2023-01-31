---
title: 4.3 Audio, Vidéo, Page de connexion
description: Comment mettre une image et/ou du son sur la page de connexion
published: true
date: 2023-01-31T21:59:35.986Z
tags: audio, video, login, screen
editor: markdown
dateCreated: 2023-01-31T17:20:28.017Z
---

Comme nous avons souvent l'habitude d'en vouloir toujours plus afin de rendre notre travail encore plus long et plus beau pour nos joueurs, il existe encore quelques méthodes afin de partager encore plus notre créativité et de rendre tout cela possible.
Dans ce tutoriel, nous allons essayer de vous aider à rendre la chose possible, avec quelques outils et un peu de patience.
<br>
### Prérequis
Il vous faudra activer l'autorisation Audio/video sur votre navigateur
- sur Chrome, il faudra autoriser le site en cliquant sur le ***Cadenas à coté de l'url de votre navigateur***, puis cliquer sur ***Paramètres de site***, et enfin à ***Son, mettre le paramètre Autoriser***
<img src="https://puu.sh/Jy5Ho/1aab4d18a2.png">
<img src="https://puu.sh/Jy5HI/227b572a6a.png">
- sur Edge
<img src="https://puu.sh/Jy5ue/6dcc4bdb19.png">
- sur Firefox
<img src="https://puu.sh/Jy5tr/ccfaf8b81e.png">
- sur Vivaldi
<img src="https://puu.sh/Jy5vT/4dde9801c2.png">

<br>

### Quelques outils, si nécessaires :
> Nous nous permettons de vous faire une petite liste d'outils qui pourront vous servir si vous voulez pousser le vis tout en restant sur des outils gratuits.
> - [Éditeur Audio, Audacity](https://www.audacityteam.org/download/)
> - [Éditeur vidéo, Davinci Resolve](https://www.blackmagicdesign.com/fr/products/davinciresolve/)
> - [Convertisseur tous formats, file-converter](https://framalibre.org/content/file-converter)
> - [Convertisseur en webp, site web](https://webp.to/)
> - [Outil de téléchargement multi-usage, JDownloader2](https://jdownloader.org/jdownloader2)
> - [Module Foundry, Login compact](https://gitlab.com/sasmira/_fr-core-logon)
{.is-info}

<br>

## Optionnel, Login de connexion compact.
Nous nous sommes souvent aperçu que sur Foundry nous n'utilisions pas la totalité des informations que nous avions sur la page de connexion, comme :
- Le titre de la partie
- La date de la prochaine partie,
- Le résumé de la campagne,
- etc etc ... 

Nous nous sommes donc atelés à faire une autre présentation afin d'avoir le strict minimum, plus compact et de pouvoir profiter d'une belle illustration ou encore, comme pour le sujet qui nous concerne, une petite vidéo.
<br>
<img src="https://puu.sh/Jy4oy/801517e986.jpg">

Si cela vous intéresse, il suffit d'installer ce module et de suivre les instructions.
> - [Module Foundry, Login compact](https://gitlab.com/sasmira/_fr-core-logon)
{.is-info}

<br>

# Création d'une page de connexion avec Audio et Vidéo.
Lorsque nous avons débuté sur Foundry, nous nous sommes aperçus que sur l'écran de connexion, nous ne pouvions pas mettre de vidéo au format MP4/WEBM/MKV et que la seule option était de mettre un GIF animé avec toutes les contraintes que cela impliquent.
Par contre nous avons que Foundry peut lire tous les formats comme le JPG/PNG/WEBP etc etc etc ... mais aviez vous que le WEBP peut être animé ? c'est sur cette découverte, que nous allons donc pouvoir effectuer notre transformation afin d'avoir une vidéo d'introduction sur notre page de connexion, par contre ce format ne gère que la partie vidéo et non l'audio, mais nous allons voir tout cela en détail ci-dessous. 
<br>
## Pour la vidéo
Si vous êtes experts en création de vidéo, vous serez sans doute ce que vous voulez comme introduction, voir la créer de toute pièce, pour les autres il existe quelques solutions plus ou moins facile à prendre en main.
<br>
### La facilité pour récupérer une vidéo
Vous êtes une "quiche" en informatique, en création de vidéo, etc, etc ... il existe donc une solution pour vous. Il faudra juste faire attention au format de la vidéo et qu'elle soit en MP4 ou WEBM. Si ce n'est pas le cas, vous pouvez utiliser facilement le convertisseur tous formats ***[file-converter](https://framalibre.org/content/file-converter)***.
Une fois le convertisseur installé, faire un clic droit sur la vidéo, et choisir dans le menu du convertisseur, le format que vous désirez, MP4 ou WEBM. Pour question de rapidité, nous vous conseillons d'encoder en MP4 de préférence et en WEBM pour un gain de taille de vidéo.

> <u>***Dans un usage strictement personnel :***</u> *[Outil de téléchargement multi-usage, JDownloader2](https://jdownloader.org/jdownloader2) permet de télécharger plus ou moins ce que vous voulez et par la même occasion, permet de télécharger les vidéos Youtube, par exemple.*
{.is-info}

<br>

### Pour les autres
Pour les sachants qui sachent faire du montage vidéo, il vous faudra une vidéo au format <u>**MP4 ou WEBM**</u>.

<br>

### Convertion de la vidéo
Maintenant que nous une vidéo au format <u>**MP4 ou WEBM**</u>, il va nous falloir la transformer en <u>**WEBP Animé**</u> afin que Foundry puisse la jouer sur la page de connexion de votre partie.
Pour cela nous allons utiliser le site de **[webp.to](https://webp.to/?lang=fr)** et en fonction de votre format de vidéo vous avez le choix entre :
- [MP4 à WebP](https://webp.to/mp4-webp/)
- [WebM à WebP](https://webp.to/webm-webp/)

Il faudra suivre les instructions sur à la page et attendre le résultat que vous pourrez télécharger sur le site. En fonction de la taille de votre vidéo, le résultat final peut prendre beaucoup de temps.

