---
title: Les effets
description: Comment créer et utiliser les effets sur un personnage ou un objet
published: true
date: 2021-05-03T13:38:23.711Z
tags: 
editor: markdown
dateCreated: 2021-05-02T08:50:41.844Z
---

# Les effets
Vous pouvez créer des effets sur un personnage ou un objet. Le fonctionnement est quasi identique.

## Fonctionnement général
La fonctionnalité des effets intégrés au système est relativement nouvelle est offre de nombreuses possibilités.
Seuls les effets standards seront décrits ici, ceux pour qui la configuration a déjà été éprouvée.

Il faut par ailleurs bien comprendre le fonctionnement des effets.
Les effets sont appliqués systématiquement avant d'afficher la feuille du personnage et sont appliqués à la FIN.

Par exemple pour les caractéristiques, il y a les champs Base, Racial, Bonus et Valeur.
Nous conseillons de ne modifier que le champ Bonus par effet.

Un exemple : soit un personnage qui a Base : 12, Racial : 2, Bonus : 0 ; soit un total de 14.
Si vous créez un effet qui ajoute 2 en Bonus ; alors le total passera à 16 et vous verrez +2 dans Bonus.
Si vous ajoutez manuellement 2 dans le champ Bonus, alors vous verrez +4 dans le champ Bonus et le Valeur sera 18.
Par contre, si vous remettez manuellement 0 dans le champ Bonus, alors celui-ci va passer à +2 e vous verrez 16 dans le champ Valeur. L'effet est TOUJOURS appliqué (tant qu'il est activé) à la fin, donc 0 auquel est ajouté +2, ça donne bien +2.

Les bonus, malus et mod étant recalculés à tout moment, c'est aussi pour ça que si vous exportez le personnage, les valeurs de ces champs ne sont pas celles que vous voyez à l'écran.

Lorsque vous créez un effet sur un objet, au moment vous donner l'objet à un personnage, il va y avoir un transfert de l'effet au personnage pour qu'il soit pris en compte.
A partir de ce moment, l'effet transféré est NON MODIFIABLE. Vous pouvez toujours l'activer ou le désactiver, mais pas le modifier.
Pour le supprimer, il suffit de supprimer l'objet de l'inventaire.
Si vous voulez le modifier, il faut supprimer l'objet, aller éditer l'objet source et le redonner au personnage.

### Créer un effet
Il suffit d'aller dans l'onglet Effets du personnage ou de l'objet. Vous avez alors accès à 3 onglets.

#### Onglet Détails
Les champs sont :
- Libellé de l'effet : Il sera affiché dans la liste des effets, Nouvel Effet par défaut.
- Icône de l'effet : Prérempli avec l'icône par défaut
- Teinte des icônes
- Effet Suspendu : Permet d'activer/désactiver l'effet
- Origine de l'effet : Uniquement dans l'onglet effet du personnage. Ce que vous voyez change en fonction de l'origine
		- l'effet a été créé directement dans l'onglet effet du personnage : Actor.IDactor
		- l'effet vient d'un objet qui lui a été donné : Actor.IDactor.OwnedItem.IDitem
 - Transférer l'effet au personnage : Uniquement sur un Item, à laisser coché

#### Onglet Durée
Beaucoup de champs sur cet onglet. Les durées ne sont utiles que si vous avez des modules tiers qui gèrent l'écoulement du temps.
Cela va vous permettre d'avoir des effets avec une durée temporaire.
Pour les effets permanents, vous pouvez laisser vide.


#### Onglet Effets
C'est là que vous allez créer toutes les modifications qu'apporte votre effet. Un effet peut en effet avoir autant de modifications que vous voulez.

- Clé d'attribut : c'est l'attribut qui va être modifié, voir la section Attributs juste après
-	Changer le mode
		-	Personnalisation
		-	Multiplier : L'attribut sera multiplié par valeur
		- Ajouter	: Valeur sera additionné à l'attribut. Si c'est un chiffre c'est une sommme, si c'est un texte ça sera une concaténation. Pour un bonus, utiliser un nombre positif, pour un malus un nombre négatif
		- Baisser : Remplace par la valeur si elle est inféieure à l'actuelle
		- Augmenter : Remplace par la valeur si elle est supérieure à l'actuelle
		- Surcharger : Remplace par la valeur quelle que soit l'actuelle
- Valeur de l'effet : la valeur utilisée avec le mode

Ce qui a été validé : Utiliser Ajouter +x pour avoir un bonus, Ajouter -x pour avoir un malus.

### Les attributs
Les attributs sont accessibles via leur clé dans le modèle de données.
TOUS les attributs sont accessibles. Pour tous les voir, vous pouvez selon votre niveau de compétence, regarder dans la console (F12) ou lire le fichier template.json du système.
Vous trouverez ici la liste des attributs les plus utiles pour simuler les capacités ou la magie.
Il faut utiliser les champs bonus ou malus lorsqu'ils existent.

Les caractéristiques
- data.stats.[stat].bonus : valeur positive pour un bonus, négative pour un malus
- data.stats.[stat].skillbonus : bonus qui sera appliqué et visible lors du jet de la caractéristique 
- data.stats.[stat].skillmalus : malus qui sera appliqué et visible lors du jet de la caractéristique 

Les valeurs possibles pour [stat] sont : str|dex|con|int|wis|cha
Par exemple, si vous voulez donner un bonus de +4 à la valeur de charisme : 
	Clé : data.stats.cha.bonus Mode : Ajouter Valeur : 4

Les attaques
- data.attacks.melee.bonus : attaque de contact
- data.attacks.melee.malus : attaque de contact
- data.attacks.ranged.bonus : attaque à distance
- data.attacks.ranged.malus : attaque à distance
- data.attacks.magic.bonus : attaque magique
- data.attacks.magic.malus : attaque magique

Les attributs secondaires
- data.attributes.hp.bonus : Points de vie
- data.attributes.fp.bonus : Points de chance (fortune points)
- data.attributes.mp.bonus : Points de magie (Mana points)
- data.attributes.rp.bonus : Points de recup (recovery points)
- data.attributes.init.bonus : score d'initiative
- data.attributes.init.malus : score d'initiative
- data.attributes.def.bonus : score de def
- data.attributes.def.malus : score de def
