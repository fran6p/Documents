En préliminaire, ce document n'est que la description de ma méthode de travail pour préparer un firmware basé sur Marlin. D'autres méthodes existent, l'important est de trouver celle qui vous sied le plus.

Ce tutoriel ne s'adresse pas dans un premier temps à de purs débutants. Toutefois, en acceptant de s'y mettre et en faisant vos propres essais / erreurs (principe de tout apprentissage) la réussite peut être en vue (la courbe d'apprentissage n'est pas extrêmement pentue mais la route qui y mène n'est pas aussi droite comme l'avait judicieusement remarqué J.-P. Raffarin en son temps).

# AU COMMENCEMENT

Quand j’ai commencé à préparer mes propres firmwares pour mes imprimantes, je reproduisais la méthode décrite dans bon nombre de tutoriels (écrits ou vidéos) : je récupérais le fichier compressé de la version de Marlin sur laquelle je me basais pour faire mes modifications (platformio.ini, configuration.h et configuration_adv.h. 

![image](./images/GD/github-code-zip.jpg)

Si parmi les [exemples de modèles fournis par l’équipe du Marlin](https://github.com/MarlinFirmware/Marlin/tree/2.0.x/config) existait l’imprimante à préparer, la tâche était facilitée :

1. copier les configuration.h et configuration_adv.h (et éventuellement s’ils étaient également fournis, _Statusscreen.h et _Bootscreen.h) puis les coller dans le dossier Marlin en remplacement de ceux par défaut.
2. ensuite l’étape de compilation
  * correction des erreurs,
  * retour à l’étape 2 (compilation)
3. finalement la compilation étant réussie alors dernière étape, flasher le firmware.

Si une nouvelle version stable de Marlin était disponible, il fallait répéter les étapes précédentes depuis la récupération du Marlin compressé jusqu’au flashage.

La gestion des différences entre les fichiers de configurations pouvait être facilitée par l’utilisation de [Winmerge](https://winmerge.org/) (ou [Notepad++](https://notepad-plus-plus.org/) et l’ajout d’[un greffon](https://github.com/pnedev/compare-plugin) ou encore avec [Visual Studio Code](https://code.visualstudio.com/) ).

S’il fallait préparer des versions différentes de firmware (modèle d’imprimante différent, variation de carte mère, présence d’un ABL, type d’ABL, détection de fin de filament, …) alors il était nécessaire d’organiser les dossiers, de les nommer correctement sinon le troisième élément de la trilogie : **hardware, software … foutoir** ne tardait pas à surgir (loi de Murphy oblige).

En gros c’était pour le moins fastidieux et nécessitait une bonne organisation 😉 De plus ça finissait par occuper de la place sur l’unité de stockage.

En résumé :

![image](./images/GD/marlin-zip.jpg)

## Un outil de gestion de versions : GIT

C’est là que j’ai découvert qu’on pouvait «bénéficier» d’outils de gestion de versions : [Git](https://git-scm.com/) (son créateur n’est autre que Linus Torvald également connu pour avoir au milieu des années 1990 développé le système Linux).

Pour aller plus loin dans l’utilisation de GIT :

- https://www.mcorbin.fr/posts/2020-06-26-git-introduction/

- https://rogerdudler.github.io/git-guide/index.fr.html

- [un cours assez complet](https://www.pierre-giraud.com/git-github-apprendre-cours/)

- [la cheat sheet](https://training.github.com/downloads/fr/github-git-cheat-sheet/) [(PDF)](https://training.github.com/downloads/fr/github-git-cheat-sheet.pdf)

Git est pratique pour travailler localement mais pour pouvoir diffuser, modifier et faire bénéficier d’autres utilisateurs de ce travail, un système distant comme [github](https://github.com/) augmente les possibilités.

Utiliser github.com (*ou gitlab si Microsoft vous hérisse le poil*) nécessite la création d’un compte uniquement si vous voulez créer vos propres dépôts.

[Créer son compte Github](https://github.com/pricing) (gratuit) en cliquant sur «Create a free orgaization» : 

![image](./images/GD/creer-compte-github.jpg)

Un [formulaire](https://github.com/account/organizations/new?plan=free&ref_cta=Create%2520an%2520organization&ref_loc=pricing%2520comparison&ref_page=%2Fpricing) s’affiche qu’il suffit de compléter : 

![image](./images/GD/formulaire-creation-compte-github.jpg)

Ne pas oublier la [phase de vérification de l’email](https://docs.github.com/en/get-started/signing-up-for-github/verifying-your-email-address) saisi lors de la création du compte.

Quelques termes à connaître, le [glossaire github](https://docs.github.com/en/get-started/quickstart/github-glossary) (en anglais)

Une fois le compte créé et validé, on peut créer ses propres dépôts directement en ligne ou en local sur son ordinateur (ligne de commandes).

Cependant un logiciel très pratique permet de faciliter les différentes tâches : [Github Desktop](https://desktop.github.com/) (raccourci en GD pour la suite) interface en anglais uniquement et pas de version pour les linuxiens 😞 .

Télécharger puis installer GD via le lien précédent. Ce logiciel utilisera vos identifiants (pseudo / mot de passe) pour se souvenir de qui vous êtes, ils vous seront demandés à la fin de l’installation.

Pour une documentation malheureusement uniquement en anglais, consulter [ce lien](https://docs.github.com/en/desktop).

## Ma méthode de travail avec Marlin

En ligne, sur le dépôt github de Marlin, en étant connecté avec votre compte, on va réaliser un «fork» du projet originel (Marlin) (=copie préservant les liens vers le dépôt originel): aller sur https://github.com/MarlinFirmware/Marlin

Cliquer sur le bouton FORK : 

![image](./images/GD/fork-marlin-github.jpg) 

 Vous venez de créer une copie identique du dépôt Marlin dans votre compte github précédemment créé

![image](./images/GD/marlin-fork.jpg) 

Votre dépôt distant contient une copie exacte y compris toutes les branches (variantes) du Marlin originel. Désormais, le dépôt Marlin originel sera dénommé «***UPSTREAM***», la version de votre dépôt, elle, sera appelée «***ORIGIN***».

![image](./images/GD/marlin-upstream-origin.jpg) 

**Le fork pour le moment n’est présent que dans notre dépôt Github**. Pour réaliser les modifications des fichiers de configuration de Marlin, je trouve plus pratique de travailler en local sur son propre ordinateur. On va donc récupérer le contenu de notre dépôt distant.

![image](./images/GD/marlin-clone-local.jpg) 

Ayant pris depuis longtemps l’habitude d’organiser et regrouper mes dossiers / répertoires sur mes disques durs (ça facilite les sauvegardes 😉 )  
=> création d’un nouveau répertoire sur une unité de stockage pour accueillir nos futurs «*développements*» ( C:\GITHUB par exemple ) 

### Première option :

Via le site github.com de notre dépôt (notre compte), clic sur le bouton «**Code**» puis => *Open with Github Desktop*

![image](./images/GD/github-code-GD.jpg) 

### Seconde option :

Via GD, menu principal, «*Clone repository…*»

![image](./images/GD/GitHubDesktop_clone-repo.jpg) 

Saisir l’URL du dépôt à cloner (notre «ORIGIN») dans l’onglet URL, modifier éventuellement le chemin d’accès (Local path) pour correspondre au lieu de stockage prévu puis clic «Clone»

![image](./images/GD/GitHubDesktop_clone-repo-URL.jpg) 

Une barre de progression signale le transfert en cours. A la fin du processus de copie en local, on nous demande comment on souhaite contribuer au développement du dépôt cloné (contribuer au projet parent ou pour son propre usage). Si par exemple on pense proposer des corrections / modifications au projet originel (pull request), il est préférable d’indiquer que l’on contribuera au projet parent.

Que ce soit avec la première option ou la seconde, toutes les branches du dépôt cloné sont maintenant accessibles sur notre matériel local (origin/x,x,x). 

![image](./images/GD/marlin-cloné-localement.jpg) 

Désormais, Github Desktop nous permet d’accéder à notre dépôt distant:
* View on Github [1]),
* à notre emplacement local (Show in Explorer [2]),
* et également d’utiliser notre éditeur de code (VSC) en cliquant (Open in visual Studio Code [3]) ( *il est évidemment possible d’indiquer un autre éditeur que VSC via les préférences du logiciel ou en cliquant «options»* (4) ).

![image](./images/GD/GitHubDesktop_interface-acces.jpg) 

## Création d’une nouvelle branche locale

Je souhaite créer un firmware basé sur la version stable de Marlin pour une nouvelle imprimante :

* Choix de la branche (version) qui servira de base (Current branch)

![image](./images/GD/GitHubDesktop_nouvelle-branche.jpg) 

* nommer cette branche puis clic sur «Create branch»

![image](./images/GD/GitHubDesktop_nouvelle-branche-nom.jpg) 

La branche est désormais créée sur notre disque **localement** :

![image](./images/GD/GitHubDesktop_nouvelle-branche-créée-locale.jpg) 

On peut maintenant via notre éditeur (VSC) faire toutes les modifications manuellement dans les fichiers ***configuration.h*** et ***configuration_adv.h*** ou quand le modèle d’imprimante dispose de fichiers exemples fournis par l’équipe de Marlin, recopier les fichiers nécessaires en remplacement de ceux du clonage.

Les modifications seront automatiquement détectées dans GD 😉

* détection des changements dans GD

![image](./images/GD/GD-modifications-detectees.jpg) 

* le contenu des modifications est précisé (history) avec un codage couleur (rouge / vert) correspondant à avant / après modifications :

![image](./images/GD/GD-VSC-modifications-detectees-1.jpg) 

Pour le moment, toutes ces modifications n’existent qu’au niveau local, il reste à synchroniser avec github => Publish branch 

![image](./images/GD/GitHubDesktop_nouvelle-branche-créée.jpg) 

![image](./images/GD/marlin-push-github.jpg) 

L’intérêt de la publication de cette branche sur github est :

* rendre accessible notre dépôt à d’autres utilisateurs en leur donnant le lien vers celui-ci et vers une branche particulière
* en cas de problème matériel en local (*expérience vécue lors de la perte d’un disque dur* ), il suffira de cloner localement notre dépôt distant pour récupérer tous nos développements

![image](./images/GD/marlin-perte-DD-reclone.jpg) 

## Maintenir à jour la version de Marlin 

Marlin est régulièrement mis à jour (bugfixes (plusieurs fois par jour) et stable (moins souvent)). ***On peut tenir notre dépôt local également à jour en suivant les modifications du dépôt originel (UPSTREAM) et en les répercutant dans nos branches.***

### Réalisations par étapes : Upstream => local

1. Dans GD, choix de la branche à mettre à jour :

* via le menu, onglet «*Branch*» => *Merge into current branch*

![image](./images/GD/gd-menu-branch-merge-into-current-branch.jpg) 

* depuis la branche sélectionnée, *Choose a branch to merge into branche-sélectionnée*

![image](./images/GD/current-branch-choose-a-branch-to-merge-into-2.0.x.jpg) 

* indiquer la version «**Upstream**» qui servira à fusionner : 

![image](./images/GD/branche-a-jour.jpg) 

Dans l’exemple ci-dessus, la version de Marlin étant la version stable, il n’y pas eu de changements (message : *this branch is up to date with upstream/2.0.x*)

Exemple avec la version «bugfixes» dont les mises à jour sont quotidiennes (plusieurs fois par jour parfois) :

![image](./images/GD/branche-non-a-jour.jpg)

Le nombre de modifications est précisé, il suffit de cliquer le bouton «***Create a merge commit***» pour fusionner ces changements dans notre branche locale (***Origin***). La majorité du temps il n’y a pas de conflits.

### CAS lors de «conflits» 

De temps en temps, la fusion (merge) ne peut se faire automatiquement seule. Github Desktop nous indique alors le nombre de fichiers pour lesquels existent un/des conflit(s) :

![image](./images/GD/conflit-détecté-lors-merge.jpg) 

En cliquant sur le bouton «***Merge upstream/… into Branch***», une nouvelle fenêtre nous précise quels fichiers sont concernés :

![image](./images/GD/résoudre-les-conflits-via-VSC.jpg) 

En cliquant sur le bouton permettant d’accéder à l’éditeur «***Open in Visual Studio Code***», le (les) conflits seront signalés, **un choix devra lors être réalisé par l'utilisateur** :

![image](./images/GD/exemple-conflit-merge.jpg) 

On pourra soit :

* ne pas mettre à jour notre branche avec les modifications de la nouvelle version (***Accept Current Change***)

* mettre à jour notre branche (***Accept Incoming Change***)

* accepter les deux dans notre fichier pour pouvoir les comparer (***Accept Both Changes***). *Il faudra ensuite faire les modifications manuellement en évitant de réaliser des doublons évidemment*.

Une fois le (les) conflit(s) résolu(s), **ne pas oublier d’enregistrer le fichier modifié dans VSC** (clic sur l’icone «disquette»)

![image](./images/GD/VSC-enregistrer-tout.jpg) 

GD détectera notre choix et continuera la fusion (merge). Nos versions de Marlin seront alors à jour. Ne restera plus qu’à compiler notre nouveau firmware «*up to date*» puis à le flasher sur la carte de l’imprimante. Sans oublier, évidemment de tester son fonctionnement 😉 

Voilà, on arrive au terme de ce long document qui relate ma manière de travailler quand je propose des firmwares aux utilisateurs de ce [forum](https://www.lesimprimantes3d.fr/forum/). D’autres méthodes, procédures existent ; à vous de trouver celles avec lesquelles vous avez le plus d’affinités.

L'idée de ce document provient d'une vidéo du Youtubeur australien [TeachingTech](https://www.youtube.com/c/TeachingTech).  
N'étant pas un adepte des vidéos pour un apprentissage, j'ai donc créé cette alternative au format écrit, qui plus est en langue française.

🙂