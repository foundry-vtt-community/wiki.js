---
title: Naheulbeuk
description: 
published: true
date: 2022-11-22T15:49:55.768Z
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

Avant de rentrer dans le détail, voici ce qui n'est pas dans le système actuellement :
 * Les sorts niveau 5 et plus
 * Les montures (il y a quand même quelques exemples avec l'extension soldat)
 * Les objets des extensions Fernol, Confins, Jungle
 * Les frappes localisées
 * Surement quelques autres petits trucs que j'ai zappé :D
 
Dernière précision, on va parler régulièrement d'objets et de compendiums, ils faut aller les chercher dans la liste des compendiums du système.

![49.jpg](/naheulbeuk/49.jpg =300x)

## Sommaire
* [Création d'un PJ](#titre1)
* [Les PNJ](#titre2)
* [L'extension soldat](#titre3)
* [L'extension ingénieur](#titre4)
* [Les objets](#titre5)
* [Les tableaux](#titre7)
* [Les macros](#titre8)

## Création d'un PJ {#titre1}
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

Un certain nombre de tokens sont inclus dans le système. Ils sont disponibles en allant à l'emplacement suivant : **systems/naheulbeuk/assets/from-2minutetabletop.com-token**
Ils viennent tous du super site : https://tools.2minutetabletop.com/token-editor/

![2.jpg](/naheulbeuk/2.jpg =250x)

Pensez bien ensuite à cliquer sur **Prototype token** en haut à droite, puis cochez **Link Actor Data** dans l'onglet **Identity**.

Puis commencez par cliquer sur **Caractéristiques** pour faire un lancer de **1d6+7** et rentrez cette valeur dans la colonne **Base** du courage.
Répétez cette opération pour l'intelligence, le charisme, l'adresse et la force.
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
* Si un [état](#titre58) (bonus, malédiction, blessure, folie...) est drag and drop sur le personnage, il apparait sur cet onglet. Cet état n'est pas appliqué tant que la case blanche n'est pas cochée.
*Par exemple, mon personnage attrape le rhume des pierres. Je drag and drop cet état depuis le compendium des maladies. Au bout de 4h, les symptômes apparaissent. Je clique donc sur le carré blanc pour activer la prise en compte des malus.*
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

On retrouve ici [les compétences](#titre60) héritées et choisies, drag and drop précédemment.
Comme pour l'onglet **Caractéristiques** vous trouverez des **d20** et **d6** en fonction de si la compétence demande un jet de dés ou non.
*Par exemple Érudition a des dés pour permettre un test d'intelligence en cas de lecture d'une langue inhabituelle*

Si mon personnage joueur gagne une compétence, je peux chercher la compétence en question dans le compendium des compétences choisies ou héritées et la drag and drop. Puis je l'édite pour cocher **Compétence de base** et indiquer comment j'ai gagné cette compétence.

![11.jpg](/naheulbeuk/11.jpg =500x)

**/!\ Ajout personnel**
J'aime bien demander à mes joueurs de faire un test sur une compétence, plutôt que sur une caractéristique.
*Par exemple, si mon personnage poursuit un PNJ, je préfère lui demander un test de courir/sauter plutôt que d'adresse.*
Et je trouve ça dommage de limiter les actions si un personnage ne possède pas certaines compétences.
*Par exemple, si mon personnage veut escalader un mur mais n'a pas la compétence, je trouve ça dommage de lui dire non.*

J'ai donc rajouté la notion de **Compétences de base**.
Une compétence de base c'est quoi ? 
Une compétence que tout le monde peut avoir (nager, escalader, tenter de voler quelqu'un, se déplacer silencieusement...), mais une compétence qui est beaucoup plus dur à réussir que la même compétence choisie ou héritée.
Ça peut aussi être une compétence qui n'existe pas dans Naheulbeuk mais qui me permet d'éviter de demander un jet de caractéristique (courir/sauter, perceptionner ref Game of roles :D)

Bref vous n'êtes pas du tout obligés d'utiliser ça, mais si vous le souhaitez il existe un compendium **Compétences de base** dédié.
Vous pouvez aussi les utiliser et modifier les malus que j'ai fixé. 
Personnellement j'aime bien rajouter [@malus-mvt-pr](#titre87) pour prendre en compte l'armure physique de mon personnage. 
*Par exemple déplacement silencieux donnerait : [@ad](#titre87) -2 - [@malus-mvt-pr](#titre87)*
Au passage [@ad](#titre87) ou [@int](#titre87)... est la convention des raccourcis du système.

![12.jpg](/naheulbeuk/12.jpg =700x)
<br/>
### Onglet Coups spéciaux {#titre14}

Cet onglet n'est visible que si on a drag and drop un [coups spécial](#titre56) sur le personnage.

Il y a 2 types de coups spéciaux.

Les coups de type Bourre Pif (BP) et les autres (GEN = généraliste, MEN = ménestrel, NIN = ninja, CS = soldat).
L'utilisation est identique pour les 2 types (toujours les d6 et d20 si existant) mais leur fiche de description est légèrement différente.

Si un lancé de dés est possible ces dés sont donc présents.
*Par exemple pour un coup de boule déstabilisant qui demande la moyenne de la force et de l'adresse on retrouve les dés*
Sinon, l'épreuve est indiquée mais il n'y a pas de dés.
*Par exemple pour un barrage d'acier qui demande une épreuve d'attaque classique, avec l'arme souhaitée il n'y a pas de dés*

![79.jpg](/naheulbeuk/79.jpg =500x)

**Remarque :** si vous ouvrez un coup spécial, ne soyez pas surpris de voir des [@fo](#titre87) pour la force ou [@bonusfo](#titre87) pour le bonus de force > 12... C'est la convention des raccourcis du système.
<br/>
### Onglet APE {#titre15}

Cet onglet n'est visible que si on a drag and drop une **A**ptitude **P**arfois **E**trange sur le personnage.

Les APE sont tellement WTF qu'il est impossible d'automatiser les bonus associés. C'est donc à vous et à vos joueurs de vous rappeler de ces bonus, ou de faire un nouvel [État](#titre58) pour s'en rappeler.

Ceci étant dit, rien de particulier à rajouter.
Simplement si vos joueurs gagnent une APE déjà existante, il ne faut pas la drag and drop mais éditer celle qui existe pour changer le niveau et le bonus.

![14.jpg](/naheulbeuk/14.jpg =500x)
<br/>
### Onglet Magie {#titre16}

Cet onglet n'est visible que si le personnage est un mage, un prêtre ou un paladin.

Il contient un sous onglet **Capacités magiques** qui permet de sélectionner les types de magie que le personnage connait. Quand c'est fait, de nouveaux onglets apparaissent en fonction des types sélectionnés.

![16.jpg](/naheulbeuk/16.jpg =500x)

Comme pour le reste, il faut ensuite drag and drop [les sorts](#titre59) à partir du compendium adapté.
*Par exemple pour la magie du feu, je vais dans le compendium **Magie : feu** et je drag and drop les sorts niveau 1*

![17.jpg](/naheulbeuk/17.jpg =500x)

**Remarques :**
* On retrouve les **d20** et **d6** habituels.
* Comme pour les coups spéciaux, on retrouve les raccourcis du système dans la fiche du sort : [@mphy](#titre87) pour la magie physique, [@bonusint](#titre87) pour le bonus d'intelligence > 12 ...
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
*Le bonus de force > 12 ou < 9, et la compétence **Tirer correctement** sont déjà pris en compte dans le système.*
* **Armures et Protection - objets portés** : cette catégorie contient tous les objets portés par le personnage et qui sont équipés. Ce sont principalement les armures, les bijoux et les vêtements.
On peut faire ici les tests de rupture.
![21.jpg](/naheulbeuk/21.jpg =500x)
* **Équipement et Trucs** : cette catégorie contient l'ensemble des objets dont ceux des catégories précédentes avant qu'ils soient équipés.
Certains objets peuvent être équipés, d'autres utilisés, d'autres encore avoir des jets de dés.
Ils sont divisés en 3 sous catégories :
  * **Les objets dans le sac :** c'est la que seront la grosse majorité des objets du personnage. Pour mieux les retrouver, ils sont placés dans un troisième niveau de catégories qui s'affichent uniquement si elles contiennent des objets. Ce sont les objets divers, les livres, les ingrédients, les potions, les armes, les armures, la nourriture, les richesses, les montures, les objets personnels...
  ![22.jpg](/naheulbeuk/22.jpg =500x)
  * **Les objets dans la bourse** : on retrouve ici les objets qui sont dans la bourse, donc cette partie est utilisée principalement pour les pièces.
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

* [Entête](#titre2.1)
* [Onglet Caractéristiques](#titre2.2)
* [Onglet Combat](#titre2.3)
* [Onglet Inventaire](#titre2.4)

Dans ce chapitre, nous n'allons pas voir comment faire un nouveau PNJ, mais plutôt regarder le fonctionnement de ceux déjà créés.
De manière générale lorsque vous souhaitez créer un nouvel objet ou PNJ il faut toujours partir de l'existant !

Le système contient des PNJ dans le compendium **Bestiaire**.

![47.jpg](/naheulbeuk/47.jpg =300x)

Je vais prendre comme exemple un Gobelin ingénieur.

![27.jpg](/naheulbeuk/27.jpg =500x)

Sa fiche est divisée en 4 parties.
<br/>
### Entête {#titre2.1}

L'entête contient :
* Le nom
* L'image
* La catégorie : Fanghien, humanoïde, animaux, végétaux...
* Les traits
Pour rajouter un nouveau trait, il faut aller le chercher depuis le compendium **Système : bestiaire données** puis le drag and drop. Il peut être ouvert en cliquant dessus.
![28.jpg](/naheulbeuk/28.jpg =400x)

* La répartition géographique
Pour rajouter une nouvelle région, il faut aller la chercher depuis le compendium **Système : bestiaire données** puis la drag and drop. Elle peut être ouverte en cliquant dessus.
![29.jpg](/naheulbeuk/29.jpg =400x)
<br/>
### Onglet Caractéristiques {#titre2.2}

L'onglet Caractéristiques contient :
* Toutes les statistiques de base d'un PNJ
![48.jpg](/naheulbeuk/48.jpg =500x)
* Des statistiques supplémentaires parfois utiles (visibles en cliquant sur **Caractéristiques +**)
![30.jpg](/naheulbeuk/30.jpg =500x)
* Les états éventuels qui ont été drag and drop. 
Théoriquement tous les états fonctionnent avec les PNJ, mais étant donné que les stats des PNJ et des PJ ne sont pas tout à fait identiques sur Naheulbeuk, attention quand même !
![33.jpg](/naheulbeuk/33.jpg =500x)
* La description du PNJ
<br/>
### Onglet Combat {#titre2.3}
L'onglet Combat contient :
* Les armes/attaques avec leur valeurs
Comme pour le reste, un **d20** permet un jet de dés directement, un **d6** permet un jet de dés customisé et un **d6 seul** permet un jet de dés spécial.
![36.jpg](/naheulbeuk/36.jpg =500x)
<u>A noter :</u> sur Naheulbeuk, les PNJ n'utilisent pas les armes des PJ. Ils ont des attaques qui intègrent directement l'épreuve et les dégâts. C'est donc un objet Foundry différent qui est utilisé.
![35.jpg](/naheulbeuk/35.jpg =500x)
* Les coups spéciaux ou les sorts qui ont été drag and drop
![32.jpg](/naheulbeuk/32.jpg =500x)
* Les informations à connaitre pour le combat du PNJ
![31.jpg](/naheulbeuk/31.jpg =500x)
<br/>
### Onglet Inventaire {#titre2.4}
L'onglet Inventaire contient :
* Tous les objets drag and drop sur le personnage.
![34.jpg](/naheulbeuk/34.jpg =500x)

## L'extension soldat {#titre3}

Toutes les informations utiles pour cette extension sont regroupées dans les fiches de métier des soldats.

![37.jpg](/naheulbeuk/37.jpg =300x)

La seule information manquante concerne les volontaires, qui sont des aventuriers normaux. Donc pour eux il reste indispensable de consulter les pdf pour connaître le fonctionnement (notamment la solde).

Dans la fiche métier, on retrouve les informations suivantes :
* La description
* Les prérequis
* La carrière en fonction du niveau, avec la solde
* Les compétences
* Les coups spéciaux
* Un lien vers tout l’équipement
* Un lien vers les tableaux de récompenses et de punitions

![38.jpg](/naheulbeuk/38.jpg =700x) 

Le fonctionnement est identique à celui d'un métier classique avec deux informations supplémentaires :
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
* La liste des plans et recettes classés par catégorie d’ingénieur accessible via **Extension ingénieur : plan**
![43.jpg](/naheulbeuk/43.jpg =500x)
* La liste des matériaux et outils des ingénieurs accessible via **Extension ingénieur : matériaux**
![44.jpg](/naheulbeuk/44.jpg =500x)
 
**Remarque :** 
* Les plans listent parfois des matériaux qui ne sont pas dans le document précédent, soit parce qu’ils existaient déjà ailleurs, soit parce qu’ils sont spéciaux (c’est comme ça dans le pdf).
J'ai quand même créé ces matériaux, et ils peuvent être trouvés en parcourant les compendiums ou en utilisant l'outil de recherche dont on reparlera dans le chapitre sur les macros.
* En ouvrant un plan, on peut voir et drag and drop l'objet qu'il permet de fabriquer.
![45.jpg](/naheulbeuk/45.jpg =500x)

* Lorsqu'on drag and drop un plan sur un personnage, il se retrouve dans la catégorie **Ingrédients**.
On retrouve un **d6** à côté du nom, permettant de faire un jet de confection en tant que spécialiste ou non spécialiste
![50.jpg](/naheulbeuk/50.jpg =500x)
Le jet calcule l'ingéniosité et prend en compte la difficulté de l'objet.
![46.jpg](/naheulbeuk/46.jpg =500x)

## Les objets {#titre5}
* [Les trucs](#titre51)
* [Les armures et autres objets portés](#titre52)
* [Les armes et autres objets en mains](#titre53)
* [Les sacs](#titre54)
* [Les états](#titre58)
* [Les sorts](#titre59)
* [Les compétences](#titre60)
* [Les coups spéciaux](#titre56)
* [Les métiers](#titre61)
* [Les origines](#titre57)
* [Les APE](#titre62)
* [Les gemmes](#titre64)
* [Les recettes](#titre61)
* [Les conteneurs](#titre641)
* [Les pièges](#titre65)
* [Les attaques (pour PNJ)](#titre55)
* [Les régions (pour PNJ)](#titre66)
* [Les traits (pour PNJ)](#titre67)
<br/>
### Les trucs {#titre51}

**Nom dans Foundry** : truc

**Type d'objets** : tous les objets qui ne rentrent pas dans les autres catégories d'objets.
 * Le matériel
 * Les livres
 * Les ingrédients
 * Les potions
 * Les pièces
 * La nourriture
 * Les stages
 * Les services
 * ...
 
**L'entête contient :**

![51.jpg](/naheulbeuk/51.jpg =500x)
* Le nom de l'objet
* L'image de l'objet
* La quantité 
* Le poids
* Le prix
* La catégorie (pour savoir où il sera rangé dans le sac)

**L'onglet Description contient :**

![52.jpg](/naheulbeuk/52.jpg =500x)
* La description de l'objet

**L'onglet Détail contient :**

![53.jpg](/naheulbeuk/53.jpg =500x)
* La formule de jet de dés
Un objet avec une formule aura un **d20** et un **d6** lorsqu'il sera drag and drop dans l'inventaire
*Par exemple si je veux faire une torche qui brûle 1d20 min, j'écris ici d20*
![54.jpg](/naheulbeuk/54.jpg =500x)
En cliquant sur le **+** à droite de **Jet de dés**, on fait apparaitre  une interface pour faire des lancers de dés plus complexes. Dans ce cas là l'objet aura uniquement un **d6** lorsqu'il sera drag and drop dans l'inventaire.
![55.jpg](/naheulbeuk/55.jpg =500x)
* Les bonus / malus pour le personnage sur lequel on activera l'objet. 
Un objet avec des bonus / malus sera activable dans l'inventaire (case blanche) mais restera dans le sac.
![54.jpg](/naheulbeuk/54.jpg =500x)
  * *La part du bonus/malus de Charisme ignoré pour la magie psychique, permet de donner un bonus de Charisme sans qu'il ne modifie la valeur de magie psychique. Dans l'exemple, le bonus de 8 en charisme est entièrement ignoré.*
  * *La part du bonus/malus de Pr ignoré pour le malus d'encombrement, permet de définir combien de Pr sur le bonus total n'entre pas en compte dans le calcul des malus pour le déplacement et l'esquive. Je peux par exemple avoir une armure donnant 10 Pr, mais dont 4 seulement rentre en compte dans le calcul du malus de déplacement. J'indique donc 6 dans ce champ.*
* Un champ **Autre** pour noter les remarques éventuelles liées à l'activation. 
Un objet avec des bonus / malus sera activable dans l'inventaire (case blanche) mais restera dans le sac.

**Remarques :**
* Si l'objet contient des bonus / malus il est donc activable. Une fois activé, il apparait lorsqu'on clique sur l'oeil à droite de **Caractéristiques** dans l'onglet **Caractéristiques** d'un personnage.
![56.jpg](/naheulbeuk/56.jpg =500x)
* En cliquant sur l'oeil en haut à droite d'un objet, on masque toutes les informations importantes et on peut définir un nouveau nom et une nouvelle description. Cette fonctionnalité existe pour permettre de ne pas révéler le détail d'un objet looté et ne fonctionne que si c'est le MJ qui clique sur l'oeil.
![57.jpg](/naheulbeuk/57.jpg =500x)
* Si jamais l'objet demande une épreuve et pas un jet de dés, il faut écrire **épreuve:difficulté** dans le champ **Jet de dés**
*Par exemple si un antidote fonctionne sur un test de force réussi, on peu écrire **épreuve:@fo***
<br/>
### Les armures et autres objets portés {#titre52}

**Nom dans Foundry** : armure

**Type d'objets** : tous les objets portés comme les armures, les bijoux ou les vêtements...

Le fonctionnement est quasiment identique aux objets de type **truc**.
Voici les différences :
* L'entête permet de définir si l'objet est enchanté ou une relique
* L'entête permet de définir l'emplacement de l'objet
* L'entête permet de définir les origines compatibles
![58.jpg](/naheulbeuk/58.jpg =500x)
* La catégorie d'inventaire est **Protections et objets portés** mais peut quand même être modifiée en double cliquant sur **Détails**
![60.jpg](/naheulbeuk/60.jpg =500x)
* Il n'y a pas de jets complexes possibles (**Jet de dés +**)
* L'objet a une valeur de rupture
![59.jpg](/naheulbeuk/59.jpg =500x)
* Ces objets seront activables dans l'inventaire (case blanche) mais seront déplacés dans la catégorie **Armures et Protections - objets portés**.
<br/>
### Les armes et autres objets en mains {#titre53}

**Nom dans Foundry** : arme

**Type d'objets** : tous les objets pris en mains comme les armes, les boucliers, les munitions, les instruments...

Le fonctionnement est quasiment identique aux objets de type **truc**.
Voici les différences :
* L'entête permet de définir si l'objet est enchanté ou une relique
* L'entête permet de définir le type de l'objet
* L'entête permet de définir les origines compatibles
![61.jpg](/naheulbeuk/61.jpg =500x)
* La catégorie d'inventaire est **Armes et objets en mains** mais peut quand même être modifiée en double cliquant sur **Détails**
![64.jpg](/naheulbeuk/64.jpg =500x)
* L'objet n'a pas de jet de dés, mais un jet de **Dégâts**
* L'objet a une valeur de rupture
![62.jpg](/naheulbeuk/62.jpg =500x)
* Pour une arme de jet (hache, lance...) équipée, il y a deux lignes de jet de dés. La première conscerne l'attaque au contact, la deuxième l'attaque à distance.
![63.jpg](/naheulbeuk/63.jpg =500x)
* Les armes à poudre sont toujours un peu complexe (dégâts en fonction de la distance, chance de fonctionnement...). Elles ont donc automatiquement des jet de dés complexes.
![66.jpg](/naheulbeuk/66.jpg =700x)
* Ces objets seront activables dans l'inventaire (case blanche) mais seront déplacés dans la catégorie **Armement et Baston - objets en mains**.
  * Une seule arme à distance peut être équipée et sans arme de contact équipée
  * Deux armes de contact maximum peuvent être équipées
  * Un seul bouclier peut être équipé
  * Une seule arme de contact peut être équipée avec un bouclier
  * Plusieurs munitions peuvent être équipées
  * Les armes de jet (hache, lance) compte comme des armes de contact dans les règles précédentes.
<br/>
### Les sacs {#titre54}

**Nom dans Foundry** : sac

**Type d'objets** : les sacs à dos et les bourses

Le fonctionnement est quasiment identique aux objets de type **truc**.
Voici les différences :
* L'entête permet de définir le type de sac : **Sac à dos** ou **Bourse**
![65.jpg](/naheulbeuk/65.jpg =500x)
* Il n'y a pas de jets complexes possibles (**Jet de dés +**)
* L'objet une fois équipé est déplacé dans la catégorie **En dehors du sac**
* On ne peut pas les changer de catégorie d'inventaire
<br/>

### Les états {#titre58}

**Nom dans Foundry** : etat

**Type d'objets** : tous les éléments qui altèrent un personnage sans être un véritable objet. 
Par exemple les bonus/malus des sorts, les maladies, les blessures, les folies, les mutations, les empoisonnements...

Ces objets fonctionnent comme ceux de type **truc** mise à part qu'ils n'ont pas de prix ou de poids et qu'ils sont activables depuis l'onglet **Caractéristiques** du personnage.

![67.jpg](/naheulbeuk/67.jpg =700x)
<br/>
### Les sorts {#titre59}

**Nom dans Foundry** : sort

**Type d'objets** : tous les sorts des mages, prêtres ou paladins

Un sort a dans l'entête le nom et l'icône, mais également un type correspondant à l'école de magie associée (magie du jeu, prêtre de Niourgl...)

![68.jpg](/naheulbeuk/68.jpg =500x)

Cet objet a également une description qu'il est très important de consulter quand l'objet a été drag and drop, car elle contient toutes les informations utiles. 
Le résumé qui sera affiché après le drag and drop ne comprend que quelques informations importantes.

![69.jpg](/naheulbeuk/69.jpg =500x)

![71.jpg](/naheulbeuk/71.jpg =500x)

Les éléments affichés après le drag and drop sont présentés dans l'onglet **Détails**. 

![70.jpg](/naheulbeuk/70.jpg =500x)

Ici tout les éléments seront uniquement documentaires (non automatisés, par exemple l'énergie astrale ne baissera pas) sauf :
* Le niveau pour le classement du sort
* L'épreuve, pour afficher les dés correspondants. 
Elle peut être de type jet de dés complexe en cliquant sur le **+**. Si c'est le cas, il n'y aura qu'un unique **d6** après le drag and drop sur la feuille de personnage.
* Les dégâts, pour afficher les dés correspondants.
Si les dégâts prennent en compte le bonus d'intelligence > 12, il faut rajouter **+[@bonusint](#titre87)**.

**Remarque :** si un sort applique un bonus / malus au personnage (un état) alors il sera présent dans la description et pourra être drag and drop sur le personnage.
<br/>
### Les compétences {#titre60}

**Nom dans Foundry** : competence

**Type d'objets** : les compétences héritées d'une origine ou d'un métier, les compétences choisies à partir d'une origine ou d'un métier, les compétences gagnées, et les compétences de base.

En entête, on retrouve le type de compétence. Si rien n'est sélectionné, c'est une compétence héritée. Sinon on peut indiquer d'où vient la compétence.
Pour une **compétence gagnée**, on doit préciser comment on a gagné la compétence.
Pour l'explication sur les **compétences de base**, je vous invite à aller voir l'onglet **Compétences** du chapitre **Création d'un PJ**.

![72.jpg](/naheulbeuk/72.jpg =500x)

Ensuite on retrouve un onglet description

![73.jpg](/naheulbeuk/73.jpg =500x)

Enfin il y a un onglet **Détails** pour le cas où la compétence demanderait un jet de dés spécifique (**jet de dés** et **[difficulté](#titre87)**)

![74.jpg](/naheulbeuk/74.jpg =500x)
<br/>
### Les coups spéciaux {#titre56}

**Nom dans Foundry** : coup

**Type d'objets** : les coups de Bourre-Pif, les coups spéciaux génériques, de ménestrel, des ninjas, des soldats...

**Les coups de type Bourre-Pif :**

![81.jpg](/naheulbeuk/81.jpg =500x)

![75.jpg](/naheulbeuk/75.jpg =500x)

* **Epreuve d'attaque :** le jet de dés à faire pour réussir le coup
* **Dégâts en PI :** les dégâts infligés si le coup touche
* **Effet spécial :** une description pour le cas où le coup aurait des conséquences spécifiques
* **Epreuve effet spécial** : un jet de dés supplémentaire pour l'effet spécial, si nécessaire.

Comme pour le reste, la description des jets de dés utilise la convention de raccourcis du système : [@att](#titre87), [@bonusfo](#titre87)...

**Les autres coups spéciaux :**

![82.jpg](/naheulbeuk/82.jpg =500x)

![83.jpg](/naheulbeuk/83.jpg =500x)

* Les champs **Épreuve** et **Dégâts** s'ils sont complétés, permettre de faire apparaître le **d20** et le **d6** une fois le coup drag and drop sur la feuille de personnage.
* Pour ces deux champs, s'ils commencent par * alors le contenu ne sera pas interprété comme une formule, mais comme une description.
* Il n'y a pas d'autres d'automatisations (pas de gain d'xp automatique en cas de coup réussi par exemple)
<br/>
### Les métiers {#titre61}

**Nom dans Foundry** : metier

**Type d'objets** : les métiers des personnages, les différents type de soldats, les différentes spécialisations d'ingénieurs

Un fiche métier contient une case pour indiquer s'il est lié à la magie, et une description dans laquelle on va le décrire, et drag and drop les compétences ou les objets nécessaires.

![84.jpg](/naheulbeuk/84.jpg =500x)
<br/>
### Les origines {#titre57}

**Nom dans Foundry** : origine

**Type d'objets** : les origines des personnages

Un fiche origine contient uniquement une description dans laquelle on va la décrire, et drag and drop les compétences ou les objets nécessaires.

![85.jpg](/naheulbeuk/85.jpg =500x)
<br/>
### Les APE {#titre62}

**Nom dans Foundry** : ape

**Type d'objets** : 
<br/>
### Les gemmes {#titre64}

**Nom dans Foundry** : gemme
**Type d'objets** : 
<br/>
### Les recettes {#titre61}

**Nom dans Foundry** : recette
**Type d'objets** : 
<br/>
### Les conteneurs {#titre641}

**Nom dans Foundry** : conteneur
**Type d'objets** : 
<br/>
### Les pièges {#titre65}

**Nom dans Foundry** : piege
**Type d'objets** : 
<br/>
### Les attaques (pour PNJ){#titre55}

**Nom dans Foundry** : attaque
**Type d'objets** : 
<br/>
### Les régions (pour PNJ){#titre66}

**Nom dans Foundry** : region
**Type d'objets** : 
<br/>
### Les traits (pour PNJ){#titre67}

**Nom dans Foundry** : trait
**Type d'objets** : 
<br/>
## Les tableaux {#titre7}

## Les macros {#titre8}
* [Chercher un objet et générer un magasin](#titre81)
* [Combat rapide](#titre82)
* [Lancer Custom](#titre83)
* [Rechercher un compendium](#titre84)
* [Rencontres : liste et "générateur"](#titre85)
* [Tirer un élément aléatoire d'un compendium](#titre86)
* [Raccourcis du système](#titre87)
* [Avancé](#titre88)

### Chercher un objet et générer un magasin {#titre81}

### Combat rapide {#titre82}

### Lancer Custom {#titre83}

### Rechercher un compendium {#titre84}

### Rencontres : liste et "générateur" {#titre85}

### Tirer un élément aléatoire d'un compendium {#titre86}

### Raccourcis du système {#titre87}
Dans le système, que ce soit pour les objets, les feuilles de personnage ou même directement dans le code, il existe une convention de nommage pour récupérer des attributs spécifiques.
Cette convention est la suivante :
* **@cou** : courage total du personnage
* **@int** : intelligence totale du personnage
* **@cha** : charisme total du personnage
* **@ad** : adresse totale du personnage
* **@fo** : force totale du personnage
* **@att** : attaque du personnage 
Attention, pour avoir l'attaque totale qu'on fera avec une arme équipée, il faut rajouter à @att le bonus d'attaque de cette arme
* **@att-distance** : valeur d'attaque à distance, avec la prise en compte de **Tirer correctement**
Attention, pour avoir l'attaque totale qu'on fera avec une arme équipée, il faut rajouter à @att-distance le bonus d'attaque à distance de cette arme
* **@att-arme-poudre** : valeur d'attaque d'une arme à poudre avec le bonus spécifique de **Tirer correctement**
Attention, pour avoir l'attaque totale qu'on fera avec une arme équipée, il faut rajouter à @att-arme-poudre le bonus d'attaque à distance de cette arme
* **@degat-contact** : bonus de dégât au corps à corps.
Attention, pour avoir les dégâts totaux qu'on fera avec une arme équipée, il faut rajouter à @degat-contact le bonus de dégât de cette arme
* **@degat-distance** : bonus de dégâts à distance.
Attention, pour avoir les dégâts totaux qu'on fera avec une arme équipée, il faut rajouter à @degat-distance le bonus de dégât à distance de cette arme
* **@prd** : parade du personnage
Attention, pour avoir la parade totale qu'on fera avec une arme ou un bouclier équipé, il faut rajouter à @prd le bonus de parade de cette arme ou de ce bouclier
* **@pr** : protection totale du personnage
* **@prm** : protection magique totale du personnage
* **@esq** : esquive totale du personnage
* **@rm** : résistance magique totale du personnage
* **@mphy** : magie physique du personnage
* **@mpsy** : magie psychique du personnage
* **@lvl** : niveau du personnage
* **@bonusfo** : bonus / malus de force > 12 ou < 9
* **@bonusint** : bonus d'intelligence > 12
* **@malus-mvt-pr** : valeur du malus de déplacement en fonction de la protection (basée sur le tableau des esquives dans les pdf)

### Avancé {#titre88}