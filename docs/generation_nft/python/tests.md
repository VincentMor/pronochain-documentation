---
title: Tests
summary: Création et vérification des tests
---

# Tests

## Création d'un test

Sous python, il existe plusieurs packages pour créer des tests. Le plus connue et surtout le plus simple d'utilisation est **pytest**.

La **convention** pour créer un test est d'ajouter un fichier **commençant** par le mot **test_**. Cette convention va permettre à **pytest** d'exécuter le test créé **sans avoir à l'importer** dans un fichier.

??? info "Configuration de pytest"
    L'automatisation de l'exécution des tests et notamment la convention de nommage s'effectue grâce au fichier ***pytest.ini***. Consultez-le si cela vous intéresse.

En ce qui concerne le **nom de la fonction** de test, elle doit aussi commencer par **test** (règle interne). Ensuite, vous pouvez appeler les différentes fonctions a tester depuis le fichier de test.

???+ warning "Ne pas utiliser assert"
    **assert** est déconseillé pour des raisons de sécurité. Ce problème de sécurité sera détecté par bandit et vous empêchera de faire un commit.
    ```py
    if not (response.status_code == 200):
        raise AssertionError("Le statut code n'est pas correct.")
    ```
    En plus d'être plus sécurisée, cette syntaxe permet d'être beaucoup plus précise au moment où le test échoue.

## Exemple
```py
def test_read_root():
    """Test read root route."""
    response = client.get("/")
    if not (response.status_code == 200):
        raise AssertionError("Le status code n'est pas correct.")
    if not (response.json() == {"Hello": "World"}):
        raise AssertionError("La réponse ne contient pas les bonnes données.")
```

## Tester votre test
Pour démarrer votre test, rien de plus simple. Vous pouvez simplement **faire la commande** à la racine du projet :

    python -m pytest

Cette commande est très utile, mais si le nombre de tests devient **trop volumineux**, il peut être long d'attendre que votre test passe.

Si vous souhaitez tester uniquement le vôtre :

    python -m pytest app\generation_nft_api\tests\test_main.py

Le désavantage de cette commande est de **connaître obligatoirement le chemin du fichier**.
