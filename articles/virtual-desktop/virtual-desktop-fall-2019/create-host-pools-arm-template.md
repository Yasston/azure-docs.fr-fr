---
title: Pool d’hôtes Azure Virtual Desktop (classique) Azure Resource Manager – Azure
description: Découvrez comment créer un pool d’hôtes dans Azure Virtual Desktop (classique) à l’aide d’un modèle Azure Resource Manager.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: efc8c93c93325e41c1f8fd9c22baf4c036012ab6
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111749918"
---
# <a name="create-a-host-pool-in-azure-virtual-desktop-classic-with-an-azure-resource-manager-template"></a>Créer un pool d’hôtes dans Azure Virtual Desktop (classique) à l’aide d’un modèle Azure Resource Manager

>[!IMPORTANT]
>Ce contenu s’applique à Azure Virtual Desktop (classique), qui ne prend pas en charge les objets Azure Virtual Desktop pour Azure Resource Manager.

Les pools d’hôtes sont des ensembles d’une ou de plusieurs machines virtuelles identiques dans des environnements de locataires Azure Virtual Desktop. Chaque pool d’hôtes peut contenir un groupe d’applications avec lequel les utilisateurs peuvent interagir comme ils le feraient sur un ordinateur de bureau physique.

Suivez les instructions de cette section afin de créer un pool d’hôtes pour un locataire Azure Virtual Desktop avec un modèle Azure Resource Manager fourni par Microsoft. Cet article explique comment créer un pool d’hôtes dans Azure Virtual Desktop, créer un groupe de ressources avec des machines virtuelles dans un abonnement Azure, joindre ces machines virtuelles au domaine AD et inscrire les machines virtuelles auprès de Azure Virtual Desktop.

## <a name="what-you-need-to-run-the-azure-resource-manager-template"></a>Exécution du modèle Azure Resource Manager

Vérifiez les points suivants avant d’exécuter le modèle Azure Resource Manager :

- Emplacement de la source de l’image que vous voulez utiliser. Provient-elle de la galerie Azure ou est-elle personnalisée ?
- Informations d’identification de jonction de domaine.
- Vos informations d’identification Azure Virtual Desktop.

Lorsque vous créez un pool d’hôtes Azure Virtual Desktop avec le modèle Azure Resource Manager, vous pouvez créer un ordinateur virtuel à partir de la Galerie Azure, d’une image managée ou d’une image non managée. Pour en savoir plus sur la création d’images de machines virtuelles, voir [Préparer un disque dur virtuel Windows à charger sur Azure](../../virtual-machines/windows/prepare-for-upload-vhd-image.md) et [Créer une image managée d’une machine virtuelle généralisée dans Azure](../../virtual-machines/windows/capture-image-resource.md).

## <a name="run-the-azure-resource-manager-template-for-provisioning-a-new-host-pool"></a>Exécuter le modèle Azure Resource Manager pour l’approvisionnement du nouveau pool d’hôtes

Pour commencer, accédez à [cette URL GitHub](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool).

### <a name="deploy-the-template-to-azure"></a>Déployer le modèle sur Azure

Si vous effectuez un déploiement dans un abonnement Entreprise, faites défiler vers le bas et sélectionnez **Déployer sur Azure**, puis renseignez les paramètres en fonction de la source de votre image.

Si vous effectuez un déploiement dans un abonnement de fournisseur de solutions cloud, suivez ces étapes pour déployer sur Azure :

1. Faites défiler vers le bas et cliquez sur **Déployer sur Azure**, puis sélectionnez **Copier l’adresse du lien**.
2. Ouvrez un éditeur de texte tel que le bloc-notes et collez le lien.
3. Juste après « https://portal.azure.com/  » et avant le dièse (#), entrez un arobase (@) suivi du nom de domaine du locataire. Voici un exemple du format que vous devez utiliser : `https://portal.azure.com/@Contoso.onmicrosoft.com#create/`.
4. Connectez-vous au portail Azure en tant qu’utilisateur avec des autorisations d’administrateur/contributeur pour l’abonnement du fournisseur de solutions cloud.
5. Collez le lien que vous avez copié dans l’éditeur de texte dans la barre d’adresse.

Pour obtenir des conseils sur les paramètres à entrer pour votre scénario, voir le [fichier Lisez-moi](https://github.com/Azure/RDS-Templates/blob/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool/README.md) d’Azure Virtual Desktop. Le fichier Lisez-moi est toujours mis à jour avec les dernières modifications.

## <a name="assign-users-to-the-desktop-application-group"></a>Affecter des utilisateurs au groupe d’applications de bureau

Une fois le modèle GitHub Azure Resource Manager terminé, attribuez l’accès aux utilisateurs avant de commencer à tester les bureaux de session complète sur vos machines virtuelles.

Si ce n’est déjà fait, commencez par télécharger et importer le [module PowerShell Azure Virtual Desktop](/powershell/windows-virtual-desktop/overview/) à utiliser dans votre session PowerShell.

Pour affecter des utilisateurs au groupe d’applications de bureau, ouvrez une fenêtre PowerShell et exécutez la cmdlet suivante pour vous connecter à l’environnement Azure Virtual Desktop :

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Ensuite, ajoutez des utilisateurs au groupe d’applications de bureau avec cette cmdlet :

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

L’UPN (nom d’utilisateur principal) de l’utilisateur doit correspondre à l’identité de l’utilisateur dans Azure Active Directory (par exemple, user1@contoso.com). Si vous voulez ajouter plusieurs utilisateurs, vous devez exécuter cette applet de commande pour chaque utilisateur.

Une fois ces étapes terminées, les utilisateurs ajoutés au groupe d’applications de bureau peuvent se connecter à Azure Virtual Desktop avec les clients Bureau à distance pris en charge et voir une ressource pour le bureau d’une session.

>[!IMPORTANT]
>Pour contribuer à sécuriser votre environnement Azure Virtual Desktop dans Azure, nous vous recommandons de ne pas ouvrir le port entrant 3389 sur vos machines virtuelles. Azure Virtual Desktop ne nécessite pas l’ouverture du port entrant 3389 pour permettre aux utilisateurs d’accéder aux machines virtuelles du pool hôte. Si vous devez ouvrir le port 3389 pour résoudre des problèmes, nous vous recommandons d’utiliser un [accès à la machine virtuelle juste-à-temps](../../security-center/security-center-just-in-time.md).