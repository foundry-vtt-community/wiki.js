---
title: Création de Monstres pour PF1
description: 
published: true
date: 2021-02-19T15:34:31.509Z
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
- Vérifiez que les scores de caractéristiques de la partie "Ability Score Links" sont bien renseignés, et renseignez-les le cas échéant. Vous pouvez aussi les modifier si la créature utilise des attributs différents de la normale (par exemple, un mort-vivant utilisant le Charisme pour le calcul des PVs).
- Si votre créature a des pouvoirs magiques ou peut lancer des sorts, cochez les cases nécessaire à l'utilisation de la magie de la créature dans la partie "Spellbooks".
"Spell-likes" sert pour les pouvoirs magiques.
"Primary", "Secondary" et "Tertiary" permettent chacun de faire apparaître un onglet pour la magie. Il n'y a aucune différence entre eux. Le fait qu'il y en ait trois sert principalement dans le cas d'une créature qui a des niveaux dans deux classes de lanceur de sort (comme un théurge mystique prêtre/magicien).

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
Pour une arme pouvant aussi bien être utilisée au corps à corps qu'à distance, renseignez les valeurs pour le Type de base et pour le Type "Sous-type à distance". Les informations sont gardées en mémoire même si les champs ne sont pas visibles.

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
- Vérifiez qu vous avez bien renseigné les dons, et renseignez ceux qu'il manque le cas échéant en les faisant glisser-déposer depuis le compendium.
Pensez à ajouter les bonus des dons du type "Talent" comme indiqué dans la partie 7.2.
- Si la créature a des niveaux de classes, ouvrez le compendium des aptitudes de classes et glissez-déposez les aptitudes.
- Si la créature possède une race de PJ, renseignez les traits raciaux dans la catégorie "Traits raciaux".
- Renseignez les capacités de monstre dans la catégorie "Divers".
- Renseignez les archétypes de monstre dans la catégorie "Catégorie".

**7.1 : Don ou aptitude de classe non-existant dans le compendium**
- Dans la catégorie "Dons" ou "Aptitudes de classe", cliquez sur "+".
- Editez le Don ou l'Apitude créée.
- Suivre les étapes de la partie 7.2.

**7.2 : Renseigner une capacité**
- Renseignez le nom de la capacité.

Onglet Description :
- Renseigez la description de la capacité.

Onglet Détails :
- Indiquez le type de la capacité.
- S'il ne s'agit pas d'un Don, indiquez si il s'agit d'une capacité Ext, Sur ou Mag si cette catégorie s'applique.
Certaines capacités, comme la Recherche des Pièges du Roublard, n'ont pas de type Ext, Sur ou Mag. Laissez cette case blanche dans ce cas.
- Indique quelles armes et armures la capacité octrois le cas échéant.
- S'il s'agit d'une aptitude de classe, cliquez sur "Classes", puis ajoutez des classes en cliquant sur "+" pour chaque classe et en indiquant le nom des classes.
- S'il s'agit d'une capacité nécessitant d'être utilisée rapidement durant la partie (comme l'Empathie sauvage) et/ou possédant un DD (comme le Tourbillon, la Maladie ou le Poison), cochez la case "Afficher dans raccourcis"
- Dans "Utilisation d'une capacité", si la capacité a une portée, une durée ou une limite d'utilisation, renseignez le coût d'activation de la capacité.
Une capacité n'ayant pas de coût d'activation est soit une capacité passive, soit une action gratuite.
- Renseignez la cible de la capacité si celle-ci n'est pas évidente et a une ou plusieurs conditions à respecter (par exemple, si la cible ne peut être qu'un humanoïde).
- Renseignez la portée, la durée et la limite d'utilisation le cas échéant.
- Si la capacité à une limite d'utilisation, vérifiez que la case "déduire automatiquement la charge" est bien cochée.
- Dans "Attaque", s'il s'agit d'une capacité de soin, renseignez "Soin". S'il ne s'agit pas d'une capacité de soin, mais d'une capacité ayant un DD, renseignez "Autre".
- Si la capacité fait des dégâts (comme un poison faisant 1d2 dégâts temporaires de Force), renseignez les dégâts dans "Formule de dégâts". Indiquez quels types de dégâts sont faits.
Pour les dégâts de caractéristiques temporaires, indiquez par exemple "For (temp)".
- Si la capacité a une durée d'effet aléatoire, renseignez dans "formule de dégâts" le jet pour la durée aléatoire, et mettez en type le type de durée (rounds, minutes, heures...)
- Si la capacité a une durée d'incubation aléatoire, renseignez dans "formule de dégâts" le jet pour la durée aléatoire, et mettez en type le type de durée (rounds, minutes, heures...) suivi de "d'incubation" (par exemple, "jours d'incubation").
- Si la capacité a un DD, renseignez-le avec une formule dans "DD", puis indiquez quel jet de sauvegarde utiliser.
Pour la formule, inspirez vous de la formule suivante : `10+floor(@attributes.hd.total/2)+@abilities.con.mod`
> Utiliser une formule permet de mettre à jour la valeur du DD en cas de modification des caractéristiques de la créature. Par exemple en lui donnant un archétype Evolué.
{.is-info}

