---
title: Warhammer 4
description: Support pour Warhammer 4 fr
published: true
date: 2020-11-26T09:09:37.235Z
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

# Système et modules nécessaires
 
 - Le système Warhammer (of course !)
 - La traduction 'core-FR' de Foundry
 - Le module 'Babele'
 - Le module de traduction FR de Warhammer installé cmme indiqué ci-dessous
 
 **Les versions du système Warhammer inférieures à 3.X ne sont plus supportées. Le module 'core' officiel de C7 (environ 15 EUR) est obligatoire pour avoir les compendiums**
 
## Pour le système WarhammerV4 version 3.X (ie version 3.X du système Warhammer v4)

Si vous avez mis à jour le système Warhammer en v3, vous devez faire les manips suivantes: 

- Arrêter votre monde
- Supprimer le module de traduction FR
- Le réinstaller en copiant ce lien : https://gitlab.com/LeRatierBretonnien/foundryvtt-wh4-lang-fr-fr/-/raw/v3/module.json 
- Relancer votre monde

**Seuls le contenu officiel de C7 est désormais supporté en termes de compendiums.**

## Pour la 0.6.6

Pour cette version, installez tout simplement le module officiel en 2.0, via les fonctions d'installation de Foundry. **ATTENTION**: Vous ne devez pas utiliser le système Warhammer4 v3.X avec Foundry 0.6.6, cela ne marchera pas.

## Pour les 0.7.X

Seules les versions 3.X du système Warhammer4 sont compatibles avec les versions 0.7.X de Foundry. Si vous utilisez encore des versions inférieures à la 3.X, sachez qu'il y a un gros risque quasi certain que vos acteurs, compendiums et données soient inutilisables. Il vous faut dans ce cas rester en Foundry v0.6.X obligatoirement pour ne pas avoir d'effets de bord.


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
 