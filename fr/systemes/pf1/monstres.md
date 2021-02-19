---
title: Création de Monstres pour PF1
description: 
published: true
date: 2021-02-19T10:17:33.502Z
tags: 
editor: markdown
dateCreated: 2021-02-17T10:02:58.859Z
---

# Introduction :
Tout d'abord, sachez qu'un compendium de monstres importés depuis le site existe déjà. Pour cela vous n'avez qu'à utiliser la macro de @Dorgendubal : **Compendium - Bestiaire** que vous trouverez dans le compendium **Macros PF1**.


## Ajouter Le compendium - Bestiaire de Pathfinder 1 :
![ajout_compendium_bestiaire.jpg](/pf1/ajout_compendium_bestiaire.jpg)
1. Installer Le module : ***Pathfinder 1 - Improvements for French***
2. Dans l'onglet Compendium, ouvrez le compendium "Macros PF1".
3. Glissez-déposez la macro "Compendium - Bestiaire" dans la barre des macros.
4. Cliquez sur cette macro.
5. Choisissez le compendium "Avec images" SI vous avez le dossier avec les images. Sinon, Choisissez "Sans images". 

Voilà vous devriez trouvez votre Bestiaire dans l'onglet compendium.

## Compléter le bestiare :
Le Bestiaire généré depuis https://www.pathfinder-fr.org/Wiki/ est déjà super pour importer et jouer rapidement des monstres. Merci encore à tous les contributeurs qui y ont participé.

Mais avons le souhait de compléter manuellement le Bestiaire pour qu'il soit complet (c.à.d, prise en charge des compétences, classes, sorts, inventaire, capacités spéciales, taille, langues...).
Pour cela nous avons besoin de votre aide, car il y a actuellement 1488 monstres que nous souhaitons compléter.

La suite de ce tutoriel permettra de vous indiquer la marche à suivre. L'idée est de le remplir du mieux possible pour ne pas à revenir sur les monstres travaillés, ainsi que de respecter la même méthodologie.

**L'idée sera de partir des monstres importés avec la macro ci-dessus afin de profiter de ce qui est déjà saisi.**

### Etape 1 : Répartition des ~~tâches~~ monstres
Indiquer sur le document [Google Sheet en cliquant ici](https://cutt.ly/Bestiaire_Foundry) quel(s) sont les monstres que vous souhaitez compléter, afin d'éviter qu'on soit plusieurs à faire le même travail plusieurs fois.

Pour les monstres provenant des Bestiaire, vous avez déjà le tableau quasi-complet. Si vous souhaitez participer avec un PNJ/Monstre venant d'une autre campagne (Prévôt Cigue de l'Eveil des Seigneurs des Runes par exemple), merci d'utiliser l'onglet Campagne.

C'est également sur ce(s) tableau(x) que vous pourrait indiquer si vous avez rencontré un soucis. Si vous ne saviez pas comment insérer une capacités, ou d'autres problèmes. Dans ce cas, n'hésitez pas à demander de l'aide dans le discord pf1 de La Fonderie.

Dans tous les cas, si vous n'avez pas pu compléter le monstre à 100%, merci de l'indiquer dans le tableau Google Sheet, une autre personne prendra le relais.

Commencez déjà par des monstres simples (sans capacités spéciales, nombreux sorts).

### Etape 2 : Les outils nécessaires
- Vous aurez besoin du site https://www.pathfinder-fr.org ou des Bestiaire.
- Vous aurez besoin des Compendiums français (races, DVs raciaux, classe, sorts, effets et autres...). Ils sont tous inclus avec le module ***Pathfinder 1 - Improvements for French***.
- Je conseille vivement le module ***Quick Insert - Search Widget***, qui vous permettra de ne plus avoir à ouvrir tous les compendiums afin d'ajouter les différents "objets" (par objets, comprendre toutes les fiches non personnages : races, sorts, capacités etc.). C'est bien plus rapide et pratique !!!
> Il suffit d'appuyer sur les touches `Ctrl + Espace` et d'écrire ce que vous rechercher. 
Vous pourrez faire un glisser déposer l'objet voulu dans une fiche personnage/monstre. 
Vous pourrez également glisser déposer un monstre directement sur la map.
{.is-success}


