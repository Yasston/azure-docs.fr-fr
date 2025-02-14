---
title: Suppression d’un pool d’hôtes Azure Virtual Desktop – Azure
description: Guide pratique pour supprimer un pool d’hôtes dans Azure Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 07/11/2020
ms.author: helohr
ms.custom: devx-track-azurepowershell
manager: femila
ms.openlocfilehash: e4621799389e738bd03e75f84f5c1706f90d69d3
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111751952"
---
# <a name="delete-a-host-pool"></a>Supprimer un pool d’hôtes

Tous les pools d’hôtes créés dans Azure Virtual Desktop sont attachés à des hôtes de session et à des groupes d’applications. Pour supprimer un pool d’hôtes, vous devez supprimer les groupes d’applications et les hôtes de session qui lui sont associés. La suppression d’un groupe d’applications est relativement simple, mais celle d’un hôte de session est plus complexe. Lorsque vous supprimez un hôte de session, vous devez vous assurer qu’il n’a pas de session utilisateur active. Toutes les sessions utilisateur sur l’hôte de la session doivent être fermées pour éviter que les utilisateurs ne perdent des données.

## <a name="delete-a-host-pool-with-powershell"></a>Supprimer un pool d’hôtes à l’aide de PowerShell

Pour supprimer un pool d’hôtes à l’aide de PowerShell, vous devez d’abord supprimer tous les groupes d’applications du pool d’hôtes. Pour supprimer tous les groupes d’applications, exécutez la cmdlet PowerShell suivante :

```powershell
Remove-AzWvdApplicationGroup -Name <appgroupname> -ResourceGroupName <resourcegroupname>
```

Ensuite, exécutez cette cmdlet pour supprimer le pool d’hôtes :

```powershell
Remove-AzWvdHostPool -Name <hostpoolname> -ResourceGroupName <resourcegroupname> -Force:$true
```

Cette cmdlet supprime toutes les sessions utilisateur existantes sur l’hôte de session du pool d’hôtes. Elle désinscrit également l’hôte de la session du pool d’hôtes. Toutes les machines virtuelles associées existent toujours dans votre abonnement.

## <a name="delete-a-host-pool-with-the-azure-portal"></a>Supprimer un pool d’hôtes à l’aide du portail Azure

Pour supprimer un pool d’hôtes dans le portail Azure :

1. Connectez-vous au [portail Azure](https://portal.azure.com/).

2. Recherchez et sélectionnez **Azure Virtual Desktop**.

3. Sélectionnez **Pools d’hôtes** dans le menu sur le côté gauche de la page, puis sélectionnez le nom du pool d’hôtes que vous souhaitez supprimer.

4. Dans le menu sur le côté gauche de la page, sélectionnez **Groupes d’applications**.

5. Sélectionnez tous les groupes d’applications dans le pool d’hôtes que vous allez supprimer, puis sélectionnez **Supprimer**.

6. Une fois que vous avez supprimé les groupes d’applications, accédez au menu sur le côté gauche de la page et sélectionnez **Vue d’ensemble**.

7. Sélectionnez **Supprimer**.

8. S’il existe des hôtes de session dans le pool d’hôtes que vous supprimez, un message apparaît pour vous demander la permission de continuer. Sélectionnez **Oui**.

9. Le portail Azure supprime alors tous les hôtes de session ainsi que le pool d’hôtes. Les machines virtuelles associées à l’hôte de session ne seront pas supprimées et seront conservées dans votre abonnement.

## <a name="next-steps"></a>Étapes suivantes

Pour savoir comment créer un pool d’hôtes, consultez les articles suivants :

- [Créer un pool d’hôtes avec le portail Azure](create-host-pools-azure-marketplace.md)
- [Créer un pool d’hôtes avec PowerShell](create-host-pools-powershell.md)

Pour savoir comment configurer les paramètres du pool d’hôtes, consultez les articles suivants :

- [Personnaliser les propriétés de RDP pour un pool d’hôtes](customize-rdp-properties.md)
- [Configuration de la méthode d’équilibrage de charge d’Azure Virtual Desktop](configure-host-pool-load-balancing.md)
- [Configurer le type d’affectation d’un pool d’hôtes de bureau personnel](configure-host-pool-personal-desktop-assignment-type.md)
