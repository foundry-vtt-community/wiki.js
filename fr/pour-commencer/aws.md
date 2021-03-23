---
title: 2.1. Installation sous AWS
description: 
published: true
date: 2021-02-16T22:29:50.474Z
tags: 
editor: markdown
dateCreated: 2020-12-05T13:54:41.441Z
---

# Déploiement automatique de Foundry VTT (10-40 min)

> Ce guide est dédié aux utilisateurs novices qui veulent déployer un serveur de Foundry VTT sur AWS en utilisant “CloudFormation”. Le processus prend moins de 30 minutes après lesquelles vous aurez un serveur Foundry VTT complétement fonctionnel. 
{.is-info}

## Simulation (sur 1 mois)

En laissant toutes les options par défaut cela ne vous coûtera pas 1€. Quelques options supplémentaires pour rendre les choses plus faciles sont incluses pour un coût minimal. 

> *Petit retour de facture sur le mois de janvier 2021 avec les options par défaut:*
{.is-info}

![prolice-screen-aws-deploy-016.png](/images/prolice-screen-aws-deploy-016.png)

> *La simultion du coût pour une configuration identique sur le mois de février (hors période de gratuité):*
{.is-warning}

*La simulation ci-dessous prend en compte:*
* Une mise sous tension de votre serveur 7/24.
* Pas d'option supplémentaire (pas de S3, pas de snapshot, etc...).
* Un trafic de 20Go sur le mois.
![prolice-screen-aws-deploy-021.png](/images/prolice-screen-aws-deploy-021.png)

> *Et enfin une simulation en mettant sous tension le EC2 qu'au moment des parties (hors période de gratuité):*
{.is-success}

La simulation ci-dessous prends en compte: 
* la mise sous tension du serveur 1 fois par semaine.
* Une session allant de 4 à 8 heures (+ préparation MJ).
* Pas d'option supplémentaire (pas de S3, pas de snapshot, etc...).
* Un trafic de 20Go.
![prolice-screen-aws-deploy-021.png](/images/prolice-screen-aws-deploy-022.png)

Le coût sera impacté par ce que vous allez stocker sur le S3. Je vous conseille de bien lire comment le stockage S3 est calculé en termes de charge financière => https://aws.amazon.com/fr/s3/pricing/. 

> Note: Utilisez cette option (S3) en connaissance de cause en ayant bien lu les tarifs AWS.
> Une fois la période gratuite expirée le **Coût peut vraiment monter**. 
{.is-warning}

#### Prérequis

Il faut avoir un compte chez Amazon (AWS).
Voir *Notes complémentaires* dans ce même article (ça prend 5 min)

#### Options Supplémentaires :
1. Sauvegarde automatique de votre instance EC2. 
- a. Ces sauvegardes sont appelées “Snapshots”, avec le modèle vous pourrez le programmer de manière quotidienne ou hebdomadaire. 
- b. AWS garde les 5 derniers “Snapshots”, et supprime le plus vieux chaque fois qu’un nouveau est créé. 
- c. https://aws.amazon.com/ebs/pricing/
2. IP publique Dédiée (Elastic IP). 
- a. Permet d’avoir une IP fixe pour votre instance. Chaque fois que vous coupez et redémarrez votre instance. 
- b. Le coût ne sera activé que lorsque vous réservez une IP que vous n’utilisez pas. Il faut que cette IP soit affectée à un server actif. 
- c. https://aws.amazon.com/premiumsupport/knowledge-center/elastic-ip-charges/
3. Instance de type “Large”. 
- a. Par défaut, le modèle utilise un t2.micro qui est éligible à la gratuité sur AWS. Je propose également une option pour avoir une instance plus large. 
- b. https://aws.amazon.com/ec2/pricing/on-demand/

## Détails du Modèle :
Actuellement le modèle ne fonctionne qu’avec les régions suivantes:
- us-east-1
- us-west-2
- eu-west-1 (Europe Irelande)
- eu-central-1
- sa-east-1
- ap-southeast-2
- eu-west-3 (Europe Paris)
- **nouvelles locations en développement update en février**

