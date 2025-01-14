---
title: Monter le service Stockage Blob Azure à l’aide du protocole NFS 3.0 (préversion) | Microsoft Docs
description: Découvrez comment monter un conteneur dans le stockage Blob à partir d’une machine virtuelle Azure ou d’un client qui s’exécute localement à l’aide du protocole NFS 3.0.
author: normesta
ms.subservice: blobs
ms.service: storage
ms.topic: conceptual
ms.date: 08/04/2020
ms.author: normesta
ms.reviewer: yzheng
ms.custom: references_regions, devx-track-azurepowershell
ms.openlocfilehash: d184569fd28e23b14ff3cb24a0c8e68477b9842d
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2021
ms.locfileid: "111888731"
---
# <a name="mount-blob-storage-by-using-the-network-file-system-nfs-30-protocol-preview"></a>Monter le stockage Blob à l’aide du protocole NFS (Network File System) 3.0 (préversion)

Vous pouvez monter un conteneur dans le stockage Blob à partir d’une machine virtuelle Azure basée sur Linux ou d’un système Linux qui s’exécute localement à l’aide du protocole NFS 3.0. Cet article fournit un guide pas à pas. Pour en savoir plus sur la prise en charge du protocole NFS 3.0 dans le service Stockage Blob, consultez [Prise en charge du protocole NFS (Network File System) 3.0 dans le service Stockage Blob Azure (préversion)](network-file-system-protocol-support.md).

## <a name="step-1-register-the-nfs-30-protocol-feature-with-your-subscription"></a>Étape 1 : Inscrivez la fonctionnalité de protocole NFS 3.0 avec votre abonnement

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Ouvrez une fenêtre de commande PowerShell. 

2. Connectez-vous à votre abonnement Azure avec la commande `Connect-AzAccount` et suivez les instructions à l’écran.

   ```powershell
   Connect-AzAccount
   ```

3. Si votre identité est associée à plusieurs abonnements, définissez votre abonnement actif.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```
   
   Remplacez la valeur d’espace réservé `<subscription-id>` par l’ID de votre abonnement.

4. Enregistrez la fonctionnalité `AllowNFSV3` à l’aide de la commande suivante.

   ```powershell
   Register-AzProviderFeature -FeatureName AllowNFSV3 -ProviderNamespace Microsoft.Storage 
   ```

5. Inscrivez le fournisseur de ressources en utilisant la commande suivante :
    
   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Storage   
   ```
   
# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. Ouvrez une fenêtre de terminal.

2. Connectez-vous à votre abonnement Azure avec la commande `az login` et suivez les instructions à l’écran.

   ```azurecli-interactive
   az login
   ```
   
3. Enregistrez la fonctionnalité `AllowNFSV3` à l’aide de la commande suivante.

   ```azurecli-interactive
   az feature register --namespace Microsoft.Storage --name AllowNFSV3 --subscription <subscription-id>
   ```

   Remplacez la valeur d’espace réservé `<subscription-id>` par l’ID de votre abonnement.

4. Inscrivez le fournisseur de ressources en utilisant la commande suivante :
    
   ```azurecli-interactive
   az provider register -n Microsoft.Storage --subscription <subscription-id>
   ```

   Remplacez la valeur d’espace réservé `<subscription-id>` par l’ID de votre abonnement.

---

## <a name="step-2-verify-that-the-feature-is-registered"></a>Étape 2 : Vérifier que l’appareil est enregistré 