### Etape 3 : Compléter la fiche
#### 1. Supprimer Ajustements et Classe générique
- Supprimer les ***ajustements*** dans l'onglet "Effets"
- Editer la ***Classe générique*** (onglet Sommaire -> Classes) afin de prendre note du nombre de ***Points de vie*** du monstre (onglet Détails)
- Supprimer la Classe générique qui n'est configuré que pour les Points de vie.

#### 2. Classe/DVs Raciaux, Dons, Traits et Inventaire :
> AVANT toute modification manuelle, il faudra d'abord ajouter les "objets" suivants, ce qui permettra de ne pas faire doublons avec vos modifications. 
{.is-warning}

*Je m'explique, si un don donne +2 Perception et que vous aviez déjà ajusté les compétences pour vous aligner sur la fiche du monstre, il faudra revenir retourner enlever 2 en Perception.*

1. Ajouter le DVs Racial. Editer le DVs Racial pour ajouter le nombre de ***Points de vie***. La plupart du temps, il correspont à celui de la Classe générique que vous avez supprimé.
S'il n'y a pas de DVs Racial, il faudra sûrement ajouter une race et une classe.
2. Ajouter les Dons
3. Ajouter l'inventaire **ET les équiper.**
3. Ajouter les traits, capacités et autres (le cas échéant)

#### 3. Réglages :
Tout est normalement bien renseigné sauf : 
- Choisir **Dextérité** pour l'**Initiative**
- Cocher Spell-likes, Primary et autres **si** votre monstre a des Pouvoirs magiques ou Peut lancer des Sorts.

#### 4. Compléter le sommaire :
- Indiquer la Taille
- Mettre le bon Alignement (roue dentée)
- Si il s'agit d'une créature qui n'a pas une race de PJ, cliquez sur le "+" à droite de "Race" pour ajouter une nouvelle race (cf 4.1 pour voir la configuration)
- Si il s'agit d'une créature qui a une race de PJ, ouvrez le compendium "Races" et glissez-déposez la race correspondante.
- Indiquer en ***feet (pieds)*** la vitesse de déplacement (base, nage, escalade, creusement et/ou vol). Foundry fera automatiquement la conversion en mètres. 
Il faudra arrondir au multiple de 5 supérieur. Exemple pour 18m, mettre 60pieds ~~(et non pas 59).~~
- Indiquer la manoeuvre de vol.
- Dans l'onglet Classes, supprimez la Classe générique si ce n'est pas déjà fait
- Pour les créatures ayant des DVs raciaux, ouvrez le compendium "DVs raciaux" et glissez-déposez le bon type de DV racial
- Pour les créatures ayant des niveaux de classe, ouvrez le compendium "Classes" et glissez-déposez la bonne classe
- Editez les DVs raciaux et les Classes pour mettre le bon niveau et le bon nombre de points de vie
Les DVs raciaux ont un niveau de classe égal au nombre de DVs de la créature
Les PVs de la créature sont à mettre dans "Points de vie". Notez les PVs sans les améliorations telles que la Constitution, le don Robustesse ou les archétypes.

##### 4.1 Race non-PJ :
- Après avoir créé la race, l'éditer
Onglet détails :
- Choisir le bon type de créature
- Si la créature a des sous-types, cliquer sur "Sous-types", puis cliquer sur "+" pour ajouter un sous-type et le renseigner en toutes lettres. Répéter pour chaque sous-type.
Onglet changements :
- Dans "changements", renseigner les modificateurs raciaux aux compétences (sauf celles pour une utilisation spécifique de la compétence)
- Dans "notes de contexte", renseigner les modificateurs raciaux aux compétences spécifiques à une utilisation de la compétence
![race.png](/pf1/race.png)
- Dans "notes de contexte", renseigner les bonus spécifiques au BMO.
Si une créature a "BMO +7 (+10 en lutte)", renseigner `[[@attributes.cmb.total+3]] en lutte | Div. | BMO`
(note : les modificateurs de DMD seront indiqués plus tard)

#### 5. Attributs :
- Vérifier que les Caractéristiques sont Correctement renseignées.
- Choisir la bonne **Taille**








