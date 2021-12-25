---
title: Pre-Commit
summary: Installation et configuration de pre-commit
---

# Pre-Commit

**pre-commit** est un outil basé sur les **git-hooks**. Un git hook est une **action réalisée avant ou après une commande git**. Dans notre cas, pre-commit va s'appliquer avant la création du commit. Lorsque vous rentrerez la commande :

    git commit

Les git-hooks de pre-commit vont se lancer. Si jamais un git-hook **détecte** que le **code n'est pas conforme**, vous ne pourrez pas créer le commit. À l'inverse, si le code est valide, vous pourrez saisir le nom du commit et le push.

## Pré-requis et listing des règles

L'outil pre-commit **par défaut** ne contient **aucune règle**. Il faut les **choisir** en fonction du langage et de ce que l'on souhaite. Dans le cas du langage Python, les règles **PEP**, règle standardisant le langage, doivent être respectées.
De base chaque règle de pre-commit se matérialise sous la forme d'un package (**lien git**). Vous n'avez pas besoin de les installer manuellement. Ils sont tous déjà dans le fichier ***requirements-dev.txt***.

Voici la **liste** des toutes les **règles** qui seront appliquées pour **notre solution** :

- **pre-commit-hooks** ^[GIT](https://github.com/pre-commit/pre-commit-hooks)^ :
    Ces règles ne sont pas propres à un langage. C'est les règles officielles du créateur de l'outil pre-commit. Elles permettent de respecter quelques notions globales dans le monde du développement. Cependant, ici, nous utilisons pre-commit version python :
     * **trailing-whitespace** : <span style="font-size:13px">supprime les **espaces** en **fin de ligne**.</span>
     * **end-of-file-fixer** : <span style="font-size:13px">s'assure que la **dernière ligne** d'un fichier, même vide, soit une **nouvelle ligne**.</span>
     * **check-docstring-first** : <span style="font-size:13px">un fichier doit toujours comporter une **documentation en premier** et non après le code.</span>
     * **check-merge-conflict** : <span style="font-size:13px">vérifie qu'**aucun merge conflit** n'est présent dans un fichier. Détecte grâce aux balises générées par GIT lorsqu'il y'en a un.</span>
     * **debug-statements** : <span style="font-size:13px">vérifie qu'**aucune fonction de debug** n'est importée. Par exemple, si un **print** est présent, il ne sera pas possible de commit le code.</span>
     * **fix-encoding-pragma** : <span style="font-size:13px">**ajoute** ***# -*- coding: utf-8 -*-*** au début de chaque fichiers python.</span>

 - **black** ^[GIT](https://github.com/psf/black)^ : **formate le code automatiquement**. Doit être combiner avec la configuration de votre visual studio code pour une utilisation optimale. Une documentation est présente sur ce sujet dans ***"Configuration" -> "Visual Studio Code"***.

 - **flake8** ^[GIT](https://github.com/PyCQA/flake8)^ : **linter**, vérifie que les règles principales de python sont respectées. Flake8 indiquera uniquement si une règle n'est pas respecter, il ne modifiera en aucun cas le fichier. Il existe, pour lui aussi, une **extension Visual Studio Code** pour vous faciliter le travail.

 - **pydocstyle** ^[GIT](https://github.com/PyCQA/pydocstyle)^ : **vérifie** que le code python soit **bien documenté**. Un raccourci existe pour ajouter un pattern d'un commentaire sous une fonction.

 - **isort** ^[GIT](https://github.com/PyCQA/isort)^ : **réorganise** les imports python avec des règles bien **précises**.

 - **vulture** ^[GIT](https://github.com/jendrikseipp/vulture)^ : **détecte** les codes **inutilisés**.

 - **gitlint** ^[GIT](https://github.com/jorisroovers/gitlint)^ : vérifie la **conformité du message du commit**. La vérification est basé sur la convention config (**config-conventional**).

    ???+ info not-expand "Example: feat(face-parts): Récupérer les parties du visages (Task(s): #254)"

???+ info "Désactiver une règle"
    Il est possible de **désactiver** une règle. Voir leur documentation pour savoir comment faire (diffère selon les règles).

??? info "Fichier de configuration"
    C'est grâce au fichier ***.pre-commit-config.yaml*** que l'on choisit les règles à installer. De plus, chaque règles a des **paramètres** qui lui est propre. En général, ils se trouvent dans le fichier ***pyproject.toml***.

??? tip "Liste de toutes les règles existantes"
    Rendez-vous sur le site [pre-commit.com](https://pre-commit.com/hooks.html) pour voir toutes les règles/hooks possibles.

## Configuration et installation


Les fichiers de configuration de **pre-commit** sont:

- ***.pre-commit-config.yaml***
- ***setup.cfg***
- ***pyproject.toml***

### Installation

Pour **installer** localement tous les **git-hooks**, il suffit de faire ces deux commandes à la racine du projet :

    pre-commit install
    pre-commit install --hook-type commit-msg


### Mettre à jour

Pour **mettre à jour** automatiquement la **configuration de pre-commit** vers les dernières versions des **dépôts** :

    pre-commit autoupdate


Pour mettre à jour le package **pre-commit** :

    python -m pip install pre-commit -U

???+ danger not-expand "Activer l'environnement virtuel avant d'exécuter la commande : pronochain/Scripts/activate"

Si vous souhaitez remettre àjour au moment du commit, il suffit de tapez cette commande :

    pre-commit clean


## Utilisation

Une fois installés, **les hooks** se lanceront à chaque commit sur les **fichiers modifiés seulement**.
Chaque git-hook va être vérifié **avant de pouvoir créer le commit**, si un fichier modifié comporte une erreur, vous ne pourrez pas créer ce commit. Comme déjà expliquer précédemment, certains git-hook vont **modifier le code directement**, certains vont juste vous **indiquer les modifications** à faire ou les **erreurs detectées**.

??? warning "Modification par un hook"
    Si le commit est modifié par un hook avec pre-commit, il suffit de **staged à nouveau** et de **recommiter**.

??? info "Lancer pre-commit indépendamment d'un commit"
    Vous pouvez aussi utiliser **pre-commit** sans les git hooks

        pre-commit run

    lance pre-commit sur les fichiers modifiés. <br><br>

        pre-commit run --files [file]

    lance pre-commit sur les fichiers indiqués. <br><br>

        pre-commit run --all-files

    lance pre-commit sur tous les fichiers. <br><br>

        pre-commit run flake8

    lance pre-commit avec le hook indiqué (ici, flake8).
