---
title: Legend of the Five Rings 5th
description: 
published: true
date: 2021-07-19T19:25:39.799Z
tags: 
editor: markdown
dateCreated: 2020-12-05T05:17:17.707Z
---

# La Legende des Cinq Anneaux (5e)
![l5r02_anc_slider.jpg](/images/l5r02_anc_slider.jpg)

> Bienvenue dans l’Empire d’Émeraude.
> 
> Dans La Légende des Cinq Anneaux : le Jeu de Rôle, les joueurs incarnent des personnages dont l’honneur et la loyauté seront éprouvés. Ils servent leurs seigneurs en tant que guerriers, courtisans, prêtres ou moines. Ils vivent des aventures empreintes de drame, de suspense, d’humour, d’amour et d’horreur. Ils doivent dans le même temps lutter contre leurs émotions, et choisir entre ce que dicte leur cœur et ce que la société (et le code du bushido) leur impose. Ces histoires personnelles de triomphe et de tragédie résonneront dans Rokugan et façonneront l’avenir du pays tout entier.

## Equipe projet "Team L5R"

- Developpement : [Mandar](https://foundryvtt.com/community/mandar) & [Vlyan](https://foundryvtt.com/community/vlyan)
- Contribution: [Sasmira](https://foundryvtt.com/community/sasmira)
- Compendiums : Carter & [Hrunh](https://foundryvtt.com/community/hrunh)
- Adaptation des aventures : Carter
- Wiki : [Hrunh](https://foundryvtt.com/community/hrunh)

Sous la validation du studio [Edge Entertainment](https://edge-studio.net/)

---

## Installer le système et sa traduction
- Installer le système [L5R5E](https://foundryvtt.com/packages/l5r5e/) depuis a catégorie SYSTEME dans l'application FoundryVTT (il comprend le système et le compendium associé)
- Installer le module [L5R5e - Rokugan map for Legend of the Five Rings (5th edition)](https://foundryvtt.com/packages/l5r5e-map/) depuis la catégorie MODULE de l'application FoundryVTT. Celui-ci importera en 3 langues la carte de Rokugan fourni par Edge Studio.
- Installer le module [Babele](https://foundryvtt.com/packages/babele/) depuis la catégorie MODULE de l'application FoundryVTT. Celui-ci prend en charge la traduction de l'Anglais au Français
- Installer le module [Dice so Nice](https://foundryvtt.com/packages/dice-so-nice/) depuis la catégorie MODULE de l'application FoundryVTT, si vous aimez jouer avec des dés 3D

## Activation des modules
Suite à l'installation du système et des modules, n'oubliez pas de les activer après la création de votre World afin que les compendiums inclus dans le système soit convenablement traduit en Français.

## Création de personnage

L5R est doté d'un système de création qui se traduit sous la forme de 20 questions.
Ces 20 questions ont été intégrés dans le système afin d'être le plus fidèle possible au jeu et à son univers.

Avant de lancer les 20 questions, vous devrez au préalable créer la feuille de personnage dans la section adéquate, et ensuite cliquer en haut de la fiche sur "20Q" :

![20q-01.png](/images/20q-01.png) ![20q-02.png](/images/20q-02.png)

Une fois activé, vous aurez accès aux 20 questions et pourrez dérouler le processus soit en cliquant sur suivant soit en cliquant sur les différents icônes à gauche.

![20q-03.png](/images/20q-03.png) ![20q-04.png](/images/20q-04.png)

Notez que chacune des infos de ces 20 Questions restera stockée et que, par conséquent, vous pourrez jongler entre chacune des sections sans que les éléments que vous y avez insérer ne soit perdus. De même lorsque le questionnaire aura généré les éléments de la feuille de personnage, tout ce que vous aurez renseigné durant le questionnaire sera conservé et restera consultable.

> **Attention !!**
Chaque fois que vous validerez la dernière section des 20 questions en cliquant sur "Générer le personnage", cela écrasera toutes les données déjà inscrites sur la feuille.
{.is-danger}


## Fonctionnement et utilisation
Le système comprend la gestion des dés spécifiques au système. Que ce soit au travers d'un lanceur dédié ou en cliquant directement sur les compétences et champs dédiés aux jets.

Le lanceur intégré aux compétences permet de renseigner le lanceur avec toutes les données nécessaires pour le jet de dés:

![skill_1.png](/images/skill_1.png) ![skill_2.png](/images/skill_2.png)

On peut également effectuer des jets en ligne depuis le chat :
/r 3ds+2dr

![chat.jpg](/images/chat.jpg)

![chat_result.jpg](/images/chat_result.jpg)

Ou effectuer des lancés depuis des boutons créés dans une note :
[[/r 2dr + 1ds]]

[[/r 2dr[fire] + 1ds[aesthetics]]]

![dice_button.jpg](/images/dice_button.jpg)

![dice_button_result.jpg](/images/dice_button_result.jpg)

Pour l'instant, il ne comprend pas la gestion intégré des dés explosifs, mais ceux-ci peuvent se gérer grâce au lanceur extérieur:

![lanceur2.png](/images/lanceur2.png) 
![lanceur_3.png](/images/lanceur_3.png)

L'intégralité des élements à remplir (Avantages / Désavantages / Techniques / Armes / Armures / Inventaire) peut l'être directement depuis la fiche mais aussi depuis le compendium (si l'item existe) ou via les objets sur Foundry grâce à un simple glisser-déplacer.

![av_des.png](/images/av_des.png) ![equipement.png](/images/equipement.png) ![technique_1.png](/images/technique_1.png) ![compendium.png](/images/compendium.png)

Notez que le **système monétaire** a été implementé par défaut et que, par conséquent, la fiche de personnage réalisera automatiquement la conversion Zeni->Bu->Koku.

*ex: Si j'ajoute 12 Zeni, il me notera automatiquement 1 Bu et 2 Zeni. Dans le même sens, si j'ajoute 65 Zeni, il me le transformera en 1 Koku, 1 Bu et 5 Zeni.*

Enfin, la fiche de personnage inclut également une **gestion de l'expérience** gagnée et dépensée, se basant sur le système de gestion de l'expérience des règles de Legends of the Five Rings.


## Utilisation des caractères spéciaux

Les symboles spéciaux utilisés dans les ouvrages de la gamme sont implémentés dans le système et peuvent être obtenus en utilisant les chaînes de caratères suivantes dans le texte :

Symboles de dés : (op) (su) (ex) (st) (ring) (skill)
Anneaux : (earth) (water) (fire) (air) (void)
Techniques: (kiho) (maho) (ninjitsu) (ritual) (shuji) (invocation) (kata) (prereq)
Clans: (imperial) (crab) (crane) (dragon) (lion) (mantis) (phoenix) (scorpion) (tortoise) (unicorn)
Autres : (courtier) (bushi) (shugenja)

![symboles.png](/images/symboles.png)


## Aventure officielle *Mariage à château Kyotei* 

L'aventure officielle [Mariage à château Kyotei](https://foundryvtt.com/packages/l5r_mariage) adaptée à Foundry est disponible en libre téléchargement.  
Elle contient tout le nécessaire pour permettre à un Maitre de Jeu de mener l'aventure, ainsi que les personnages prétirés fournis gratuitement par l'éditeur. 

![pretires.jpg](/images/pretires.jpg) 

## Aventure officielle *Le parchemin ou le Sabre*
L'aventure officielle [Le Parchemin ou le Sabre](https://foundryvtt.com/packages/l5r5e-world-scroll) adaptée à Foundry est disponible en libre téléchargement.  
Elle contient tout le nécessaire pour permettre à un Maitre de Jeu de mener l'aventure fourni gratuitement par l'éditeur. 

![scroll.jpg](/images/scroll.jpg)

## Aventure officielle *Dans le Palais du Champion d'Emeraude*
L'aventure officielle [Dans le Palais du Champion d'Emeraude](https://foundryvtt.com/packages/l5r5e-world-palace) adaptée à Foundry est disponible en libre téléchargement.  
Elle contient tout le nécessaire pour permettre à un Maitre de Jeu de mener l'aventure fourni gratuitement par l'éditeur. 

![palace.jpg](/images/palace.jpg)

## Aventure officielle *Les Queues Nouées*
L'aventure officielle [Les Queues Nouées](https://foundryvtt.com/packages/l5r5e-world-tails) adaptée à Foundry est disponible en libre téléchargement.  
Elle contient tout le nécessaire pour permettre à un Maitre de Jeu de mener l'aventure fourni gratuitement par l'éditeur. 

![shadowland.jpg](/images/shadowland.jpg)

## Informations

Vous trouverez grâce à l'icone L5R situé en haut à gauche de votre écran, des liens qui vous permettront d'accéder à des sites en rapport avec L5R, que ce soit le site d'Edge Studio, celui de la boutique Drivethrurpg d'Edge et le lien vers le discord francophone de Foundry.

![anneaux.png](/images/anneaux.png)

> Nous vous souhaitons un bon jeu et espérons que vous l'apprécierez autant que nous avons pris du plaisir à le réaliser.
{.is-success}

