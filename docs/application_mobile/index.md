---
title: Accueil - initialisation flutter
summary: Accueil de la documentation docker
---

# Accueil

## Application mobile

L'application mobile sera disponible sur **Android et IOS**. Cette application permettra de faire toutes les functionnalités spécifié dans le cahier des charges.

- [Getting Started](#getting-started)<br>
  **1** . [Installation de** Docker Engine ** ](#first)<br>
  **2** . [Clonage de la solution ](#second)<br>
  **3** . [Installation et démarrage du conteneur docker](#tree)

***

## Getting Started

Pour pouvoir commencer à développer dans les meilleures conditions, plusieurs installations sont nécessaires. Cette documentation est réalisée pour **Windows**,  **Linux** ou tous autre systemes d'exploitation supportant docker. Pour les systemes d'exploitation ne suportant pas docker il vous faudra suivre des tutoriels en ligne.

***

### <a id="first"></a><a style="font-size: 24px;">**1**</a>. **Installation** de **Docker Engine**
Rendez-vous sur le site officiel de docker pour télécharger la derniere version : 
https://docs.docker.com/engine/install/

Effectué ensuite l'installation en suivant les différents écrans.


### <a id="second"></a><a style="font-size: 24px;">**2**</a>. **Clonage** de la solution
Une fois docker installé et pret à l'emploie, nous allons récupérer notre solution en ligne à l'aide de la commande : 
git clone "lien_du_repository"

Une fois notre solution en local nous allons nous rendre dans le dossier racine du projet. On peu voir un fichier Dockerfile c'est grace à lui que nous allons générer notre environement de développement.

***

### <a id="tree"></a><a style="font-size: 24px;">**3**</a>. **Installation et démarrage** du **conteneur docker**

Un conteneur docker a été configuré pour vous faciliter le développement, mais aussi le déploiement. Une fois le conteneur docker lancé en mode développement.

Ouvez la solution dans **Visual Studio Code** et télécharger l'extension **Docker**.

Cliquez ensuite sur l'icone en bas de votre ecran Visual Studio Code :

![Variable path](../pictures/application_mobile/docker.png)

Puis selectionnez Open Folder in Container.
Sélectionnez le répertoire racine qui contient le Dockerfile

Pour les fois suivante il vous faudrat selectionné Reopen in container afin de ne pas recréer un container.

Vous voila pret !


