---
title: 02 - Modules
description: Description de la configuration des modules (Midi-QOL) 
published: true
date: 2021-02-24T08:22:04.739Z
tags: modules, dnd5e, module
editor: markdown
dateCreated: 2021-02-23T18:26:12.302Z
---

# Midi-QOL Config

Midi-QOL est à la base un module d&#39;améliorations de la « Qualité de Vie » (Quality Of Life) ayant évolué vers un module permettant une automatisation poussée des combats en mettant en place des workflows pour les tâches d&#39;«administration » du MJ.

Cette page explique les différents paramètres de Midi-QOL pour l&#39;automatisation des combats.

La partie API de Midi-QOL n&#39;est pas abordée ici. Des compléments seront ajoutés (plus tard) pour décrire les autres fonctionnalités (Concentration, Trasnfert d'effets aux cibles).

## L&#39;installation de Midi-QOL

Midi-QOL s&#39;installe vie le gestionnaire des Add-ons dans Foundry. Il a comme dépendances les modules « Dynamic Active Effects using Active Effects » (recommandé) et libwrapper (quasi obligatoire).

> **Note Importante : l&#39;utilisation de midi-QOL en concurrence avec d&#39;autres modules influant sur les jets de dés (Better Rolls, MARS 5e) n&#39;est absolument pas recommandée**
> {.is-info}
{.is-warning}



## Options de Midi-QOL

### Généralités

La configuration des options du module se fait dans « Configure Settings ». Le bouton « Workflow Settings » permet de paramétrer finement les options des workflows de combat.

La plupart des paramètres sont fixés par le MJ (user Gamemaster) mais certaines options sont configurables par chacun des joueurs.

> Note : dans les images de configuration ci-dessous, le module « DF Clarity Settings » a été utilisé. Il ajoute des icônes devant chaque paramètre pour plus de clarté :  « Terre » (indique un paramètre uniquement définissable par le MJ) ou « Personne » (indique un paramètre que chaque joueur peut définir de façon personnnelle) .
{.is-info}

A noter que les paramètres s&#39;appliquent à tous les objets. Il est impossible de les paramétrer objet par objet. Il faudra donc paramétrer un worfklow qui convient à 90%-95% de votre façon de jouer et gérer les cas particuliers manuellement.

#### Workflow de Combat

En simplifiant, à chaque utilisation d&#39;un objet qui possède une action de combat (en gros les armes, les sorts, les features, les équipements qui ont des Actions d&#39;attaque...), Midi-QOL va déclencher un workflow qui va dérouler et valider les étapes plus ou moins automatiquement en fonction de la configuration.

Par exemple, un workflow typique d&#39;une action d&#39;attaque (avec une arme) effectuée par un joueur sera :

1. Le joueur choisit ses cibles
2. Le joueur fait son jet d&#39;attaque en incluant les bonus et effets adéquats (bonus de maîtrise, avantage, désavantage)
3. Le MJ vérifie que l&#39;attaque touche (pour un jet d&#39;attaque c&#39;est un jet opposé à la CA de l&#39;adversaire)
4. Si l&#39;attaque réussit, le joueur lance les dégâts en appliquant les effets et bonus adéquats (critiques, capacités de classes par exemple)
5. Le MJ déduit les dégâts des PV des monstres en appliquant les effets adéquats (vulnérabilités, immunités, résistances…)

Dans le workflow ci-dessus, hormis l&#39;étape 1 (le choix des cibles), tout est automatisable par Midi-QOL.

#### Le « Fast Forward »

Le Fast Forward est une notion assez courante (dans Foundry dnd5) qui permet d&#39;accélérer certaines actions en contournant des dialogues (choix du type de jet -avantagé, désavantagé, normal- ; choix des dégâts – normal, critique), soit en utilisant des options par défaut (normal pour les jets d&#39;attaque ou de dégâts), soit en récupérant les raccourcis clavier utilisés (en standard : Alt pour avantage, Ctrl pour désavantage, MAJ pour versatile).
Quand le « Fast Forward » n&#39;est pas activé, Foundry VTT affichera un dialogue de choix sur chaque jet.