Le modèle utilise un serveur de type vanilla Amazon Linux 2 AMI. Les “Snapshots” se font tous les jours à 12h00 GMT si vous choississez “Quotidien”, et chaque lundi à 12h00 GMT si vous choisissez hebdomadaire. Par défaut le port 22 est le seul port ouvert pour les autres instances dans le même VPC. Si vous voulez vous connecter via une console SSH (Putty, Ssh windows, ...), il vous faudra ouvrir le port 22 vers l'extérieur.

## Avant de commencer :

1. Se connecter à AWS, et dans le coin en haut à droite sur la page principale vous devriez voir votre emplacement (localisation).
![prolice-screen-aws-deploy-001.png](/images/prolice-screen-aws-deploy-001.png){.align-center}
![prolice-screen-aws-deploy-002.png](/images/prolice-screen-aws-deploy-002.png){.align-center}
2. Cliquez sur la liste déroulante. Il y a 7 régions disponibles pour le déploiement. Sélectionnez-en une. 

- a. US East (N. Virginia) us-east-1
- b. US West (Oregon) us-west-2
- c. Europe (Ireland) eu-west-1
- d. Europe (Paris) eu-west-3
- e. Europe (Frankfurt) eu-central-1
- f. South America (São Paulo) sa-east-1
- g. Asia Pacific (Sydney) ap-southeast-2

3. Prenez celle qui est la plus proche de vous. Mais si vous n’êtes pas proche d’une de ces régions ça devrait fonctionner aussi avec une légère latence. Vous pouvez toujours ajouter une instance dans votre région par la suite.

## 1ère Etape :
La seule étape manuelle sur AWS consiste à créer une paire/clé SSH, si vous en avez déjà une vous
pouvez passer cette étape, et passer à la ligne suivante. 

1. Connectez vous à AWS, et accédez à EC2 dans le tableau de bord. Pour trouver le service EC2, rendez-vous dans le menu “services” en haut à gauche de la page. C’est le premier service en- dessous de Compute.
![prolice-screen-aws-deploy-003.png](/images/prolice-screen-aws-deploy-003.png){.align-center}
2. Dans le menu de gauche, cliquez sur l’option “Paires de clés”.
![prolice-screen-aws-deploy-004.png](/images/prolice-screen-aws-deploy-004.png){.align-center}
3. Dans le coin supérieure droit cliquez sur “Créer Paires de clés”. 
4. Entrez un nom pour votre clé. 
5. Sélectionnez pem ou ppk (si vous ne maitrisez pas ce type de fichier, choisissez pem car vous pourrez toujours le convertir facilement en ppk plus tard).
6. La clé/pairz devrait se télécharger automatiquement. 

> ***ATTENTION GARDEZ CE FICHIER, C’EST VOTRE SEULE VOIE D’ENTRÉE SUR VOTRE SERVEUR ...***
> *Même amazon ne pourra pas vous en produire une nouvelle.*
{.is-danger}


## 2ème Etape :
Cette étape permettra de déployer le serveur Foundry VTT dans son entièreté
1. Téléchargez “modele-ec2-prolice” avec le lien ci-dessous
https://bucket-prolice-s3.s3.eu-west-3.amazonaws.com/Installation/modele-ec2-prolice
2. Si vous n’êtes pas encore connecté à AWS, faites-le maintenant. 
3. Sur la page principale d’AWS, cherchez “CloudFormation” et cliquez dessus dans les résultats de recherche.
![prolice-screen-aws-deploy-005.png](/images/prolice-screen-aws-deploy-005.png){.align-center}
4. A partir du tableau de bord de CloudFormation cliquez sur le bouton “Créer une pile”.
![prolice-screen-aws-deploy-005.png](/images/prolice-screen-aws-deploy-006.png){.align-center}
5. Sur la page suivante activez les options “Le modèle est prêt” et “Charger un fichier du modèle”,
![prolice-screen-aws-deploy-007.png](/images/prolice-screen-aws-deploy-007.png){.align-center}
6. Choisissez “Charger un fichier de modèle”, puis appuyez sur “Suivant”.
7. Remplissez tous les paramètres de la pile. La plupart d’entre-eux ont une description détaillée mais vous trouverez quelques explications complémentaires ci-dessous: 

![prolice-screen-aws-deploy-008.png](/images/prolice-screen-aws-deploy-008.png){.align-center}

- **AdminUserPW**: 
Ce script crée un compte Administrateur pour votre compte AWS, ce qui permet de ne pas utiliser systématiquement votre accès “root”. Ce paramètre sera le mot de passe de ce compte. 

