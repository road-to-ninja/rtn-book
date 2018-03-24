Git
===

## Definition 
Git est un outils qui permet de gérer les version de ses fichiers, de façon distribué et centralisé.

Dans la plus part des cas,il est utilisé dans le développement (code), mais il peut aussi être utilisé pour la gestion de tout autre type de fichier comme, les images, excels, pptx etc.

Quelques notions
===
Avant d'aller plus loin, il est peut être intéressant de s'intéresser aux termes "centralisé" et "distrbué" et surtout comment ces termes s'appliquent sur git.

### Centralisé
Dans une architecture centralisé, la liste de fichiers qui forment la version principal (ou stable) de votre projet, se trouve dans un serveur central. Pour que les contributeurs puissent travailler sur ce même projet, ils doivent faire une ==copie== de ce projet, cette copie est ensuite stocké dans l'ordinateur du contributeur. 

Une fois que le contributeur a terminé ses changements/modifications, il doit envoyer sont ses modifications vers le serveur central, où se trouve le version principal. L'envoie(commit) se fait à travers une connexion internet.



![](https://snipcademy.com/code/img/tutorials/git/introduction/cvs.svg)


### Distribué
Dans une version distribué. Si une personne veut contribuer au projet. Elle va faire un =="clone"== et non pas une =="copie"== de la version principal. Le clone se trouvera dans l' ordinateur du contributeur. 

Ainsi, on peut dire que chaque personne possède la ==vrai version== de la version stable dans son répositoire local. Lorsque le contributeur  poussera (push) un changement, il n'aura pas besoin d'internet. Puisque la version principal existe aussi dans son ordinateur.
Donc en théorie on peut travailler sur son ordinateur et avoir une gestion de version de son projet sans connexion internet. 

## Git Centralisé ET Distribué
Git est centralisé car on va stocker notre version définitive ou stable dans un service cloud. Comme Github ou Gitlab ou encore BitBucket. Pour que le projet puisse être accéssible par les collaborateurs.

Distribué, car on peut travailler en local, avec notre Local Repository,sans avoir la connexion internet. A la fin de chaque release on devra tout de même envoyer(push) notre travail et le mélanger avec la version principal, qui se trouve dans le serveur cloud (Central Repository).

**Pour représenter le fonctionnement voici un schéma**

![](http://i.stack.imgur.com/tRfgX.png)

Introduction
====

## Avatanges
Git permet de mieux gérer :
1. Les backups/sauvegardes (clone)
2. Développement de features et patch (merge/branching)
3. Historiques,logs (commits/logs)
4. Déploiement en production (heroku etc)
## Comment ça marche


![](https://cdn-images-1.medium.com/max/1600/1*u_4bmAEBDr0jAaW5h_6y-g.png)

Dans votre **répertoire local**, git est divisé en 3 parties :
 
* Le working directory est votre répertoire physique.
* Staging area est l'endroit de transition, les fichiers qui se trouvent dedans sont suivis par git.

* Local Repo, est le repo git, tout les fichiers/changements qui se trouvent dans la partie stage seront copies vers cette partie. Le Local repo garde les "snapshot" (capture) de votre projet à un moment X, on peut les appeler des versions. 

Le **Remote(distant) repository** est en général votre repo central qui se trouve dans un service cloud.

Started
===

## Etapes
- Création d'un repo local git sur le dossier du projet souhaité
 ```shell=
    git init
 ```
- Paramètre username et email, pour identifier chaque commit. Pas en global (recommandé)
- Ajouter les fichiers sur le repo git
- Sauvegarder une version du projet

Basics commandes
====

Toutes les commandes sont composées de la façon suivante : git nom_de_commande, exemple 
```
git init, git add, git commit ...
```


:::info
* Toutes les commandes sont rentrées sur le terminal/cmd
* Chaque commande s'effectue dans le répertoire courant de votre projet.
:::



## Création d'un repo git / config

Il existe deux façon de créer un repo. Soit en clonant un projet (il aura déjà un repo git à l'intérieur), soit en le créant de zero. 
Ci-dessous les deux façon de faire.

### Création de zéro

Création d'un repo git. C'est où on enregistre les changements/versions. Ce répertoire est nommé ".git". Cette commande est à lancer dans le répertoire du projet qu'on veut gérer avec git.

```git==
git init
```

### Cloner un repo

S'il existe déjà un repo existant dans un serveur et qu'on veut démarrer à partir de celui-ci. Il faut le cloner. Ainsi on aura cette version du repo, dans notre repo local (notre machine).

Il suffit de préciser l'URL du remote repo. Ici monProjet est stocké dans le serveur github sur mon compte.

```git==
git clone https://github.com/MonCompte/monProjet
```

### Configuration du compte

Afin d'être identifié sur chaque action qu'on va faire sur le repo git. Il nous faut déclarer notre nom et adresse mail au minimum. Le prefixe "--global" est utilisé pour ne pas avoir à préciser nos informations à chaque création d'un répo.

```git==
git config --global user.name 'monNom'
git config --global user.email 'monadresse@gmail.com'
git config --global color.ui 'auto'
```

Pour la configuration pour 1 seul projet donnée, il suffit d'enlever le paramètre --global. 


## Gestion de suivie et de l'état 
Pour ajouter vos fichiers dans la partie "staging" du repo git. Cette partie nous sert à suivre un fichier et à stocker les fichiers/changements qui vont être ajoutées dans le prochain commit.

### Ajouter/Mise à jour dans le stage(index)

Donc si on vient de créer un fichier ce fichier sera dis "untracked" (non suivie).
Pour l'ajouter dans notre stage :
```git==
git add nomFichier
```

Une fois que votre fichier a été ajoutée. Il est suivie. Si vous faites des changements sur celui-ci,et que vous désirez l'ajouter dans le stage. Vous pouvez aussi utiliser la même commande.

Si on souhaite mettre à jour (update) tous nos fichiers qui sont suivis :
```git==
git add -u
```

Pour ajouter tous les fichier qui sont untrack(non suivis) et tracked(suivis) dans l’index (stage)
```git==
git add -A
```
### Affichier l'état du repo git 
```
git status
```
Cette commande va nous dire quels fichiers ne sont pas suivies ou si changements ont été effectuées et qu'il faudra soit les commit ou les ajouter dans le stage.

Pour voir la liste des commits que vous avez fait, tapez :
```git==
git log
```

### Difference entre deux commit
On fait la different entre deux id de commits, ici HEAD c'est le dernier et HEAD~1 l'avant dernier.

```git==
git diff HEAD..HEAD~1
```

## Gestion d'une version du projet

### Commit des changements
Lorsque vous voulez garder une trace de votre projet à un moment précis. Vous devez effectuer un commit. Cette action va copier tous vos changement qui se trouvent dans la partie "stage(index)" vers le HEAD(repo).
```
git commit -m "mon message de changement"
```
On accompagne notre commit d'un message pour préciser ce qu'on a fait dans cette version du projet.


### Revenir en arrière après un commit

Si on souhaite revenir à un moment X de notre projet. Par exemple au commit précedent
On doit préciser le HEAD ou alors le SHA1 (c'est le identifiant d'un commit).
```
git reset --hard HEAD~1
```
ou 
```
git reset --soft HEAD~1
```
Ici on ajout "~1" au HEAD (actuel),pour revenir au avant dernier commit (N-1).

Explication du soft et hard plus tard.


### Reprendre une version d'un fichier 
Si vous êtes entrain de mofidier un fichier et que vous vous apercevez que le changement est faux, si vous voulez reprendre la copie de ce fichier qui se trouve dans votre dernier commit.

Vous pouvez faire :
```git==
git checkout nomDuFichier
```

## L'Ignore 
Si par exemple vous avez un fichier de configuration. Qui ne doit être partagé avec les autres collaborateurs.

On peut ignorer un fichiers/un type de fichiers/dossier etc. 
Pour cela il faut d'abord ajouter un fichier qu'on nomme .gitignore, dans notre répértoire.

```
touch .gitignore
```
Ensuite on peut modifier le contenu de ce fichier pour créer vos pattern(s). Pour plus d'information, regardez la section Liens sympa/GitIgnore.

Branching et merging 
===

### Intro
Git a un fonctionnement par branches. Lors ce que vous crée votre premier repo git. Vous êtes sur la branche principal de votre projet. C'est en quelque sort la version principal de votre projet. On appel cette branche "master".

Vous avez la possibilité de créer d'autres branches qui seront des copies d'un version X de votre branche principal(master). Une branche peut être crée pour ne pas altérer et commit sur une version stable.

Un exemple d'utilisation est lors ce que vous avez des nouvelles fonctionnalités ou bugs à développer, on peut créer une branche dans ce genre de cas. Puis ensuite on pourra fusioner cette branche secondaire(s) avec la principal (qui est appelé "master").


![](https://blog.seibert-media.net/wp-content/uploads/2015/07/Git-Branches-1.png)

### Commandes

|commande|description|
|-|-|
|git checkout branche|deplacement vers une autre branch|
|git checkout -b nomBranche|crée et se déplace vers cette branche|
|git branch nom| crée une branche|
|git branch|liste toutes les branches|
|git branch -d nomBranche|Supprime une branche|

Lors que vous avez terminé de debuger ou votre feature. Pour fusioner la branche à votre master. Vous devez vous déplacer vers votre branche master et taper la commande :
```
git merge nomDeLaBrancheSecondaire
```


Les repo distants
===

Lors qu'on travail avec un repo remote (distant). Les changements sont envoyés avec la commande push et lorsqu'on veut ramener/récuperer les changements de ce repo qu'on appelle "origin", on utilise un pull.


|Commande|définition|
|-|-|
|git push||
|git fetch||
|git pull||
|git remote add name url||


## Merge Rebase

Dans github quand on séléctionne 'merge' pour une pull request cela crée automatiquement un commit de merge, même si le commit de la branche (feature) est directement liée au head du master



## Liens sympa
Essentiels : http://rogerdudler.github.io/git-guide/
Demo/Practice : https://www.sitepoint.com/git-for-beginners/
GitIgnore : https://www.atlassian.com/git/tutorials/gitignore
Présentation complète : http://sbecker.github.io/intro_to_git/
Distruted and Centralized : https://www.quora.com/What-is-the-difference-between-distributed-revision-control-and-centralized-revision-control-Give-examples-and-how-can-I-use-a-distributed-control-system-locally-for-my-own-projects/answer/Peter-Hosey?srid=37q11
