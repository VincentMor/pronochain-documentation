---
title: Détection d'un visage
summary: Détection d'un visage sur une image
---

# Détection du visage

## Définition

La **détection d'un visage** est très répandue dans le **monde du machine learning**. Il existe plusieurs types de modèles de machine learning afin de détecter un visage sur une image. Lors de cette documentation, nous verrons la **liste des modèles** en énumérant leurs avatnages et leurs inconvénients afin d'en choisir un.

<div style="display:flex;flex-direction:row;justify-content:space-around;margin-top:8px;margin-bottom:10px;">
    <div style="display:flex;flex-direction:column;">
        <div style="display:flex;flex-direction:row;align-items:center; justify-content:center;margin-bottom:20px;">
            <img src="../../../pictures/generation_nft/face_detect/error.png" style="width:24px;height:24px;margin-right: 2px;">
            <h3 style="margin:0 0 0 6px !important;">Pas de visage</h3>
        </div>
        <img src="../../../pictures/generation_nft/face_detect/paysage.jpg" style="height:200px;">
    </div>
    <div style="display:flex; flex-direction: column;">
        <div style="display:flex;flex-direction:row;align-items:center; justify-content:center;margin-bottom:20px;">
            <img src="../../../pictures/generation_nft/face_detect/check.png" style="width:24px;margin-right:2px;">
            <h3 style="margin:0 0 0 6px !important;">VISAGE</h3>
        </div>
        <img src="../../../pictures/generation_nft/face_detect/kilian.png" style="height:200px;">
    </div>
</div>

---

## Contexte

Le but est de pouvoir générer une **image originale tirée d'un visage d'un joueur de foot**. La problématique était de détecter un visage. Il existe plusieurs modèles pour détecter un visage. Ce qui a été le plus compliqué, c'est de choisir le modèle. Pour information, les critères pour le choix d'un modèle ont été : **la précision et la performance**. Il faut que le modèle soit très **précis**, c'est-à-dire qu'il **ne détecte pas** des visages quand il n'y en a pas et **vice-versa**. De plus, il faut que la détection du visage soit **rapide**. Cette rapidité dépend notamment du **matériel informatique** qui est utilisé pour faire tourner l'algorithme. Dans les cas d'algorithme sur des images, donc du graphisme, c'est la **carte graphique** qui sera la plus puissante : **le GPU**. Cependant, ce système doit être activé pour certains packages comme **OpenCV**.

??? info "Carte graphique et compatibilité"
    Toutes les **cartes graphiques** ne sont **pas compatibles** pour pouvoir réaliser ce genre de tâche. Il est donc préférable que la détection du visage se fasse sur le**processeur** : **le CPU**.

??? tip "OpenCV et Dlib"
    Petit rappel pour la suite, **OpenCV** est une librairie python. Elle est très utilisée dans la **manipulation d'image**. La librairie **Dlib** est similaire, mais est plus tournée vers le machine learning.

## Les différents modèles