> `floor` indique que l'on veut arrondir à l'entier inférieur la valeur entre parenthèse.
`@attributes.hd.total` donne le nombre total de DVs de la créature.
`@abilities.con.mod` donne le modificateur de Constitution. Remplacez `con` par la bonne caractéristique si votre capacité utilise autre chose que la Constitution pour son DD (`str` pour la Force, `dex` pour la Dextérité, `con` pour la Constitution, `int` pour l'Intelligence, `wis` ou `cha` pour le Charisme).
La formule donnée en exemple se traduit donc : "10 + la moitié des DVs de la créature arrondie à l'inférieur + le modificateur de Constitution de la créature".
{.is-info}
- Indiquez dans "Notes sur les effets" des notes contextuelles qui apparaitront sous la capacité dans le chat lorsque vous utiliserez la capacité.
![exemple_capacite_poison.png](/pf1/exemple_capacite_poison.png)

Onglet Changements :
- Dans "Changements", indiquez les bonus fixes qui ne sont pas des dés bonus aux dégâts apportés par la capacité.
- Dans "Notes de contexte", renseignez les bonus conditionnels apportés par la capacité.

#### 8. Combat :

**8.1 : Arme manufacturée**

Onglet Détails :
- Retournez sur l'onglet "Inventaire".
- Editez l'arme pour laquelle vous voulez créer une attaque.
- Dans "Détails", cliquez sur "créer une attaque" tout en bas.
- Retournez sur l'onglet "Combat".
- Vérifiez que les valeurs de l'attaque sont correctes.
> Vous remarquerez que la formule de dégâts est maintenant de la forme `sizeRoll(1,4,@size)`. Il s'agit d'une formule permettant de calculer le dé de dégât en fonction de la taille. Le premier nombre indique le nombre de dés à lancer pour une arme de taille M, et le deuxième nombre le type de dé à lancer pour une arme de taille M. Vous pouvez voir les dés qui seront finalement lancés dans la colonne "dégâts" du tableau de l'onglet Combat.
{.is-info}
- Si le dé de dégât est incorrect, c'est probablement que la créature considère son arme comme d'une taille différente par rapport aux autres créatures. Si le dé est trop gros, diminuez le deuxième nombre de la formule `sizeRoll()` pour atteindre la bonne taille. Si le dé est trop petit, augmentez-le à la place.
- Si la créature a un BBA assez élevé pour réaliser plusieurs attaques, cliquez sur "Add Extra Attack" pour chaque attaque supplémentaire. Renseignez le malus d'attaque itératif pour chaque attaque supplémentaire ainsi créée.
- Si l'attaque provoque un effet (comme étreinte, croc-en-jambe ou un poison), renseignez-le dans le champ "Notes sur les attaques". Vous pouvez séparer les effets en allant à la ligne. Renseignez aussi le nom de l'effet entre parenthèses dans le nom de l'attaque.
- Si l'attaque demande un jet de sauvegarde à cause d'un effet (comme un poison), récupérez la formule du DD de l'effet (comme indiqué en 7.2) et indiquez-la dans le DD. Renseignez également le type de jet de sauvegarde à effectuer.

**8.2 : Armes naturelles**

(TODO : explication sur pourquoi les armes naturelles principales ont pas les mêmes stats que sur le bestiaire pour un monstre qui a une arme manufacturée)

**8.2.1 : Avec le compendium**

> Le compendium des armes naturelles n'est pas encore partagé sur le module. Il sera mis à disposition sous peu.
{.is-warning}
- Ouvrez le compendium "Attaques naturelles"
- Glissez-déposez l'attaque naturelle souhaitée.
- Editez l'attaque naturelle.
- Si une capacité du monstre indique qu'il considère une de ses attaques naturelles comme une attaque principale ou secondaire, refletez cette capacité en cochant ou décochant "Attaque principale".
Si c'est une attaque secondaire devenue principale, changez le multiplicateur de la caractéristique pour les dégâts à x1.
Si c'est une attaque principale devenue secondaire, changez le multiplicateur de la caractéristique pour les dégâts à x0.5.
- Si la créature a le don Attaques multiples et que l'attaque naturelle est une attaque naturelle secondaire, indiquez "3" dans le champ "Bonus au jet d'attaque".
Pour rappel, une attaque secondaire subit normalement -5 au jet d'attaque et son multiplicateur de Force aux dégâts est de 0.5. Le don Attaques multiples permet de passer ce malus de -5 à -2. Rajouter +3 à l'attaque permet donc de faire passer le malus de -5 à -2.
- S'il s'agit de l'unique attaque de la créature, cochez la case "Attaque principale".
- S'il s'agit de l'unique attaque de la créature et qu'elle ne peut l'effectuer qu'une fois, changez le multiplicateur de la caractéristique pour les dégâts à x1.5.
- Si la créature a le don Attaques en finesse, passez la caractéristique pour l'attaque à Dextérité.
- Si la créature a le don Attaques en finesse (mythique), passez la caractéristique pour ls dégâts à Dextérité.
- Si l'attaque possède des dégâts bonus aux dégâts critiques, renseignez-les dans "Formule bonus dégâts critiques".
- Si l'attaque possède des dégâts bonus aux dégâts uniquement non-critiques, renseignez-les dans "Formule bonus dégâts critiques".
- Vérifiez que les valeurs de l'attaque sont correctes.
> A ce stade, vous remarquerez peut-être que certaines attaques naturelles principales n'ont pas les bonnes valeurs sur une créature possédant des armes manufacturées. C'est parce que les attaques naturelles deviennent toutes secondaires au court d'une attaque à outrance incluant des armes manufacturées. Elles subissent donc un malus de -5 au jet d'attaque et leur modificateur aux dégâts passe à x0.5. Ces malus sont déjà comptés dans les blocs de statistiques des monstres, mais pas dans Foundry.
Il n'y a malheureusement pas de méthode élégante pour automatiser ce comportement actuellement. Lors du jet d'attaque pour le monstre, il faudra donc, sur la pop-up du jet pour l'attaque, décocher la case "Attaque principale" et indiquez un malus de -5 au jet d'attaque.
{.is-info}
- Si le dé de dégât est incorrect, c'est probablement que la créature considère son arme comme d'une taille différente par rapport aux autres créatures. Si le dé est trop gros, diminuez le deuxième nombre de la formule `sizeRoll()` pour atteindre la bonne taille. Si le dé est trop petit, augmentez-le à la place.
- Si l'attaque naturelle peut être effectuée plusieurs fois (par exemple, 2 sabots), cliquez sur "Add Extra Attack" pour chaque attaque supplémentaire.
- Si l'attaque naturelle possède des attaques supplémentaires en trop, cliquez sur la poubelle à côté de l'attaque supplémentaire jusqu'à avoir supprimé toutes les attaques en trop.
- Si l'attaque naturelle a des dés de dégâts bonus permanents (comme un Ver du froid qui a 4d6 dégâts de froid supplémentaires), cliquez sur "Add Damage Formula" et renseignez les dégâts bonus et leur type.
- Si l'attaque provoque un effet (comme étreinte, croc-en-jambe ou un poison), renseignez-le dans le champ "Notes sur les attaques". Vous pouvez séparer les effets en allant à la ligne. Renseignez aussi le nom de l'effet entre parenthèses dans le nom de l'attaque.
- Si l'attaque demande un jet de sauvegarde à cause d'un effet (comme un poison), récupérez la formule du DD de l'effet (comme indiqué en 7.2) et indiquez-la dans le DD. Renseignez également le type de jet de sauvegarde à effectuer.
![morsure_reaction_allergique.png](/pf1/morsure_reaction_allergique.png)

**8.2.2 : Sans le compendium**

- Dans la catégorie "Armes naturelles", cliquez sur "+Ajouter".
- Editez l'attaque naturelle.
- Sélectionnez une icone pour l'attaque en cliquant sur l'icone.
- Cochez la case "Attaque principale" si l'arme naturelle est une attaque principale.
- Si une capacité du monstre indique qu'il considère une de ses attaques naturelles comme une attaque principale ou secondaire, refletez cette capacité en cochant ou décochant "Attaque principale".
Si c'est une attaque secondaire devenue principale, changez le multiplicateur de la caractéristique pour les dégâts à x1.
Si c'est une attaque principale devenue secondaire, changez le multiplicateur de la caractéristique pour les dégâts à x0.5.
- Si la créature a le don Attaques multiples et que l'attaque naturelle est une attaque naturelle secondaire, indiquez "3" dans le champ "Bonus au jet d'attaque".
Pour rappel, une attaque secondaire subit normalement -5 au jet d'attaque et son multiplicateur de Force aux dégâts est de 0.5. Le don Attaques multiples permet de passer ce malus de -5 à -2. Rajouter +3 à l'attaque permet donc de faire passer le malus de -5 à -2.
- S'il s'agit de l'unique attaque de la créature, cochez la case "Attaque principale".
- S'il s'agit de l'unique attaque de la créature et qu'elle ne peut l'effectuer qu'une fois, changez le multiplicateur de la caractéristique pour les dégâts à x1.5.
- Indiquez le coup d'activation. Il s'agit en général d'une action simple.
- Renseignez la portée (en pieds) et le nombre maximum d'incréments de portée.
Pour rappel, une arme de jet a au maximum 5 incréments de portée, et une arme à munition a au maximum 10 incréments de portée.
Pour une arme pouvant aussi bien être utilisée au corps à corps qu'à distance, renseignez les valeurs pour le Type de base et pour le Type "Sous-type à distance". Les informations sont gardées en mémoire même si les champs ne sont pas visibles.
- Si l'attaque peut être utilisée qu'un certain nombre de fois, renseignez la partie "Utilisations limitées". Vérifiez que "Déduire automatiquement la charge" est coché.
- Dans "Action", renseignez le type d'action, le facteur de critique et le multiplicateur de critique, la caractéristique utilisée pour l'attaque, la caractéristique utilisée pour les dégâts et le multiplicateur du bonus au dégâts.
- Dans "Formule de dégâts", indiquez la formule de dégâts avec la fonction `sizeRoll()` décrite en 8.1 pour les attaques faisant des dégâts contondants, perçants ou tranchants. Renseignez également le type de dégâts (C, P, T...).
Pour rappel, la formule est `sizeRoll(X, Y, @size)`, où X est le nombre de dés à lancer pour une arme de taille M et Y le type de dés à lancer pour une arme de taille M.
- Dans "Formule de dégâts", indiquez les dès de dégâts comme indiqués sur le profil de la créature pour les autre types de dégâts.
- Si la créature possède plusieurs types de dégâts (comme un Ver du Froid qui a des dégâts normaux et 4d6 dégâts de froid supplémentaires), cliquez sur "Add Damage Formula" afin de créer un jet séparé pour chaque type de dégâts.
> Notez que le bonus d'attribut aux dégâts ne s'applique qu'à la première attaque de la liste.
{.is-info}
- Vérifiez que les valeurs de l'attaque sont correctes.
> A ce stade, vous remarquerez peut-être que certaines attaques naturelles principales n'ont pas les bonnes valeurs sur une créature possédant des armes manufacturées. C'est parce que les attaques naturelles deviennent toutes secondaires au court d'une attaque à outrance incluant des armes manufacturées. Elles subissent donc un malus de -5 au jet d'attaque et leur modificateur aux dégâts passe à x0.5. Ces malus sont déjà comptés dans les blocs de statistiques des monstres, mais pas dans Foundry.
Il n'y a malheureusement pas de méthode élégante pour automatiser ce comportement actuellement. Lors du jet d'attaque pour le monstre, il faudra donc, sur la pop-up du jet pour l'attaque, décocher la case "Attaque principale" et indiquez un malus de -5 au jet d'attaque.
{.is-info}
- Si le dé de dégât est incorrect, c'est probablement que la créature considère son arme comme d'une taille différente par rapport aux autres créatures. Si le dé est trop gros, diminuez le deuxième nombre de la formule `sizeRoll()` pour atteindre la bonne taille. Si le dé est trop petit, augmentez-le à la place.
- Si l'attaque naturelle peut être effectuée plusieurs fois (par exemple, 2 sabots), cliquez sur "Add Extra Attack" pour chaque attaque supplémentaire.
- Si l'attaque possède des dégâts bonus aux dégâts critiques, renseignez-les dans "Formule bonus dégâts critiques".
- Si l'attaque possède des dégâts bonus aux dégâts uniquement non-critiques, renseignez-les dans "Formule bonus dégâts critiques".
- Si l'attaque provoque un effet (comme étreinte, croc-en-jambe ou un poison), renseignez-le dans le champ "Notes sur les attaques". Vous pouvez séparer les effets en allant à la ligne. Renseignez aussi le nom de l'effet entre parenthèses dans le nom de l'attaque.
- Si l'attaque demande un jet de sauvegarde à cause d'un effet (comme un poison), récupérez la formule du DD de l'effet (comme indiqué en 7.2) et indiquez-la dans le DD. Renseignez également le type de jet de sauvegarde à effectuer.

**8.3 : Attaques spéciales**
- Dans la catégorie qui correspond le mieux à l'attaque spéciale, autre que (Attaques avec armes" et "Attaques naturelles" (par exemple "Capacités raciales" pour une attaque de Toile d'araignée), cliquez sur "+Ajouter".
- Editez l'attaque.
- Dans "Description", renseignez la description de l'attaque.
![exemple_toile.png](/pf1/exemple_toile.png)
- Suivez les mêmes étapes que pour renseigner une attaque naturelle sans compendium (cf 8.2.2).


