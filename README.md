# Documentation

Toute notre documentation sera réalisé sur ce **répertoire GIT**. La technologie utilisée nous permet d'avoir une **documentation hébergée** sur une **image docker** et accessible via **localhost:9090**. Cette technologie utilise essentiellement du **markdown** comme pour le WIKI d'Azure DevOPS ou le .README.

Pour information, cette documentation sera hébergée **en ligne**, il ne sera **pas nécessaire** de démarrer l'image docker si vous souhaitez **uniquement la consulter**. L'étape qui suit va vous permettre de démarrer la documentation afin de pouvoir **créer/modifier** des pages.

---

# Initialisation

## Variables d'environnements

**Copier coller** le fichier ***.env.example*** et supprimer le ***.example***. Vérifier que le port de la documentation soit en **9090**, si ce n'est pas le cas, modifié le. Si jamais le port est **déjà utilisé** par une autre image, vous pouvez le changer.

<br>

## Installation/démarrage

L'installation et le démarrage se résument en une étape, une commande.

Rendez-vous à la **racine de la solution** de la solution et tapez la commande dans une **console** (**powershell** de préférence si vous êtes sous Windows) :

    docker-compose up --build -d

Cette commande va **build l'image** et **démarrer la documentation**. Si vous souhaitez seulement la démarrer, car vous l'avez déjà build :

    docker-compose up -d

---

# Comment l'utiliser ?

Une **documentation** a été faite sur la **documentation** (WOUAH). Pour y accéder, il faut bien évidemment **démarrer l'image** de la documentation et se rendre dans la catégorie ***Global/Documentation***.

---

# Déployer les nouveautés

Lorsque vous avez **modifié** la documentation, il est impératif de **redéployer** les nouveautées. Une fois que la pull request de **votre branche à master** a été **validée et mergée**, faites ces commandes.

Pour se faire rendez-vous sur la branche **gh-pages**.

    git checkout gh-pages

Si vous ne l'avez pas faites :

    git pull

Ensuite faites :

    git rebase master

Puis:

    docker run --rm -it -v ~/.ssh:/root/.ssh -v ${PWD}:/docs squidfunk/mkdocs-material gh-deploy

Cette commande va vous demandez l'**email du github** et le **PTA** (personal access token). Rendez-vous sur le **google drive** pour récupérer les **identifiants**.
Une fois que la commande est exécutée, le site va automatiquement se mettre à jour.
