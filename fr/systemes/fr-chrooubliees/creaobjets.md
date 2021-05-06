---
title: CoF : Création d'Objets
description: Bonjour l'Artisan ! Tu ne trouve pas assez d'Objets dans la boutique ? Et bien créons-en ensemble
published: true
date: 2021-04-21T15:02:46.027Z
tags: 
editor: markdown
dateCreated: 2020-12-24T15:21:29.345Z
---

Sur cette page nous traiterons de la façon de créer des Objets de différents type

> La création d'"Objet" permet de créer des Objets de plusieurs catégories: 
Armure - Arme de Contact - Arme à distance - Bouclier - Sort - Bijou - Munition - Contenant - Monture - Monnaie - Divers
{.is-info}

---

**Pour créer un Objet, il suffit de cliquer sur Créer un Objet.
**Dans "**Nom**" : Le nom de votre Objet
Dans "**Type**" : Item

![créa_objet1.png](/images/chroniquesoubliees/customisation/créa_objet1.png)

> La création d'objet étant infinie, nous allons voir ici les différents **traits** accessibles.
{.is-warning}

> Pour le tuto nous allons créer une armure
{.is-info}


# 1. Le Type d'équipement
C'est dans l'onglet "Détail" que vous pourrez choisir le type d'Objet que vous aller créer.
Il suffit de choisir dans la liste de la **"Sous-Catégorie**"

![créa_objet_type.png](/images/chroniquesoubliees/customisation/créa_objet_type.png)

Je choisis ensuite un ou plusieurs traits pour mon Objet.

> Je peux aussi ne choisir aucun "Trait" ni "Sous-Catégorie" afin de créer un objet "Cosmétique" : par exemple l'instrument de musique dans le profil de Barde.
{.is-info}


# 2. Le trait "Equipement"
C'est le trait de base pour tout objet physique.

**Propriété** (possible d'en affecter plusieurs à un objet) : 
- "**Equipable**" : Votre Objet pourra se retrouver dans l'Onglet "*Combat*" s'il est équipé (en cliquant sur le petit bouclier depuis l'"*Inventaire*"
- "**Empilable**" : Donne la capacité à votre Objet de se cumuler. Par exemple un objet "Flèches" Empilable pourra avoir une quantité de "x" au lieu d'avoir "x" fois le même objet dans l'"*Inventaire*"
- "**Unique**" : A l'inverse, l'objet créer sera le seul de son type dans l'"*Inventaire*"
- "**Sur Mesure**" : 
- "**A deux mains**" : L'objet est utilisable avec ou uniquement deux mains

**Prix** (en PA) :
Vous permet de donner une valeur à votre objet

**Rareté** :
Vous permet de définir la rareté de votre objet

**Emplacement** (si "*Equipable*" est sélectionné) :
Endroit du corps où doit être équipé l'objet

> Pour mon Armure de Cuir de l'Ombre:
> - Sous Catégorie : "Armure"
> - Trait : Equipement
> - Propriété : Equipable / Unique
> - Emplacement : Torse
> - Prix : 8000 PA
> - Rareté : Rare
{.is-info}

![armurecuir1.png](/images/chroniquesoubliees/customisation/armurecuir1.png)

# 3. Le trait "Arme"
A sélectionner dès que votre objet peut être utilisé pour attaquer et faire des dégâts

**Propriétés d'armes** : 
- "**Dommages (base)**" : C'est la formule de calcul des dégats. *Pour l'exemple un arc court 1d6*
- **"Dommages (Carac)"**: Si l'objet prend en compte une Carac pour le calcul des dégâts. *Une épée aura Force de choisi*
- "**Dommages (Bonus)**": Si besoin des dégâts peuvent être rajoutés.
- "**Compétence**" : La compétence utilisée pour le jet de dés
- "**Compétence (Bonus)**" : Si besoin rajouter un bonus au résultat du jet de dés
- "**Critique**" : Défini la valeur à partir de laquelle le jet est considéré en réussite critique. Prend la valeur comprise entre "x" et 20
- "**Propriété** (possible d'en affecter plusieurs à un objet) : " : Des options en plus

> Mon Armure n'étant pas une Arme, voici un exemple d'Arc Court
{.is-info}

![armure2.png](/images/chroniquesoubliees/customisation/armure2.png)

# 4. Le trait "Protection"
A sélectionner si votre objet vous apporte de la défense et/ou une réduction de DM.

**Protection** :
- "**Défense (Base)**" : La valeur d'armure de base de l'objet
- "**Défense (Bonus)**" : Le bonus accordé par l'Objet
- "**Propriétés**" : Cocher si l'objet accorde une réduction de dommages
- "**Réduction de dégâts**" : Donne la valeur de dégât réduit

> Mon Armure de Cuir de l'Ombre est un objet Magique :
Elle accorde +2 en DEF, +1 en DEF(bonus) et sa bonne conception accorde une résistance de dégât de 1
{.is-info}

![armure_3.png](/images/chroniquesoubliees/customisation/armure_3.png)

# 5. Le trait "A distance"
A sélectionner si votre objet peut être utiliser/lancé lors d'un combat à distance.

**Propriété d'Armes à Distance**
- **"Portée"** : la portée en mètres
- **"Classe"** : Choisir le type d'arme
- **"Propriétés"** : Choisir si l'arme est rechargeable ou pas
- **"Rechargement"** : Si la propriété "rechargeable" est choisie, alors défini la façon dont l'arme peut être rechargée : Pas d'action / Action Simple / Action Longue

> Mon Armure n'étant pas une Arme à distance, voici un exemple d'Arc Court
{.is-info}

![armure_4.png](/images/chroniquesoubliees/customisation/armure_4.png)

# 6. Le trait "Sort"
A sélectionner si votre objet peut être utiliser/lancé comme un sort.
Ce trait sert avant tout à définir l'objet comme un sort.

> Voici un exemple de 2 Sorts: 1 sort offensif et un Sort défensif.
> Vous remarquerez qu'ils ont été construit comme respecticement une Arme à distance et une Protection
{.is-info}

![armure5.png](/images/chroniquesoubliees/customisation/armure5.png)

> **FELICITATIONS !** Vous êtes un expert Artisan !
{.is-success}

> [Retour au sommaire](/fr/systemes/fr-chrooubliees)
