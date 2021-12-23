---
title: Règles
summary: Règles et nomenclature de python
---

# Règles et nomenclature

Cette page énumère les **différentes règles** autour du langage python et fournie **plusieurs informations supplémentaires** afin de vous faciliter le développement. Tous les commentaires sont à rédiger en **Français**.

## Commentaire

Vous devez ajouter des commentaires à différents endroits :

- **Au début du fichier** : ajouté le **template Pronochain** accessible depuis les fichiers déjà créés et remplacer le chemin du fichier par le nouveau.
- **Après une classe** : petite phrase pour expliquer à quoi correspond la classe.

        """ [summary]. """

- **Après une fonction** : dois expliquer à quoi elle sert, les paramètres, les erreurs et ce qu'elle retourne.

        """[summary]

            Args:
                parameter (int): [description]

            Raises:
                Exception: [description]

            Returns:
                str: [description]
        """
- **Pour du code** : positionner le commentaire sur **la ligne du code** ou **au-dessus** en **minuscule** sans **"."** à la fin.

    ```py
    var test = "hello" # [commentaire]
    ```

    ```py
    # [commentaire]
    if test == "hello":
        test = "world"
    ```

???+ tip not-expand "Pour créer le template plus rapidement, positionnez-vous à l'endroit où vous souhaitez mettre un commentaire et faites ++ctrl+shift+2++."

???+ warning not-expand "Ne pas oublier le . (period en anglais) pour les premières lignes de commentaires"


## Type hinting

Par défaut, python **n'oblige en aucun cas** de typer les **variables** ni les **fonctions**. Dans les dernières versions de python, la fonctionnalité **type hinting** a été ajoutée et permet de spécifier le type. Néanmoins, il n'impose **aucune obligation**. Si vous stipulez que le paramètre est un **string** mais que vous lui passez un **int,** python ne **détectera pas l'incohérence**.

Le type hinting est surtout utile pour la **documentation** et concerne généralement les **fonctions**.

### Exemple

```py
def test(parametre: int, parametre_deux: int = 0) -> str:
```
Deux paramètres de type **int** dont un paramètre a une valeur par défaut **0**. La fonction quant à elle retourne un **str**.

??? info "Types"
    Les types python sont très génériques : **str, int, bool**. Vous pouvez aussi spécifier une **classe**.

??? info "Plusieurs types pour une même notion"
    Si le paramètre ou la valeur de retour peut retourner plusieurs possibilités. Il faudra importer **Union** du module **typing**.
    ```py
    from typing import Union

    def test(parametre: Union[int, str], parametre_deux: Union[int, str] = 0) -> Union[str, bool]:
    ```

## Particularités nomenclature

### Fichiers

Certains fichiers, correspondant à des fonctionnalités bien précises, sont réglementés par des **nomenclatures**. Tous les noms des fichiers sont en **minuscule** et en snake case : **snake_case**.

- **Fichier de test** : commence par **test_**.
- **Fichier pour l'API et les routes** : commence par le **nom de l'élément** et fini par **_controller**.

### Nom des routes

Le nom des routes doit commencer par la méthode : **get, post, put, delete** et indiquer l'**action de la route** avec le **nom de l'élément**.

```py
@app.get("/team/{team_id}")
def get_team_by_id(team_id: int) -> dict:
```

### Nom des variables, fonctions et classes

Le nom des variables et des fonctions doivent être en **snake_case**.
Le nom des classes en **PascalCase**.