#### Les guérisons

Les guérisons sont gérées comme des attaques rendant des PV plutôt que faisant des dégâts. Elles suivent donc un workflow similaire.

### Options Standards
</br>

    #### INSERER IMAGE modules-qol-settings-gen1.png
    #### INSERER IMAGE modules-qol-settings_gen2.png

Ce sont toutes les options de Midi-QOL. Pour l&#39;automatisation des attaques, seules les deux premières sont intéressantes.

Workflow settings ouvre une nouvelle page de configuration détaillées des options et «  Enable roll automation support » doit être cochée (sinon aucun workflow n&#39;est déclenché). A noter que cette option est configurable par les joueurs qui peuvent donc désactiver cette option pour leur personnage.

### Options de Workflow

#### Pour le MJ

</br>

    #### INSERER IMAGE modules-qol-settings-gm1.png

Ces options définissent le fonctionnement des workflows pour les attaques effectuées par le MJ.


| Paramètre | Valeurs possibles | Commentaires |
|:---|:---:|:---|
| Auto-Roll Attack | Oui / Non | **Oui**  : Lorsque l&#39;on clique sur un item qui possède une attaque, le workflow d&#39;attaque est déclenché </br> **Non**  : Lorsque l&#39;on clique sur un item, sa « chat card » s&#39;affiche avec les boutons qui permettent de déclencher les workflows |
| Auto Fast forward attack | Oui / Non | *Cf la description du Fast Forward* <br /> **Oui**  : utilise une attaque normale (par défaut) ou selon les raccourcis claviers (Alt / Ctrl) </br> **Non** : le dialogue du type d&#39;attaque s&#39;affiche. |
| Auto Roll Damage | Never</br>Always</br>Attack Hits | Cette option permet de déterminer si le jet de dégâts est lancé automatiquement. </br>**Never**  : Le jet de dégâts n&#39;est pas lancé automatiquement </br> **Always** : le jet de dégâts est toujours lancé en même temps que le jet d&#39;attaque (quel que soit le résultat de l&#39;attaque)</br> **Attack Hits**  : le jet de dégâts est lancé uniquement si l&#39;attaque touche sa cible (voir les options « Hits » dans le troisième onglet de configuration).</br></br> *`Note : si aucune cible n'est sélectionnée, Always Hits et Attack Hit sont équivalents (les jets de dégâts sont effectués)`* |
| Auto Fast Forward damage | Oui / Non | *Cf la description du Fast Forward* </br>**Oui**  : utilise les dégâts normaux (par défaut) ou selon les raccourcis claviers (Alt / Ctrl / MAJ) </br>**Non**  : le dialogue du choix du type de dégâts s&#39;affiche. |
| Remove Chat card buttons after roll | Off</br>Attack Only</br>Damage Only</br>Attack and Damage | Cette option indique quels boutons sont laissés dans la chat card après la complétion du workflow d&#39;attaque. Cela permet de relancer des attaques ou de relancer des dégâts quand cela est nécessaire. Toutefois en fonction de certaines autres options (notamment le jet de dégât automatique), il est possible que le workflow se termine automatiquement et que ces boutons ne soient plus utilisables.</br>**Off**  : tous les boutons restent dans la chat card</br>**Attack Only**  : les boutons d&#39;attaque sont enlevés (on ne peut pas relancer l&#39;attaque depuis la chat card)</br>**Damage Only**  : les boutons de dégâts sont enlevés (on ne peut pas relancer les dégâts depuis la chat card) **Attack &amp; Damage**  : la chat card n&#39;affiche aucun des boutons à la fin du workflow. |
| Hide Roll Details | None</br>Roll Formula</br>Show Attack d20</br>Entire Roll | Permet de masquer aux joueurs (le MJ voit toujours tout) le détail des jets pour éviter que les joueurs ne déduisent les caractéristiques des monstres/PNJ en fonction des jets et des annonces de touche du MJ. </br> **None**  : tout est affiché (jet d&#39;attaque, de dégâts et formule de l&#39;attaque) </br> **Roll Formula**  : les formules d&#39;attaque et de dégâts sont masquées (remplacées par ????) dans les chat card des joueurs **Show Attack d20**  : seul le dé est visible. Les formules d&#39;attaque et de dégâts sont masquées ainsi que les lancers de dés de dégâts </br> **Entire Roll**  : tout est masqué aux joueurs (formules , lancer d&#39;attaque, lancer de dégâts).. </br></br>  *`Remarque : cette option a un effet étrange -mais probablement explicable- sur les jets de dés 3D effectués avec Dice So Nice ! notamment certains jets de dégâts ne sont pas rendus côté MJ (même si l'option ne devrait masquer les résultats qu'aux joueurs)`*

#### Pour les Joueurs
</br>
    
    #### INSERER IMAGE modules-qol-settings-play1.png

Ces options sont identiques à celles disponibles pour le MJ mais concernent les jets effectués par les joueurs. La seule différence notable est qu&#39;il y a une liste déroulante pour la configuration du Fast Forward, plutôt que 2 cases à cocher (mais on retrouve bien les 4 choix).

</br>
</br>

##### Autres Options de worklow

</br>

###### Targeting
</br>

    #### INSERER IMAGE modules-qol-settings-targ1.png

Ces options permettent de définir le comportement vis-à-vis des cibles avant une attaque mais est surtout utile pour cibler avec les gabarits (de sorts généralement).

| Paramètre | Valeurs possibles | Commentaires |
| :--- | :---: | :--- |
| Auto target on template draw | None</br>Always</br>Walls Block | Cette option définit le comportement de ciblage quand un gabarit est placé (suite à l&#39;utilisation d&#39;un sort ou en posant directement le gabarit) </br> **None**  : aucune cible </br>**Always**  : tous les tokens dans le gabarit sont ciblés </br> **Walls Block**  : tous les tokens dans le gabarit sont ciblé mais le gabarit ne « traverse » pas les murs </br> _**Note** : cette option ne fait pas de différence entre tokens amis/ennemis/neutres_ |
| Auto Target for ranged spells/attacks | Oui / Non | Cette option définit le comportement de ciblage pour les sorts sans gabarit mais avec une portée centrée sur la cible (généralement définie comme portée (xx + unité) + Ennemi / Créature / Allié). Elle ne concerne pas les sorts à gabarit (cône, cube, cylindre, rayon, sphère, carré, mur, etc..) </br></br>**Oui** : tous les tokens du type correct situés dans la zone de portée sont ciblés en respectant leur faction (Ami/ Hostile) |
| Requires targets to be selected before rolling | Oui / Non | **Oui** : vérifie qu&#39;une (au moins) cible est bien sélectionnée avant le déclenchement d&#39;une attaque sur des cibles. Dans le cas contraire, une notification est affichée et l&#39;attaque n&#39;est pas lancée.

###### Speciales
</br>

    #### INSERER IMAGE modules-qol-settings-spe1.png
    
Cette section définit des options diverses.

| Paramètre | Valeurs possibles | Commentaires |
| --- | :---: | --- |
| Add Macro to call on Use | Oui / Non | Permet d&#39;ajouter sur chaque fiche d&#39;objet un champ qui peut contenir un nom de macro. Cette macro sera lancée après la résolution de l&#39;item (attaque ou effet).Le champ est ajouté à la toute fin de la fiche de l&#39;objet. |
| Enable Concentration Automation | Oui / Non | Permet (avec les modules CUB et DAE) l&#39;automatisation de la Concentration pour les jeteurs de sorts.</br>_A détailler plus tard_ |
| Single Concentration Check | Oui / Non | Fonctionne avec l&#39;option ci-dessus : </br> **Oui** : si une même source de dégâts a plusieurs jets de dégâts, Midi-QOL effectuera un seul jet de concentration avec un DC correspondant au total des dégâts reçus. </br> **Non** : effectue un jet de concentration différent par jet de dégâts d&#39;une même source de dégâts </br></br> _Par exemple, avec une épée qui fait 1d8 + 1d8[Feu] et ayant lancé 5+7 </br>Oui : effectue un jet de concentration DC 12</br>Non : effectue 2 jets de concentration DC 10 chacun_
| Auto Apply item effects to targets | Oui / Non | Cette fonction est essentielle pour l&#39;automatisation des effets de sorts (autres que dégâts ou guérisons). Elle permet de transférer à la cible, les effets présents sur l&#39;item en cas d&#39;attaque réussie (ou d&#39;un jet de sauvegarde raté).</br>_A détailler plus tard_. |

###### Toucher
</br>

    #### INSERER IMAGE modules-qol-settings-hit1.png
    
Cette option définit le contrôle des touches lors d&#39;une attaque.

| Paramètre | Valeurs possibles | Commentaires |
| --- | :---: | :--- |
| Auto check if attacks hits target | None</br>Check – All See Result</br>Check – Only GM Sees | Cette option détermine si Midi-QOL effectue automatiquement le contrôle de toucher (contre la CA) dans le cadre d&#39;un workflow d&#39;attaque.</br>**Non**  : aucun contrôle n&#39;est effectué. Dans ce cas, Midi-QOL considère que l&#39;attaque touche et continue le workflow. </br>**Check**  : le contrôle est effectuée et Midi-QOL poursuit le workflow si au moins une cible est touchée </br>**All See Result**  : tous les joueurs voient le résultat du contrôle </br>**Only GM sees**  : seul le MJ voit le résultat du contrôle |

###### Sauvegardes
</br>

    #### INSERER IMAGE modules-qol-settings-save1.png
    
Ces options déterminent le fonctionnement des attaques à Jet de Sauvegarde et sont donc plutôt applicables aux sorts.

| Paramètre | Valeurs possibles | Commentaires |
| :--- | :---: | :--- |
| Auto check Saves | Off</br>Save – All See result</br>Save – Only GM Sees</br>Save – All See Result + Roll| Cette option détermine si Midi-QOL effectue automatiquement le jet de sauvegarde (contre ce qui est défini dans l&#39;action) dans le cadre d&#39;un workflow</br>**Off**  : aucun jet de sauvegarde n&#39;est effectué. Dans ce cas, Midi-QOL considère que toutes les cibles ont raté leur jet de sauvegarde. </br>**Save**  : le jet de sauvegarde est effectuée et Midi-QOL poursuit le workflow. </br>**All See Result**  : tous les joueurs voient les résultats des jets de sauvegarde </br>**Only GM sees**  : seul le MJ voit les résultats des jets de sauvegarde </br>**All See Result + Roll**  : tous les joueurs voient les résultats et les lancers |
| Display Saving Throw DC | Oui / Non | **Oui**  : masque aux joueurs le niveau de difficulté (DC) du jet de sauvegarde </br>**Non**  : le niveau de difficulté est affiché |
| Search Spell Description | Oui / Non | **Oui**  : recherche la description du sort pour trouver des occurrences de « half as much » pour savoir si un sort fait ½ dégâts (la description contient « half as much ») ou aucun dégât (la description ne contient pas « half as much ») en cas de réussite du jet de sauvegarde. </br>**Non**  : ne fait pas la recherche et considère qu&#39;un jet de sauvegarde réussi entraîne ½ dégâts |
| Prompt Players to roll Saves | None</br>Let Me Roll That for You</br>LMRTFY + Query</br>Chat Message| Définit comment les jets de sauvegardes à faire sont communiqués au joueur. </br>**None**  : le MJ devra demander manuellement aux joueurs de les faire </br>**LMRTFY**  : utilise le module Let Me Roll That For You (LMRTFY) pour envoyer une requête aux joueurs </br>**Query / Chat Message**  : envoie un Chat Message aux joueurs pour leur demander d&#39;effectuer les jets de sauvegarde |
| Prompt GM to Roll Saves | Auto</br>Let Me Roll That for You | **Auto**  : Midi-QOL fait les jets de sauvegarde automatiquement pour les NPC gérés par le MJ </br>**Let Me Roll That for You**  : utilise le module LMRTFY pour envoyer une requête de jet de sauvegarde au MJ. |
| Delay before rolling for players | Numérique (X) | Délai en secondes avant que le jet de sauvegarde ne soit automatiquement lancé par le système en cas de demande de jet de sauvegarde non répondue par les joueurs (après X secondes) |

###### Dégâts
</br>

    #### INSERER IMAGE modules-qol-settings-dmg1.png

Ces options gèrent la façon dont sont appliqués les dégâts

| Paramètre | Valeurs possibles | Commentaires |
| --- | :---: | --- |
| Auto Apply Damage to Target | Auto Apply : Oui / Non</br>Damage Card : Oui / Non | Cette option détermine si Midi-QOL applique automatiquement les dégâts aux cibles et s&#39;il affiche les icônes d&#39;application des dégâts.</br>**Oui**  : Midi-QOL applique automatiquement les dégâts aux cibles </br>**Non**  : Midi-QOL n&#39;applique pas automatiquement les dégâts</br></br>**Damage Card**  : définit si les icônes d&#39;application des dégâts d&#39;affichent. </br>**Oui**  : une « damage card » par cible s&#39;affiche avec des icônes permettant d&#39;appliquer ou d&#39;annuler les dégâts (Cf la damage card di dessous) </br>**Non**  : aucune damage card ne s&#39;affiche
| Apply Damage Immunities | Never</br>Apply Immunities</br>Apply Immunities + Physical | Cette option permet d&#39;appliquer les vulnérabilités, résistances, immunités des cibles lors de la détermination des dégâts.</br>**Never**  : aucun contrôle n&#39;est fait (le MJ gère à la main) </br>**Apply Immunities**  : applique les résistances / immunités / vulnérabilités aux dégâts en fonction des traits de la cible et des modificateurs de l&#39;attaque. </br>_A vérifier : __**Apply Immunities + Physical**__ : même chose que ci-dessus + vérifie les immunités Physiques. ??_  |
| Roll Other formula on failed save for rwak/mwak | Oui / Non | **Oui**  : utilise la formule définie dans le champ « Autre formule » en cas de jet de sauvegarde raté de la cible pour les attaques d&#39;armes à distance (rwak) ou les attaques d&#39;armes de mêlée (mwak). Si les options « ½ Damage on Save », noDamSave et fullDamSave sont activées, ces options sont prises en compte en priorité. |

###### Options Diverses
</br>

    #### INSERER IMAGE modules-qol-settings-misc1.png

Ces options permettent principalement de fixer le comportement des « chat cards » affichées lors de l&#39;utilisation des objets.

| Paramètre | Valeurs possibles | Commentaires |
| --- | --- | --- |
| Show Item details in Chat car |None</br>Card Only</br>Card + Details : PC Only</br>Card / Details : NPC + PC | **None / Card Only**  : semblent faire la même chose. Seuls le nom de l&#39;item et son icône sont affichés. </br>**Card + Details**  : affiche la carte de l&#39;item et sa description pour les capacités des PC et/ou des NPC.</br></br> _Le détail peut être utile au MJ pour avoir un rappel rapide des capacités déclenchées par les joueurs._ |
| Merge Rolls to one card | Oui / Non | **Oui**  : crée une chat card condensée avec les informations attaque, touchers, dégâts, sauvegardes au même endroit</br> **Non**  : utilise les chat cards standards |
| Condense Attack/Damage Rolls | Oui / Non | **Oui**  : permet de mettre les informations des formules/jets d&#39;attaque et formules/jets de dégâts sur la même ligne (pour condenser la chat card)</br> **Non**  : utilise le format standard (attaque et dégâts sur des lignes différentes= |
| Chat Cards use token name | Oui / Non | **Oui**  : le nom du token est utilisé dans la chat card (propriétaires et cibles) </br>**Non**  : le nom de l&#39;acteur est utilisé dans la chat card (propriétaires et cibles) |
| Enable Speed Item Rolls | Oui / Non+ détails | **Oui**  : utilise les raccourcis standard pour les fast rolls (Alt, Ctrl, MAJ) </br>**Non**  : permet de redéfinit les raccourcis pour les fast forward rolls et de les appliquer également aux jet de sauvegarde / compétences / attributs |
| Enable midi-qol custom sounds | Oui / Non + détails | **Non**  : pas de son spécifique joué lors des événements </br>**Oui**  : permet de définir des sons à jouer sur certains événements</br></br>**Sounds Playlist** : définit la playlist où aller chercher les sons.</br>Les autres options permettent de choisir un son spécifique dans la playlist. |

## Comment utiliser concrètement Midi-QOL

Cette section détaille des configurations possibles de midi-QOL pour aboutir à différents niveaux d&#39;automatisation (avec leurs avantages et leurs inconvénients).

### Niveau d&#39;automatisation Moyen+, MJ « Secret »

Dans cette configuration, la plupart des jets sont automatisés et très peu d&#39;informations sont communiquées aux joueurs.

**Avantages**  : permet de limiter les manipulations et les contrôles à effectuer par le MJ durant les combats. La validation finale du retrait/ajout de PV est quand même faite par le MJ (ça évite de faire des annulations quand un joueur se trompe de cible et le MJ garde la main sur les PV de ses monstres).

**Inconvénients**  : certaines options rendent difficile l&#39;utilisation des dégâts « versatiles » ou « Other Damage ». Le MJ est également obligé de valider les guérisons.
</br>

    #### INSERER IMAGE modules-qol-conf1-1.png

Automatisation complète côté MJ (auto roll et fast forward des jets d&#39;attaque et de dégâts). Les résultats des jets sont masqués aux joueurs.

    #### INSERER IMAGE modules-qol-conf1-2.png

Automatisation poussée. Les jets de dégâts sont toujours effectués pour rendre le combat un peu plus rapide (le MJ n&#39;a plus besoin d&#39;annoncer « Tu peux lancer les dégâts ») tout en gardant un peu de contrôle par le MJ (c&#39;est lui qui annonce si l&#39;attaque a touché plutôt que le système).
</br>

    #### INSERER IMAGE modules-qol-conf1-3.png

    #### INSERER IMAGE modules-qol-conf1-4.png

Les choses importantes :

Requires targets to be selected before rolling : pour éviter que les joueurs ne lancent des attaques « dans le vide »

Hits / Saves Check – Only GM Sees : accélère la gestion des combats en faisant automatiquement la résolution des touchers / sauvegardes.

Auto Apply damage to target : No + damage card : le MJ garde la main sur l&#39;application des dégâts, mais 80% du temps il suffira juste d&#39;appuyer sur « Tout mettre à jour ».
</br>

    #### INSERER IMAGE modules-qol-conf1-5.png

Le MJ connaît les capacités de ses monstres par cœur donc les détails ne sont pas affichés. Il ne connaît pas forcément les capacités de tous les joueurs donc un rappel est bienvenu.
 Les chat cards sont condensées au maximum.

Les raccourcis Fast Forward sont activés y compris sur les sauvegardes/jets de compétences/aptitudes. Les raccourcis sont standards (Alt = Avantage, etc…).