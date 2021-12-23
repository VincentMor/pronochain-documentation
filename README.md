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

**ATTENTION, NE JAMAIS PULL REQUEST, REBASE OU MERGE LA BRANCHE origin/gh-pages OU gh-pages SUR master OU UNE AUTRE BRANCHE !**

Lorsque vous avez **modifié** la documentation, il est impératif de **redéployer** les nouveautées. Une fois que la pull request de **votre branche à master** a été **validée et mergée** avec l'une de vos branches en local, faites ces commandes.

Installer sur votre **ordinateur** le package **mkdocs-material** (voir documentation de la solution **Génération NFT -> Initialisation** pour installer correctement Python sur votre ordinateur).

    pip install mkdocs-material


Une fois que votre **documentation est écrite**, **commiter vos changements** et **pusher votre branche**.

    git push nom-de-ta-branche

La section qui va suivre nécessite de se rendre sur le **Google DRIVE -> Pronochain -> Informations -> Identifiants -> Identifiants compte**.
Il se peut qu'une **popup GitHub** s'ouvre pour vous demander les informations de connexion pour la **première action** sur le répertoire. Pour la première popup avec le logo de GitHub visible, remplissez avec **l'email et le mot de passe**. La seconde popup : **l'email**, et la dernière : le **PAT** (**personal access token**).
Une fois votre branch push en distant, faites en sorte de mettre votre **branche sur master** grâce à la **pull request** du github (vous pouvez directement sans reviewers).
Une fois **master à jour en distant et en local**, faites:

    mkdocs gh-deploy --force

Et voilà !
