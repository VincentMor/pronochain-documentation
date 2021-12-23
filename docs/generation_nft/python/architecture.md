---
title: Architecture
summary: Architecture et gestion des dépendances interne au sein du projet
---

# Architecture

## Explication

Python n'est **pas** un langage qui **préconise des architectures compliquées**. Il est possible d'appliquer tous les design pattern commun à la plupart des autres langages, cependant, dans le monde pythonic, ce n'est absolument **pas la norme**. L'architecture d'une application Python est la plus basique qui existe et se base sur le système de module et de package.<br>
Notre application est structurée en **plusieurs dossiers**, si ce dossier comporte le fichier ***\_\_init\_\_.py*** : c'est un module, sinon c'est juste un dossier lambda qui ne sera pas atteignable par des fichiers externes au dossier.

???+ info not-expand "Le fichier \_\_init\_\_.py peut être laissé vide ou comporter du code."

## Règles de nommage

Le monde du langage python est très réglementé et notamment grâce au PEP. Une règle sur les **noms des fichiers, des dossiers, des variables, des fonctions et des classes** doit être spécifiée.

 - **nom_du_dossier**
 - **nom_du_fichier.py**
 - **test_nom_du_fichier.py** : pour un fichier de test, le nom doit commencer par **"test"**.
 - **nom_variable**
 - **nom_fonction**
 - **NomClasse**

## Structure

Tous nos développements, excepté pour la partie documentation, se situera dans le module parent **app**.
Ce module est **divisé** en **deux modules** parents.
 - **generation_nft** : comporte toute la logique autour du NFT.
 - **generation_nft_api** : API et tous ce qui est lié.

Cette structure ne changera pas.

Pour vous représenter plus facilement l'architecture, le squelette ci-dessous montre les différentes parties.

```py
.
├── app
│   ├── __init__.py
│   ├── .env
│   ├── .env.example
│   ├── logging_configuration.py
│   ├── launch.py
│   ├── settings.py
│   ├── logs
│   ├── generation_nft
│   │   ├── tests
│   │   │   ├── ... (fichiers test_*.py)
│   │   │   └── __init__.py
│   │   ├── ... (listes des modules : machine learning, deep learning ...)
│   │   └── __init__.py
│   └── generation_nft_api
│       ├── tests
│       │   ├── ... (fichiers test_*.py)
│       │   └── __init__.py
│       ├── ... (listes des fichiers de routes)
│       └── __init__.py
├── dockerfiles
├── documentation (uniquement en version développement)
├── pronochain (environnement local)
├── docker-compose.dev.yml
├── docker-compose.prod.yml
├── docker-compose.yml
├── setup.cfg
├── setup.py
├── pytest.ini
├── .pyproject.toml
├── .pre-commit-config.yaml
├── requirements-dev.txt
└── requirements.txt
```

## Setuptools

**Plusieurs méthodes existent** pour importer les modules internes dans d'autres fichiers d'autres modules. Si ce sont des fichiers qui se trouvent dans le **même module** ou que le fichier importé se trouve plus bas dans le module, un simple import fonctionne.

```py
from sous_module import le_fichier
from sous_module.le_fichier import la_fonction
```

Cependant, si vous vous situez dans le fichier ***fichier_de_l_import.py*** et que vous souhaitez importer le fichier ***fichier_a_importer.py***, il vous faudra faire attention à comment l'importer.

```py
.
├── app
│   ├── __init__.py
│   ├── generation_nft
│   │   ├──  fichier_de_l_import.py
│   │   └── __init__.py
│   └── generation_nft_api
│       ├──  fichier_a_importer.py
│       └── __init__.py
└── __init__.py
```

Il y a plusieurs méthodes pour importer le fichier. Pour tous les exemples, nous nous situons dans le fichier ***fichier_de_l_import.py***.

 - **Import relatif**, consiste à simplement importer le fichier en respectant l'architecture comme pour les commandes linux.
  ```py
  from ..generation_nft_api import fichier_a_importer
  ```

 - **Import depuis un package inséré dans les chemins de python**. De base, python a une liste de chemin de dossiers qui seront **accessibles** directement par tous les fichiers **peu importe où vous vous situez**. Pour ajouter un dossier en particulier, il faut **exécuter du code** dans un fichier. Cette méthode est la plus utilisée dans le monde de python et permet d'avoir le **contrôle total** sur ce qui est accessible ou non. Nous n'utiliserons pas cette méthode, car nous souhaitons que tous les modules soient accessibles.

 - **Import grâce à Setuptools**. Setuptools est un package permettant de créer nous-mêmes nos propres packages. Lorsque vous voulez utiliser une librairie, il suffit de simplement d'importer le nom de ce package et ce que vous souhaitez. Setuptools fait en sorte de **créer un package** pour chaque dossier qui comporte un fichier ***\_\_init\_\_.py***. Il suffira par la suite d'importer depuis le module le plus haut.
  ```py
  from app.generation_nft_api import fichier_a_importer
  ```

Comme vous l'aurez compris, nous utilisons **Setuptools**. Vous n'avez absolument aucune manipulation à faire pour importer un nouveau module mise à part ajouter le fichier ***\_\_init\_\_.py*** dans le dossier.
