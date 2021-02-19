---
title: Création de Monstres pour PF1
description: 
published: true
date: 2021-02-19T11:45:59.280Z
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

**4.1 Race non-PJ :**
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

> A ce stade, toutes les valeurs dans Infos rapides et Points de vie ne seront pas forcément bonnes. C'est normal : elles se corrigeront au fil du remplissage de la fiche. 
{.is-info}

#### 5. Attributs :
- Vérifier que les Caractéristiques sont Correctement renseignées.
Si une créature a une caractéristique nulle (par exemple, "Int -"), videz la cellule correspondante. Le modificateur devrait passer à "+0".
- Choisir la bonne **Taille**
- Renseignez les **Sens**, **Réduction aux dégâts**, **Régénération**, **Guérison accélérée**, **Résistance à l'énergie**, **Résistances**, **Immunités aux dégâts**, **Vulnérabilités aux dégâts**, **Immunités** et **Langues**.
Si vous ne trouvez pas la bonne valeur pour l'immunité aux dégâts, la vulnérabilité aux dégâts, l'immunité et la langue, vous pouvez les rajouter dans le champ "spéciale" en les séparant avec un point-virgule <kbd>;</kbd>.

#### 6. Inventaire :

> Ne pas renseigner ici ni les armes naturelles ni l'armure naturelle.
{.is-warning}

**6.1 : Cas où l'équipement est dans un compendium**
- Si la créature a un inventaire et que ce n'est pas déjà fait, renseigner son inventaire ici. Pensez bien à équiper les armes et les armures pour qu'elles soient prises en compte. Pour ça, vérifiez si la coche de la colonne "équipé" (la colonne avec un casque, une épée et un bouclier) est bien sur ✔️. Si ce n'est pas le cas, cliquez sur la coche pour la changer d'état.

**6.2 : Baguettes, Potions et Parchemins**
- Ouvrez le compendium de sorts et cherchez-y le sort dont vous voulez créer une baguette, une potion ou un parchemin.
- Glissez-déposez le sort dans l'onglet Inventaire.
- Sur la pop-up qui apparait, sélectionnez le type d'objet que vous voulez créer.
- Si la créature possède plusieurs fois cet objet, vous pouvez corriger le nombre soit en éditant l'objet et en renseignant le nombre dans "Quantité", soit en cliquant sur le "+" ou le "-" à droite de l'objet dans la liste.

**6.3 : Cas où l'arme n'est pas dans un compendium**
- Dans la catégorie "Armes", cliquez sur "+".
- Editez la nouvelle arme créée.
- Sélectionnez une icone pour l'arme en cliquant sur l'icone.
Si vous créez cette arme pour la partager avec d'autres, privilégiez les icones qui sont soit dans le *System PF1*, soit dans *le Module Pathfinder 1 - Improvement for French*, afin d'éviter que les autres utilisateurs aient une icone "cassée".
- Renseignez le nom de l'arme.
- Dans la "Description", renseignez la description de l'arme, ainsi que son prix (en po), son poids (en kg), ses PVs et sa solidité. Si la créature possède plusieurs exemplaires de la même arme, indiquez-en le nombre dans "Quantité".
![exemple_description_arme.png](/pf1/exemple_description_arme.png)

Onglet "Détails" :
- Renseignez la catégorie, le type et la taille de l'arme.
- Assurez-vous que la case "Equipé" est cochée.
- Renseignez les propriétés spéciales de l'arme.
- Renseignez les dégâts de base de l'arme (pour une taille moyenne), les types de dégâts, la zone et le multiplicateur de critique, la portée (en pieds) et le nombre maximum d'incréments de portée.
Pour rappel, une arme de jet a au maximum 5 incréments de portée, et une arme à munition a au maximum 10 incréments de portée.

