---
title: Packages
summary: Liste des packages et méthodologie pour l'ajout d'un nouveau
---

# Packages

## Liste des packages

Cette partie va **lister tous les packages** et expliquer leurs utilités. Il faut absolument **maintenir à jour** cette liste pour éviter les **packages obsolètes**, faire un suivi des **versions installées** et **informer** les autres **développeurs**.

### Packages globaux

- **fastapi** [^0.70.1^](https://fastapi.tiangolo.com/) : librairie de l'**API**.
- **gdown** [^4.2.0^](https://github.com/wkentaro/gdown) : librairie google pour **télécharger des fichiers hébergés sur le drive**.
- **mediapipe** [^0.8.9.1^](https://mediapipe.dev/) : librairie pour **récupérer 468 points d'un visage**.
- **numpy** [^1.21.4^](https://numpy.org/) : librairie pour les **calculs scientifiques** complexes notamment la **manipulation de matrice**.
- **opencv-python** [^4.5.4.60^](https://opencv.org/) : librairie de **computer vision** et de **manipulation d'image**.
- **pandas** [^1.3.5^](https://pandas.pydata.org/) : librairie de **data analyse**.
- **pytest** [^6.2.5^](https://docs.pytest.org/en/6.2.x/) : librairie **officielle** et **conseillée** par python pour effectuer les tests.
- **python-dotenv** [^0.19.2^](https://pypi.org/project/python-dotenv/) : librairie pour **ajouter les variables d'environnements** du fichier ***.env** dans des variables constantes python.
- **scikit-learn** [^1.0.1^](https://scikit-learn.org/stable/) : librairie pour faciliter la **mise en place** de **modèle de machine learning**.

??? info "Autres packages"
    Les autres packages dans le fichier ***requirements.txt*** sont des **packages natifs** à python ou des packages importés grâce aux packages cités au-dessus.

### Packages développements

- **bandit** [^1.7.1^]() : voir documentation sur [pre-commit](../configuration/pre_commit.md).
- **black** [^21.12b0^]() : voir documentation sur [pre-commit](../configuration/pre_commit.md).
- **flake8** [^4.0.1^]() : voir documentation sur [pre-commit](../configuration/pre_commit.md).
- **isort** [^5.10.1^]() : voir documentation sur [pre-commit](../configuration/pre_commit.md).
- **pre-commit** [^2.16.0^]() : voir documentation sur [pre-commit](../configuration/pre_commit.md).
- **pydocstyle** [^6.1.1^]() : voir documentation sur [pre-commit](../configuration/pre_commit.md).
- **vulture** [^2.3^]() : voir documentation sur [pre-commit](../configuration/pre_commit.md).
- **semantic-release** [^7.23.0^]() : permet de **gérer les versions**. Lorsqu'un **nouveau commit** est **détecté** par rapport à la version **précédente**. Il suffira de faire la commande ***python release.py*** pour en créer une nouvelle. Ce fichier va créer un **commit avec le tag** de la nouvelle version et remplir le fichier **CHANGELOG.md**. Il est possible de rajouter des tags comme **"-p"** pour un patch (dernier numéro), **"-mi"** (numéro du centre) ou **"-ma"** (premier numéro) pour indiquer le niveau de la version.

## Comment ajouter un nouveau package

En mode développement, vous devez installer un **nouveau package** dans l'environnement virtuel. Il faut **suivre plusieurs étapes** pour **ajouter** correctement un package et pour pouvoir l'**utiliser**.

1. **Activer** l'environnement virtuel.
    ```shell
    pronochain/Scripts/activate
    ```


2. **Installer** le package sur l'environnement virtuel. **TOUJOURS** installer avec une **version prédéfinie** (garder de côté cette version).
    ```shell
    python -m pip install nomdupackage==versiondupackage
    ```

3. **Ajouter** le package dans le fichier ***requirements.txt*** ou ***requirements-dev.txt***. Dépends de l'utilisation du module.
    ```text
    nomdupackage==versiondupackage
    ```

4. **Rebuild** le conteneur docker.
    ```shell
    docker-compose pull && docker-compose -f docker-compose.yml -f docker-compose.dev.yml up --build -d
    ```

Une fois ces étapes réalisées, vous pourrez **développer** correctement et la version **production** pourra profiter de ce nouveau package sans problème.
