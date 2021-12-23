---
title: Visual Studio Code
summary: Configuration de Visual Studio Code
---

# Visual Studio Code

## Extensions VSCode

Visual Studio Code permet d'**installer plusieurs extensions** afin de **simplifier le développement**. Voici une liste exhaustive de plusieurs d'entre elles pour faciliter le développement sur la solution python.

### Pre-commit

Pour que votre Visual Studo Code **affiche les erreurs** dans votre code et qu'il **formate** les **fichiers** à chaque **sauvegarde**, installez l'extension **Python**.

Cette extension va permettre de faire fonctionner le linter **flake8** et le formatter **black**. Cependant pour que Visual Studio Code applique les règles, il faut que les deux packages soient installés sur le python de votre ordinateur.

    pip install flake8
    pip install black

???+ danger not-expand "Désactiver l'environnement virtuel avant d'exécuter les commandes : deactivate"

Une fois ces deux installations effectuées, faites ++ctrl+shift+p++ puis tapez **Settings** dans la barre de recherche. Sélectionnez ***"Preferences: Open Settings (JSON)"*** (version anglaise).

Insérez ce bout de code JSON et sauvegardez.

```json
"python.linting.enabled": true,
"python.linting.flake8Enabled": true,
"python.formatting.provider": "black",
"[python]": {
  "editor.formatOnSave": true,
  "editor.insertSpaces": true,
  "editor.tabSize": 4
},
"editor.rulers": [
    88
],
```

Avec la configuration ci-dessus, Visual Studio Code **affichera les erreurs** dans votre code et **formatera le code à chaque sauvegarde**.

???+ warning not-expand "Identifier votre fichier en tant que fichier Python"

### Commentaire

Une extension Visual Studio Code existe pour **générer un template d'un commentaire**. Très utile sous les **fonctions** et les **classes**. L'extension s'appel **Python Docstring Generator**. Après l'installation positionnez vous en dessous d'une fonction et faites ++ctrl+shift+2++.

???+ info not-expand "Nous utilisons la convention de google pour générer les commentaires"
