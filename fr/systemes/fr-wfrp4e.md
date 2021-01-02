---
title: Warhammer 4
description: Support pour Warhammer 4 fr
published: true
date: 2021-01-02T16:57:02.601Z
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

Vidéos de démos en Français : 
- Le passage de [Foundry VTT en Français](https://www.youtube.com/watch?v=VQClVes0JyI) par Hervé Darritchon
- Une vidéo de demo en Français [sur la création de personnages par Herve Darrichton](https://www.youtube.com/watch?v=AO6ONw1Zmxs).

# Système et modules nécessaires
 
 - Le système Warhammer (of course !)
 - La traduction 'core-FR' de Foundry
 - Le module 'Babele'
 - Le module de traduction FR de Warhammer installé comme indiqué ci-dessous. **Votre version du module doit être supérieure à 1.2.X.** , c'est à dire en 1.3.X ou plus.
 
 **Les versions du système Warhammer inférieures à 3.X ne sont plus supportées. Le module 'core' officiel de C7 (environ 15 EUR) est obligatoire pour avoir les compendiums**
 
## Installation du module de traduction FR pour le système WarhammerV4 version 3.X (ie version 3.X du système Warhammer v4)

- Installez le en copiant ce lien : `https://gitlab.com/LeRatierBretonnien/foundryvtt-wh4-lang-fr-fr/-/raw/v3/module.json`.

Si vous avez mis à jour le système Warhammer en v3 avec le "core" de Cubicle7, vous devez faire les manips suivantes: 

- Arrêter votre monde
- Supprimer le module de traduction FR dans la liste des modules. Si vous n'avez pas de module intitulé "Traduction du module WH4 en Français", vous pouvez ignorer cet étape. Pour le supprimer, il suffit de cliquer sur le bouton "Désinstaller"
- Le réinstaller en copiant ce lien : `https://gitlab.com/LeRatierBretonnien/foundryvtt-wh4-lang-fr-fr/-/raw/v3/module.json` 
- Relancer votre monde

**Seuls le contenu officiel de C7 est désormais supporté en termes de compendiums : 
https://www.cubicle7games.com/product/foundrys-warhammer-fantasy-roleplay-core-module/
Il faut donc l'acheter pour bénéficier des traductions de compendiums.
**


## Pour la 0.6.6

Pour cette version, installez tout simplement le module officiel en 2.0, via les fonctions d'installation de Foundry. **ATTENTION**: Vous ne devez pas utiliser le système Warhammer4 v3.X avec Foundry 0.6.6, cela ne marchera pas.


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
 