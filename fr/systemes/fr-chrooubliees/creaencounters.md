---
title: Création des Rencontres
description: Un monde sans opposition ? Vous n'y pensez pas !
published: true
date: 2023-04-15T15:51:36.489Z
tags: 
editor: markdown
dateCreated: 2020-12-23T18:58:15.043Z
---

Sur cette page nous traiterons de la façon de créer une Rencontre.

> L'outil de création de Personnage permet de créer à la fois des Personnages Jouables (PJ), des Personnages Non-Jouables (PNJ), des Rencontres (pour faire simple : tout ce qui sera opposé aux héros) et des Coffres (pour présenter une liste d'objets aux joueurs).
{.is-info}

> Pour plus de lisibilité, je vous conseille de créer un Dossier spécifique à chaque type de personnage que vous aller créer.
{.is-info}
---
> Il y a 2 façons de créer une Rencontre :
-Soit en utilisant le **Compendium Rencontres** (qui contient un très large choix de créatures du SRD).
-Soit en la créant de vous même.
{.is-warning}

## 1. Récupérer une rencontre depuis le Compendium
**La méthode hyper simple.** 

J'ouvre le **Compendium Rencontres**, je choisis le type d'opposition que je souhaite. 
Ici je prendrai un "Ours-Hibou" (parce que j'aime les Ours Hibou).
Je le fais glisser du Compendium vers mon onglet Personnages ou directement vers la scène jouée.

Un clic gauche sur la rencontre importée permet d'ouvrir sa Fiche de Personnage.

![crea_renc_compendium.webp](/images/chroniquesoubliees/customisation/crea_renc_compendium.webp)

> **Pour tous les personnages issus du Compendium, les Caractéristiques, Attaques, Voies et Capacités sont déjà pré-remplies selon les règles du SRD**
Vous pouvez toutefois les modifier à votre guise.
{.is-success}

En cliquant sur le portrait noir, vous pouvez personnaliser l'image de votre rencontre via le gestionnaire de fichier de FoundryVTT.
Dans l'onglet "Prototype Token" en haut à droite, puis "Images", vous pouvez également modifier l'**Image du token** qui apparaitra sur les scènes jouées.

## 2. Créer soit même sa Rencontre
**La technique personnalisable à volonté**
Suivons l'étape de création d'une Gargouille (parce que j'aime aussi les Gargouilles...)

### 1. La Naissance
Pour créer une "Rencontre" il faut d'abord aller dans l'onglet "Personnage" de la barre d'outils. 
Cliquez ensuite sur "Créer Personnage".
Dans l'onglet "Nom" : choisissez le nom de votre Rencontre
Dans l'onglet "Type" : selectionnez "Rencontre"

![crea_encounter.webp](/images/chroniquesoubliees/customisation/crea_encounter.webp)

### 2. Un Monstre moche comme je souhaite
En cliquant sur le portrait noir, vous pouvez personnaliser l'image de votre rencontre via le gestionnaire de fichier de FoundryVTT.
Dans l'onglet "Prototype Token" en haut à droite, puis "Images", vous pouvez également modifier l'**Image du token** qui apparaitra sur les scènes jouées.

### 3. La Carte d'identité de la Rencontre
A partir de maintenant, on voit la grande différence avec les feuilles de PJ et PNJ. Il n'est plus question de Profils et de Race.

A vous de choisir les points suivants en fonction du Livre de Règles p.211 :
- NC
- Taille
- Archétype
- Catégorie
- Boss

![crea_rencontre_id.webp](/images/chroniquesoubliees/customisation/crea_rencontre_id.webp)

### 4. Les Caractéristiques de la Rencontre
Ici aussi un changement majeur : **vous ne rentrez que les MOD**, c'est la valeur qui sera mise automatiquement à jour par le système.

Vous devez ensuite definir vous-même les valeurs des **Attaques** (CONTACT, DISTANCE, MAGIQUE).

La **DEF**, les **PV**, et l'**INIT** sont aussi à remplir manuellement.

![crea_renc_caract.webp](/images/chroniquesoubliees/customisation/crea_renc_caract.webp)

### 5. L'Onglet Combat
Ici pas d'arme à équiper.
En général les attaques de la Rencontre sont données dans sa description et sont souvent propres à la créature. Il faut donc les créer soi même.

- Pour commencer il vous faut choisr le "**Nom**" de l'attaque.
- Puis dans la case "**Mod**" y inscrire le modificateur représentant la chance de toucher.
- La plage des **Critiques** est modifiable: vous pouvez par exemple choisir qu'un adversaire ait un coup critique à partir de 17. Vous inscrivez donc 17 et le système prendra en Critique tous dés compris entre 17 et 20.
- Dans la Case "**DM**" vous devez entrer la formule de dégats.

> La créature est prête à attaquer. Une fois en combat cliquez sur le petit **D20** pour lancer l'attaque avec son test de réussite ou sur les 2 **D6** pour faire uniquement un jet de dégâts.
{.is-success}

![créa_renc_combat.webp](/images/chroniquesoubliees/customisation/créa_renc_combat.webp)

### 6. L'Onglet Inventaire
En général les Rencontres ne possèdent pas ou peu d'inventaire.
Mais vous pouvez y glisser des Bourses pleines d'argent, ou des Objets Magiques par exemple.
Un clic droit sur cet onglet ouvre le **Compendium des Equipements**, permettant d'ajouter directement des objets parmis la liste.

> Pour des infos sur la création de nouveaux Objets, c'est [ici](/fr/systemes/fr-chrooubliees/creaobjets)
{.is-info}

### 7. L'Onglet Voies & Capacités
Un clic droit sur le nom de l'onglet permet d'ouvrir le **Compendium "Voies des rencontres"**. Vous y trouverez l'intégralité des voies des créatures.

Ensuite faites les glisser dans la fiche pour que la créature en bénéficie. Vous n'avez plus qu'à selectionner les capacités qu'elle maitrise dans cette voie.

Vous pouvez également utiliser le **Compendium "Capacités des rencontres**", pour retrouver l'intégralité des capacités de créatures crées dans le système.
> **Une fois une Voie ou Capacité importée vous pouvez la modifier depuis votre fiche de Rencontre**.
C'est ce que je vais faire pour la Capacité "Vol (Pixie)" en Modifiant juste l'intitulé de la Capacité.
{.is-info}

![créa_rencontre_cap1.png](/images/chroniquesoubliees/customisation/créa_rencontre_cap1.png)

> Si vous ne trouvez pas de **Voies** ou **Capacités** qui vous correspondent vous pouvez en créer de nouvelles. 
Pour des infos sur la création de nouvelles **Voies** ou **Capacités**, c'est [ici](/fr/systemes/fr-chrooubliees/customisation)
{.is-info}

Ici j'avais besoin de créer la Capacité *Immobilité* :

![créa_renc_cap3.png](/images/chroniquesoubliees/customisation/créa_renc_cap3.png)

### 8. L'Onglet "Description"
Tout ce que vous voulez écrire, décrire, inventer sur votre Rencontre.

> **FELICITATIONS** ! Vous être prêt à ~~faire mordre la poussière à vos PJ~~ proposer un peu de résistance à vos PJ
{.is-success}

> [Retour au sommaire](/fr/systemes/fr-chrooubliees)