**8.4 : Ajouter les modificateurs conditionnels aux attaques**

Onglet Modificateurs conditionnels :
- Si l'arme a des dégâts bonus ou des bonus non-permanents grace à des capacités ou des propriétés spéciales (comme une arme de feu, une Attaque sournoise ou le Réservoir Arcanique de Magus), cliquez sur "ajouter une condition".
- Renseignez le nom de la condition.
- Cliquez sur "Ajouter un effet".
- Renseignez la formule des dés bonus ou des bonus non-permanents.
- S'il s'agit d'un bonus au jet d'attaque, sélectionnez "Jet d'attaque", puis renseignez sur quel jet l'effectuer, le type du bonus, et si le bonus s'applique à tous les jets ("normal") ou juste au jet de confirmation de critique.
- S'il s'agit de dégâts bonus, sélectionnez "Dégâts", puis renseignez sur quel jet l'effectuer, le type de dégâts, et si le bonus s'applique à tous les jets ("normal") ou juste aux jets critiques ou juste aux jets non-critiques.
- Répétez pour chaque bonus.
- Cochez la case devant les bonus qui sont actifs par défaut, comme les bonus gratuits sans conditions tels que "de feu". Cela évitera de devoir les sélectionner à chaque fois qu'on fera une attaque, mais permettra de les désactiver si nécessaire.
![modificateurs_conditionnels.png](/pf1/modificateurs_conditionnels.png)