> Attention, il faut que votre mot de passe administrateur soit fort. Au minimum: 
a. entre 8 et 128 caractères
b. au moins une minuscule
c. au moins une majuscule
d. au moins un caractère spécial
e. au moins un chiffre
{.is-warning}

Veillez à produire votre mot de passe avec les règles ci-dessus annoncées pour éviter un "rollback" de la pile avant la fin d'installation. Ce qui vous obligera à tout recommencer ;-)

- **S3BucketName**:
Le nom unique ne doit comporter que des lettres en minuscule

- **FoundryDownloadLink**: 
Ce paramètre nécessite un lien de téléchargement, que ce soit sur le Patreon si vous y participez ou en en accès public via Google Drive afin de télécharger FoundryVTT sur votre installation Linux. Attention si vous choisissez de placer votre fichier FoundryVTT.zip sur Google  ...
> N’oubliez pas de LE RETIRER après l’installation. 
{.is-warning}


### Guide pour le lien Patreon:
Attention, il est bien question ici du Patreon, et pas de la page de téléchargement de votre profil si vous avez simplement acheté la licence.
Rendez-vous sur votre page patreon et récupérez le lien pour la version Linux de FoundryVTT, cela
devrait ressembler à ceci:
https://foundryvtt.s3-us-west-2.amazonaws.com/releases/[AccessKey]/FoundryVirtualTabletop-linux-x64.zip
Copiez ce lien dans le bon paramètre de la pile. 

### Passer de FoundryVTT à Google Drive:
- a. Téléchargez l’installation Linux .zip de FoundryVTT depuis https://foundryvtt.com/ après vous être connecté
- b. Importez le fichier zip sur Google Drive. 
- c. Assurez-vous que le lien n'est pas en mode limité.
![prolice-screen-aws-deploy-015.png](/images/prolice-screen-aws-deploy-015.png){.align-center}
- d. Bouton droit sur le fichier, et “Obtenir le lien”. 
- e. Cliquez sur “Copier le lien” et collez le dans le paramètre de la pile. Le lien va ressembler à quelque chose comme https://drive.google.com/file/d/xxxxxxxxxx/view?usp=sharing

8. Appuyez sur “Suivant” 
9. Appuyez encore sur “Suivant”
10. Descendez au bas de la page pour accepter le texte légal et puis “Créer la pile".
11. Faites le fou et cliquez sur le picto rafraîchir et priez pour qu’il n’y ait pas d’erreur. 
![prolice-screen-aws-deploy-013.png](/images/prolice-screen-aws-deploy-013.png){.align-center}
12. Le script se lance, puis effectue un redémarrage. Ensuite, **laissez lui un peu de temps** pour exécuter le script de déploiement avant de continuer (**environ 5 minutes**). 

## 3ème Etape :
> Félicitations ! Si vous avez atteint cette étape c’est que vous avez votre propre serveur FoundryVTT avec un lien au stockage S3. 
{.is-success}

Maintenant, il ne vous reste plus qu’à récupérer l’IP de votre machine AWS et à vous connecter: 
1. Rendez-vous sur le tableau de bord EC2, pour ce faire cliquez sur Service en haut à gauche de la page principale et recherchez EC2.
2. Sur la page principale, cliquez sur “Instances en cours d’exécution”.

![prolice-screen-aws-deploy-009.png](/images/prolice-screen-aws-deploy-009.png){.align-center}

3. Vous devriez voir apparaître dans la liste des instances une instance nommée “FoundryServer”. A la droite du nom vous trouverez les colonnes “DNS IPv4 publique” & “Adresse IPv4 publique”. Copiez une des deux valeurs. 
4. Collez cette valeur dans votre navigateur internet et ajoutez “:30000” à la fin (sans les guillemets). Faites bien attention à vous connecter en **http** et pas en https !

![prolice-screen-aws-deploy-010.png](/images/prolice-screen-aws-deploy-010.png){.align-center}
5. FoundryVTT vous demandera d’introduire votre numéro de licence. 
6. Profitez de FoundryVTT et de toutes ses possibilités. 
> NOTE IMPORTANTE: Renseignez-vous bien sur AWS car le cloud n’est jamais sûr !!
{.is-danger}

# Quelque chose ne s'est pas bien passé ?

