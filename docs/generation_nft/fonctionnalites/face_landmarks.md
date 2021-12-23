---
title: Masques du visage
summary: Masques du visage correspondant à 468 points importants.
---

# Masques du visage

## Définition

Le **face landmarks**, en français, points de repère du visage, est un algorithme permettant **de récupérer plusieurs points (468) d'un visage**. Cet algorithme intervient juste après la détection du visage et une nouvelle fois après la rotation du visage.

<div style="display:flex;justify-content:space-around;margin-bottom:10px;height:300px;">
    <img src="../../../pictures/generation_nft/face_landmarks/face-1.png">
    <img src="../../../pictures/generation_nft/face_landmarks/face-2.png">
</div>

---

## Contexte

Le but est de pouvoir générer une **image originale tirée d'un visage d'un joueur de foot**. La problématique était de détecter un visage. Grâce à plusieurs packages externes : **OpenCV**.<br>
Outre le fait de détecter un visage, il fallait obligatoirement **récupérer les éléments du visage** afin de pouvoir réaliser un **dessin fidèle**. Pour ce faire, il existe **plusieurs librairies** plus ou moins précises. Cependant, contrairement à la détection du visage, le but est uniquement de reprendre le **plus de points possibles** pour reproduire fidèlement le visage. La librairie **MediaPipe** est l'une des meilleures dans ce domaine, elle permet d'avoir **468 points** et est **extrêmement simple et précise**.


## Détecter des landmarks

Le code pour pouvoir récupérer les points tient en **4 lignes**. Il faut tout simplement importer la fonction de MediaPipe appelée **face_mesh**. Elle prend en paramètre une image. Il est préférable de **coloriser l'image en nuances de gris** afin que la fonction soit la plus performantes possible.

```py
import cv2 as open_cv
from mediapipe.python.solutions import face_mesh as mediapipe_fm,

results = face_mesh.process(
    open_cv.cvtColor(image, open_cv.COLOR_BGR2RGB)
)
face_landmarks = results.multi_face_landmarks[0].landmark # LISTE DES POINTS
```

**face_landmarks** va comporter une liste de plusieurs **points triés dans un ordre très précis**.

```json
[
    {
        "x": "0.14771",
        "y": "0.48782",
        "z": "0.1563"
    }
]
```

L'ordre correspond aux **numéros de chaque point**. Pour voir en quelle position se trouve la pointe du nez ou d'autres parties, il suffit de regarder cette image.

<div style="display:flex;justify-content:center;margin-bottom:10px;height:500px;">
    <img src="../../../pictures/generation_nft/face_landmarks/point_landmark.png">
</div>

Voici le code pour récupérer un **point précis**. Je souhaite récupérer dans cette documentation le **point gauche de la bouche**, qui est le numéro **61**.

???+ warning not-expand "Attention, c'est très compliqué"

```python
left_mouth = face_landmarks[61]
```

Pour finir, les valeurs renvoyées sont **normalisées**, elles sont toutes mises sur la même échelle. En ce qui concerne le **X** et le **Y**, il suffit de **multiplier** la valeur par la **longueur** et par la **largeur** de l'image pour retrouver les valeurs en pixels. Pour la valeur **Z**, la profondeur, elle dépend d'une **feuille à plat** placée en parallèle au visage, qui fera office du 0. Tout ce qui sera **avant sera positif**, tout ce qui sera **après sera négatif**.
