---
title: Naheulbeuk
description: 
published: true
date: 2023-06-06T11:40:12.302Z
tags: naheulbeuk
editor: markdown
dateCreated: 2022-11-15T16:04:44.061Z
---

# Naheulbeuk

Le système pour le JDR Naheulbeuk est maintenant officiel ! Il est donc directement trouvable dans le store de Foundry :)

Le système est fait pour la version 10 de Foundry.

En cas de problème ou de question, vous pouvez demander de l'aide sur le Discord **La Fonderie** canal naheulbeuk.

Avant de rentrer dans le détail, voici ce qui n'est pas dans le système actuellement :
 * Les sorts niveau 5 et plus
 * Les objets des extensions Fernol, Confins, Jungle
 * Les frappes localisées
 * Surement quelques autres petits trucs que j'ai zappé :D
 
Dernière précision, on va parler régulièrement d'objets et de compendiums, ils faut aller les chercher dans la liste des compendiums du système.

![49.jpg](/naheulbeuk/49.jpg =300x)

## Sommaire
* [Vidéo d'introduction MJ](#titre01)
* [Vidéo d'introduction PJ](#titre02)
* [Création d'un PJ](#titre1)
* [Les PNJ](#titre2)
* [L'extension soldat](#titre3)
* [L'extension ingénieur](#titre4)
* [Les objets](#titre5)
* [Les tableaux](#titre7)
* [Le drag and drop depuis un personnage vers la barre des macros](#titre9)
* [Les macros](#titre8)


## Vidéo d'introduction MJ {#titre01}
Petite vidéo pour faire une introduction à l'utilisation du système en tant que Maitre du Jeu.
https://www.youtube.com/watch?v=Lc8tfl2d8CY

## Vidéo d'introduction PJ {#titre02}
Petite vidéo pour faire une introduction à l'utilisation du système en tant que Personnage Joueur.
https://www.youtube.com/watch?v=PqpFlrQXbto

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
*On est sur du fonctionnement Foundry, mais je préfère le préciser car c'est indispensable pour que la modification d'un token/fiche d'une scène soit prise en compte sur les autres scènes, et c'est fréquemment oublié*

Puis commencez par cliquer sur **Caractéristiques** pour faire un lancer de **1d6+7** et rentrez cette valeur dans la colonne **Base** du courage.
Répétez cette opération pour l'intelligence, le charisme, l'adresse et la force.
Vous pouvez bien sûr modifier ces valeurs suivant votre façon de créer un personnage ;)
Vous pouvez déjà remarquer que la résistance magique et l'esquive ont été mis à jour à partir de ces valeurs.

***Remarque** : un champ **Initiative** a été rajouté sous la Parade. De base il vaut la valeur de courage, mais il peut être modifié pour permettre d'adapter le RP.*

En fonction de ces résultats, vous pouvez choisir une origine et un métier.

Cliquez sur **Origine**, puis faites un drag and drop de l'origine souhaitée sur la feuille de personnage.

Cliquez sur **Métier**, puis faites un drag and drop du métier souhaité sur la feuille de personnage.

Si vous choisissez un mage, un prêtre ou un paladin, vous voyez apparaitre de nouvelles statistiques : l'énergie astrale, la magie physique et la magie psychique.
Un onglet **Magie** apparait également, mais on en reparlera plus tard !

<u>Les origines et les métiers ne mettent pas à jour automatiquement la fiche du personnage.</u> Il faut donc cliquer sur le métier et l'origine choisie, puis lire la description et mettre à jour les stats (énergie vitale, énergie astrale, charge maximum, attaque, parade...).
*Il faut également compléter la <u>PR max</u>, qui a été rajoutée depuis que cette capture a été faite.*

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
* La valeur de **PR max** permettra de bloquer l'équipement d'une armure qui n'est pas adaptée à un héros. A noter que la PR qui n'impacte pas les déplacements (Truc de mauviette, armure de glace...) n'intervient pas dans le calcul de la PR max, c'est également le cas pour les boucliers.
*Exemple : J'ai PR max 4 et j'ai truc de mauviette. Ma PR est donc à 1. Je peux équiper une armure qui donne PR + 4, et également un bouclier qui donne PR +2. Je serai donc à 7 PR tout en respectant ma valeur de PR max à 4.*
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

Le tirage aléatoire d'une APE se fait via les compendiums **Tableaux : APE**
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
<br/>
### Onglet Inventaire {#titre17}

L'inventaire contient 3 catégories.
* **Armement et baston - objets en mains** : cette catégorie contient tous les objets tenus en main et qui sont équipés. Ce sont principalement les armes, les boucliers et les munitions, mais on peut y retrouver d'autres objets (des instruments, des bannières...).
C'est par ici qu'on fait les lancer de dés pour l'attaque, les dégâts, la parade, et la rupture.
![20.jpg](/naheulbeuk/20.jpg =500x)
Comme pour les sorts, un **d6** tout seul signifie un jet un peu particulier, c'est le cas notamment des armes à poudre.
*Le **bonus de force** > 12 ou < 9, la compétence **Tirer correctement**  et le bonus de **dégâts à 2 mains des nains**, sont déjà pris en compte dans le système.*
Enfin il y a des limitations sur les objets équipés dans cette catégorie (2 armes, 1 arme à distance, 1 bouclier...), elles peuvent être ignorées avec un **shift+clic**
* **Armures et Protection - objets portés** : cette catégorie contient tous les objets portés par le personnage et qui sont équipés. Ce sont principalement les armures, les bijoux et les vêtements.
On peut faire ici les tests de rupture.
![21.jpg](/naheulbeuk/21.jpg =500x)
* **Équipement et Trucs** : cette catégorie contient l'ensemble des objets dont ceux des catégories précédentes avant qu'ils soient équipés.
Certains objets peuvent être équipés, d'autres utilisés, d'autres encore avoir des jets de dés.
Ils sont divisés en 3 sous catégories :
  * **Les objets dans le sac :** c'est la que seront la grosse majorité des objets du personnage. Pour mieux les retrouver, ils sont placés dans un troisième niveau de catégories qui s'affichent uniquement si elles contiennent des objets. Ce sont les objets divers, les livres, les ingrédients, les potions, les armes, les armures, la nourriture, les richesses, les objets personnels...
  ![22.jpg](/naheulbeuk/22.jpg =500x)
  * **Les objets dans la bourse** : on retrouve ici les objets qui sont dans la bourse, donc cette partie est utilisée principalement pour les pièces.
  * **Les objets en dehors du sac** : tout ce qui n'est pas dans le sac ou dans la bourse devrait être rangé ici : une guitare en bandoulière, une épée à la ceinture, un sac sur le dos...
  
**Remarques :**
* Les charges max du sac et de la bourse se mettent à jour lorsqu'un ou plusieurs sacs/bourses sont équipés.
![23.jpg](/naheulbeuk/23.jpg =500x)
* Les boutons à droite des objets permettent de changer la catégorie d'inventaire d'un objet. Depuis les captures précédentes, ils ont changé car Foundry a rajouté de nouvelles icônes. 
Le sac simple envoie les objets dans le sac, le sac avec le dollar envoie dans la bourse et le sac avec une croix envoie en dehors du sac.
![26.jpg](/naheulbeuk/26.jpg =700x)
* Un objet équipé peut être consulté, mais pas modifié.


## Les PNJ {#titre2}

* [Entête](#titre2.1)
* [Onglet Caractéristiques](#titre2.2)
* [Onglet Combat](#titre2.3)
* [Onglet Inventaire](#titre2.4)

Dans ce chapitre, nous n'allons pas voir comment faire un nouveau PNJ, mais plutôt regarder le fonctionnement de ceux déjà créés.
De manière générale lorsque vous souhaitez créer un nouvel objet ou PNJ il faut toujours partir de l'existant !
*Mais si vous souhaitez quand même partir de 0, il faut aller dans Actor puis Create Actor et sélectionner le type npc.*

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

Pour tous les objets qui seront traités ici, je vous conseille dans le cas où vous souhaiteriez faire vos propres objets de partir d'une copie d'un objet similaire existant.
Mais bien sûr, rien ne vous empêche de partir de 0 (Items puis Create new item).
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
  * *La part du bonus/malus de Pr ignoré pour le malus d'encombrement, permet de définir combien de Pr sur le bonus total n'entrent pas en compte dans le calcul des malus pour le déplacement et l'esquive. 
Par exemple une armure de glace niveau 1 donne 3 Pr qui n'entrent pas en compte pour le malus d'encombrement. J'ai donc 3 en bonus de Protection et 3 dans ce champ.*
* Un champ **Autre** pour noter les remarques éventuelles liées à l'activation. 
Un objet avec des bonus / malus sera activable dans l'inventaire (case blanche) mais restera dans le sac.

**Remarques :**
* Si l'objet contient des bonus / malus il est donc activable. Une fois activé, il apparait lorsqu'on clique sur l'oeil à droite de **Caractéristiques** dans l'onglet **Caractéristiques** d'un personnage.
![56.jpg](/naheulbeuk/56.jpg =500x)
* En cliquant sur l'oeil en haut à droite d'un objet, on masque toutes les informations importantes et on peut définir un nouveau nom et une nouvelle description. Cette fonctionnalité existe pour permettre de ne pas révéler le détail d'un objet looté et ne fonctionne que si c'est le MJ qui clique sur l'oeil.
![57.jpg](/naheulbeuk/57.jpg =500x)
* Si jamais l'objet demande une épreuve et pas un jet de dés, il faut écrire **épreuve:difficulté** dans le champ **Jet de dés**
*Par exemple si un antidote fonctionne sur un test de force réussi, on peu écrire **épreuve:@fo***
* Si vous créez un nouvel objet, il existe de très nombreuses icônes rangées dans le dossier : **systems/naheulbeuk/assets/from-rexard-icons/**
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
* Ces objets seront activables (équipables) dans l'inventaire (case blanche) mais seront déplacés dans la catégorie **Armures et Protections - objets portés** une fois équipés.
* Si équiper un de ces objets fait dépasser la PR max, alors l'objet ne sera pas équipé et il y aura un message d'erreur. Pour ignorer la restriction liée à la PR max, il faut appuyer sur shift lors de l'équipement.
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
* Ces objets seront activables (équipables) dans l'inventaire (case blanche) mais seront déplacés dans la catégorie **Armement et Baston - objets en mains** une fois équipés.
  * Une seule arme à distance peut être équipée et ce, sans arme de contact équipée
  * Deux armes de contact maximum peuvent être équipées
  * Une seule arme deux mains peut être équipée
  * Un seul bouclier peut être équipé
  * Une seule arme de contact peut être équipée avec un bouclier
  * Plusieurs munitions peuvent être équipées
  * Les armes de jet (hache, lance) compte comme des armes de contact dans les règles précédentes.
  * En appuyant sur shift lors du clic, on peut ignoré les règles précédentes.
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

**Remarque** : depuis la version 10.1.0, les états ont une option "Affichage de l'état sur le token" qui si elle est activée rend visible l'état sur le token.

![110.png](/naheulbeuk/110.png)
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
Pour l'explication sur les **compétences de base**, je vous invite à aller voir l'onglet **Compétences** du chapitre [Création d'un PJ](#titre13).

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

**Type d'objets** : les aptitudes parfois étranges

Une APE contient une description de l'aptitude, l'effet correspondant et le niveau.
Comme indiqué dans la [création de personnage](#titre15), le traitement de ces APE n'est pas automatisé.

![86.jpg](/naheulbeuk/86.jpg =500x)
<br/>
### Les gemmes {#titre64}

**Nom dans Foundry** : gemme

**Type d'objets** : les pierres précieuses

La fiche d'une pierre précieuse contient :
* La quantité
* Le poids (calculé automatiquement à partir des UG)
* Le prix
* La catégorie d'inventaire
* Le type de taille
* Le nombre d'UG
* La description de la pierre

![87.jpg](/naheulbeuk/87.jpg =500x)

L'onglet description donne tout le détail de l'évaluation du type et du prix d'une gemme.

![88.jpg](/naheulbeuk/88.jpg =500x)
<br/>
### Les recettes {#titre61}

**Nom dans Foundry** : recette

**Type d'objets** : plans et recettes pour les ingénieurs

La fiche d'une recette contient toutes les informations sur qui peut la réaliser, à partir de quand, et comment.
Si la recette permet de fabriquer un objet, il sera dans la partie **Confection** et pourra être drag and drop.
Les matériaux et outils nécessaires existent tous dans le système, mais ils sont rangés dans différents compendiums (suivant les pdf de Naheulbeuk). Je vous encourage donc à utiliser [l'outil de recherche](#titre81) pour les retrouver.

![45.jpg](/naheulbeuk/45.jpg =500x)

Une fois drag and drop, le personnage peut réaliser un **jet de confection** (**d6** à droite du plan) qui prendra en compte l'ingéniosité du personnage et la difficulté de fabrication de l'objet. 
La spécialisation nécessaire est bien évidemment indiquée dans la fiche de l'objet.

![50.jpg](/naheulbeuk/50.jpg =500x)

![46.jpg](/naheulbeuk/46.jpg =500x)
<br/>
### Les conteneurs {#titre641}

**Nom dans Foundry** : conteneur

**Type d'objets** : les coffres, et autres objets contenant d'autres objets.

*Avant de décrire le fonctionnement, une petite remarque. De base Foundry ne permet pas de mettre un objet dans un objet. Le système a donc exploité des fonctionnalités complexes pour le permettre et nous ne sommes pas à l'abri de problèmes dans des scénarios qui n'ont pas été anticipés. N'hésitez donc pas à remonter tout comportement anormal sur le discord !*

![89.jpg](/naheulbeuk/89.jpg =500x)

**L'entête contient :**
* La quantité (qui logiquement devrait rester à 1 pour un conteneur)
* La place qu'il y a dedans
* Le poids du conteneur
* Le poids total avec les objets contenus (calculé automatiquement)
* Le prix du conteneur
* La catégorie d'inventaire dans lequel il sera rangé

**L'onglet Détails contient :**
* Les objets qu'on drag and drop dedans 
*L'objet drag and drop disparaitra de l'emplacement d'origine*
*On ne peut pas drag and drop un objet équipé*
* Un bouton pour sortir l'objet du conteneur
*L'objet sera enlevé du conteneur et déplacé à l'endroit où le conteneur se trouve : les objets Foundry ou la fiche d'un personnage.*
* Un bouton pour éditer un objet
*Logiquement un objet placé dans un conteneur ne devrait pas être édité, mais c'est quand même possible*
*Un conteneur peut contenir un autre conteneur, mais sa fiche est en lecture seul pour éviter les bugs*
* Un bouton pour supprimer un objet

**L'onglet Description contient la description du conteneur**
<br/>
### Les pièges {#titre65}

**Nom dans Foundry** : piege

**Type d'objets** : tous les pièges de Naheulbeuk

**Attention :** les pièges sont des objets Foundry, mais ils sont uniquement là pour aider le MJ à se souvenir des caractéristiques, ainsi que faire les jets de dés.
Ils ne peuvent donc pas être drag and drop sur un personnage où sur la scène.

Ils contiennent donc toutes les caractéristiques du pièges, une formule éventuelle pour les dégâts (avec le **d20** pour faire le lancer) et une description dans laquelle peut se trouver un [état](#titre58) (malus pour les personnages touchés).

![90.jpg](/naheulbeuk/90.jpg =500x)
<br/>
### Les attaques (pour PNJ){#titre55}

**Nom dans Foundry** : attaque

**Type d'objets** : les attaques des PNJ

Comme expliqué dans le [chapitre sur les PNJ](#titre2.3), les PNJ n'utilisent pas les armes comme les PJ et les statistiques d'attaque.
Ils ont directement des attaques qui intègrent l'épreuve et les dégâts.
C'est ce qu'on retrouve dans un objet **attaque**.

![91.jpg](/naheulbeuk/91.jpg =500x)

Si l'attaque du PNJ a plus d'information que l'épreuve et les dégâts, on peut cliquer sur le **+** à droite d'**Attaque** pour faire un jet de dés plus complexe.

![92.jpg](/naheulbeuk/92.jpg =500x)
<br/>
### Les régions (pour PNJ){#titre66}

**Nom dans Foundry** : region

**Type d'objets** : les régions dans lesquelles on peut trouver les PNJ

Le bestiaire papier de Naheulbeuk contient les régions dans lesquelles se trouvent les PNJ. Logiquement ils sont tous dans le système, vous ne devriez donc pas avoir besoin d'en rajouter.
On les trouve dans le compendium : **Système : bestiaire données**

![93.jpg](/naheulbeuk/93.jpg =500x)
<br/>
### Les traits (pour PNJ){#titre67}

**Nom dans Foundry** : trait

**Type d'objets** : les traits des PNJ

Le bestiaire papier de Naheulbeuk contient des traits correspondant à des caractéristiques spéciales des PNJ. Logiquement ils sont tous dans le système, vous ne devriez donc pas avoir besoin d'en rajouter.
On les trouve dans le compendium : **Système : bestiaire données**

![94.jpg](/naheulbeuk/94.jpg =500x)
<br/>
## Les tableaux {#titre7}

Foundry permet de faire des tableaux pour en tirer un élément aléatoire.
Je ne vais pas présenter ici le fonctionnement des tableaux, uniquement vous donner la liste de ceux qui sont dans le système :
* **Tableaux : APE**
Un tableau existe pour chaque race de Naheulbeuk 
* **Tableaux : critiques et maladresses**
Des tableaux pour les critiques des attaques et parades
Des tableaux pour les maladresse des attaques, parades et sorts
* **Tableaux : divers**
Des tableaux pour les débilibeuk et mankdebol
Des tableaux pour les folies, et mutations
Un tableau pour les sorts et prodiges entropiques
Un tableau pour le toucher de Niourgl
Des tableaux pour les gemmes et les objets exclusifs
* **Tableaux : extension soldat**
Des tableaux pour les punitions et les récompenses

## Le drag and drop depuis un personnage vers la barre des macros {#titre9}

Il y a pas mal de choses à dire pour ce chapitre. Il est en effet un petit peu complexe mais c'est pour la bonne cause, puisqu'il a pour vocation de simplifier l'utilisation :D

Pour commencer, que peut-on drag and drop dans la barre de macros ?
A ma connaissance, à peu prêt tout : une macro (oui oui), une note, un objet, un tableau...
Mais la partie qui m'intéresse dans ce chapitre, c'est le **drag and drop d'un objet depuis le personnage vers la barre de macros**.
En effet dans ce cas là, le système va générer du code pour pouvoir mettre en place des comportements un peu sympathiques :)

Plus spécifiquement, nous allons parler des **armes, des sorts, des coups spéciaux et des attaques de PNJ**. Pour le reste, la macro de drag and drop ne fera qu'afficher l'objet.

Lors du drag and drop, le code généré ressemble à ça :
<pre>
let mode = 1;
game.naheulbeuk.rollItemMacro(`Hache correcte 1m`,mode);
</pre>

Le mode correspond à l'effet qu'aura la macro :
* 1 --> ouvre une interface permettant de choisir si on veut voir l'objet, ou si possible l'utiliser
![111.png](/naheulbeuk/111.png =500x)
* 2 --> si possible, on lance directement l'option "utiliser l'objet".
C'est par exemple utile pour lancer une attaque normale avec une arme, ou un sort. S'il y a une épreuve custom, on retombe sur le mode 1 pour choisir entre les différentes possibilités.
![112.png](/naheulbeuk/112.png =500x)
* 3 --> si possible, on lance directement une attaque rapide
C'est par exemple utile pour une arme de contact, ou une attaque de PNJ. Si c'est impossible de faire une attaque rapide, ou qu'il y a plusieurs choix (épreuve custom, attaque à distance en plus de corps à corps...) on retombe sur le mode 2 ou 1.
![113.png](/naheulbeuk/113.png =500x)
Pour lancer l'attaque rapide, il faut commencer par choisir une cible, puis définir son placement, les bonus/malus éventuelle et enfin comment la cible va se défendre.
* 4 --> même interface que le mode 1, mais avec en plus un bouton attaque rapide
![114.png](/naheulbeuk/114.png =500x)

Le joueur peut donc modifier la valeur du mode pour avoir le comportement qu'il souhaite.

La valeur par défaut est fixée par le MJ. Pour celà, il doit lancer la macro **Sélection mode drag and drop** présente dans le compendium des macros.
![115.png](/naheulbeuk/115.png =500x)
**Remarque :**
Drag and drop chaque attaque de chaque PNJ peut être un peu lourd.
Pour permettre malgré tout d'utiliser les options du drag and drop et notamment le combat rapide, il existe 2 options utiles :
* Si le mode vaut 3 ou 4 (on utilise le combat rapide) un d20 supplémentaire apparait sur la ligne de l'attaque du PNJ pour permettre de faire l'équivalent d'un clique sur l'attaque drag and drop (donc lancer le combat rapide, mode 3, ou lancer l'interface avec le combat rapide, mode 4).
![116.png](/naheulbeuk/116.png =500x)
* Dans le compendium des macros, récupérer la macro **Actions d'attaque** permet en la lançant, de proposer l'action équivalente au drag and drop pour chaque attaque du PNJ. Cet outil permet donc de gérer toutes les attaques de tous les PNJ, il suffit de sélectionner au préalable le bon token.
![117.png](/naheulbeuk/117.png =500x)

## Les macros {#titre8}
Les 6 premiers chapitres concernent des macros qui existent dans le compendium des macros et qui peuvent être drag and drop dans la barre dédiée.
* [Chercher un objet et générer un magasin](#titre81)
* [Chercher ou générer une rencontre](#titre85)
* [Lancer Custom](#titre83)
* [Compétences](#titre89)
* [Sélection mode drag and drop ET Actions d'attaque](#titre82)
* [Raccourcis du système](#titre87)
* [Avancé](#titre88)
<br/>
### Chercher un objet et générer un magasin {#titre81}
C'est outil est extrêmement puissant puisqu'il va vous permettre de retrouver des objets parmi la multitude existante dans Naheulbeuk.
Je m'en sers à chaque préparation de partie, mais également pour réagir rapidement lorsqu'un joueur souhaite un objet spécifique.

Il contient un grand nombre de filtres, et liste les objets qu'il trouve. Ces objets peuvent ensuite être drag and drop n'importe où.

Il permet également de remplir directement un journal qui fait office de magasin.

![magicsearch.png](/naheulbeuk/magicsearch.png =500x)

* **Choix du compendium**
Permet de ne lister que les objets issus du compendium sélectionné.
* **Nom**
Permet de chercher des mots clés dans le nom des objets.
On peut chercher plusieurs mots clés : mots clés 1 && mots clés 2
On peut chercher des mots clés ou d'autres : mots clés 1 || mots clés 2
* **Type d'objet** 
Permet de chercher un type d'objets au sens Foundry. Ce sont tous les types d'objets décris dans le [chapitre précédent](#titre5).
Si plusieurs types sont sélectionnés, on cherche l'un ou l'autre
* **Catégorie d'inventaire**
Permet de chercher une catégorie d'objets au sens catégorie dans l'inventaire (livres, potions, armes, armures, nourritures...).
Si plusieurs types sont sélectionnés, on cherche l'un ou l'autre
* **Types d'armes**
Si on cherche une arme (= un objet en mains), on peut définir son type (enchantée, relique, arme de contact...).
Si plusieurs types sont sélectionnés, on cherche l'un et l'autre
* **Types d'armures**
Si on cherche une armure (= un objet porté), on peut définir son type (enchantée, pr tête, pr jambes...).
Si plusieurs types sont sélectionnés, on cherche l'un et l'autre
* **Mots clés**
Permet de chercher des mots clés (par exemple épée, sang de corbeaux...) dans tous les attributs des objets (le nom, la description...)
Ce champ permet donc de chercher un objet, mais aussi dans quel contexte il est utilisé.
Par exemple, si je cherche **sang de corbeaux** je vais trouver l'objet en question ainsi que les recettes, plans, rituels qui l'utilisent.
On peut chercher plusieurs mots clés : mots clés 1 && mots clés 2
On peut chercher des mots clés ou d'autres : mots clés 1 || mots clés 2
* **Prix min / max**
Permet de chercher des objets avec un prix minimum ET / OU un prix maximum fixé.
Si on rentre une des deux valeurs, seuls les objets avec un prix seront listés, du moins cher au plus cher.
* **Un seul résultat**
Permet de n'afficher qu'un résultat aléatoire parmis les résultats.

**Exemple 1 : génération d'un ingrédient aléatoire pour du loot**
Je sélectionne le compendium "Equipements : ingrédients" puis je coche un seul résultat.
![magicsearch2.png](/naheulbeuk/magicsearch2.png)

**Exemple 2 : recherche d'une épée de qualité**
J'entre dans le champs "Nom" : épée && qualité.
![magicsearch4.png](/naheulbeuk/magicsearch4.png)

**Exemple 3 : recherche d'une arme au corps à corps enchantée, de moins de 500 PO**
Je sélectionne les types d'arme "Armes de contact" et "Echantée", puis je mets le Prix max à 500.
![magicsearch5.png](/naheulbeuk/magicsearch5.png)

**Exemple 4 : recherche des sorts d'eau, level 2**
Je sélectionne le compendium "Magie : eau et glace" et j'entre "2" dans le champs du "Nom".
![magicsearch6.png](/naheulbeuk/magicsearch6.png)

**Génération d'un magasin**
On peut également utiliser cet outil pour créer des magasins sous forme de Journaux.
Il faut au préalable créer un journal avec une page dédiée, puis rentrer leur nom dans la catégorie **Remplir un magasin** de l'outil.
En relançant la recherche, on a des options supplémentaires. On peut désormais modifier le nom de l'objet, modifier son prix et l'ajouter au magasin.

![magicsearch7.png](/naheulbeuk/magicsearch7.png)

Dans la page dédiée du journal, l'objet est rajouté avec le nom et le prix choisis, et le lien vers l'objet visible uniquement pour le MJ.

![magicsearch8.png](/naheulbeuk/magicsearch8.png)
<br/>

### Chercher ou générer une rencontre {#titre85}

### Lancer Custom {#titre83}

Une macro permettant simplement de préparer des jets de dés.

![99.jpg](/naheulbeuk/99.jpg =500x)

En éditant cette macro, on peut par exemple l'appeler "Test de courage", puis remplacer la difficulté par diff="[@cou](#titre87)" et le nom par name="Test de courage" pour avoir une macro toute prète pour les tests de courage.
Pour faire des jets de dés directs, sans interface, il faut remplacer option="interface" par option="simple"
<br/>

### Compétences {#titre89}

Cette macro permet de lister les compétences possédées par un joueur.

![109.jpg](/naheulbeuk/109.jpg =300x)

En cliquant sur le nom de la compétence, on ouvre sa fiche.
En cliquant sur la valeur, on fait un jet de dés simple avec interface.
<br/>

### Sélection mode drag and drop ET Actions d'attaque {#titre82}

La macro **Sélection mode drag and drop** permet de définir le comportement qu'aura le drag and drop d'un objet dans la barre de macros.
La macro **Actions d'attaque** permet de proposer les différentes actions d'attaque possible pour un personnage, afin de remplacer le drag and drop de chacune d'elles.
Ces deux macros sont expliquées dans le chapitre sur [le drag and drop depuis un personnage vers la barre des macros](#titre9).
<br/>

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
L'objectif de ce chapitre est de donner quelques bases pour le cas où vous voudriez créer vos propres macros.

Pour commencer, il faut savoir qu'un PJ ou un PNJ est un **actor** Foundry. Tous les objets sont des **items**. Il y a aussi des tableaux des journaux... mais je ne vais pas en parler ici.

Ensuite, un actor comme un item a un type.
* Pour les actors : character ou npc 
* Pour les items : ape, arme, armure, attaque, competence, conteneur, coup, etat, gemme, metier, origine, piege, recette, region, sac, sort, trait, truc

On récupère le type d'un élément avec actor.type ou item.type

On peut aussi récupèrer le nom d'un élément avec actor .name ou item .name

Les actors et les items ont des attributs qui permettent d'enregistrer ce qu'ils contiennent. Ces attributs sont visibles dans le fichier **template.json** à la racine du système.
On les récupères avec actor.system ou item.system.
*Par exemple pour avoir la valeur totale du courage, il faut faire :*
```js
let courage = actor.system.abilities.cou.value + actor.system.abilities.cou.bonus + actor.system.abilities.cou.bonus_man
```
*value est la valeur de base, bonus le modificateur lié aux objets, et bonus_man est le modificateur lié aux bonus renseignés par le joueur/mj dans l'onglet Caractéristiques*

Les actors possèdent des items qui ont été drag an drop, ils sont accessibles via actor.items.
*Par exemple, je peux afficher le nom des items possédés dans la console avec les lignes :*
```js
for (let item of actor.items){
  console.log(item.name)
}
```

Pour finir, il y a quelques outils intégrés qui permettent de vous simplifier la tâche :

**let cible = game.naheulbeuk.macros.getSpeakersTarget()**
Renvoie une cible unique sélectionnée 

**let actor_source = game.naheulbeuk.macros.getSpeakersActor()**
Renvoie l'acteur unique sélectionné (PJ contrôlé par le joueur)

**let formula = game.naheulbeuk.macros.replaceAttr(expr, actor)**
Renvoie la valeur d'une expression (expr) en remplaçant les [raccourcis du système](#titre87) (@fo, @bonusfo...) par les valeurs correspondantes de l'actor passé en paramètre.
*Par exemple, si mon actor a 13 de force, game.naheulbeuk.macros.replaceAttr("d6+@bonusfo", actor) renverra d6+1
Autre exemple, si je veux connaitre la valeur totale de protection de mon PJ, je peux faire : game.naheulbeuk.macros.replaceAttr("@pr", actor)*

**game.naheulbeuk.macros.onRoll(actor,item,dataset,option)**
Fait un jet de dés simple, avec ou sans interface.
En paramètre on retrouve :
* l'actor qui fait le jet
* l'item qui n'est plus utilisé mais conservé au cas où. Vous pouvez mettre un objet vide {} pour ce paramètre.
* l'objet dataset qui contient les valeurs du lancer.
*Par exemple :*
```js
let datasetRoll = {}
datasetRoll.dice = "d20" //dés à lancer
datasetRoll.diff = "@cou" //difficulté
datasetRoll.name = "test de courage" //nom de la fenêtre
```
* Une option pour indiquer si on veut faire le lancer avec interface (option="interface") ou sans (option="simple")

**game.naheulbeuk.macros.onRollCustom(actor,item,dataset)**
Fait un jet de dés complexe.
En paramètre on retrouve :
* l'actor qui fait le jet
* l'item est utilisé pour l'affichage du nom du jet dans le chat.
*Par exemple si mon objet s'appelle "épée" et que je fais un "test de force", le nom dans le chat sera : test de force - épée*
On peut passer un objet vide {} pour ignorer cette possibilité, ou alors passer un objet réel en paramètre pour utiliser son nom.
* l'objet dataset qui contient les valeurs du lancer (max dice7, diff7, name7).
*Par exemple :*
```js
let datasetRoll = {}
datasetRoll.dice1 = "d20" //dés à lancer pour le premier jet
datasetRoll.diff1 = "@cou" //difficulté du premier jet
datasetRoll.name1 = "test de courage" //nom du bouton pour le premier jet
datasetRoll.dice2 = "d20"
datasetRoll.diff2 = "@fo"
datasetRoll.name2 = "test de force"
datasetRoll.dice3 = "d20"
datasetRoll.diff3 = "@int"
datasetRoll.name3 = "test d'intelligence"
```

**Exemple :**
Je souhaite faire un test de parade avec les objets équipées.
```js
//on récupère l'acteur du joueur
let actor_source = game.naheulbeuk.macros.getSpeakersActor()
//on regarde ses objets pour trouver les armes et les boucliers équipés
let arme_ou_bouclier=[]
for (let item of actor.items){
  if (item.system.equipe==true && (item.system.prbouclier==true || item.system.arme_cac==true)) {arme_ou_bouclier.push(item)}
}
//on construit le jet de dés (il y a maximum 2 résultats : une arme + un bouclier ou 2 armes)
let datasetRoll = {}
//si on a un premier résultat
if (arme_ou_bouclier[0]!=undefined) {
  datasetRoll.dice1="d20" //on lance 1d20
  datasetRoll.diff1="@prd+"+arme_ou_bouclier[0].system.prd //La difficulté vaut la parade du personnage : @prd + la parade de l'objet : arme_ou_bouclier[0].system.prd
  datasetRoll.name1="Parade "+arme_ou_bouclier[0].name //Le nom du bouton sera Attaque et le nom de l'objet
}
//si on a un deuxième résultat
if (arme_ou_bouclier[1]!=undefined) {
  datasetRoll.dice2="d20" //on lance 1d20
  datasetRoll.diff2="@prd+"+arme_ou_bouclier[1].system.prd //La difficulté vaut la parade du personnage : @prd + la parade de l'objet : arme_ou_bouclier[1].system.prd
  datasetRoll.name2="Parade "+arme_ou_bouclier[1].name //Le nom du bouton sera Attaque et le nom de l'objet
}

//On fait le jet de dés complexe
game.naheulbeuk.macros.onRollCustom(actor_source,{},datasetRoll)
```
Ce qui donne :

![108.jpg](/naheulbeuk/108.jpg =700x)
