---
title: Extraire du texte à partir des images
titleSuffix: Azure Cognitive Search
description: Traitez et extrayez du texte et d’autres informations d’images dans des pipelines Recherche cognitive Azure.
manager: nitinme
author: LuisCabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 68186c5294c0a3a2f376a93ef1902307780f48bb
ms.sourcegitcommit: bd65925eb409d0c516c48494c5b97960949aee05
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2021
ms.locfileid: "111538287"
---
# <a name="how-to-process-and-extract-information-from-images-in-ai-enrichment-scenarios"></a>Comment traiter et extraire des informations d’images dans des scénarios d’enrichissement de l’IA

La Recherche cognitive Azure intègre plusieurs fonctionnalités pour l’utilisation d’images et de fichiers image. Pendant le décodage d’un document, vous pouvez utiliser le paramètre *imageAction* pour extraire du texte de photos ou d’images contenant du texte alphanumérique, tel que le mot « STOP » dans le panneau de signalisation d’arrêt. D’autres scénarios incluent la génération d’une représentation textuelle d’une image, telle que « pissenlit » pour une photo de pissenlit ou la couleur « jaune ». Vous pouvez également extraire des métadonnées de l’image, telles que sa taille.

Cet article couvre plus en détail le traitement d’image et fournit des conseils pour l’utilisation d’images dans un pipeline d’enrichissement de l’IA.

<a name="get-normalized-images"></a>

## <a name="get-normalized-images"></a>Obtenir des images normalisées

Dans le cadre du décodage d’un document, il existe un nouvel ensemble de paramètres de configuration de l’indexeur pour gérer des fichiers image ou des images incorporées dans des fichiers. Ces paramètres permettent de normaliser des images en vue d’un traitement ultérieur en aval. La normalisation des images rend celles-ci plus uniformes. Les images de grande taille sont redimensionnées à une hauteur et une largeur maximales afin de les rendre utilisables. Pour les images fournissant des métadonnées d’orientation, la rotation est ajustée pour un chargement vertical. Les ajustements de métadonnées sont capturés dans un type complexe créé pour chaque image. 

Vous ne pouvez pas désactiver la normalisation d’image. Les compétences qui itèrent sur des images attendent des images normalisées. L’activation de la normalisation d’image sur un indexeur nécessite l’attachement d’un ensemble de compétences à cet indexeur.

| Paramètre de configuration | Description |
|--------------------|-------------|
| imageAction   | Défini sur « none » si aucune action ne doit être effectuée quand des images incorporées ou des fichiers image sont rencontrés. <br/>Défini sur « generateNormalizedImages » pour générer un tableau d’images normalisées dans le cadre du décodage de document.<br/>Défini sur « generateNormalizedImages » pour générer un tableau d’images normalisées où, pour les PDF de votre source de données, chaque page s’affiche en tant qu’image de sortie unique.  La fonctionnalité est la même que « generateNormalizedImages » pour les types de fichiers autres que PDF.<br/>Pour toute option autre que « none », les images seront exposées dans le champ *normalized_images*. <br/>La valeur par défaut est « none ». Cette configuration est pertinente uniquement pour des sources de données d’objet blob quand le paramètre « dataToExtract » est défini sur « contentAndMetadata ». <br/>Au maximum, 1 000 images seront extraites d'un document donné. Si un document contient plus de 1 000 images, les 1 000 premières seront extraites et un avertissement sera généré. |
|  normalizedImageMaxWidth | Largeur maximale (en pixels) des images normalisées générées. La valeur par défaut est 2000. La valeur maximale autorisée est 10000. | 
|  normalizedImageMaxHeight | Hauteur maximale (en pixels) des images normalisées générées. La valeur par défaut est 2000. La valeur maximale autorisée est 10000.|

> [!NOTE]
> Si vous ne définissez pas la propriété *imageAction* sur « none », vous ne pouvez définir la propriété *parsingMode* que sur « default ».  Vous ne pouvez définir qu’une seule de ces deux propriétés sur une valeur autre que la valeur par défaut dans la configuration de votre indexeur.

