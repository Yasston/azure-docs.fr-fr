---
title: Définir plusieurs instances d’une variable dans Bicep
description: Utilisez une boucle de variable Bicep pour effectuer une itération lors de la création d’une variable.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: be97b88156600121deb1b870940a1a40af84fbfc
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2021
ms.locfileid: "111025823"
---
# <a name="variable-iteration-in-bicep"></a>Itération de variable dans Bicep

Cet article explique comment créer plusieurs valeurs pour une variable dans votre fichier Bicep. Vous pouvez ajouter une boucle à la section `variables` et définir de façon dynamique le nombre d’éléments pour une variable pendant le déploiement. Vous évitez également de répéter la syntaxe dans votre fichier Bicep.

Vous pouvez également utiliser l’élément copy avec des [ressources](loop-resources.md), des [propriétés dans une ressource](loop-properties.md) et des [sorties](loop-outputs.md).

## <a name="syntax"></a>Syntaxe

Vous pouvez utiliser des boucles pour déclarer plusieurs variables en procédant comme suit :

- Itération sur un tableau.

  ```bicep
  var <variable-name> = [for <item> in <collection>: {
    <properties>
  }]

  ```

- Itération sur les éléments d’un tableau.

  ```bicep
  var <variable-name> = [for <item>, <index> in <collection>: {
    <properties>
  }]
  ```

- Utilisation d’un index de boucle.

  ```bicep
  var <variable-name> = [for <index> in range(<start>, <stop>): {
    <properties>
  }]
  ```

## <a name="loop-limits"></a>Limites des boucles

Le nombre d’itérations de boucle du fichier Bicep ne peut être ni négatif, ni supérieur à 800. Pour déployer des fichiers Bicep, installez la dernière version des [outils Bicep](install.md).

## <a name="variable-iteration"></a>Itération de variable

L’exemple suivant montre comment créer un tableau de valeurs de chaîne :

```bicep
param itemCount int = 5

var stringArray = [for i in range(0, itemCount): 'item${(i + 1)}']

output arrayResult array = stringArray
```

La sortie retourne un tableau avec les valeurs suivantes :

```json
[
  "item1",
  "item2",
  "item3",
  "item4",
  "item5"
]
```

L’exemple suivant montre comment créer un tableau d’objets avec trois propriétés : `name`, `diskSizeGB` et `diskIndex`.

```bicep
param itemCount int = 5

var objectArray = [for i in range(0, itemCount): {
  name: 'myDataDisk${(i + 1)}'
  diskSizeGB: '1'
  diskIndex: i
}]

output arrayResult array = objectArray
```

La sortie retourne un tableau avec les valeurs suivantes :

```json
[
  {
    "name": "myDataDisk1",
    "diskSizeGB": "1",
    "diskIndex": 0
  },
  {
    "name": "myDataDisk2",
    "diskSizeGB": "1",
    "diskIndex": 1
  },
  {
    "name": "myDataDisk3",
    "diskSizeGB": "1",
    "diskIndex": 2
  },
  {
    "name": "myDataDisk4",
    "diskSizeGB": "1",
    "diskIndex": 3
  },
  {
    "name": "myDataDisk5",
    "diskSizeGB": "1",
    "diskIndex": 4
  }
]
```

## <a name="example-templates"></a>Exemples de modèles

Les exemples suivants illustrent des scénarios courants de création de plusieurs valeurs pour une variable.

|Modèle  |Description  |
|---------|---------|
|[Variables de boucle](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/loopvariables.bicep) | Montre comment effectuer une itération sur des variables. |
|[Règles de sécurité multiples](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.bicep) |Déploie plusieurs règles de sécurité sur un groupe de sécurité réseau. Crée les règles de sécurité à partir d’un paramètre. Pour le paramètre, consultez [plusieurs fichiers de paramètre NSG](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json). |

## <a name="next-steps"></a>Étapes suivantes

- Pour connaître d’autres utilisations possibles des boucles, consultez :
  - [Itération de ressource dans les fichiers Bicep](loop-resources.md)
  - [Itération de propriété dans les fichiers Bicep](loop-properties.md)
  - [Itération de sortie dans les fichiers Bicep](loop-outputs.md)
- Pour plus d’informations sur les différentes sections d’un fichier Bicep, consultez [Présentation de la structure et de la syntaxe des fichiers Bicep](file.md).
- Pour plus d’informations sur le déploiement de plusieurs ressources, consultez [Utiliser des modules Bicep](modules.md).
- Pour définir des dépendances sur des ressources créées dans une boucle, consultez [Définir des dépendances de ressources](./resource-declaration.md#set-resource-dependencies).
- Pour savoir comment effectuer un déploiement avec PowerShell, consultez [Déployer des ressources avec Bicep et Azure PowerShell](deploy-powershell.md).
- Pour savoir comment effectuer un déploiement avec Azure CLI, consultez [Déployer des ressources avec Bicep et Azure CLI](deploy-cli.md).
