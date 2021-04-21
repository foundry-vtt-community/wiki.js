---
title: 0.0. Spécifications requises
description: 
published: true
date: 2021-04-21T16:23:15.383Z
tags: 
editor: markdown
dateCreated: 2021-02-25T05:50:38.831Z
---

>*Bien qu'il ne soit pas possible de partager les exigences matérielles exactes pour le logiciel car la performance de l'application dépend fortement du type de contenu et des fonctionnalités qui sont utilisées dans un "World", il est possible de fournir quelques recommandations et exigences de base pour une utilisation minimale.*

# PRÉ-REQUIS CLIENT
## CLIENT (JOUEUR ou GM) MINIMUM REQUIS.
>Ordinateur relativement moderne fonctionnant sous Windows 10, MacOS Catalina (ou plus récent) ou Linux, avec prise en charge d'une architecture 64 bits.
>Un GPU intégré pour permettre ***l'accélération matérielle***.
>8GB de RAM
>Un moniteur d'une résolution minimale de 1366x768. Dans cette résolution minimale, de nombreux aspects de l'UI se sentiront à l'étroit.
>Un navigateur web moderne comme Chrome, Firefox, Opera ou Edge avec accélération matérielle activée. **Ce dernier doit être à jour avec la dernière mise à jour possible**.
**_(Safari n'est pas un navigateur pris en charge pour le moment)_**.
{.is-warning}

## CLIENT (JOUEUR ou GM) CONFIGURATION RECOMMANDÉE.
>Ordinateur relativement moderne fonctionnant sous Windows 10, MacOS Big Sur (ou plus récent) ou Linux avec support d'une architecture 64 bits.
>Un GPU dédié qui supporte **WebGL 2.0**.
>16GB de RAM
>Un moniteur avec une résolution de **1920x1080 ou plus**.
>Une souris. Vous pouvez utiliser le logiciel avec un pavé tactile, **mais le logiciel actuel est conçu pour la souris et le clavier**.
>Le navigateur Chrome ou un navigateur à base de Chromium tel que Brave, Opéra, MS EDGE offrant une expérience se rapprochant le plus de l'application de bureautique FVTT. **Ce dernier doit être à jour avec la dernière mise à jour possible**. 
{.is-warning}

# PRÉ-REQUIS SERVEUR
>*Il existe trois modes principaux pour héberger un serveur Foundry : Auto-hébergement, hébergement Cloud et hébergement en partenariat. Le mode "Partner Hosted" décharge la responsabilité du matériel du serveur sur le partenaire d'hébergement, il n'a donc pas besoin de liste de conditions ici.*

## MINIMUM REQUIS POUR L'AUTO-HÉBERGEMENT.
*_Tout ce qui a déjà été énuméré ci-dessus pour les exigences minimales du client, ainsi que :_*

>Un fournisseur de services Internet permettant la redirection de port via IPv4, et qui n'utilise pas le *Carrier Grade Network Address Translation (CG NAT)*.
Un routeur configuré pour transférer les connexions entrantes vers votre PC.
La configuration du pare-feu de votre système d'exploitation permettant les connexions vers le port utilisé par le FVTT.
**Une vitesse de téléchargement d'au moins 1,5 Mo/s (12mbps)*** est recommandée, car l'hôte doit transférer des images, des vidéos ou des fichiers audio vers les lecteurs connectés. Pour les hôtes dont la vitesse de téléchargement est lente, le fait de servir vos fichiers multimédia à partir d'un emplacement de stockage dans le Cloud (par exemple : en utilisant l'intégration de stockage de fichiers S3) évitera cette limitation.
{.is-warning}

**Grâce à une optimisation minutieuse des ressources et à l'utilisation de S3, le FVTT peut être hébergé à des vitesses aussi faibles que 6mbps mais peut connaître des temps de chargement initial extrêmement lents*.

>Note : Si votre FAI fournit une adresse IPv6 et que vos joueurs peuvent se connecter via IPv6 (confirmé par ce lien: https://test-ipv6.com/), il est fort possible que vous n'ayez pas besoin de mettre en place une redirection de port, et que vos joueurs puissent se connecter à vous via IPv6 à la place, ce qui rend inutile la redirection de port.
{.is-warning}

## MINIMUM REQUIS POUR UN SERVEUR DÉDIÉ.
>Au moins 1 vCPU **(2vCPU recommandés)**.
>Au moins 1 Go d'espace de stockage.
>1 GB de RAM
>Pare-feu et paramètres de sécurité (selon l'hôte) configurés pour permettre les connexions sur le port requis.
{.is-warning}

*_Sources :_ https://foundryvtt.com/article/requirements/*