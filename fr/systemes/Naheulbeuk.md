---
title: Naheulbeuk
description: 
published: true
date: 2022-11-20T08:51:14.994Z
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
Ca peut aussi être une compétence qui n'existe pas dans naheulbeuk mais qui me permet d'éviter de demander un jet de caractéristiques (courir/sauter, perceptionner ref Game of roles :D)

Bref vous n'êtes pas du tout obligé d'utiliser ça, mais si vous le souhaitez il existe un compendium **Compétences de base** dédié.
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
* **Armures et Protection - objets portés** : cette catégorie contient tous les objets portés par le personnage et qui sont équipés. Ce sont principalement les armures, les bijoux et les vêtements.
On peut faire ici les tests de rupture.
![21.jpg](/naheulbeuk/21.jpg =500x)

Comme pour les sorts, un d6 tout seul signifie un jet un peu particulier, c'est le cas notamment des armes à poudre.
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
Dans ce chapitre, nous n'allons pas voir comment créer un nouveau PNJ, mais plutôt regarder le fonctionnement de ceux déjà créés.
De manière générale lorsque vous souhaitez créer un nouvel objet ou PNJ il faut toujours partir de l'existant !

Le système contient des PNJ dans le compendium **Bestiaire**.
Avant de commencer, presque tous les objets précédents s’utilisent sur les rencontres (les états, les coups spéciaux, les sorts…). Seul l’équipement est différent puisque pour une rencontre il ne sert qu’à remplir l’inventaire. Les attaques passent par un nouveau type d’objet.
J’ai donc rajouté 3 types d’objets en plus pour les rencontres : 
•	Les attaques (une rencontre n’utilise pas les armes classiques comme je l’ai dit plus haut)
•	Les traits (pour ceux qui ont le bestiaire papier c’est un truc sympa pour améliorer le gameplay)
•	Les régions (pour dire d’où vient une rencontre)
Maintenant pour la feuille de rencontre, on commence par choisir une catégorie, puis on drag and drop les traits et les régions depuis le compendium Système : bestiaire données. Ensuite on rentre les caractéristiques du monstre.
 
En cliquant sur le + à droite de Carac, on a accès à quelques stats supplémentaires, utiles notamment pour les mages :
  
En dessous on retrouve les états (cf la première capture) puis la description.
Ensuite, le deuxième onglet est dédié au combat.
On y retrouve les attaques, les sors, les coups spéciaux et la description des infos utiles.
 
N’oublie pas que tu peux créer des états (par exemple un empoisonnement par dague empoisonnée), ou en prendre un existant, et le drag and drop dans la description du monstre pour pouvoir le retrouver facilement 😊
Enfin l’onglet inventaire permet de gagner du temps si les aventuriers veulent loot cette rencontre !
 

Comme d’habitude, je te conseille de regarder ce qui existe dans le bestiaire pour t’en inspirer avant de créer ta propre rencontre ! Tu verras que contrairement aux PJ, c’est vraiment très simple.
 

## L'extension soldat {#titre3}
Pour l’extension soldat, j’ai regroupé toutes les informations utiles dans la fiche de métier.
Tu as donc uniquement besoin d’aller dans le compendium Divers : métiers et de cliquer sur le métier souhaité :
 
Au passage, le volontaire reste un aventurier, donc pour lui, à toi de voir comment le gérer. Il faudra que tu ailles consulter le pdf pour connaitre sa solde 😉
Lorsque j’ouvre un métier, je retrouve les informations suivantes :
•	La description
•	Les prérequis
•	La carrière en fonction du niveau, avec la solde
•	Les compétences
•	Les coups spéciaux
•	Un lien vers tout l’équipement
•	Un lien vers les tableaux de récompenses et de punitions
 

Tout ce contenu est standard, donc je vais uniquement parler de l’équipement car j’ai utilisé une nouvelle façon de centraliser les données.
Je clique donc sur Extension soldat : équipement
Le document qui s’ouvre est de type « Note ».
J’ai repris le contenu des PDF en remplaçant les objets par leur lien vers un compendium.
Ils peuvent donc être ouvert ou drag and drop à partir de cette Note.
 

Dernière remarque, je n’ai pas encore ajouté les montures au système, mais j’ai quand même bricolé un truc pour cette extension. Ce sont donc des objets à drag and drop sur la fiche de héros.
Il y a maintenant une catégorie « Monture » dans l’inventaire, et à partir d’ici on peut cliquer sur Equiper/Appliquer pour symboliser le fait de monter sur la monture puis faire un jet d’attaque si applicable.
A toi de rajouter un token si tu veux 😉 Personnellement je n’en rajoute pas, je trouve ça trop pénible pour mes joueurs de devoir bouger ce token en permanence, c’est pour ça que c’est un objet et pas un acteur. Mais le fonctionnement évoluera probablement quand j’attaquerai les montures car pour l’instant c’est très basique.

## L'extension ingénieur {#titre4}
Pas mal de petites nouveautés pour ce supplément.
Déjà j’ai rajouté des fiches métiers pour toutes les spécialisations de l’ingénieur :
 

Chaque fiche contient le détail de la spécialisation avec les objets qui peuvent être drag and drop :
 
Il y a aussi la liste des plans et recettes et les matériaux et outils des ingénieurs. Si tu ne veux pas que tes joueurs connaissent cette info, n’hésite pas à la passer en secret via l’éditeur 😊
La liste des plans contient tous les plans classés par catégorie d’ingénieur :
 
La liste des matériaux et outils contient… les matériaux et les outils de l’ingénieur, logique :D
 
Petite remarque, les plans ont des matériaux qui ne sont pas listés ici : soit parce qu’ils existaient déjà ailleurs, soit parce qu’ils sont exotiques (c’est pas moi qui ai décidé ça, je ne fais que réécrire les PDF dans le système ^^ )
Les plans sont un nouveau type d’objet. Ils peuvent être drag and drop sur les feuilles de personnage.
Ils vont dans la catégorie Ingrédients (ce qui se rapproche le plus de l’artisanat) et ont de nouveau lancer de dés.

 

Lorsqu’on clique dessus, on peut faire un jet de confection de spécialiste (on maitrise la spécialité d’ingénieur nécessaire) ou de non spécialiste. Les calculs avec l’ingéniosité sont complètement automatique 😊

 

Si j’ouvre un plan, je retrouve :
•	la spécialité
•	le niveau nécessaire pour fabriquer l’objet
•	la catégorie (pour le classement dans l’inventaire)
•	les matériaux nécessaires
•	les outils nécessaires
•	le temps de confection
•	la difficulté (utilisée pour les calculs d’ingéniosité)
•	l’XP en cas de réussite
•	Le malus pour le non-spécialiste (utilisé pour les calculs d’ingéniosité)
•	Le prix
•	Le détail de la confection, avec le lien vers l’objet créé (si approprié) qui peut être drag and drop dans l’inventaire.
 
Pour finir, si tu cherches un matériau ou un outil, comme je disais précédemment ils ne sont pas tous dans la liste issue du PDF. Donc je te conseille d’utiliser plutôt l’outil de recherche dispo dans le compendium des macros ! En plus tu pourras directement l’ajouter à un magasin.
 
 

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