**6.4 : Cas où l'armure/bouclier/objet donnant de la CA n'est pas dans un compendium**
- Dans la catégorie "Armure/Equipement", cliquez sur "+".
- Editez la nouvelle armure créée.
- Sélectionnez une icone pour l'armure en cliquant sur l'icone.
Si vous créez cette armure pour la partager avec d'autres, privilégiez les icones qui sont soit dans le *System PF1*, soit dans *le Module Pathfinder 1 - Improvement for French*, afin d'éviter que les autres utilisateurs aient une icone "cassée".
- Renseignez le nom de l'armure.
- Dans la "Description", renseignez la description de l'armure, ainsi que son prix (en po), son poids (en kg), ses PVs et sa solidité.
![exemple_description_armure.png](/pf1/exemple_description_armure.png)

Onglet "Détails" :
- Renseignez la catégorie, le type et la taille de l'armure.
- Assurez-vous que la case "Equipé" est cochée.
- Renseignez le bonus à la Classe d'Armure, le modificateur maximum de Dextérité, la Pénalité d'armure et le Risque d'échec des sorts profanes de l'armure.

**6.5 : Cas où un autre objet n'est pas dans un compendium**
- Dans la catégorie de votre choix, cliquez sur "+".
- Editez le nouvel objet créé.
- Sélectionnez une icone pour l'objet en cliquant sur l'icone.
Si vous créez cette armure pour la partager avec d'autres, privilégiez les icones qui sont soit dans le *System PF1*, soit dans *le Module Pathfinder 1 - Improvement for French*, afin d'éviter que les autres utilisateurs aient une icone "cassée".
- Renseignez le nom de l'objet.
- Dans la "Description", renseignez la description de l'objet, ainsi que son prix (en po), son poids (en kg), ses PVs et sa solidité. Si la créature possède plusieurs exemplaires du même objet, indiquez-en le nombre dans "Quantité".

**6.6 : Version de maître d'un équipement**

- Ajoutez "de maître" dans le nom de l'objet.
- Dans l'onglet "Détails", cochez la case "De maître".
- Renseignez "1" dans le champ "Formule de bonus d'attaque".

**6.7 : Version magique d'un équipement**

- Ajoutez le bonus d'altération et les propriétés spéciales de l'objet dans le nom de l'objet.

Onglet "Détails" : 
- Renseignez le Niveau de lanceur de sort pour l'objet.
Pour une arme, une armure ou un bouclier, le NLS est égal au plus grand entre les NLS de ses propriétés spéciales et trois fois son bonus d'altération.
- Renseignez le bonus d'altération s'il s'agit d'une arme ou d'une armure.
- Si une propriété donne des bonus fixes aux dégâts ou au toucher en dehors de son bonus d'altération, renseignez-les dans "Formule de bonus d'attaque" 

Onglet "Changements" :
- Dans "Changements", renseignez tous les bonus fixes apportés par l'objet. Si il s'agit d'une arme, ne renseignez pas les bonus aux dégâts, les bonus au toucher, ou les dés bonus aux dégâts
> Les dés bonus seront renseignés plus tard. Les bonus d'attaque et de dégâts sont à renseigner dans l'onglet Détails.
{.is-info}
- Dans "Notes de contexte", renseignez les bonus et effets conditionnels.

#### 7. Capacités :

#### 8. Combat :

**8.1 : Arme manufacturée**
(TODO : penser aux armes avec des dés de dégâts bonus (de feu) et aux attaques sournoises)

**8.2 : Armes naturelle**

(TODO : explication sur pourquoi les armes naturelles principales ont pas les mêmes stats que sur le bestiaire pour un monstre qui a une arme manufacturée)

**8.2.1 : Avec le compendium**

> Le compendium des armes naturelles n'est pas encore partagé sur le module. Pour l'obtenir, demandez-le sur le serveur Discord français PF1.
{.is-warning}

**8.2.2 : Sans le compendium**

#### ??? Vérifications :
Repassez sur les différents onglets, et vérifiez que toutes les valeurs correspondent bien à la fiche d'origine de la créature. Ajustez en fonction.