L’approbation d’inscription peut prendre jusqu’à une heure. Pour vérifier que l’inscription est terminée, utilisez les commandes suivantes.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName AllowNFSV3
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az feature show --namespace Microsoft.Storage --name AllowNFSV3 --subscription <subscription-id>
```

Remplacez la valeur d’espace réservé `<subscription-id>` par l’ID de votre abonnement.

---

## <a name="step-3-create-an-azure-virtual-network-vnet"></a>Étape 3 : Créez un réseau virtuel Azure (Vnet)

Votre compte de stockage doit se trouver dans un réseau virtuel. Un réseau virtuel permet aux clients de se connecter en toute sécurité à votre compte de stockage. Pour en savoir plus sur le réseau virtuel et sur la façon d’en créer un, consultez la [documentation relative au réseau virtuel](../../virtual-network/index.yml).

> [!NOTE]
> Les clients du même réseau virtuel peuvent monter des conteneurs dans votre compte. Vous pouvez également monter un conteneur à partir d’un client qui s’exécute sur un réseau local, mais vous devrez d’abord connecter votre réseau local à votre réseau virtuel. Consultez [Connexions réseau prises en charge](network-file-system-protocol-support.md#supported-network-connections).

## <a name="step-4-configure-network-security"></a>Étape 4 : Configurer la sécurité réseau

La seule façon de sécuriser les données dans votre compte consiste à utiliser un réseau virtuel et d’autres paramètres de sécurité réseau. Tous les autres outils utilisé pour sécuriser les données, notamment l’autorisation de clé de compte, la sécurité Azure Active Directory (AD) et les listes de contrôle d’accès (ACL), ne sont pas encore pris en charge dans les comptes sur lesquels la prise en charge du protocole NFS 3.0 est activée.

Pour sécuriser les données dans votre compte, consultez les recommandations suivantes : [Recommandations de sécurité réseau pour Stockage Blob](security-recommendations.md#networking).

## <a name="step-5-create-and-configure-a-storage-account"></a>Étape 5 : Créer et configurer un compte de stockage

Pour monter un conteneur à l’aide de NFS 3.0, vous devez créer un compte de stockage **après** avoir inscrit la fonctionnalité avec votre abonnement. Vous ne pouvez pas activer les comptes qui existaient avant l’inscription de la fonctionnalité.

Dans la préversion de cette fonctionnalité, le protocole NFS 3.0 est pris en charge pour les comptes de stockage universel v2 standard et pour les comptes de stockage d’objets blob de blocs Premium. Pour plus d’informations sur ces types de comptes de stockage, consultez [Vue d’ensemble des comptes de stockage](../common/storage-account-overview.md).

Au fur et à mesure que vous configurez le compte, choisissez les valeurs suivantes :

|Paramètre | Niveau de performance Premium | Niveau de performance Standard  
|----|---|---|
|Emplacement|Toutes les régions disponibles |Toutes les régions disponibles    
|Performances|Premium| Standard
|Type de compte|BlockBlobStorage| Universel v2
|Réplication|Stockage localement redondant (LRS)| Stockage localement redondant (LRS), Stockage redondant interzone (ZRS)
|Méthode de connectivité|Point de terminaison public (réseaux sélectionnés) pour point de terminaison privé |Point de terminaison public (réseaux sélectionnés) pour point de terminaison privé
|Transfert sécurisé requis|Désactivé|Désactivé
|Espace de noms hiérarchique|activé|activé
|NFS V3|activé |activé 

Vous pouvez accepter les valeurs par défaut pour tous les autres paramètres. 

## <a name="step-6-create-a-container"></a>Étape 6 : Créez un conteneur.

Créez un conteneur dans votre compte de stockage à l’aide de l’un de ces outils ou kits de développement logiciel (SDK) :

|Outils|Kits SDK|
|---|---|
|[Azure portal](https://portal.azure.com)|[.NET](data-lake-storage-directory-file-acl-dotnet.md#create-a-container)|
|[AZCopy](../common/storage-use-azcopy-v10.md#transfer-data)|[Java](data-lake-storage-directory-file-acl-java.md)|
|[PowerShell](data-lake-storage-directory-file-acl-powershell.md#create-a-container)|[Python](data-lake-storage-directory-file-acl-python.md#create-a-container)|
|[Azure CLI](data-lake-storage-directory-file-acl-cli.md#create-a-container)|[JavaScript](data-lake-storage-directory-file-acl-javascript.md)|
||[REST](/rest/api/storageservices/create-container)|

## <a name="step-7-mount-the-container"></a>Étape 7 : Montez le conteneur

Créez un répertoire sur votre système Linux, puis montez un conteneur dans le compte de stockage.

1. Sur un système Linux, créez un répertoire.

   ```
   mkdir -p /mnt/test
   ```

2. Montez un conteneur à l’aide de la commande suivante.

   ```
   mount -o sec=sys,vers=3,nolock,proto=tcp <storage-account-name>.blob.core.windows.net:/<storage-account-name>/<container-name>  /mnt/test
   ```

   - Remplacez la valeur d’espace réservé `<storage-account-name>` qui s’affiche dans cette commande par le nom de votre compte de stockage.  

   - Remplacez l’espace réservé `<container-name>` par le nom de votre conteneur.

---

## <a name="resolve-common-issues"></a>Résoudre les problèmes courants

|Problème/erreur | Résolution|
|---|---|
|`Access denied by server while mounting`|Vérifiez que votre client s’exécute sur un sous-réseau pris en charge. Consultez les [emplacements réseau pris en charge](network-file-system-protocol-support.md#supported-network-connections).|
|`No such file or directory`| Vérifiez que le conteneur que vous montez a été créé après avoir vérifié l’inscription de la fonctionnalité. Consultez [Étape 2 : Vérifiez que la fonctionnalité est inscrite](#step-2-verify-that-the-feature-is-registered). De plus, veillez à taper la commande de montage et ses paramètres directement dans le terminal. Si vous copiez et collez une partie de cette commande dans le terminal à partir d’une autre application, les caractères masqués dans les informations collées peuvent provoquer l’apparition de cette erreur.|

## <a name="see-also"></a>Voir aussi

[Prise en charge du protocole NFS (Network File System) 3.0 dans le service Stockage Blob Azure (préversion)](network-file-system-protocol-support.md)