## Instance FoundryVTT
Testez foundryvtt
```
node /foundry/resources/app/main.js --dataPath=/foundrydata 
```
Vous devriez voir apparaître ceci ou quelque chose de similaire:
```
FoundryVTT | 2020-12-06 10:56:09 | [info] Foundry Virtual Tabletop - Version 0.7.8
FoundryVTT | 2020-12-06 10:56:09 | [info] Running on Node.js - Version 14.15.1
FoundryVTT | 2020-12-06 10:56:09 | [info] Loading data from user directory - /foundrydata
FoundryVTT | 2020-12-06 10:56:09 | [info] Application Options:
{
  "port": 30000,
  "upnp": true,
  "fullscreen": false,
  "hostname": null,
  "routePrefix": null,
  "sslCert": null,
  "sslKey": null,
  "awsConfig": "******/AWS.json",
  "dataPath": "/foundrydata",
  "proxySSL": false,
  "proxyPort": null,
  "minifyStaticFiles": true,
  "updateChannel": "release",
  "language": "fr.fr-FR",
  "world": null,
  "serviceConfig": null,
  "isElectron": false,
  "isNode": true,
  "isSSL": false,
  "demo": false,
  "noupdate": false,
  "adminKey": "****************"
}
FoundryVTT | 2020-12-06 10:56:09 | [info] License verification succeeded
FoundryVTT | 2020-12-06 10:56:09 | [info] Configured AWS credentials using /home/ec2-user/foundrydata/Config/AWS.json
FoundryVTT | 2020-12-06 10:56:09 | [info] Requesting UPnP port forwarding to destination 30000
FoundryVTT | 2020-12-06 10:56:09 | [info] Server started and listening on port 30000
```
`Ctrl+C`

## Transfert du fichier zip
### Dossier /foundry/\<vide\>
*Diagnostique*

Le serveur ne se lance pas et le dossier foundry sur l'instance est vide !
Il s'agit probablement d'un problème au téléchargement du fichier *.zip

*Solution*

Vous pouvez toujours monter le fichier manuellement:
Connectez-vous en ssh à la console linux et suivez les instructions ci-dessous:

```
cd /foundry/
sudo wget -O foundry.zip \<lien google drive\>
sudo unzip foundry.zip
sudo rm foundry.zip
echo 'node /foundry/resources/app/main.js --dataPath=/foundrydata' >>/etc/rc.local
sudo chmod a+x /etc/rc.local
```
Il faudra dès lors configurer manuellement le S3

```
sudo nano /foundrydata/Config/AWS.json
```
Ajouter le texte suivant:
```
{
    "accessKeyId": "<votre clé d'accès>"
    "secretAccessKey" : "<votre clé d'accès secrète>"
    "region" : "<votre région> ex: EU (Paris) eu-west-3"
}
```
`Ctrl+X`

Ensuite il faut renseigner le fichier de configuration AWS.json dans les options de FoundryVTT

```
sudo nano /foundrydata/Config/option.json
```
Ajouter la ligne après "sslkey":null,
```
"awsConfig": "/foundrydata/Config/AWS.json",
```
`Ctrl+X`

Et redémarrer le serveur
```
sudo reboot now
```

# Notes complémentaires

## Création d'un compte AWS

Pour accéder aux différentes possibilités expliquées dans cette page, il faut au préalable créer un compte AWS.

Pour ce faire rendez vous sur https://portal.aws.amazon.com/billing/signup#/start
et suivez les instructions de création:

![prolice-screen-aws-deploy-011.png](/images/prolice-screen-aws-deploy-011.png){.align-center}
![prolice-screen-aws-deploy-012.png](/images/prolice-screen-aws-deploy-012.png){.align-center}

Vous devrez renseigner une carte de crédit même sur le niveau gratuit, la carte sera testée avec un débit de 1 USD qui vous sera crédité quelques jours plus tard.

Après la confirmation par e-mail le compte est actif et vous pourrez vous connecter.

# Note d'administration
Ceci est un tutorial vivant, n'hésitez pas à remonter les problèmes que vous rencontrez pendant votre installation/déploiement afin d'adapter le texte.
Sur la communauté [discord francophone](https://discord.gg/pPSDNJk)

---
[Prolice#9101 - 4-2-2021][en version written by Lupert]