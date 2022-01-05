---
title: Base de données
summary: Information sur la bdd
---

# Base de données

## Installation

Pour l'installation sur window vous pouvez installer pgAdmin [ici](https://www.pgadmin.org/download/pgadmin-4-windows/).

Pour linux vous devrez utiliser [l'image docker](https://www.pgadmin.org/download/pgadmin-4-container/).

## Structure

Vous pouveez consulter le schéma de la bdd sur [cette page](https://dbdiagram.io/d/61c0b5323205b45b73c5fa35)

## Règles métier

Cette partie détaille les règles métier non explicites sur le schéma de la bdd par notion.

### Partie de nft

Cette parite expliqe toutes **les règles métier** liées aux **parties de nft**.

#### Raretés

Toutes les parties de nft on une rareté. La somme de la fréquence des rareté ne foit pas dépaser 100. Cette règle métier n'est pas contrainte directement dans la bdd car cela pourrait poser des problème au momnent ou on voudras ajuster les fréquence de toutes les raretés.Une rareté ne peut être supprimée si elle est utilisée.

#### Couleur

Certaines parties de nft ont une couleur. Une couleur dans la bdd est representée par un id, **un code hexa valide** (un # suivis de 3 ou 6 caractère alphanumérique) et d'indiquateur boolean pour les parites de nft pouvant utliser cette couleur.
Les indiquateur boolean sont les suivant:
 * is_card_color utilisé pour vérifier si la couleur ajoutée a un CarBackground est bien une couleur de carte
 * is_hairiness_color pour vérifier si la couleur ajoutée aux Hairs,Beards,Eyebrows est bien une couleur de poils
 * is_eye_color pour vérifier si la couleur ajoutée aux yeux est bien une couleur d'oeil
 * is_skin_color pour vérifier si la coouleur ajoutée aux Necks,Faces,Ears est bien une couleur de paux
 * is_club_color pour vérifier les couleurs primaire et secondaire d'un club est bien une couleur de club. De plus la couleur primaire doit être différente de la couleur secondaire.
Ces vérifications sont faites à l'aide de trigger before insert or update qui **bloquent l'insertion si la couleur n'est pas bonne**.
Une couleur ne peut être supprimée si elle est utilisée.

#### Ecusson et t-shirt

Les ecussons et t-shirts sont stockés dans la table template. Un template a un type (écusson ou t-shirt) et une rareté. Les écusson et t-shirt reprennent la couleur pincipale du club.

### Utilisateur

Les utilisateurs sont sotckés dans la table Tipster avec un id de portefeuille métamask et un pseudo unique. Un tipster peut ajouter d'autre tipster en amis.Ces amis sont stocké dans la table TipsterFriend. Pour qu'un amis soit valide les deux identifiants de tipser doivent être différent. La réponse à l'invitation est un boolean null tant que l'autre tipster n'a pas répondu. Puis vrai au faux suivant la réponse. La limite de 3 inviation au même utilisateur ainsi que l'imposibilité de ré-inviter quelqu'un qui à déjà accepté n'est pas mise en place sur la bdd pour des raison de performance.Même si un trigger fait perdre peut de performance, sur une fonctionalité potentielement très utilisé avec des performance requise élévée ce n'est pas recommendé.
La suppression d'un utilisateur entraine la suppression de ses équipes, pronostique et amis. La supression d'une équipe entraine la suppression de tous ses joueurs ainsi que leur relation.

### Pronostique

Les pronositiques sont sotckés dans la table prédiction. La contraint un utilisatuer peut faire un seul pronositque par match est garantie par une contrainte d'unicité sur le couple id de match et id de tipster.La contraint de 11 pronostique sans résultat à la fois n'est pas mise sur la bdd pour des raisons de performance. On ne peut pas se permattre  de perdre en performance sur une des fonctionalité au coeur de notre projet.On ne peut pas supprimer un match si il est associé a un pronostique. et no ne peut pas supprimer un club si il est associé à un match ou à un pronostique.