Définissez le paramètre **parsingMode** avec la valeur `json` (pour indexer chaque objet blob en tant que document unique) ou `jsonArray` (si vos objets blob contiennent des tableaux JSON et que vous souhaitez traiter chaque élément de tableau comme un document distinct).

La valeur par défaut de 2000 pixels pour la hauteur et la largeur maximales des images normalisées est basée sur les tailles maximales prises en charge par la [compétence de reconnaissance optique de caractères](cognitive-search-skill-ocr.md) et la [compétence d’analyse d’image](cognitive-search-skill-image-analysis.md). La [compétence de reconnaissance optique de caractères](cognitive-search-skill-ocr.md) (OCR) prend en charge une largeur et une hauteur maximales de 4200 pour les langues autres que l'anglais et de 10000 pour l'anglais.  Si vous augmentez les limites maximales, le traitement des images plus volumineuses peut échouer en fonction de la définition de vos compétences et de la langue des documents. 

Vous spécifiez la propriété imageAction dans votre [définition d’indexeur](/rest/api/searchservice/create-indexer) comme suit :

```json
{
  //...rest of your indexer definition goes here ...
  "parameters":
  {
    "configuration": 
    {
        "dataToExtract": "contentAndMetadata",
        "imageAction": "generateNormalizedImages"
    }
  }
}
```

Si la propriété *imageAction* est définie sur une valeur autre que « none », le nouveau champ *normalized_images* contiendra un tableau d’images. Chaque image est un type complexe comprenant les membres suivants :

| Membre de l’image       | Description                             |
|--------------------|-----------------------------------------|
| data               | Chaîne codée en Base64 de l’image normalisée au format JPEG.   |
| width              | Largeur de l’image normalisée en pixels. |
| height             | Hauteur de l’image normalisée en pixels. |
| originalWidth      | Largeur d’origine de l’image avant la normalisation. |
| originalHeight      | Hauteur d’origine de l’image avant la normalisation. |
| rotationFromOriginal |  Rotation dans le sens inverse des aiguilles d’une montre exprimée en degrés pour créer l’image normalisée. Valeur comprise entre 0 et 360 degrés. Cette étape lit les métadonnées de l’image générée par un appareil photo ou un scanneur. La valeur est généralement un multiple de 90 degrés. |
| contentOffset | Offset de caractère à l’intérieur du champ de contenu dont l’image a été extraite. Ce champ est applicable uniquement aux fichiers contenant des images incorporées. |
| pageNumber | Si l'image a été extraite ou rendue à partir d'un PDF, ce champ contient le numéro de page du PDF à partir de laquelle elle a été extraite ou rendue, à partir de 1.  Si l’image ne provient pas d’un fichier PDF, ce champ a la valeur 0.  |

 Exemple de valeur de *normalized_images* :
```json
[
  {
    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
    "width": 500,
    "height": 300,
    "originalWidth": 5000,  
    "originalHeight": 3000,
    "rotationFromOriginal": 90,
    "contentOffset": 500,
    "pageNumber": 2
  }
]
```

## <a name="image-related-skills"></a>Compétences liées à l’image

Pour accepter des images en tant qu’entrées, vous pouvez utiliser une compétence personnalisée ou des compétences intégrées. Les compétences intégrées sont l’[OCR](cognitive-search-skill-ocr.md) et l’[Analyse d’image](cognitive-search-skill-image-analysis.md). 

Actuellement, ces compétences fonctionnent uniquement avec des images générées à partir de l’étape de décodage du document. Par conséquent, la seule entrée prise en charge est `"/document/normalized_images"`.

### <a name="image-analysis-skill"></a>Compétence d’analyse d’image

La [compétence d’analyse d’image](cognitive-search-skill-image-analysis.md) extrait un ensemble riche de caractéristiques visuelles basées sur le contenu de l’image. Par exemple, à partir d’une image, vous pouvez générer une légende, générer des balises ou identifier des célébrités et des points de repère.

