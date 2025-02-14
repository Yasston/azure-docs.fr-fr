---
title: Fichiers conteneurs de profils FSLogix Azure Virtual Desktop – Azure
description: Cet article décrit les conteneurs de profils FSLogix dans Azure Virtual Desktop et Azure Files.
author: Heidilohr
ms.topic: conceptual
ms.date: 01/04/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: c7b8b400d2f927fa4b5d6f713b29dbda34eec959
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111757676"
---
# <a name="fslogix-profile-containers-and-azure-files"></a>Conteneurs de profil FSLogix et fichiers Azure

Le service Azure Virtual Desktop recommande comme solution de profil utilisateur les conteneurs de profils FSLogix. FSLogix est conçu pour l’itinérance des profils dans des environnements informatiques à distance comme Azure Virtual Desktop. Il stocke un profil utilisateur complet dans un seul conteneur. Lors de la connexion, ce conteneur est dynamiquement attaché à l’environnement informatique en utilisant le disque dur virtuel et le disque dur virtuel Hyper-V pris en charge en mode natif. Le profil utilisateur est immédiatement disponible et apparaît dans le système exactement comme un profil utilisateur natif. Cet article décrit le fonctionnement des conteneurs de profils FSLogix utilisés avec Azure Files dans Azure Virtual Desktop.

>[!NOTE]
>Si vous recherchez des éléments de comparaison sur les différentes options de stockage pour les conteneurs de profil FSLogix sur Azure, consultez [Options de stockage pour les conteneurs de profil FSLogix](store-fslogix-profile.md).

## <a name="user-profiles"></a>Profils utilisateur

Un profil utilisateur contient des éléments de données sur une personne, notamment des informations de configuration comme les paramètres du poste de travail, des connexions réseau persistantes et des paramètres d’application. Par défaut, Windows crée un profil utilisateur local qui est étroitement intégré au système d’exploitation.

Un profil utilisateur à distance fournit une partition entre les données utilisateur et le système d’exploitation. Il permet le remplacement ou le changement du système d’exploitation sans affecter les données utilisateur. Dans l’hôte de session Bureau à distance (RDSH) et Virtual Desktop Infrastructure (VDI), le système d’exploitation peut être remplacé pour les raisons suivantes :

- Une mise à niveau du système d’exploitation
- Le remplacement d’une Machine virtuelle existante
- Un utilisateur faisant partie d’un environnement RDSH ou VDI (non persistant) mis en pool

Les produits Microsoft fonctionnent avec plusieurs technologies pour les profils utilisateur distants, notamment ces technologies :
- Profils utilisateur itinérants (RUP, Roaming user profiles)
- Disques de profil utilisateur (UPD, User profile disks)
- Enterprise State Roaming (ESR)

UPD et RUP sont les technologies les plus fréquemment utilisées pour les profils utilisateur dans les environnements Hôte de session Bureau à distance (RDSH) et Disque dur virtuel (VHD).

### <a name="challenges-with-previous-user-profile-technologies"></a>Problèmes posés par les technologies de profil utilisateur antérieures

Les solutions Microsoft existantes et héritées pour les profils utilisateur posaient différents problèmes. Aucune solution antérieure ne gérait tous les besoins en termes de profils utilisateur liés à un environnement RDSH ou VDI. Par exemple, UPD ne peut pas gérer les grands fichiers OST et RUP ne conserve pas les paramètres modernes.

#### <a name="functionality"></a>Fonctionnalités

Le tableau suivant présente les avantages et les limitations des technologies de profil utilisateur antérieures.

| Technology | Paramètres modernes | Paramètres Win32 | Paramètres du système d’exploitation | Données utilisateur | Prise en charge sur la référence SKU de serveur | Stockage back-end sur Azure | Stockage back-end local | Prise en charge de la version | Moment de la connexion suivante |Notes|
| ---------- | :-------------: | :------------: | :---------: | --------: | :---------------------: | :-----------------------: | :--------------------------: | :-------------: | :---------------------: |-----|
| **Disques de profil utilisateur (UPD)** | Oui | Oui | Oui | Oui | Oui | Non | Oui | Win 7+ | Oui | |
| **Profils utilisateur itinérants (RUP), mode Maintenance** | Non | Oui | Oui | Oui | Oui| Non | Oui | Win 7+ | Non | |
| **Enterprise State Roaming (ESR)** | Oui | Non | Oui | Non | Voir les remarques | Oui | Non | Win 10 | Non | Fonctions sur la référence SKU de serveur, mais pas d’interface utilisateur de prise en charge |
| **User Experience Virtualization (UE-V)** | Oui | Oui | Oui | Non | Oui | Non | Oui | Win 7+ | Non |  |
| **Fichiers cloud OneDrive** | Non | Non | Non | Oui | Voir les remarques | Voir les remarques  | Voir les remarques | Win 10 RS3 | Non | Non testé sur la référence SKU de serveur. Le stockage back-end sur Azure dépend du client de synchronisation. Le stockage back-end local dépend du client de synchronisation. |

#### <a name="performance"></a>Performances

UPD nécessite des [espaces de stockage direct (S2D, Storage Spaces Direct)](/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment/) pour répondre aux exigences de performances. UPD utilise le protocole SMB (Server Message Block) Il copie le profil sur la machine virtuelle à laquelle l’utilisateur se connecte.

