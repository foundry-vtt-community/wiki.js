---
title: Git
description: 
published: true
date: 2021-02-10T15:22:12.784Z
tags: 
editor: markdown
dateCreated: 2021-02-10T15:22:09.093Z
---

# Guide de bonnes pratiques Git

Alors je ne vais pas refaire un tuto sur git lui-même, il y en a déjà beaucoup sur le net.
C'est plutôt quelques conseils pour ceux qui découvrent git en même temps que Foundry.
Je ne reviens pas non plus sur le détail de la commande à taper.

Quelques liens pour commencer : 
- La base : [git the simple guide](http://rogerdudler.github.io/git-guide/) (EN)
- Alors ce site permet de voir visuellement ce que font les lignes de commandes : [Explain git with D3](http://onlywei.github.io/explain-git-with-d3/) (EN)
- Une visualisation des différentes zones de travail : [Git cheat sheet](http://ndpsoftware.com/git-cheatsheet.html) (FR)
- [Le site d'Atlassian](https://www.atlassian.com/git) (EN). Une vraie mine d'informations. Evitez les parties qui parlent spécifiquement de Bitbucket (un concurrent à Gitlab). Commencez par la partie Beginner !
- Et un dernier pour la route. J'aime beaucoup Gitkraken Pro comme client graphique Git, mais la licence est payante, par contre leur site avec des rappels ne l'est pas : [Learn Git](https://www.gitkraken.com/learn/git) (EN)
 

Au final, il faut comprendre la notion de workspace, staging et remote.
Il faut comprendre a minima les commandes git suivantes : fetch, merge, rebase, commit, push

Pour la différence entre Merge et Rbase : [Mergin vs Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing) (EN)
 
Et maintenant, quelques bonnes pratiques pour ceux qui commencent. Dans un premier temps, pour ceux qui utilisent Git pour travailler avec eux-mêmes.
En effet, rien de mieux pour ne pas perdre ce qu'on a fait et retourner facilement à un état qui marche :)
 
Alors l'unité de travail, c'est le commit. Un commit c'est un ensemble de changements.
Alors il y a plusieurs façons de faire, plein de petits commits, un gros commit de 100 fichiers modifiés.

Il y a une façon de faire qui est plus efficace, j'en parle juste après.
 
- **Nom et taille d'un commit**

Il faut qu'un commit regroupe un ensemble logique de modifications, avec un nom de commit explicite. 
Du genre : "Amélioration de la gestion de l'initiative", "Correction du montant des dégâts", "Ajout de la capacité xxx". L'idée c'est que rien qu'en voyant le nom du commit, quelqu'un d'autre que toi sache de quoi ça parle. Après il va aller voir les fichiers modifiés. S'il faut aller regarder les fichiers modifiés pour savoir à quoi correspond un commit, ce n'est pas bon.

Donc plutôt que d'avoir un commit qui embarque 3 modifications (exemple : une modification qui concerne 3 fichiers, une autre 1 fichier, une autre 7 fichiers, et à chaque fois des fichiers différents) qui n'ont rien à voir les unes avec les autres, et bien il vaut mieux faire 3 commits (1 de 3 fichiers, 1 de 1 fichier et 1 de 7 fichiers) avec un nom explicite.

L'idée aussi derrière c'est qu'on peut facilement prendre un commit ou pas s'il est atomique (il ne concerne qu'une fonctionnalité). Et s'il y a une régression, c'est plus facile de trouver le commit qui a introduit le bug.

Il faut bien évidemment éviter le nom de commit tel que "cool", "pof" ou "boum" :yum:
De la même façon, en général, on évite un commit qui a pour nom une version. Un commit intitulé "v1.2", ça n'est utile QUE si les modifications ne concernent des fichiers liés à l'intégration via gitlab ou github.

Pour gérer les versions, il faut utiliser les Tag. Je reviens dessus plus loin.

L'idée générale derrière tout ça, c'est qu'en revoyant un historique, vous comprenez rapidement ce qui a été fait simplement en lisant le nom des commits.
 
- **Amend a commit** : [tuto vidéo avec VSC](https://www.youtube.com/watch?v=tXZc6-fH2pg) (EN)

Alors amend c'est vachement pratique quand tu as oublié quelque chose dans le commit que tu viens juste de créer. C'est valable tant que tu n'as pas push. 
Pour rappel, tant que ce n'est pas pushé, tu es plus libre de faire ce que tu veux. Donc imaginons, tu modifies 2 fichiers, tu les "stage" tous les 2, et tu commit en donnant un message "Ajout de xxx". Et tu te rends compte que tu as oublié de modifier un fichier (le même ou un autre). Et bien tu fais ta modif, tu "stage" et tu utilises ***git commit --amend***. Ca va ajouter ta modif au commit précédent en reprenant le message de commit.
Ca ne te fait qu'un seul commit. Ca évite d'avoir 2 commits. Dont 1 qui est en fait inutile.
Attention, bien se souvenir que ce n'est simple à faire que si tu n'as pas pushé.
 
- **Gestion des versions**

En général, on évite que le nom du commit soit le numéro de version. Typiquement si 4 derniers commits intitulés "v2.0.6" portent sur des fonctionnalités différentes, on perd l'information.
Pour les versions, on utilise les tags. Tags/Create Tag dans VSC. Tu crées un tag 2.0.6 que tu vas pushé sur gitlab. Le tag c'est ce qui permet de retrouver facilement une version précise.

