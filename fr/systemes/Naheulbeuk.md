---
title: Naheulbeuk
description: 
published: true
date: 2022-11-20T13:36:24.425Z
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

## Sommaire
* [Création d'un personnage](#titre1)
* [Les PNJ](#titre2)
* [L'extension soldat](#titre3)
* [L'extension ingénieur](#titre4)
* [Les objets](#titre5)
* [Les tableaux](#titre7)
* [Les macros](#titre8)

## Création d'un personnage {#titre1}
* [Onglet Caractéristiques](#titre11)
* [Onglet Description](#titre12)
* [Onglet Compétences](#titre13)
* [Onglet Coups spéciaux](#titre14)
* [Onglet APE](#titre15)
* [Onglet Magie](#titre16)
* [Onglet Inventaire](#titre17)
<br>

### Onglet Caractéristiques {#titre11}

Créez un nouveau personnage de type "character"

![1.jpg](/naheulbeuk/1.jpg =500x)

Cliquez sur le portrait pour choisir une image associée à votre personnage.

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

<u>Les origines et les métiers ne mettent pas à jour automatiquement la fiche du personnage.</u> Il faut donc cliquer sur le métier et l'origine choisie, puis lire la description et mettre à jour les stats (énergie vitale, énergie astrale, charge maximum, attaque, parade...).

Au passage, à partir des métiers et origines, vous devez aussi drag and drop les **Compétences héritées**, et le nombre de **Compétences choisies** correspondant au niveau de votre personnage.

![4.jpg](/naheulbeuk/4.jpg =700x)

Vous pouvez maintenant renseigner le **Sexe** et les **Signes particuliers** de votre personnage.

Il ne reste plus qu'à cliquer sur **Point de destin** pour générer la valeur initiale. 

**Remarques :**
* Lorsqu'il y a un **d20**, on peut cliquer dessus pour faire un lancer de dés. Par exemple si je clique sur le d20 de l'intelligence, j'obtiens :
![5.jpg](/naheulbeuk/5.jpg =200x)
* Lorsqu'il y a un **d6**, on peut cliquer dessus pour ouvrir une interface permettant de modifier les valeurs du lancer :
![6.jpg](/naheulbeuk/6.jpg =400x)
* Si un état (bonus, malédiction, blessure, folie...) est drag and drop sur le personnage, il apparait sur cet onglet. Cet état n'est pas appliqué tant que la case blanche n'est pas cochée.
*Par exemple, mon personnage attrape le rhume des pierres. Je drag and drop cet état depuis le compendium des maladies. Au bout de 4h, les symptomes apparaissent. Je clique donc sur le carré blanc pour activer la prise en compte des malus.*
![25.jpg](/naheulbeuk/25.jpg =400x)
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

On retrouve ici les compétences héritées et choisies, drag and drop précédemment.
Comme pour l'onglet **Caractéristiques** vous trouverez des **d20** et **d6** en fonction de si la compétence demande un jet de dés ou non.
*Par exemple Erudition a des dés pour permettre un test d'intelligence en cas de lecture d'une langue inhabituelle*

Si mon personnage joueur gagne une compétence, je peux chercher la compétence en question dans le compendium des compétences choisies ou héritées et la drag and drop. Puis je l'édite pour cocher **Compétence de base** et indiquer comment j'ai gagné cette compétence.

![11.jpg](/naheulbeuk/11.jpg =500x)

**/!\ Ajout personnel**
J'aime bien demander à mes joueurs de faire un test sur une compétence, plutôt que sur une caractéristique.
*Par exemple, si mon personnage pousuit un PNJ, je préfère lui demander un test de courir/sauter plutôt que d'adresse.*
Et je trouve ça dommage de limiter les actions si un personnage ne possède pas certaines compétences.
*Par exemple, si mon personnage veut escalader un mur mais n'a pas la compétence, je trouve ça dommage de lui dire non.*

J'ai donc rajouté la notion de **Compétences de base**.
Une compétence de base c'est quoi ? 
Une compétence que tout le monde peut avoir (nager, escalader, tenter de voler quelqu'un, se déplacer silencieusement...), mais une compétence qui est beaucoup plus dur à réussir que la même compétence choisie ou héritée.
Ca peut aussi être une compétence qui n'existe pas dans naheulbeuk mais qui me permet d'éviter de demander un jet de caractéristique (courir/sauter, perceptionner ref Game of roles :D)

Bref vous n'êtes pas du tout obligés d'utiliser ça, mais si vous le souhaitez il existe un compendium **Compétences de base** dédié.
Vous pouvez aussi les utiliser et modifier les malus que j'ai fixé. 
Personnelement j'aime bien rajouter "@malus-mvt-pr" pour prendre en compte l'armure physique de mon personage. 
*Par exemple déplacement silencieux donnerait : @ad -2 -@malus-mvt-pr*
Au passage "@ad" ou "@int"... est la convention des raccourcis du système.

![12.jpg](/naheulbeuk/12.jpg =700x)
<br/>
### Onglet Coups spéciaux {#titre14}

Cet onglet n'est visible que si on a drag and drop un coup spécial sur le personnage.

Il y a 2 types de coups spéciaux.

Les coups de type Bourre Pif (BP) et les autres (GEN = généraliste, MEN = ménestrel, NIN = ninja, CS = soldat).
L'utilisation est identique pour les 2 types (toujours les d6 et d20 si existant) mais la fiche de description du coup est légèrement différente.

Si un lancé de dés est possible ils sont donc présents.
*Par exemple pour un coup de boule déstabilisant qui demande la moyenne de la force et de l'adresse*
Sinon, l'épreuve est indiquée mais il n'y a pas de dés.
*Par exemple pour un barrage d'acier qui demande une épreuve d'attaque classique, avec l'arme souhaitée*

**Remarque :** ne soyez pas surpis de voir des "@fo" pour la force ou "@bonusfo" pour le bonus de force > 12. C'est la convention des raccourcis du système.
J'en reparlerai dans le chapitre avancé sur les macros.

![13.jpg](/naheulbeuk/13.jpg =500x)
<br/>
### Onglet APE {#titre15}

Cet onglet n'est visible que si on a drag and drop une **A**ptitude **P**arfois **E**trange sur le personnage.

Les APE sont tellement WTF qu'il est impossible d'automatiser les bonus associés. C'est donc à vous et à vos joueurs de vous rappeler de ces bonus, ou de faire un nouvel "Etat" pour s'en rappeler.

Ceci étant dit, rien de particulier à rajouter.
Simplement si vos joueurs gagnent une APE déjà existante, il ne faut pas la drag and drop mais éditer celle qui existe pour changer le niveau et le bonus.

![14.jpg](/naheulbeuk/14.jpg =500x)
<br/>
### Onglet Magie {#titre16}

Cet onglet n'est visible que si le personnage est un mage, un prêtre ou un paladin.

Il contient un sous onglet **Capacités magiques** qui permet de sélectionner les types de magie que le personnage connait. Quand c'est fait, de nouveaux onglets apparaissent en fonction des types sélectionnés.

![16.jpg](/naheulbeuk/16.jpg =500x)

Comme pour le reste, il faut ensuite drag and drop les sorts à partir du compendium adapté.
*Par exemple pour la magie du feu, je vais dans le compendium **Magie : feu** et je drag and drop les sorts niveau 1*

![17.jpg](/naheulbeuk/17.jpg =500x)

**Remarques :**
* On retrouve les **d20** et **d6** habituels.
* Comme pour les coups spéciaux, on retrouve les raccourcis du système : "@mphy" pour la magie physique, "@bonusint" pour le bonus d'intelligence > 12 ...
Par contre ici on peut aussi trouver des formules du type : "cible:@rm". Elles signifient que le PNJ cible doit faire un jet de dés (ici de résistance magique). Dans ces cas là, soit le MJ fait le jet de RM pour la cible, soit le joueur doit cibler le PNJ avant de faire le jet.
* L'incantation, la durée, la portée <u>ET LE COÛT</u> ne sont pas automatisés. C'est à vous et vos joueurs de prendre en compte ces valeurs.
* En cliquant sur le stylo, on ouvre la description qui contient beaucoup plus d'information. <u>**C'est indispensable d'aller la lire car le résumé affiché n'est pas suffisant**</u>.
* Si un sort a uniquement un **d6** pour son épreuve ou ses dégât, c'est qu'il nécessite un peu plus qu'un simple lancer de dés. En cliquant dessus on ouvre une fenêtre avec plusieurs actions à faire.
![18.jpg](/naheulbeuk/18.jpg =700x)
* En cliquant sur **Niveau X** on peut masquer tous les sorts d'un niveau. Bien pratique quand on commence à avoir beaucoup de sorts !
* Comme indiqué en introduction, seuls les sorts niveau 1-4 sont présents. A plus haut niveau il faut donc faire vos propres sorts, en s'inspirant des sorts existants.
* Les sorts peuvent être drag an drop dans la barre de macro.
![19.jpg](/naheulbeuk/19.jpg =700x)
<br/>
### Onglet Inventaire {#titre17}

L'inventaire contient 3 catégories.
* **Armement et baston - objets en mains** : cette catégorie contient tous les objets tenus en main et qui sont équipés. Ce sont principalement les armes, les boucliers et les munitions, mais on peut y retrouver d'autres objets (des instruments, des bannières...).
C'est par ici qu'on fait les lancer de dés pour l'attaque, les dégâts, la parade, et la rupture.
![20.jpg](/naheulbeuk/20.jpg =500x)
Comme pour les sorts, un **d6** tout seul signifie un jet un peu particulier, c'est le cas notamment des armes à poudre.
* **Armures et Protection - objets portés** : cette catégorie contient tous les objets portés par le personnage et qui sont équipés. Ce sont principalement les armures, les bijoux et les vêtements.
On peut faire ici les tests de rupture.
![21.jpg](/naheulbeuk/21.jpg =500x)
* **Equipement et Trucs** : cette catégorie contient l'ensemble des objets dont ceux des catégories précédentes avant qu'ils soient équipés.
Certains objets peuvent être équipés, d'autres utilisés, d'autres encore avoir des jets de dés.
Ils sont divisés en 3 sous catégories :
  * **Les objets dans le sac :** c'est la que seront la grosse majorité des objets du personnage. Pour mieux les retrouver, ils sont placés dans un troisième niveau de catégories qui s'affichent uniquement si elles contiennent des objets. Ce sont les objets divers, les livres, les ingrédients, les potions, les armes, les armures, la nourriture, les richesses, les montures, les objets personnels...
  ![22.jpg](/naheulbeuk/22.jpg =500x)
  * **Les objets dans la bourse** : on retrouve ici les objets qui sont dans la bourse, donc cette partie est utilisée principalements pour les pièces.
  * **Les objets en dehors du sac** : tout ce qui n'est pas dans le sac ou dans la bourse devrait être rangé ici : une guitare en bandoulière, une épée à la ceinture, un sac sur le dos...
  
**Remarques :**
* Les charges max du sac et de la bourse se mettent à jour lorsqu'un ou plusieurs sacs/bourses sont équipés.
![23.jpg](/naheulbeuk/23.jpg =500x)
* Les boutons à droite des objets permettent de changer la catégorie d'inventaire d'un objet. Depuis les captures précédentes, ils ont changé car Foundry a rajouté de nouvelles icônes. 
Le sac simple envoie les objets dans le sac, le sac avec le dollar envoie dans la bourse et le sac avec une croix envoie en dehors du sac.
![26.jpg](/naheulbeuk/26.jpg =700x)
* Tous les objets peuvent être drag and drop dans la barre de macro. Si c'est une arme, on pourra faire les différents jet de dés à partir d'ici.
![24.jpg](/naheulbeuk/24.jpg =700x)
* Un objet équipé peut être consulté, mais pas modifié.


## Les PNJ {#titre2}
Dans ce chapitre, nous n'allons pas voir comment faire un nouveau PNJ, mais plutôt regarder le fonctionnement de ceux déjà créés.
De manière générale lorsque vous souhaitez créer un nouvel objet ou PNJ il faut toujours partir de l'existant !

Le système contient des PNJ dans le compendium **Bestiaire**.

![47.jpg](/naheulbeuk/47.jpg =300x)
Je vais prendre comme exemple un Gobelin ingénieur.

![27.jpg](/naheulbeuk/27.jpg =500x)

Sa fiche est divisée en 4 parties.

**L'entête contient :**
* Le nom
* L'image
* La catégorie : Fanghien, humanoïde, animaux, végétaux...
* Les traits
Pour rajouter un nouveau trait, il faut aller le chercher depuis le compendium **Système : bestaire données** puis le drag and drop. Il peut être ouvert en cliquant dessus.
![28.jpg](/naheulbeuk/28.jpg =400x)

* La répartition géographique
Pour rajouter une nouvelle région, il faut aller la chercher depuis le compendium **Système : bestaire données** puis la drag and drop. Elle peut être ouverte en cliquant dessus.
![29.jpg](/naheulbeuk/29.jpg =400x)

**L'onglet Caractéristiques contient :**
* Toutes les statistiques de base d'un PNJ
![48.jpg](/naheulbeuk/48.jpg =500x)
* Des statistiques supplémentaires parfois utiles (visibles en cliquant sur **Caractéristiques +**)
![30.jpg](/naheulbeuk/30.jpg =500x)
* Les états éventuels qui ont été drag and drop. 
Théoriquement tous les états fonctionnent avec les PNJ, mais étant donné que les stats des PNJ et des PJ ne sont pas tout à fait identiques sur Naheulbeuk, attention quand même !
![33.jpg](/naheulbeuk/33.jpg =500x)
* La description du PNJ

**L'onglet Combat contient :**
* Les armes/attaques avec leur valeurs
Comme pour le reste, un **d20** permet un jet de dés directement, un **d6** permet un jet de dés customisé et un **d6 seul** permet un jet de dés spécial.
![36.jpg](/naheulbeuk/36.jpg =500x)
<u>A noter :</u> sur Naheulbeuk, les PNJ n'utilisent pas les armes des PJ. Ils ont des attaques qui intègrent directement l'épreuve et les dégâts. C'est donc un objet Foundry différent qui est utilisé.
![35.jpg](/naheulbeuk/35.jpg =500x)
* Les coups spéciaux ou les sorts qui ont été drag and drop
![32.jpg](/naheulbeuk/32.jpg =500x)
* Les informations à connaitre pour le combat du PNJ
![31.jpg](/naheulbeuk/31.jpg =500x)

**L'onglet Inventaire contient :**
* Tous les objets drag and drop sur le personnage.
![34.jpg](/naheulbeuk/34.jpg =500x)

## L'extension soldat {#titre3}

Toutes les informations utiles pour cette extension sont regroupées dans les fiches de métier des soldats.
![37.jpg](/naheulbeuk/37.jpg =300x)
La seule information manquante conscerne les volontaires, qui sont des aventuriers normaux. Donc pour eux il reste indispensable de consulter les pdf pour connaître le fonctionnement (notamment la solde).

Dans la fiche métier, on retrouve les informations suivantes :
* La description
* Les prérequis
* La carrière en fonction du niveau, avec la solde
* Les compétences
* Les coups spéciaux
* Un lien vers tout l’équipement
* Un lien vers les tableaux de récompenses et de punitions

![38.jpg](/naheulbeuk/38.jpg =700x) 

Le fonctionnement est identique celui d'un métier classique avec deux informations supplémentaires :
* La liste de l'équipement via **Extension soldat : équipement**
En cliquant dessus, un document de type journal s'ouvre. Il contient la liste de tous les objets des soldats qui peuvent être drag and drop sur la feuille du personnage.
![39.jpg](/naheulbeuk/39.jpg =500x)
* Les tableaux de **Récompenses** et **Punitions**
Ce sont des tableaux qui permettent de tirer au hasard une récompense ou une punition.
![40.jpg](/naheulbeuk/40.jpg =500x)

## L'extension ingénieur {#titre4}
Cette extension apporte plusieurs nouveautés.
* Les fiches de métiers des différents ingénieurs.
![41.jpg](/naheulbeuk/41.jpg =300x)
Chaque fiche contient le détail de la spécialisation avec les objets qui peuvent être drag and drop.
![42.jpg](/naheulbeuk/42.jpg =500x)
* La liste des plans et recettes classés par catégorie d’ingénieur : **Extension ingénieur : plan**
![43.jpg](/naheulbeuk/43.jpg =500x)
* La liste des matériaux et outils des ingénieurs : **Extension ingénieur : matériaux**
![44.jpg](/naheulbeuk/44.jpg =500x)


 
**Remarque :** 
* Les plans listent parfois des matériaux qui ne sont pas dans le document précédent, soit parce qu’ils existaient déjà ailleurs, soit parce qu’ils sont spéciaux (c’est comme ça dans le pdf).
J'ai quand même créé ces matériaux, et ils peuvent être trouvés en parcourant les compendiums ou en utilisant l'outil de recherche dont on reparlera dans le chapitre sur les macros.
* En ouvrant un plan, on peut voir et drag and drop l'objet qu'il permet de fabriquer.
![45.jpg](/naheulbeuk/45.jpg =500x)

* Lorsqu'on drag and drop un plan sur un personnage, il se retrouve dans la catégorie **Ingrédients**.
On retrouve un **d6** à côté du nom, permettant de faire un jet de fabrication en tant que spécialiste ou non spécialiste
*Le jet calcule l'ingéniosité et prend en compte la difficulté de l'objet.*
![46.jpg](/naheulbeuk/46.jpg =500x)

## Les objets {#titre5}
* [Les trucs](#titre51)
* [Les armures et autres objets portés](#titre52)
* [Les armes et autres objets en mains](#titre53)
* [Les sacs](#titre54)
* [Les reliques](#titre55)
* [Les instruments de musique](#titre56)
* [Les armes à poudre](#titre57)
* [Les états](#titre58)
* [Les sorts](#titre59)
* [Les compétences](#titre60)
* [Les métiers et origines](#titre61)
* [Les APE](#titre62)
* [Les coups spéciaux](#titre63)
* [Les gemmes](#titre64)
* [Les conteneurs](#titre641)
* [Les pièges](#titre65)
* [Les régions](#titre66)
* [Les traits](#titre67)

### Les trucs {#titre51}

### Les armures et autres objets portés {#titre52}

### Les armes et autres objets en mains {#titre53}

### Les sacs {#titre54}

### Les reliques {#titre55}

### Les instruments de musique {#titre56}

### Les armes à poudre {#titre57}

### Les états {#titre58}

### Les sorts {#titre59}

### Les compétences {#titre60}

### Les métiers et origines {#titre61}

### Les APE {#titre62}

### Les coups spéciaux {#titre63}

### Les gemmes {#titre64}

### Les conteneurs {#titre641}

### Les pièges {#titre65}

### Les régions {#titre66}

### Les traits {#titre67}

## Les tableaux {#titre7}

## Les macros {#titre8}
* [Chercher un objet et générer un magasin](#titre81)
* [Combat rapide](#titre82)
* [Lancer Custom](#titre83)
* [Rechercher un compendium](#titre84)
* [Rencontres : liste et "générateur"](#titre85)
* [Tirer un élément aléatoire d'un compendium](#titre86)
* [Avancé](#titre87)

### Chercher un objet et générer un magasin {#titre81}

### Combat rapide {#titre82}

### Lancer Custom {#titre83}

### Rechercher un compendium {#titre84}

### Rencontres : liste et "générateur" {#titre85}

### Tirer un élément aléatoire d'un compendium {#titre86}

### Avancé {#titre87}