#### 9. Compétences :

- Si la créature a un modificateur racial lui permettant d'utiliser un autre attribut que celui normalement utilisé pour la compétence, indiquez-le dans la liste déroulante correspondante.
- Renseignez les rangs dans chaque compétence.
- Il est possible que vous n'ayez pas assez de points de compétences pour atteindre le bon score. Certains DVs raciaux, comme les Elémentaires, proposent de choisir des compétences au choix sur la créature. De même, certaines classes, comme l'Ensorceleur, ont des sous-classes ajoutant des compétences de classe supplémentaires au personnage. Si c'est le cas de la créature, ouvrez son DV racial ou sa classe depuis la partie "Classes" dans l'onglet "Sommaire". Dans "Détails", cochez les compétences de classe qui semblent manquer à la créature (un élémentaire du feu gagne par exemple "Acrobaties" en compétence de classe supplémentaire).
Il est également possible que la créature possède un don du type Talent (cf 7) ou un bonus racial (cf 4.1) que vous n'avez pas correctement complété.

#### 10. Sorts

> Cet onglet n'apparaîtra que si vous avez paramétré au moins un grimoire pour la créature.
{.is-warning}

- Si la créature a un bonus à la concentration venant d'un don, d'une capacité ou d'un objet, renseignez-le dans "Concentration".
- Si la créature a un bonus au NLS venant d'un don, d'une capacité ou d'un objet, renseignez-le dans "Niveau de lanceur de sort".
- Cliquez sur les roues dentées en haut à droite.
- Dans "Classe utilisant la magie", sélectionnez la classe correspondant à ce grimoire.
Les Pouvoirs Magiques utilisent la classe "Dé de vie".
- Renseignez la caractéristique utilisée pour les sorts.
Les Pouvoirs Magiques utilisent le Charisme.
- Renseignez le risque d'échec des sorts profanes et le nombre d'emplacement de domaine/d'école de magie supplémentaire par niveau le cas échéant.
- Cochez les cases à droite selon le cas de la créature.