#### <a name="cost"></a>Coût

Si les clusters S2D atteignent bien les performances nécessaires, le coût est élevé pour les entreprises clientes, mais il l’est encore davantage pour les PME clientes. Pour cette solution, les entreprises paient pour des disques de stockage ainsi que pour les machines virtuelles qui utilisent les disques pour un partage.

#### <a name="administrative-overhead"></a>Charge d’administration importante

Les clusters S2D nécessitent un système d’exploitation corrigé, mis à jour, et maintenu dans un état sécurisé. Ces processus et la complexité de la configuration de la reprise d’activité de S2D le rendent possible seulement pour les entreprises disposant d’une équipe informatique dédiée.

## <a name="fslogix-profile-containers"></a>Conteneurs de profil FSLogix

Le 19 novembre 2018, [Microsoft a acquis FSLogix](https://blogs.microsoft.com/blog/2018/11/19/microsoft-acquires-fslogix-to-enhance-the-office-365-virtualization-experience/). FSLogix résout de nombreux problèmes liés aux conteneurs de profil. Les plus importants sont :

- **Performances :** Les [conteneurs de profil FSLogix](/fslogix/configure-profile-container-tutorial/) sont à hautes performances et résolvent les problèmes de performances qui bloquaient jusque là le mode d’échange avec mise en cache.
- **OneDrive :** Sans les conteneurs de profil FSLogix, OneDrive Entreprise n’est pas pris en charge dans les environnements RDSH ou VDI non persistants. La [page de support de OneDrive VDI](/onedrive/sync-vdi-support) vous indique comment ils interagissent. Pour plus d’informations, consultez [Utiliser le client de synchronisation sur les Bureaux virtuels](/deployoffice/rds-onedrive-business-vdi/).
- **Dossiers supplémentaires :** FSLogix offre la possibilité d’étendre les profils utilisateur pour y inclure des dossiers supplémentaires.

Depuis cette acquisition, Microsoft a commencé à remplacer les solutions de profil utilisateur existantes, comme UPD, par les conteneurs de profil FSLogix.

## <a name="azure-files-integration-with-azure-active-directory-domain-service"></a>Intégration d’Azure Files au service de domaine Azure Active Directory

Les fonctionnalités et les performances des conteneurs de profil FSLogix tirent parti du cloud. Le 7 août 2019, Microsoft Azure Files a annoncé la mise à la disponibilité générale de l’[authentification Azure Files avec Azure Active Directory Domaine Services (AD DS)](../storage/files/storage-files-active-directory-overview.md). En résolvant les problèmes liés aux coûts et à la charge d’administration, Azure Files avec authentification Azure AD DS constitue une solution de premier plan pour les profils utilisateur dans le service Azure Virtual Desktop.

## <a name="best-practices-for-azure-virtual-desktop"></a>Meilleures pratiques pour Azure Virtual Desktop

Azure Virtual Desktop offre un contrôle total sur la taille, le type et le nombre de machines virtuelles utilisées par les clients. Pour plus d’informations, consultez [Présentation d’Azure Virtual Desktop](overview.md).

Vérifiez que votre environnement Azure Virtual Desktop suit les meilleures pratiques :

- Le compte de stockage Azure Files doit se trouver dans la même région que les machines virtuelles hôtes de la session.
- Les autorisations Azure Files doivent correspondre aux autorisations décrites dans [Exigences - Conteneurs de profil](/fslogix/fslogix-storage-config-ht).
- Chaque machine virtuelle du pool d’hôtes doit être générée à partir de la même image principale et être de même type et de même taille.
- Chaque machine virtuelle du pool d’hôtes doit être dans même groupe de ressources afin de faciliter la gestion, la mise à l’échelle et la mise à jour.
- Pour des performances optimales, la solution de stockage et le conteneur de profil FSLogix doivent se trouver à un même emplacement du centre de données.
- Le compte de stockage contenant l’image principale doit être dans la même région et le même abonnement où les machines virtuelles sont provisionnées.

## <a name="next-steps"></a>Étapes suivantes

Utilisez les guides suivants pour configurer un environnement Azure Virtual Desktop.

- Pour commencer à créer votre solution de virtualisation des postes de travail, consultez [Création d’un locataire dans Azure Virtual Desktop](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md).
- Pour créer un pool d’hôtes dans votre locataire Azure Virtual Desktop, consultez [Création d’un pool d’hôtes avec la Place de marché Azure](create-host-pools-azure-marketplace.md).
- Pour configurer des partages de fichiers complètement managés dans le cloud, consultez [Configurer un partage Azure Files](/azure/storage/files/storage-files-active-directory-enable/).
- Pour configurer des conteneurs de profils FSLogix, consultez [Créer un conteneur de profils pour un pool d'hôtes à l'aide d'un partage de fichiers](create-host-pools-user-profile.md).
- Pour affecter des utilisateurs à un pool d’hôtes, consultez [Gestion des groupes d’applications pour Azure Virtual Desktop](manage-app-groups.md).
- Pour accéder à vos ressources Azure Virtual Desktop dans un navigateur web, consultez [Connexion à Azure Virtual Desktop](connect-web.md).