### <a name="ocr-skill"></a>Compétence de reconnaissance optique des caractères

La [compétence de reconnaissance optique des caractères](cognitive-search-skill-ocr.md) extrait du texte de fichiers image, par exemple, aux formats JPG, PNG et bitmap. Elle peut extraire du texte ainsi que des informations de disposition. Les informations de disposition fournissent des rectangles englobants pour chacune des chaînes identifiées.

### <a name="custom-skills"></a>Compétences personnalisées

Des images peuvent également être transmises et retournées dans des compétences personnalisées. L’ensemble de compétences encode en base64 l’image transmise dans la compétence personnalisée. Pour utiliser l’image dans la compétence personnalisée, définissez `/document/normalized_images/*/data` comme entrée dans la compétence personnalisée. Dans votre code de compétence personnalisée, décodez en base64 la chaîne avant de la convertir en image. Pour retourner une image à l’ensemble de compétences, encodez-la en base64 au préalable.

 L’image est retournée en tant qu’objet avec les propriétés suivantes.

```json
 { 
  "$type": "file", 
  "data": "base64String" 
 }
```

Le référentiel d’[exemples Python pour Recherche Azure](https://github.com/Azure-Samples/azure-search-python-samples) présente un exemple complet implémenté en Python d’une compétence personnalisée qui enrichit les images.

## <a name="embedded-image-scenario"></a>Scénario d’image incorporée

Un scénario courant implique la création d’une chaîne comprenant tous les contenus d’un fichier, tant le texte que le texte d’origine de l’image, en procédant comme suit :  

1. [Extraction de normalized_images](#get-normalized-images).
1. Exécution de la compétence de reconnaissance optique des caractères en utilisant `"/document/normalized_images"` en entrée.
1. Fusion de la représentation textuelle de ces images avec le texte brut extrait du fichier. Vous pouvez utiliser la compétence de [fusion de texte](cognitive-search-skill-textmerger.md) pour combiner les deux blocs de texte dans une seule chaîne de grande taille.

L’exemple suivant de jeu de jeu de compétences crée un champ *merged_text* comprenant le contenu textuel de votre document. Il inclut également le texte obtenu par reconnaissance optique des caractères de chacune des images incorporées. 

### <a name="request-body-syntax"></a>Syntaxe du corps de la demande

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```

À présent que vous avez un champ merged_text, vous pouvez le mapper en tant que champ pouvant faire l’objet d’une recherche dans la définition de votre indexeur. Tout le contenu de vos fichiers, y compris le texte des images, sera disponible pour une recherche.

## <a name="visualize-bounding-boxes-of-extracted-text"></a>Visualiser les rectangles englobants du texte extrait

Un autre scénario courant est la visualisation des informations de disposition des résultats de recherche. Par exemple, dans le cadre de vos résultats de recherche, vous pouvez vouloir mettre en évidence l’endroit où un élément de texte a été trouvé dans une image.

Étant donné que l’étape de reconnaissance optique des caractères est effectuée sur les images normalisées, les coordonnées de disposition figurent dans l’espace de l’image normalisée. Lorsque vous affichez l’image normalisée, la présence de coordonnées n’est généralement pas un problème mais, dans certains cas, il peut être utile d’afficher l’image d’origine. Dans ce cas, vous pouvez convertir chaque point de coordonnée dans la disposition vers le système de coordonnées de l’image d’origine. 

Pour vous aider, si vous avez besoin de convertir des coordonnées normalisées vers l’espace des coordonnées d’origine, vous pouvez utiliser l’algorithme suivant :

```csharp
        /// <summary>
        ///  Converts a point in the normalized coordinate space to the original coordinate space.
        ///  This method assumes the rotation angles are multiples of 90 degrees.
        /// </summary>
        public static Point GetOriginalCoordinates(Point normalized,
                                    int originalWidth,
                                    int originalHeight,
                                    int width,
                                    int height,
                                    double rotationFromOriginal)
        {
            Point original = new Point();
            double angle = rotationFromOriginal % 360;

            if (angle == 0 )
            {
                original.X = normalized.X;
                original.Y = normalized.Y;
            } else if (angle == 90)
            {
                original.X = normalized.Y;
                original.Y = (width - normalized.X);
            } else if (angle == 180)
            {
                original.X = (width -  normalized.X);
                original.Y = (height - normalized.Y);
            } else if (angle == 270)
            {
                original.X = height - normalized.Y;
                original.Y = normalized.X;
            }

            double scalingFactor = (angle % 180 == 0) ? originalHeight / height : originalHeight / width;
            original.X = (int) (original.X * scalingFactor);
            original.Y = (int)(original.Y * scalingFactor);

            return original;
        }
```

## <a name="passing-images-to-custom-skills"></a>Passage d’images aux compétences personnalisées

Pour les scénarios où vous avez besoin d’une compétence personnalisée pour travailler sur les images, vous pouvez faire passer des images à la compétence personnalisée et faire en sorte qu’elle retourne du texte ou des images. [L’exemple Python](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Image-Processing) de traitement d’image illustre le workflow. L’ensemble de compétences suivant provient de l’exemple.

L’ensemble de compétences suivant prend l’image normalisée (obtenue lors du craquage de document) et génère des tranches de l’image.

### <a name="sample-skillset"></a>Exemple d’ensemble de compétences

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
          "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
          "name": "ImageSkill",
          "description": "Segment Images",
          "context": "/document/normalized_images/*",
          "uri": "https://your.custom.skill.url",
          "httpMethod": "POST",
          "timeout": "PT30S",
          "batchSize": 100,
          "degreeOfParallelism": 1,
          "inputs": [
            {
              "name": "image",
              "source": "/document/normalized_images/*"
            }
          ],
          "outputs": [
            {
              "name": "slices",
              "targetName": "slices"
            }
          ],
          "httpHeaders": {}
        }
  ]
}
```

### <a name="custom-skill"></a>Compétence personnalisée

La compétence personnalisée elle-même est externe à l’ensemble de compétences. Dans ce cas, il s’agit d’un code Python qui effectue une boucle complète sur le lot d’enregistrements des requêtes au format de la compétence personnalisée, puis convertit la chaîne encodée en Base64 en une image.

```python
# deserialize the request, for each item in the batch
for value in values:
  data = value['data']
  base64String = data["image"]["data"]
  base64Bytes = base64String.encode('utf-8')
  inputBytes = base64.b64decode(base64Bytes)
  # Use numpy to convert the string to an image
  jpg_as_np = np.frombuffer(inputBytes, dtype=np.uint8)
  # you now have an image to work with
```

Comme pour le retour d’une image, une chaîne encodée en Base64 est retournée dans un objet JSON avec une propriété `$type` de `file`.

```python
def base64EncodeImage(image):
    is_success, im_buf_arr = cv2.imencode(".jpg", image)
    byte_im = im_buf_arr.tobytes()
    base64Bytes = base64.b64encode(byte_im)
    base64String = base64Bytes.decode('utf-8')
    return base64String

 base64String = base64EncodeImage(jpg_as_np)
 result = { 
  "$type": "file", 
  "data": base64String 
}
```

## <a name="see-also"></a>Voir aussi

+ [Créer un indexeur (REST)](/rest/api/searchservice/create-indexer)
+ [Compétence d'analyse d'image](cognitive-search-skill-image-analysis.md)
+ [Compétence de reconnaissance optique des caractères](cognitive-search-skill-ocr.md)
+ [Compétence de fusion de texte](cognitive-search-skill-textmerger.md)
+ [Guide pratique pour définir un ensemble de compétences](cognitive-search-defining-skillset.md)
+ [Guide pratique pour mapper des champs enrichis](cognitive-search-output-field-mapping.md)
+ [Guide pratique pour passer des images aux compétences personnalisées](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Image-Processing)