- [OpenCV et Haar cascades](#haar-cascades)
- [Deep learning de détection de visager intégré à la librairie OpenCV](#opencv-face-detector-integre)
- [Dlib HOG + Linear SVM implementation](#dlib-hog-linear-svm)
- [Dlib CNN détection de visage](#dlib-cnn-face-detector)

### Haar cascades

L'**haar cascades** est un algorithme permettant de trouver **plusieurs caractéristiques** pour une image. Il ne détecte pas uniquement les visages. Il se base notamment sur le modèle de machine learning **Adaboost**. Le principe est plutôt simple, il a été **entrainé** avec plusieurs catégories d'image, avec pour **chaque image**, une **description**. Il aura ensuite la faculté de prédire des objets dans l'image grâce aux **caractéristiques** similaires aux **images d'entraînements**.

**<a style="font-size: 20px;">Pour :</a>**

 - Très **rapide**.
 - **Pas besoin** de le faire tourner sur un **GPU**.

**<a style="font-size: 20px;">Contre :</a>**

 - **Haut pourcentage de faux négatif**, en d'autres termes, une **précision médiocre**.
 - Dois être **configuré** pour qu'il fonctionne correctement (correctement signifie médiocre).

<br>

### OpenCV face detector intégré

La détection du visage **intégré** dans la librairie d'**OpenCV** utilise le système de **SSD** (**Single Shot Detector**). Comme son nom l'indique, en un seul essai, il peut **détecter plusieurs objets**. Il ne détecte pas seulement les visages, mais peut détecter une **multitude d'objets** sur la même image en une seule fois. Il est plus compliqué d'expliquer le fonctionnement de cet algorithme, si vous voulez avoir plus d'information rendez-vous sur ce **[tutoriel](https://towardsdatascience.com/ssd-single-shot-detector-for-object-detection-using-multibox-1818603644ca)**.

**<a style="font-size: 20px;">Pour :</a>**

- **Efficace**.
- Utilise un modèle de **deep learning récent**.
- **Pas de paramétrage**.
- **Pas besoin** de faire tourner l'algorithme sur un **GPU**. Mais sera tout de même plus rapide dessus.

**<a style="font-size: 20px;">Contre :</a>**

- **Plus efficace** que le modèle **Haar cascades**, mais reste encore **assez peu précis**.
- Possibilité de **biais** et d'**erreurs** à cause des couleurs des images.

<br>

### Dlib HOG + Linear SVM

HOG signifie **Histograms of Oriented Gradients** et **linear SVM** est le modèle de machine learning utilisé pour cette méthode de détection de visage. Le concept HOG est plus compliqué à comprendre que le linear SVM. Pour ces deux concepts, je vous conseille d'aller voir ce **[tutoriel](https://medium.com/@mithi/vehicles-tracking-with-hog-and-linear-svm-c9f27eaf521a)**.

**<a style="font-size: 20px;">Pour :</a>**

- **Plus précis** que le modèle **Haar cascades**.
- Beaucoup plus **stable** que les deux modèles d'avant.
- **Facile d'utilisation** et continue d'être mise à jour par son créateur grâce à la librairie **Dlib**.
- **Extrêmement bien documenté**.

**<a style="font-size: 20px;">Contre :</a>**

- Fonctionne uniquement sur les **visages frontales** (**nous arrange grandement**).
- Pas aussi précis que d'autres modèles basés sur le deep learning (**reste assez négligeable, se focus sur les visages de face**).
- Assez **peu coûteux** en termes de performance, mais reste un peu plus long que les deux modèles précédant. **De l'ordre de la milliseconde de plus** (0.001 seconde).

<br>

### Dlib CNN face detector

CNN signifie **Convolutional Neural Network**, comme son nom l'indique, il utilise un système de **réseau neuronal** pour détecter le visage. Dans le monde de l'intelligence artificielle, voici l'ordre des performances : **machine learning < deep learning < réseau neuronal**. La détection sera très précise. Cependant, les réseaux neuronaux sont très **gourmands**.

**<a style="font-size: 20px;">Pour :</a>**

- **Très précis** pour détecter les visages.
- Taille du modèle (fichier) **inférieur à 1MB**.
- **Facile** à **implémenter** et très **documenté**.

**<a style="font-size: 20px;">Contre :</a>**

- Le code est **plus verbeux** et nécessite de **multiples de conversions** de variables.
- Très **lent** sur CPU.

<br>

## Choix

Il a été décidé de prendre le modèle **HOG + linear SVM**. Plusieurs raisons à ce choix :

- Il ne détecte pas les **visages de profil**. Ce qui nous facilite le machine learning : **tilt learning** ([voir la documentation](./tilt_learning.md)).
- Il est **rapide** sur **CPU**.
- Il est **plus précis** que les **deux autres modèles** fonctionnant sur CPU.
- **Facile d'implémentation** avec beaucoup de **documentation**.
- Enfin, il est encore **maintenu**.

## Utilisation du modèle

Comme indiqué dans les différentes raisons du choix du modèle, il est très facile d'implémenter le modèle **HOG + linear SVM**. Cela signifie entre autres, qu'en très **peu de ligne de code**, le modèle va **détecter un visage** sur une image et retourner les **coordonnées** de celui-ci afin de pouvoir **l'isoler** du reste de l'image.

Il faut tout d'abord avoir le fichier du modèle. Vous le trouverez dans le chemin *"app/generation_nft/librairies/face_detect/pre_trained"*.

??? info "Téléchargement du fichier"
    Le fichier n'est **pas** dans le **répertoire GIT**. Une vérification au premier lancement est réalisée pour détecter la présence du modèle. Si ce n'est pas le cas, le script le **télécharge**. Ce modèle est **hébergé** sur notre **Google Drive**.

Puis l'importer dans le code :
```py
detector = open_cv.dnn.readNetFromCaffe(caffe_prototxt_path, caffe_model_path)
```

Pour pouvoir utiliser ce modèle, il faut impérativement **convertir l'image en blob**, pour ce faire, la librairie **OpenCV** comporte une fonction pour le faire simplement.

```py
image = open_cv.imread(image_path) # Récupère l'image est le converti en matrice numpy
(height_image, width_image) = image.shape[:2]
blob_image = open_cv.dnn.blobFromImage(
    open_cv.resize(image, (width_image, height_image)),
    1.0,
    (width_image, height_image),
    (104.0, 177.0, 123.0),
)
```

- **1.0** correspond à un **changement d'échelle**. Ici, aucune modification.
- **(104.0, 177.0, 123.0)** correspond à une **colorisation de l'image**.

Il ne reste plus qu'à **intégrer le blob** dans le modèle de détection et de récupérer les **coordonnées** du/des **visages détectés**.
```python
detector.setInput(blob_image)
face_detections = detector.forward()
```

Par la suite, avec chaque coordonnée, on pourra **isoler une partie de l'image**. Bien sûr, il faudra prendre une **marge** car la détection du visage se focalise sur **les éléments du visage et non la tête entière**.

<div style="display:flex;justify-content:center;margin-bottom:10px;height:300px;">
    <img src="../../../pictures/generation_nft/face_detect/face-detect.png">
</div>

??? info "Calcul de la marge"
    Pour être sûr d'avoir la **totalité de la tête**, la marge est calculée en **fonction de la taille du visage détecté**.
