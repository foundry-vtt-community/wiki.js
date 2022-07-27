---
title: 4.2 Utilisateurs & Permissions
description: 
published: true
date: 2022-05-19T13:25:26.353Z
tags: 
editor: markdown
dateCreated: 2021-04-12T11:29:32.370Z
---

# Aperçu
Chaque joueur qui se connecte à une session Foundry Virtual Tabletop est un utilisateur. Chaque utilisateur se voit attribuer un rôle qui détermine ce que l'utilisateur est autorisé à faire dans les limites du monde du jeu. Les droits peuvent être configurées afin de contrôler plus finement les fonctionnalités disponibles pour les utilisateurs dans n'importe quel monde hébergé. Foundry VTT dispose de deux niveaux de paramètres de droits qui peuvent être configurés pour autoriser autant ou aussi peu de restrictions que nécessaire pour votre jeu et vos utilisateurs.

## Permissions : Rôle
Rôle est un droit attribué lors de la configuration du compte joueur. il affecte un ensemble de droits pré-configurées par défaut pour ce type de compte dans votre monde de jeu.

## Permissions : Entité
Droit Affecté à chaque élément tel que acteur, objet, article ou autre, pour permettre aux utilisateurs d'accéder aux documents. Les droits contrôlent la manière dont un utilisateur spécifique peut interagir avec cette entité.

# Création et configuration des utilisateurs
Les utilisateurs sont créés dans un monde de jeu actif et l'ensemble des utilisateurs est spécifique à ce monde. Il n'y a pas de compte utilisateur global ou inter-monde dans Foundry VTT. Chaque monde maintient sa propre liste de joueurs et des contrôles des droits au niveau de l'utilisateur

Pour créer un nouvel utilisateur, cliquez sur l'icône Paramètres du jeu (icône à trois rouages) dans la barre latérale droite, puis cliquez sur **"Configuration des joueurs"**. De là, vous devriez voir l'utilisateur du maître de jeu et un bouton indiquant **"Ajouter un joueur"**. En cliquant sur ce bouton, Foundry créera un nouvel emplacement utilisateur générique, généralement nommé "Player 2" ou similaire.

Vous pouvez ensuite renommer l'utilisateur, lui donner un mot de passe et définir son niveau de permissions. Pour les besoins de ce guide, les joueurs n'ont pas besoin de se voir attribuer un rôle au-delà de "Joueur". Gardez également à l'esprit que les mots de passe des utilisateurs ne sont pas sécurisés par cryptographie et ne doivent pas être réutilisés à partir d'autres comptes ou services importants que vous ou vos joueurs utilisez.

Une fois que vous avez configuré tous vos comptes de joueur, cliquez sur **"Lancer la session de jeu"** et vous serez renvoyé à votre jeu. Les joueurs peuvent désormais rejoindre et se connecter avec les informations d'identification que vous leur avez fournies. Bien qu'il y ait un peu plus de configuration à faire avant de pouvoir jouer.

<span style="display:block;text-align:center">![config_joueur.webp](/community/french/users_permissions/config_gestion_joueurs.webp)
*L'écran **"Gestion des Joueurs"** vous permet de créer, modifier et supprimer des comptes d'utilisateurs de votre **univers (World)**.*</span>

## Paramètres : Configuration utilisateur
Une fois qu'un compte utilisateur a été créé, il existe des options de configuration supplémentaires pour chaque utilisateur qui peuvent être personnalisées par le GameMaster ou par le joueur lui-même en faisant un clic droit sur son nom dans la **liste des joueurs** (située en bas à gauche l'interface utilisateur).

<span style="display:block;text-align:center">![config_joueur.webp](/community/french/users_permissions/config_joueur.webp)
*L'application de **"Configuration du Joueur"** permet à un joueur de choisir son personnage préféré et d'autres paramètres spécifiques à l'utilisateur*</span>

# Rôles des utilisateurs
Chaque utilisateur a un **rôle** spécifique qui configure son jeu de droits de base d'actions qu'il peut effectuer dans un jeu Foundry VTT. Le rôle de chaque utilisateur est configuré à l'aide de la page **Gestion des utilisateurs** décrite ci-dessus. Chacun des niveaux de rôle et leur signification sont décrits ci-dessous:
- **Aucun**
L'utilisateur n'est pas autorisé à effectuer des actions dans Foundry Virtual Tabletop. Vous pouvez utiliser ce rôle pour interdire temporairement ou définitivement à un utilisateur de rejoindre le jeu.

- **Joueur**
L'utilisateur peut rejoindre le jeu avec des droits disponibles pour un joueur standard. Ils ne peuvent pas entreprendre des actions plus avancées qui nécessitent des droits de confiance, mais ils disposent des fonctionnalités de base nécessaires pour fonctionner sur la table virtuelle.

- **Joueur de Confiance**
Similaire au rôle de joueur, sauf qu'un utilisateur de confiance a la possibilité d'effectuer des actions plus avancées telles que créer des dessins, des modèles mesurés ou même (éventuellement) télécharger des fichiers multimédias sur le serveur.

- **Assistant MJ**
Un utilisateur spécial qui possède plusieurs des mêmes commandes en jeu qu'un utilisateur maître du jeu, mais qui n'a pas la capacité d'effectuer des actions administratives telles que la modification des rôles d'utilisateur ou la modification des paramètres de niveau mondial.

- **Maître du Jeu**
Un utilisateur spécial qui a un contrôle administratif sur ce monde spécifique. Les maîtres de jeu se comportent très différemment des joueurs en ce sens qu'ils ont la possibilité de voir toutes les entités et objets du monde ainsi que la possibilité de configurer les paramètres du monde.

<span style="display:block;text-align:center">![config_joueur.webp](/community/french/users_permissions/config_droits.webp)
*Permissions par défaut en fonction du rôle attribué.*</span>

# Droits des entités
Chaque entité a son propre modèle de droits qui déterminent ce qu'un utilisateur particulier est autorisé à faire avec cette entité. Pour en savoir plus sur la manière dont les droits d'entité affectent une entité spécifique, vous devez examiner chaque article d'entité: **acteurs, objets, articles, macro**

Un bref aperçu des droits d'entité:
- **Aucune**
Restreint cette entité afin qu'ils ne puissent même pas voir cette entité dans la barre latérale.

- **Limité**
L'utilisateur peut interagir avec cette entité de manière très basique, elle apparaîtra généralement dans la barre latérale, mais la feuille d'entité n'affichera généralement que des informations limitées (telles que définies par les règles du système de jeu).

- **Observateur**
L'utilisateur a la possibilité de visualiser cette entité comme s'il était propriétaire, mais il s'agit d'un accès en lecture seule. Ils ne peuvent apporter aucun changement.

- **Propriétaire**
L'utilisateur peut afficher et apporter des modifications à l'entité comme s'il était le MJ, la seule exception étant qu'il ne peut pas supprimer l'entité.

<span style="display:block;text-align:center">![config_joueur.webp](/community/french/users_permissions/config_actor.webp)
*La fenêtre de **Configuration des Droits** vous permet de déterminer qui a le contrôle d'une entité et de ses données.*</span>