---
title: Récupération des parties d'un visage
summary: Récupération des parties d'un visage sous forme de contour.
---

# Récupération des parties

## Définition

La **récupération des parties d'un visage** intervient en plus du module **face landmarks**. Pour rappel, **face landmarks** permet d'avoir **468 points** très précis pour des éléments du visage. Cependant, les **oreilles**, la **forme de la tête**, les **cheveux** et la **barbe** ne sont pas identifiés. La méthode pour identifier des éléments sur une image s'appelle la **segmentation**. Cette méthode est notamment utilisée dans les algorithmes pour la **conduite autonome** de Tesla (en bien plus évoluée).

<div style="display:flex;flex-direction:row;justify-content:space-around;margin-top:8px;margin-bottom:10px;">
    <div style="display:flex;flex-direction:column;">
        <div style="display:flex;flex-direction:row;align-items:center; justify-content:center;margin-bottom:20px;">
            <h3 style="margin:0 0 0 6px !important;">Image originale</h3>
        </div>
        <img src="../../../pictures/generation_nft/face_parsing/giroud.jpg">
    </div>
    <div style="display:flex; flex-direction: column;">
        <div style="display:flex;flex-direction:row;align-items:center; justify-content:center;margin-bottom:20px;">
            <h3 style="margin:0 0 0 6px !important;">Masque de l'image</h3>
        </div>
        <img src="../../../pictures/generation_nft/face_parsing/mask_giroud.jpg">
    </div>
</div>

???+ info "Contours"
    Pour le bien de la documentation, les zones ont été colorées. Dans l'algorithme réel, nous reprenons uniquement les contours lissés pour améliorer le rendu.

???+ warning not-expand "Le modèle utilisé ne détecte pas encore la barbe !"


## Contexte

Le but est de pouvoir générer une **image originale tirée d'un visage d'un joueur de foot**. La problématique était de récupérer les **parties manquantes** du visage que n'**identifiait** pas le module **face landmarks**. De **nombreux modèles** existent plus ou moins **complexes** et **précis** existent pour effectuer la **"segmentation"**.
Ce qui a été le plus compliqué, c'est de **choisir le modèle**. Heureusement, il existe un **répertoire GIT** nommé **face_parsing** qui nous permet de réaliser ce que l'on souhaite de façon **performante** pour une **première version**.
Comme vous avez pu le voir, le modèle choisi dans un premier temps ne **récupère pas la barbe**. C'est un **modèle pré-entrainé** par un **organisme externe**. Le plus optimal serait de **réentrainer un modèle similaire** avec l'ajout de la barbe. Cependant, l'entraînement d'un modèle est **très long**. Il doit être entrainé sur des **millions de données** et doit être **paramétré** de façon **optimale pour un rendu optimale**.

## Utilisation du modèle

L'utilisation du modèle peut être découpée en **3 étapes**.

### Récupération du modèle pré-entrainé

Dans un premier temps, il faut télécharger ce modèle pré-entrainé. Il se présente sous la forme d'un fichier **.pth**.
Il est **hébergé** sur le **Google Drive** de l'équipe et est automatiquement **téléchargé** lorsque la solution est lancée s'il n'est pas encore présent.
Situé dans le dossier **app/generation_nft/libraries/face_parsin/pre-trained/face_parts.pth**, il est appelé dans la fonction **load_state_dict** du package **pytorch**.

```py
model = BiSeNet(
    resnet="https://drive.google.com/u/1/uc?id=1e8HiZK_0C_56wYXc9nbDl9CyFVMfp151&export=download", # modèle pour équilibrer le BiSeNet
    n_classes=self.NUM_CLASSES, # Nombre de parties à détecter (par défaut 19)
)
model.to(device)
model.load_state_dict(
    torch.load(
        f"{pretrained_model_path.parent}/{pretrained_model_path.name}",
        map_location=device,
    )
)
model.eval()
```

Le modèle **pré-entrainé** a utilisé **BiSeNet** comme modèle de **segmentation**. Il est nécessaire de reprendre exactement les **mêmes paramètres**.

???+ info not-expand "L'exécution du modèle s'effectue sur le CPU et se lance en mode évaluation."

### Traitement de la donnée

La **donnée**, ou ici, **le visage**, dois avoir un **format précis** afin qu'il puisse être **traité par le modèle**. **Pytorch**, le package de deep learning, utilise le type **Tensor**, qui s'apparente grandement à un **tableau de matrice** comme pour le package **numpy** et qui représente l'image. **En entrée**, notre image est sous le format du **tableau numpy**, il faut le **convertir** en Tensor puis supprimer la **première dimension** (colonne) qui correspond au **nombre de classes**.

```py
infer_transforms = transforms.Compose(
    [
        transforms.ToTensor(),
        transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225)),
    ]
)

initialH, initialW, _ = face.shape

if initialH != input_size or initialW != input_size:
    face = open_cv.resize(
        face, (input_size, input_size), interpolation=open_cv.INTER_NEAREST
    )  # redimensionne le visage

face_tensor = infer_transforms(face).unsqueeze(0).to(device)
```

???+ info not-expand "Les **opérations** sur le type Tensor, sont effectuées sur le **CPU**."

### Prédiction

La **prédiction** est simplement l'étape où l'**image traitée** est fournie au modèle. Ce modèle retournera une **prédiction**, toujours dans le format **Tensor**.

```py
prediction = model(face_tensor)[0]
prediction = F.interpolate(
    prediction,
    size=(initialW, initialH),
    mode="bilinear",
    align_corners=True,
)
prediction = prediction.squeeze(0).cpu().numpy().argmax(0)
```

La prédiction se présente sous **cette forme** :

<img src="../../../pictures/generation_nft/face_parsing/prediction.jpg">

### Nettoyage de la prédiction

Comme vous l'aurez remarqué, la prédiction concerne **19 zones**. Seulement **4** nous intéressent : **les oreilles (2), la forme de la tête et les cheveux**.
Il faut **coloriser** de la bonne couleur les **parties** à l'**intérieur de la zone bleue** et supprimer (mettre en blanc) les **zones non souhaitées** (**cou et vêtement**).
Une fois que seules les **zones souhaitées** sont récupérées, nous récupérons uniquement **leur contour**.


```py
cleaned_prediction = self.clean_mask(prediction)
visual_mask_color = self.colorize_parts(cleaned_prediction)
visual_parts = self.get_parts(visual_mask_color)
```

???+ danger not-expand "Code trop technique."


## Résultat

Pour finir, la fonction **get_parts** au-dessus, stocke dans une **variable** chaque **contour** avec leur **correspondance**.

<img src="../../../pictures/generation_nft/face_parsing/hair_mask.jpg">