**10.1 : Avec le compendium**
- Pour chaque sort, cherchez-le dans le compendium des sorts et glissez-déposez-le dans le grimoire correspondant.
- Si le sort a une condition, éditez le sort et indiquez la condition dans le nom du sort entre parenthèses.
- Si le sort est au mauvais niveau, éditez le sort et changez la valeur du "Niveau du sort" en conséquences.

**10.2 : Sans le compendium**
- Au niveau du sort que vous souhaitez ajouter, cliquez sur "+".
- Editez le sort.
- Renseignez le nom du sort. Si le sort a une condition, renseignez-la également ici entre parenthèses.
- Choisissez une icone pour le sort.
Si vous souhaitez garder les mêmes icones que celles utilisées dans le compendium, en voici la liste par école :
> - Abjuration : `systems/pf1/icons/spells/protect-sky-2.jpg`
> - Divination : `systems/pf1/icons/spells/evil-eye-eerie-1.jpg`
> - Enchantement : `systems/pf1/icons/spells/wind-grasp-air-2.jpg`
> - Evocation : `systems/pf1/icons/spells/fire-arrows-2.jpg`
> - Illusion : `systems/pf1/icons/spells/fog-blue-3.jpg`
> - Invocation : `systems/pf1/icons/spells/wild-orange-2.jpg`
> - Nécromancie : `systems/pf1/icons/spells/horror-eerie-2.jpg`
> - Transmutation : `systems/pf1/icons/spells/vines-eerie-1.jpg`
- Renseignez l'école du sort, la branche (indiquée entre parenthèses) et le type (indiqué entre crochets).
- Indiquez si le sort est soumis à la Résistance à la Magie, peut être lancé à volonté, fait des dégâts non-létaux
- Renseignez les composantes du sort
- S'il s'agit d'un pouvoir magique, indiquez le nombre d'utilisations restantes.
- Si nécessaire, renseignez les autres informations de la partie "Détails du sort".
- Indiquez dans "Lancement de sort" l'action à réaliser pour lancer le sort.
- Renseignez la cible, la portée, la zone d'effet et la durée du sort.
Pour la durée, vous pouvez la renseigner en utilisant une formule. Par exemple : `[[10*@cl]] minutes` pour un sort durant 10 minutes par niveau.
Les portées courte, moyenne et longue sont calculées automatiquement.
- Renseignez dans "Effets de sort" le type d'action utilisé.
- 

> A terminer
{.is-danger}

#### 11. Effets :

Si la créature a des effets spéciaux que vous souhaitez renseigner, comme la transformation d'un élémentaire de l'eau grace à la capacité "Vortex" ou l'effet d'un sort d'Armure de mage, vous pouvez les indiquer ici. Les effets fonctionnent comme des capacités, mis à part qu'ils peuvent être activés ou désactivés.
Si vous voulez faire apparaître l'effet sur le jeton de la créature, donnez un icone à cet effet afin de pouvoir le reconnaître.
Si vous ne voulez pas voir apparaître l'icone de l'effet, cochez "Caché du jeton" dans l'onglet Détails.

#### 12. Notes :

Renseignez ici le bloc de statistiques de la créature.

#### 13 Vérifications :
Repassez sur les différents onglets, et vérifiez que toutes les valeurs correspondent bien à la fiche d'origine de la créature. Ajustez en fonction.

> Vous venez de créer votre propre entrée du bestiaire ! Félicitations 🥳
{.is-success}
