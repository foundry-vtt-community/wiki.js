---
title: Naheulbeuk
description: 
published: true
date: 2022-11-19T13:17:52.098Z
tags: naheulbeuk
editor: markdown
dateCreated: 2022-11-15T16:04:44.061Z
---

# Naheulbeuk
Le système pour le JDR Naheulbeuk n'est pas officiel.
Il n'est pas non plus trouvable directement dans Foundry, pour l'installer il faut donc utiliser l'URL :
https://github.com/Dipess/naheulbeuk/releases/latest/download/system.json
Le système est fait pour la version 10 de Foundry.

En cas de problème ou de question, vous pouvez demander de l'aide sur le Discord **La Fonderie** canal naheulbeuk.

Avant de rentrer dans le détails, voici ce qui n'est pas dans le système actuellement :
 * Les sorts niveau 5 et plus
 * Les montures (il y a quand même quelques exemples avec l'extension soldat)
 * Les objets des extensions Fernol, Confins, Jungle
 * Les frappes localisées
 * Surement quelques autres petits trucs que j'ai zappé :D
 
[Création d'un personnage](#titre1)
* [Onglet Caractéristiques](#titre11)
* [Onglet Description](#titre12)
* [Onglet Compétences](#titre13)
* [Onglet Coups spéciaux](#titre14)
* [Onglet APE](#titre15)
* [Onglet Magie](#titre16)
* [Onglet Inventaire](#titre17)

## Création d'un personnage {#titre1}

### Onglet Caractéristiques {#titre11}

Créez un nouveau personnage de type "character"

![1.jpg](/naheulbeuk/1.jpg =500x)

Cliquez sur le portrait pour choisir une image associée à votre personnage.

**Remarque :** 
Un certain nombre de tokens sont inclus dans le système. Ils sont disponibles en allant à l'emplacement suivant : systems/naheulbeuk/assets/from-2minutetabletop.com-token
Ils viennent tous du super site : https://tools.2minutetabletop.com/token-editor/

![2.jpg](/naheulbeuk/2.jpg =250x)

Pensez bien ensuite à cliquer sur **Prototype token** en haut à droite, puis cochez **Link Actor Data** dans l'onglet **Identity**.

Puis commencez par cliquer sur **Caractéristiques** pour faire un lancer de dés de **1d6+7** et rentrez cette valeur dans la colonne **Base** du **Courage**.
Répetez cette opération pour l'intelligence, le charisme, l'adresse et la force.
Vous pouvez bien sûr modifier ces valeurs suivant votre façon de créer un personnage ;)
Vous pouvez déjà remarquer que la résistance magique et l'esquive ont été mis à jour à partir de ces valeurs.

En fonction de ces résultats, vous pouvez choisir une origine et un métier.

Cliquez sur **Origine**, puis faites un drag and drop de l'origine souhaitée sur la feuille de personnage.

Cliquez sur **Métier**, puis faites un drag and drop du métier souhaité sur la feuille de personnage.

Si vous choisissez un mage, un prêtre ou un paladin, vous voyez apparaitre de nouvelles statistiques : l'énergie astrale, la magie physique et la magie psychique.
Un onglet **Magie** apparait également, mais on en reparlera plus tard !

Les origines et les métiers ne mettent pas à jour automatiquement la fiche du personnage. Il faut donc cliquer sur le métier et l'origine choisie, puis lire la description et mettre à jour les stats (énergie vitale, énergie Astrale, charge maximum, attaque, parade...).

Au passage, vous pouvez drag and drop les **Compétences héritées**, et le nombre de **Compétences choisies** correspondant au niveau de votre personnage.

![4.jpg](/naheulbeuk/4.jpg =700x)

Vous pouvez maintenant renseigner le **Sexe** et les **Signes particuliers** de votre personnage.

Il ne reste plus qu'à cliquer sur **Point de destin** pour générer la valeur initiale. 

**Remarques :**
* Lorsqu'il y a un **d20**, on peut cliquer dessus pour faire un lancer de dés. Par exemple si je clique sur le d20 de l'intelligence, j'obtiens :
![5.jpg](/naheulbeuk/5.jpg =200x)
* Lorsqu'il y a un **d6**, on peut cliquer dessus pour ouvrir une interface permettant de modifier les caractéristiques du lancer :
![6.jpg](/naheulbeuk/6.jpg =400x)
* L'oeil à droite de **Caractéristiques** permet de voir les objets équipés qui impactent mon personnage.
![7.jpg](/naheulbeuk/7.jpg =400x)
* Si l'adresse du personnage passe au dessus de 12 ou en dessous de 9, une fenêtre s'ouvre pour permettre de donner le bonus/malus.
![8.jpg](/naheulbeuk/8.jpg =400x)
* Si le personnage monte de niveau, une fenêtre s'ouvre pour lui indiquer les changements à faire.
![9.jpg](/naheulbeuk/9.jpg =400x)
* La valeur de **Charge** se met à jour automatiquement en fonction de l'inventaire.
* En cliquant sur **Protection** on peut voir la répartition de l'armure physique
![15.jpg](/naheulbeuk/15.jpg =400x)
* Un calcul basique des déplacements en fonction des PR est disponible à droite de **Protection**. La première valeur est le déplacement possible depuis l'arrêt, la deuxième valeur est le déplacement possible lorsqu'on est lancé.
<br/>
### Onglet Description {#titre12}

Vous pouvez rentrer ici tout ce qui est important à savoir sur le personnage. 
Un template de base est présent pour vous donner des idées.

![10.jpg](/naheulbeuk/10.jpg =500x)
<br/>
### Onglet Compétences {#titre13}

On retrouve ici les compétences héritées et choisies drag and drop précédemment.
Comme pour l'onglet **Caractéristiques** vous trouverez des d20 et d6 en fonction de si la compétence demande un jet de dés ou non.
*Par exemple Erudition à des dés pour permettre un test d'intelligence en cas de lecture du langue inhabituelle*

Si mon personnage joueur gagne une compétence, je peux chercher la compétence en question dans le compendium des compétences choisies ou héritées et la drag and drop. Puis je l'édite pour cocher **Compétence de base** et indiquer comment j'ai gagné la compétence.

![11.jpg](/naheulbeuk/11.jpg =500x)

**/!\ Ajout personnel**
J'aime bien demander à mes joueurs de faire un test sur une compétence, plutôt que sur une caractéristique. 
Et je trouve ça dommage de limiter les actions si un personnage ne possède pas certaines compétences.
*Par exemple, si mon personnage veut escalader un mur mais n'a pas la compétence, je trouve ça dommage de lui dire non.*

J'ai donc rajouté la notion de **Compétences de base**.
Une compétence de base c'est quoi ? 
Une compétence que tout le monde peut avoir (nager, escalader, tenter de voler quelqu'un, se déplacer silencieusement...), mais une compétence qui est beaucoup plus dur à réaliser que la même compétence choisie ou héritée.
Ca peut aussi être une compétence qui n'existe pas dans naheulbeuk mais qui me permet d'éviter de demander un jet de caractéristiques (courir/sauter, perceptionner ref Game of roles :D)

Bref vous n'êtes pas du tout obligé d'utiliser ça, mais si vous le souhaitez il existe un compendium **Compétences de base** dédié.

![12.jpg](/naheulbeuk/12.jpg =700x)
<br/>
### Onglet Coups spéciaux {#titre14}

Cet onglet n'est visible que si on a drag and drop un coup spécial sur le personnage.

Il y a 2 types de coups spéciaux.

Les coups de type Bourre Pif (BP) et les autres (GEN = généraliste, MEN = ménestrel, NIN = ninja).
L'utilisation est identique pour les 2 types (toujours les d6 et d20 si existant) mais la fiche de description du coup est légèrement différente.

Si un lancé de est possible ils sont donc présents.
*Par exemple un coup de boule destabilisant qui demande la moyenne de la force et de l'adresse)*
Sinon, l'épreuve est indiqué.
*Par exemple un barrage d'acier demande une épreuve d'attaque classique, avec l'arme souhaitée*

**Remarque :** ne soyez pas surpis de voir des "@fo" pour la force ou "@bonusfo" pour le bonus de force > 12. C'est la convention des raccourcis du système.
J'en reparlerai dans le chapitre avancé sur les macros.

![13.jpg](/naheulbeuk/13.jpg =500x)
<br/>
### Onglet APE {#titre15}

Cet onglet n'est visible que si on a drag and drop une Aptitude Parfois Etrange sur le personnage.

Les APE sont tellement WTF qu'il est impossible d'automatiser les bonus associés. C'est donc à vous et à vos joueurs de vous rappeler de ces bonus, ou de faire un nouvel "Etat" pour s'en rappeler.

Ceci étant dit, rien de particulier à rajouter.
Simplement si vos joueurs gagnent une APE existante, il ne faut pas la drag and drop mais éditer celle qui existe pour changer le niveau et le bonus.

![14.jpg](/naheulbeuk/14.jpg =500x)
<br/>
### Onglet Magie {#titre16}

Cet onglet n'est visible que si le personnage est un mage, un prêtre ou un paladin.

Il contient un sous onglet **Capacités magiques** qui permet de sélectionner le type de magie du personnage, puis de nouveaux onglets en fonction des types sélectionnés.

![16.jpg](/naheulbeuk/16.jpg =500x)

Comme pour le reste, il faut ensuite drag and drop les sorts à partir du compendium adapté.
*Par exemple pour la magie du feu, je vais dans le compendium **Magie : feu** et je drag and drop les sorts niveau 1*

![17.jpg](/naheulbeuk/17.jpg =500x)

**Remarques :**
* Comme pour les coups spéciaux, on retrouve les raccourcis du système : "@mphy" pour la magie physique, "@bonusint" pour le bonus d'intelligence > 12 ...
Par contre ici on peut aussi trouver des formules du type : "cible:@rm". Elles signifient que le PNJ cible doit faire un jet de dés (ici de résistance magique). Dans ces cas là, soit le MJ fait le jet de RM pour la cible, soit le joueur doit cibler le PNJ avant de faire le jet.
* L'incantation, la durée, la portée ET LE COÛT ne sont pas automatisés. C'est à vous et vos joueurs de prendre en compte les valeurs
* En cliquant sur le stylo, on ouvre la description qui contient beaucoup plus d'information. **C'est indispensable d'aller la lire car le résumé affiché n'est pas suffisant**.
* Si un sort a uniquement un **d6** pour son épreuve/dégât, c'est qu'il nécessite un peu plus qu'un simple lancer de dés. En cliquant dessus on ouvre une fenêtre avec plusieurs actions à faire.
![18.jpg](/naheulbeuk/18.jpg =700x)
* En cliquant sur **Niveau X** on peut masquer tous les sorts d'un niveau. Bien pratique quand on commence à avoir beaucoup de sort !
* Comme indiqué en introduction, seuls les sorts niveau 1-4 sont présents. A plus haut niveau il faut donc faire vos propres sorts, en s'inspirant des sorts existants.
* Les sorts peuvent être drag an drop dans la barre de macro.
![19.jpg](/naheulbeuk/19.jpg =700x)
<br/>
### Onglet Inventaire {#titre17}

L'inventaire contient 3 catégories.
* **Armement et baston - objets en mains** : cette catégorie contient tous les objets tenus en main et qui sont équipés. Ce sont principalement les armes, les boucliers et les munitions, mais on peut y retrouver d'autres objets (des instruments, des bannières...).
C'est par ici qu'on fait les lancer de dés pour l'attaque, les dégâts, la parade, et la rupture.
![20.jpg](/naheulbeuk/20.jpg =500x)
* **Armures et Protection - objets portés** : cette catégorie contient tous les objets portés par le personnage et qui sont équipés. Ce sont principalement les armures, les bijoux et les vêtements.
On peut faire ici les tests de rupture.
![21.jpg](/naheulbeuk/21.jpg =500x)
* **Equipement et Trucs** : cette catégorie contient l'ensemble des objets dont ceux des catégories précédentes avant qu'ils soient équipés.
Certains objets peuvent être équipés, d'autres utilisés, d'autres encore avoir des jets de dés.
Il est divisé en 3 sous catégories :
  * **Les objets dans le sac :** c'est la que seront la grosse majorité des objets du personnage. Pour mieux les retrouver, ils sont placés dans un troisième niveau de catégories qui s'affichent uniquement si elles contiennent des objets. Ce sont les objets divers, les livres, les ingrédients, les potions, les armes, les armures, la nourriture, les richesses, les montures, les objets personnels...
  ![22.jpg](/naheulbeuk/22.jpg =500x)
  * **Les objets dans la bourse** : on retrouve ici les objets qui sont dans la bourse, donc elle est utilisée principalements pour les pièces.
  * **Les objets en dehors du sac** : tout ce qui n'est pas dans le sac ou dans la bourse devrait être rangé ici : une guitare en bandoulière, une épée à la ceinture, un sac sur le dos...
  
**Remarques :**
* Les charges max du sac et de la bourse se mettent à jour lorsqu'un ou plusieurs sacs/bourses sont équipés.
![23.jpg](/naheulbeuk/23.jpg =500x)
* Les boutons à droite des objets permettent de changer la catégorie d'inventaire d'un objet : la grosse valise met l'objet dans le sac, la petite valise met l'objet dans la bourse et la croix place l'objet en dehors du sac. 
* Tous les objets peuvent être drag and drop dans la barre de macro. Si c'est une arme, on pourra faire les différents jet de dés à partir d'ici.
![24.jpg](/naheulbeuk/24.jpg =700x)
* Un objet équipé peut être consulté, mais pas modifié.


## Les PNJ {#titre2}

## L'extension soldat

## L'extension ingénieur

## Les "objets" 

### Les trucs

### Les armures et autres objets portés

### Les armes et autres objets en mains

### Les sacs

### Les reliques

### Les instruments de musique

### Les armes à poudre

### Les états

### Les sorts

### Les compétences

### Les métiers et origines

### Les APE

### Les coups spéciaux

### Les gemmes

### Les pièges

### Les régions

### Les traits

## Les tableaux

## Les macros

### Chercher un objet et générer un magasin

### Combat rapide

### Lancer Custom

### Rechercher un compendium

### Rencontres : liste et "générateur"

### Tirer un élément aléatoire d'un compendium

### Avancé