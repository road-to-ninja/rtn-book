---
slideOptions:
  transition: slide
---

<!-- .slide: data-background="#1A237E" -->
# Better Git style

----

#### Workflow

![](https://i.imgur.com/sQqXVmN.png)

---

### Git Rules
- History
- Commits rules

---

<!-- .slide: data-background="#1A237E" -->
# 
> “La dette constitue l’écart entre l’existant et l’état de l’art des composants (code, logiciel, infrastructure ...)”

----

### Nasty :cry: 

![](https://i.imgur.com/48IqLE8.png)


----

### Linear History ! :+1: 

![](https://i.imgur.com/mK4IURp.png)



----

### Avoid Merge --no-ff

![](https://i.imgur.com/hNZADrt.gif)



----

### Rebase

![](https://i.imgur.com/hlt8M5c.gif)

----

### Merge ff

![](https://i.imgur.com/e1pZ7xH.gif)


---

<!-- .slide: data-background="#1A237E" -->
#
> “Le remboursement de la dette constitue l’investissement mis en oeuvre pour permettre au système d’être dans l’état de l’art”

----

### Rules

- [x] 72 chars title 
- [x] Body explains why and what
- [x] Imperative (add, fix, change)
- [x] English
- [x] Avoid many changes in one commit (split commits)

----

### Source

[source des règles](https://hashnode.com/post/what-tips-and-guidelines-do-you-follow-while-writing-git-commit-messages-cimorctip0010oz53hibbt5a3)

----


## Naming

Respectez ce pattern :exclamation:  `[type] contexte ou scope: sujet (#ticket)`

- __type__: Type du commit (feat, docs, style...)  
- __scope__: Sur quoi le changement s'applique, une route, un composant, un fichier, une feature
- __sujet__: Expliquer quoi et pourquoi
- __ticket__: Numéro du ticket (facultatif)

`Le #ticket est facultatif.`

----

### Types


type(contexte): le sujet, résumé 

- __feat__: nouvelle fonctionnalités pour l'utilisateur
- __fix__: bug fix
- __docs__: documentations
- __style__: css, fonts, ajout de classes html
- __typo__: format, syntaxe, eslint, orthographe
- __refactor__: refactorisation, renommmage de fichier
- __test__: ajout/modification de tests
- __chore__: tâches de build, configurations, installation de libs (package.json, gitignore ...)


----

#### Exemples

```markdown
feat(comments): add edition page component (#253)
fix(login): fix redirection after sign in (#7311)
test(route user/news): add unit test with Jasmine
style(items): change font color ...
typo(comments) js: fix eslint object spaces 
refactor(user component): rename ...  
docs(changelog): add release feature for 3.0.0
chore(gitignore): add idea ignoring files 
```

----

### Angular commits

![](https://i.imgur.com/my0yxb4.png)

---

<!-- .slide: data-background="#1A237E" -->
# Gitflow

[gitflow](https://nvie.com/posts/a-successful-git-branching-model/)

---

<!-- .slide: data-background="#1A237E" -->
Git tips
==

----

## Git rebase

- Historique propre

```shell=
git checkout cible
git rebase branche origine
```

----

<!-- .slide: data-background="#1A237E" -->
## Git stash

----

### Cas d'utilisation
- Changer de branche et sauvegarder sont travail sans pour autant commit le travail en cours


```shell=
git stash save "mon stash"
git stash apply stash@{id}
git stash show stash@{id} -p
```

[liens utiles sur stash](https://medium.freecodecamp.org/useful-tricks-you-might-not-know-about-git-stash-e8a9490f0a1a)


----

## Git amend

- Changer le titre de mon __dernier commit__
- J'ai oublié de rajouter une changement dans __le dernier commit__

```shell=
git commit --amend
ou
git add -A
git commit --amend
```

----

## Git rebase iteratif

- Rassembler des commits en 1 seul
- Editer (renommer, rajouter) un ancien commit

```shell=
git squash 
git fixup //pareil que squash met ne garde pas les noms des commits à fusioner
git edit
```

----

## Git reset --soft --hard

- Revenir sur un commit précis et ne pas perdre les changements

```shell=
git reset --soft commitId
```