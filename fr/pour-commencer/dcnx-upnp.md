---
title: 5.0 Déconnexions régulières
description: 
published: true
date: 2021-04-21T16:49:05.666Z
tags: 
editor: markdown
dateCreated: 2021-04-06T11:48:39.667Z
---

# Déconnexions régulières	

Si vous hébergez Foundry chez vous et que vous avez des déconnexions régulières de vos joueuses et joueurs, c'est dans 99% des cas à cause de l'UPnP.

Pour résoudre ce problème, il faut : 
- Désactiver 'UPnP' dans l'interface de Foundry et le relancer
- Rebooter vor box ADSL/Fibre (pour qu'elle supprime la règle UPnP précédente)
- Aller dans l'interface de votre box et trouver son menu 'NAT/PAT'
- Mettre le port externe 30000, redirigé vers le port 30000 de l'ordinateur hébergeant Foundry
- Ouvrir le port 30000 dans le firewall de votre PC hébergeant Foundry.

Tester !

Si vous ne savez ouvrir un port dans votre box, effectuez une petite recherche avec votre moteur préféré du type : 'ouvrir des ports dans ma box XXXX' avec XXXX le nom de votre box adsl/fournisseur.

