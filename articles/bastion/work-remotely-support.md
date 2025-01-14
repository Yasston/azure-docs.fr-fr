---
title: 'Travail à distance avec Bastion : Azure Bastion'
description: Découvrez comment utiliser Azure Bastion pour travailler à distance dans le contexte de la pandémie de COVID-19.
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mialdrid
ms.openlocfilehash: 60709bf337c945d1ece7ecc5453350b0e126b046
ms.sourcegitcommit: 9ad20581c9fe2c35339acc34d74d0d9cb38eb9aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2021
ms.locfileid: "110540347"
---
# <a name="working-remotely-using-azure-bastion"></a>Travail à distance à l’aide d’Azure Bastion

Azure Bastion joue un rôle pivot dans la prise en charge des scénarios de travail à distance en permettant aux utilisateurs disposant d’une connectivité Internet d’accéder aux machines virtuelles Azure. En particulier, il permet aux administrateurs informatiques de gérer leurs applications s’exécutant dans Azure à tout moment et où qu’ils se trouvent dans le monde.

>[!NOTE]
>Cet article décrit comment vous pouvez tirer parti d’Azure Bastion, d’Azure, du réseau Microsoft et de l’écosystème de partenaires Azure pour travailler à distance et atténuer les problèmes de réseau auxquels vous faites face dans le contexte de la crise de COVID-19.
>

## <a name="securely-access-virtual-machines"></a>Accès sécurisé aux machines virtuelles

Plus précisément, Azure Bastion offre une connectivité RDP/SSH sans interruption et sécurisée aux machines virtuelles au sein du réseau virtuel Azure, directement dans le portail Azure, sans utiliser d’adresse IP publique. Pour plus d’informations sur l’architecture et les fonctionnalités clés d’Azure Bastion, consultez [Qu’est-ce qu’Azure Bastion](bastion-overview.md).

Azure Bastion est déployé par réseau virtuel, ce qui signifie que les entreprises peuvent configurer et gérer un bastion Azure pour prendre rapidement en charge l’accès des utilisateurs distants aux machines virtuelles au sein d’un réseau virtuel Azure. Pour obtenir de l’aide sur la création et la gestion de bastions Azure, consultez [Créer un hôte bastion](./tutorial-create-host-portal.md).

## <a name="next-steps"></a>Étapes suivantes

* Configurez Azure Bastion avec le [portail Azure](./tutorial-create-host-portal.md), [PowerShell](bastion-create-host-powershell.md) ou Azure CLI.

* Pour plus d’informations, lisez les [questions fréquentes (FAQ) sur Bastion](bastion-faq.md).