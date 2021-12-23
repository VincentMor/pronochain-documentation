---
title: Variables d'envrionnements
summary: Explication et listing des variables d'environnements
---

## Explication

La génération NFT doit se **connecter** à plusieurs **services externes** et **démarrer** plusieurs **services internes**. Tous ces services doivent être **configurer pour s'y connecter, le paramétrer et le démarrer/utiliser**.
Pour ce faire, **un fichier** va permettre d'ajouter et de modifier les variables d'environnements.

 - ***.env***: toutes les variables d'environnements en commun puis les variables pour la version développement prefixé de **DEV** et les variables de la version production préfixé **PROD**.

## Ajouter une variable d'environnement
Plusieurs étapes sont nécessaires pour ajouter une variables d'environnements.

1. Modifier le fichier **.env**. Si c'est une variable utilisée en **production** et en **développement** pas besoin de préfixé le nom de la variable. Sinon préfixé le pour identifier sur quel environnement doit être appliquée la variable.

    ???+ info not-expand "Pour atteindre une variable préfixé, il suffira d'enlever le préfix."

    ???+ danger not-expand "Ne pas nommer les variables d'environnements préfixé comme les variables d'environnements communes."

2. Accéder au fichier **app/settings.py** et ajouter la variable dans la classe **GlobalSettings**.<br>
    Si c'est une variable commune :
    ```py
    VAR_COMMUNE: Optional[TYPE DE LA VAR] = Field(None, env="VAR_COMMUNE")
    ```
    Si c'est une variable préfixée :
    ```py
    VAR_SANS_LE_PREFIX: Optional[int] = None
    ```

3. **Rédémarrer le conteneur docker**.

Pour utiliser une variable d'environnement dans le code, il vous suffira d'importer la **variable settings** dans le fichier **settings.py**
```py
from app.settings import settings

debug = settings.DEBUG
```

## Listing

Cette page nous permet de faire la **liste** de toutes les variables d'environnements, d'expliquer leur utilitées et de renvoyer vers un **fichier externe** comportant **les valeurs à remplir** pour faire fonctionner la solution.

### Variables communes

 - **DEBUG** <br>
    <p style="font-size: 13px">Si **DEBUG = True** signifie que la solution démarre sur la version de développement. C'est la propriété qui permet de savoir sur quel environnement est simulée l'application.<p>

 - **API_PORT** <br>
    <p style="font-size: 13px">Port de l'API, par défaut 8000. Laisser cette valeur sauf si ce port est **déjà utilisé** sur votre système.<p>

- **API_IP** <br>
    <p style="font-size: 13px">IP l'API, par défaut 0.0.0.0.<p>

### Variables développements

***

### Variables productions

***
