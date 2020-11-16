---
title: Warhammer 4
description: Support pour Warhammer 4 fr
published: true
date: 2020-11-16T08:25:22.770Z
tags: 
editor: markdown
dateCreated: 2020-10-16T18:54:36.683Z
---

# Warhammer v4

Il convient de lire la FAQ de MooMan ici : https://github.com/moo-man/WFRP4e-FoundryVTT/wiki
La visualisation des vidéos présentées sur le README est aussi très utile pour comprendre l'utilisation du système : 

https://youtu.be/HMjXCLDDfWE
https://www.youtube.com/watch?v=XMEJt5OB4Bc
https://www.youtube.com/watch?v=-CthIoE9o2E

Une FAQ complémentaire sur le système est disponible sur [cette page](/fr/faq/faq-wfrp4e)

## Pour le système warhammer 3.X

Si vous avez mis à jour le système Warhammer en v3, vous devez faire les manips suivantes: 

- Arrêter votre monde
- Supprimer le module de traduction FR
- Le réinstaller en copiant ce lien : https://gitlab.com/LeRatierBretonnien/foundryvtt-wh4-lang-fr-fr/-/raw/v3/module.json 
- Relancer votre monde

**Seuls le contenu officiel de C7 est désormais supporté en termes de compendiums.**

## Pour la 0.6.6

Pour cette version, installez tout simplement le module officiel en 2.0, via les fonctions d'installation de Foundry.

## Pour les 0.7.X

Il faut modifier le manifest et installer les versions 2.1.X : https://raw.githubusercontent.com/moo-man/WFRP4e-FoundryVTT/0.7.x/system.json

* Pour versions 3.1 et inférieures (16/11/2020) : Si vous avez le module RNHD de C7, il vient avec une jolie horloge et une option de synchronisation avec l'horloge de About/Time : **ne l'utilisez pas, elle provoque des grosses latences pour tout les joueurs**. En cours de fix par MooMan

#### Modules conseillé par la communauté

- Token HUD
- GM Toolkit
- DungeonDraft Importer
- Pings
- AboutTime
- Calendar/Weather

#### Modules à éviter

 - Pick-up-sixt : Casse les fiches de personnage et nécessite souvent de re-créer le monde
 - Chat Scrolling : Empêche l'édition des tests dans le tchat lorsque installé
 