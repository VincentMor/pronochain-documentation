---
title: Logging
summary: Système de log
---

# Logging

## Explication

Le système de logs en python est **intégré**. Il n'y a pas besoin de télécharger un package externe. Il permet de gérer plusieurs **méthodes de logs** avec **différents niveaux** et des détails d'erreurs très évolués.
De plus, ce système est complètement configurable. Pour notre solution, elle se trouve dans le fichier **logging_configuration** dans le dossier **app**.

### Formatter
Le formatter est un élément permettant d'indiquer les **éléments** présents dans le **message du log**. Il existe **plusieurs variables** qui récupèrent les informations souhaitées. Notre solution en définit **deux**. Une pour les logs dans les fichiers et une pour les logs dans la console en mode développement.

Format console :

```json
'simpleFormatter': {
  'format': '[%(levelname)s] - %(asctime)s - %(name)s: %(message)s'
}
```
corresponds à ce message dans la console :

```shell
[DEBUG] - 2021-12-19 11:12:09,765 - generation_nft_api - : {'Hello': 'World'}
```
<br><br>
Format fichier :

```json
'verboseFormatter': {
  'format': '[%(levelname)s] - %(asctime)s - %(name)s - (%(module)s.py | def %(funcName)s() | l. %(lineno)d): %(message)s'
}
```
corresponds à ce message dans la console :

```shell
[DEBUG] - 2021-12-19 11:12:09,765 - generation_nft_api - (main.py | def read_root() | l. 44): {'Hello': 'World'}
```

### Handler
Les handlers correspondent à tous les types de logs. Il en existe plusieurs :

- Dans la **console**.
- Dans un **fichier**.
- Dans un **mail**.
- ...

Dans notre solution, nous en utilisons deux : **fichier et console**. Cependant nous en avons **trois**, **deux pour deux** fichiers de logs concernant l'API et la génération du NFT et **un** pour la console en mode développement.

La configuration d'un handler s'effectue sous cette forme :
```json
'generationNftHandler': {
    'level': 'NOTSET',
    'formatter': 'verboseFormatter',
    'class': 'logging.FileHandler',
    'filename': 'app/logs/generation_nft.log',
    'mode': 'a',
},
```
Le **level** correspond aux informations que l'on souhaite récupérer.<br>
Pas besoin de réexpliquer le formatter.<br>
La **class** est le **type** du handler, pour voir toutes les possibilités, la **documentation python** est très complètes.<br>
Le **filename et le mode** sont liés aux faits que le handler est un **FileHandler**, il indique le chemin du fichier et le mode d'édition. Le **"a"** correspond à l'**écriture** d'un fichier **déjà existant**. Voir la documentation officielle **file system** de python pour voir tous les modes.<br>

??? info "Les différents level"
    Les levels sont **triés et ordonnés** en fonction du **niveau de détails**. Plus le level est **bas** (dans la liste), plus il est **détaillé** et comporte le niveau de détail du level du dessus.

    - **DEBUG** : **informations détaillées**, généralement utiles pour le diagnostic des problèmes.
    - **INFO** : confirmation que les choses fonctionnent comme **prévu**.
    - **WARNING** : indication que **quelque chose d'inattendu s'est produit**, ou indication d'un **problème dans un avenir proche** (par exemple, "espace disque faible"). Le logiciel fonctionne toujours comme prévu. C'est le level par défaut si aucun n'est spécifié.
    - **ERROR** : en raison d'un **problème plus grave**, le logiciel n'a **pas** été **en mesure** d'exécuter certaines **fonctions**.
    - **CRITICAL** : une **erreur sérieuse**, indiquant que le programme lui-même n'est **pas capable de continuer à fonctionner**.
    - **NOTSET** : tous les levels du dessus réunis.

### Logger

Le logger est l'élément qui sera appelé dans les fichiers pour écrire les logs ou pour catch des erreurs. Sa configuration est assez simple :
```json
'generation_nft': {
    'handlers': ['consoleHandler', 'generationNftHandler'] if is_dev else ['generationNftHandler'],
    'level': 'DEBUG',
    'propagate': False,
    'qualname': 'generation_nft'
},
```
Dans un premier temps, on spécifie les **handlers**. Si c'est la version de **développement** qui tourne, les logs seront visibles dans la console en plus des fichiers. Si c'est en version **production**, la **console est désactivée** et tous les logs iront dans les fichiers.<br>
Le **level** ici n'est **pas important**, il sera écrasé par le level du handler. Si aucun level n'était spécifié dans le handler, c'est ce level qui ferait loi.<br>
La propriété **propagate** doit être à **False**, on ne veux pas qu'une erreur **remonte les logs des fichiers parents**.<br>
Enfin **qualname**, la propriété qui indique quel **nom** indiquer lorsque l'on souhaite **appeler un logger** en particulier dans le **code**.

### Initialisation du logger
Les logs doivent être **initialisés** au **démarrage de l'application**. Dans notre cas, elle se fait dans le fichier ***app/__init__.py***.
```py
import logging
from app.settings import GlobalSettings
from logging_configuration import config
from logging.config import dictConfig

dictConfig(config(GlobalSettings().DEBUG))
logger = logging.getLogger("generation_nft")
logger_api = logging.getLogger("generation_nft_api")
```

**dictConfig** est la fonction qui permet de **reprendre la configuration** des logs et l'**ajouter** dans la **solution entière**.
Ensuite, nous définissons **deux variables** pour les **deux loggers**. Il faudra importer l'une des deux en fonction de la **localisation du fichier**. Si le fichier est **dans l'API** alors il faudra importer ***logger_api***, sinon il faudra importer ***logger***.

### Utilisation du logger
L'une des implémentations des logs **les plus simples** que j'ai eu l'occasion de voir. Lorsque vous souhaitez logger une erreur, information ou simplement une variable, il suffit d'**importer l'une des deux variables** de la partie **Initialisation**. Une fois importer, utilisé la variable pour tout type d'événement.

```py
from app import logger_api

logger_api.debug(variable)
```

??? info "Toutes les fonctions événements"
    Les différentes fonctions événements sont les différents levels possibles :

    - **debug**
    - **warning**
    - **info**
    - **error**
