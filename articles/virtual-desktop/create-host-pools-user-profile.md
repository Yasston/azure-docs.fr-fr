---
title: Partage de conteneurs de profil FSLogix Azure Virtual Desktop - Azure
description: Comment configurer un conteneur de profils FSLogix pour un pool d’hôtes Azure Virtual Desktop à l’aide d’un partage de fichiers basé sur une machine virtuelle.
author: Heidilohr
ms.topic: how-to
ms.date: 08/20/2019
ms.author: helohr
manager: femila
ms.openlocfilehash: a1398c23adcadaf245bae7271ed91e7d2a6763da
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111756092"
---
# <a name="create-a-profile-container-for-a-host-pool-using-a-file-share"></a>Créer un conteneur de profils pour un pool hôte à l’aide d’un partage de fichiers

Le service Azure Virtual Desktop offre des conteneurs de profils FSLogix en tant que solution de profil utilisateur recommandée. Nous recommandons de ne pas utiliser la solution Disque de profil utilisateur (UPD), qui sera dépréciée dans les futures versions d’Azure Virtual Desktop.

Cet article vous indique comment configurer un partage de conteneur de profils FSLogix pour un pool d’hôtes à l’aide d’un partage de fichiers basé sur machine virtuelle. Nous vous recommandons fortement d’utiliser Azure Files au lieu du partage de fichiers. Pour plus d’informations sur FSLogix, consultez le [site FSLogix](https://docs.fslogix.com/).

>[!NOTE]
>Si vous recherchez des éléments de comparaison sur les différentes options de stockage pour les conteneurs de profil FSLogix sur Azure, consultez [Options de stockage pour les conteneurs de profil FSLogix](store-fslogix-profile.md).

## <a name="create-a-new-virtual-machine-that-will-act-as-a-file-share"></a>Créer une machine virtuelle qui fera office de partage de fichiers

Lorsque vous créez la machine virtuelle, veillez à la placer sur le même réseau virtuel que les machines virtuelles du pool hôte ou sur un réseau virtuel disposant d’une connectivité avec les machines virtuelles du pool hôte. Vous pouvez créer une machine virtuelle de plusieurs façons :

- [Créer une machine virtuelle à partir d’une image Azure Gallery](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Créer une machine virtuelle à partir d’une image managée](../virtual-machines/windows/create-vm-generalized-managed.md)
- [Créer une machine virtuelle à partir d’une image non managée](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/vm-from-user-image)

Après avoir créé la machine virtuelle, joignez-la au domaine en procédant comme suit :

1. [Connectez-vous à la machine virtuelle](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) avec les informations d’identification que vous avez indiquées lors de la création de la machine virtuelle.
2. Sur la machine virtuelle, lancez le **Panneau de configuration** et sélectionnez **Système**.
3. Sélectionnez successivement **Nom de l’ordinateur**, **Modifier les paramètres** et **Modifier…**
4. Sélectionnez **Domaine**, puis entrez le domaine Active Directory sur le réseau virtuel.
5. Authentifiez-vous avec un compte de domaine qui dispose de privilèges d’accès sur les machines de jonction de domaine.

## <a name="prepare-the-virtual-machine-to-act-as-a-file-share-for-user-profiles"></a>Préparer la machine virtuelle pour qu’elle serve de partage de fichiers pour les profils utilisateur

Vous trouverez ci-dessous des instructions générales sur la préparation d’une machine virtuelle pour qu’elle serve de partage de fichiers pour les profils utilisateur :

1. Ajoutez les utilisateurs Active Directory d’Azure Virtual Desktop à un [groupe de sécurité Active Directory](/windows/security/identity-protection/access-control/active-directory-security-groups/). Ce groupe de sécurité sera utilisé pour authentifier les utilisateurs d’Azure Virtual Desktop auprès de la machine virtuelle du partage de fichiers que vous venez de créer.
2. [Connectez-vous à la machine virtuelle du partage de fichiers](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine).
3. Sur la machine virtuelle du partage de fichiers, créez un dossier sur le **lecteur C** qui sera utilisé comme partage de profil.
4. Cliquez avec le bouton droit sur le nouveau dossier, puis sélectionnez successivement **Propriétés**, **Partage** et **Partage avancé...**
5. Sélectionnez successivement **Partager ce dossier**, **Autorisations...**  et **Ajouter...**
6. Recherchez le groupe de sécurité dans lequel vous avez ajouté les utilisateurs d’Azure Virtual Desktop, puis vérifiez que ce groupe dispose de l’accès **Contrôle total**.
7. Après avoir ajouté le groupe de sécurité, cliquez avec le bouton droit sur le dossier, sélectionnez **Propriétés**, puis **Partage**, puis copiez le **Chemin d’accès réseau** pour une utilisation ultérieure.

Pour plus d’informations sur les autorisations, consultez la [documentation FSLogix](/fslogix/fslogix-storage-config-ht/).

## <a name="configure-the-fslogix-profile-container"></a>Configurer le conteneur de profils FSLogix

Pour configurer les machines virtuelles avec le logiciel FSLogix, procédez comme suit sur chaque machine inscrite dans le pool hôte :

1. [Connectez-vous à la machine virtuelle](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) avec les informations d’identification que vous avez indiquées lors de la création de la machine virtuelle.
2. Lancez un navigateur Internet et accédez à [ce lien](https://go.microsoft.com/fwlink/?linkid=2084562) pour télécharger l’agent FSLogix.
3. Accédez à \\\\Win32\\Release ou \\\\X64\\Release dans le fichier .zip et exécutez **FSLogixAppsSetup** pour installer l’agent FSLogix.  Pour en savoir plus sur l’installation de FSLogix, consultez la page [Télécharger et installer FSLogix](/fslogix/install-ht/).
4. Accédez à **Program Files** > **FSLogix** > **Apps** pour vérifier l’agent installé.
5. Dans le menu Démarrer, exécutez **RegEdit** en tant qu’administrateur. Accédez à **Computer\\HKEY_LOCAL_MACHINE\\software\\FSLogix**.
6. Créez une clé nommée **Profils**.
7. Créez les valeurs suivantes pour la clé Profils :

| Nom                | Type               | Données/Valeur                        |
|---------------------|--------------------|-----------------------------------|
| activé             | DWORD              | 1                                 |
| VHDLocations        | Valeur à chaînes multiples | « Chemin d’accès réseau pour le partage de fichiers »     |

>[!IMPORTANT]
>Pour contribuer à sécuriser votre environnement Azure Virtual Desktop dans Azure, nous vous recommandons de ne pas ouvrir le port entrant 3389 sur vos machines virtuelles. Azure Virtual Desktop ne nécessite pas l’ouverture du port entrant 3389 pour permettre aux utilisateurs d’accéder aux machines virtuelles du pool hôte. Si vous devez ouvrir le port 3389 pour résoudre des problèmes, nous vous recommandons d’utiliser un [accès à la machine virtuelle juste-à-temps](../security-center/security-center-just-in-time.md).
