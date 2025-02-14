---
title: Portail d’attachement d’application MSIX Azure Virtual Desktop – Azure
description: Comment configurer l’attachement d’application MSIX pour Azure Virtual Desktop à l’aide du portail Azure.
author: Heidilohr
ms.topic: how-to
ms.date: 04/13/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 0fe61e5ef36064234b65da4d059f02beed18f9a6
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111745706"
---
# <a name="set-up-msix-app-attach-with-the-azure-portal"></a>Configurer l’attachement d’application MSIX avec le portail Azure

Cet article vous guide tout au long de la configuration de l’attachement d’application MSIX dans un environnement Azure Virtual Desktop.

## <a name="requirements"></a>Spécifications

Voici ce dont vous avez besoin pour configurer l’attachement d’application MSIX :

- Un déploiement Azure Virtual Desktop opérationnel. Pour apprendre à déployer Azure Virtual Desktop (classique), consultez [Création d’un locataire dans Azure Virtual Desktop](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md). Pour apprendre à déployer Azure Virtual Desktop avec intégration Azure Resource Manager, consultez [Création d’un pool d’hôtes avec le portail Azure](./create-host-pools-azure-marketplace.md).
- Un pool d’hôtes Azure Virtual Desktop avec au moins un hôte de session actif.
- Outil de d’empaquetage MSIX.
- Une application empaquetée au format MSIX et développée en image MSIX qui est chargée dans un partage de fichiers.
- Un partage de fichiers dans votre déploiement Azure Virtual Desktop où le package MSIX sera stocké.
- Le partage de fichiers dans lequel vous avez chargé l’image MSIX doit également être accessible à toutes les machines virtuelles du pool d’hôtes. Les utilisateurs auront besoin d’autorisations en lecture seule pour accéder à l’image.
- Si le certificat n’est pas approuvé publiquement, suivez les instructions dans [Installer des certificats](app-attach.md#install-certificates).

## <a name="turn-off-automatic-updates-for-msix-app-attach-applications"></a>Désactiver les mises à jour automatiques pour les applications d’attachement d’application MSIX

Avant de commencer, vous devez désactiver les mises à jour automatiques pour les applications d’attachement d’application MSIX. Pour désactiver les mises à jour automatiques, vous devez exécuter les commandes suivantes dans une invite de commandes avec élévation de privilèges :

```cmd
rem Disable Store auto update:

reg add HKLM\Software\Policies\Microsoft\WindowsStore /v AutoDownload /t REG_DWORD /d 0 /f
Schtasks /Change /Tn "\Microsoft\Windows\WindowsUpdate\Automatic app update" /Disable
Schtasks /Change /Tn "\Microsoft\Windows\WindowsUpdate\Scheduled Start" /Disable

rem Disable Content Delivery auto download apps that they want to promote to users:

reg add HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v PreInstalledAppsEnabled /t REG_DWORD /d 0 /f

reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\Debug /v ContentDeliveryAllowedOverride /t REG_DWORD /d 0x2 /f

```

## <a name="configure-the-msix-app-attach-management-interface"></a>Configuration de l’interface de gestion de l’attachement d’application MSIX

Ensuite, vous devez télécharger et configurer l’interface de gestion de l’attachement d’application MSIX pour le portail Azure.

Pour configurer l’interface de gestion :

1. [Ouvrez le portail Azure](https://portal.azure.com).
2. Si vous recevez une invite vous demandant si vous jugez l’extension digne de confiance, sélectionnez **Autoriser**.

      > [!div class="mx-imgBorder"]
      > ![Capture d’écran de la fenêtre des extensions non approuvées. « Autoriser » est mis en évidence en rouge.](media/untrusted-extensions.png)


## <a name="add-an-msix-image-to-the-host-pool"></a>Ajouter une image MSIX au pool d’hôtes

Vous devez ensuite ajouter l’image MSIX à votre pool d’hôtes.

Pour ajouter l’image MSIX :

1. Ouvrez le portail Azure.

2. Entrez **Azure Virtual Desktop** dans la barre de recherche, puis sélectionnez le nom du service.

3. Sélectionnez le pool d’hôtes dans lequel vous envisagez de placer les applications MSIX.

4. Sélectionnez **Packages MSIX** pour ouvrir la grille de données avec tous les **packages MSIX** actuellement ajoutés au pool d’hôtes.

5. Sélectionnez **+ Ajouter** pour ouvrir l’onglet **Ajouter un package MSIX**.

6. Dans l’onglet **Ajouter un package MSIX**, entrez les valeurs suivantes :

      - Pour **Chemin d’accès de l’image MSIX**, entrez un chemin d’accès UNC valide pointant vers l’image MSIX sur le partage de fichiers. (Par exemple, `\\storageaccount.file.core.windows.net\msixshare\appfolder\MSIXimage.vhd`.) Lorsque vous avez terminé, sélectionnez **Ajouter** pour interroger le conteneur MSIX afin de vérifier si le chemin d’accès est valide.

      - Pour **Package MSIX**, sélectionnez le nom du package MSIX approprié dans le menu déroulant. Ce menu n’est renseigné que si vous avez entré un chemin d’image valide dans **Chemin d’accès de l’image MSIX**.

      - Pour **Applications de package**, assurez-vous que la liste contient toutes les applications MSIX que vous souhaitez mettre à la disposition des utilisateurs dans votre package MSIX.

      - Facultativement, entrez un **nom d’affichage** si vous souhaitez que votre package dispose d’un nom plus convivial dans vos déploiements utilisateur.

      - Assurez-vous que le numéro de version approprié est renseigné dans la propriété **Version**.

      - Sélectionnez le **type d’inscription** que vous souhaitez utiliser. Celui que vous utilisez dépend de vos besoins :

    - **L’inscription à la demande** diffère l’inscription complète de l’application MSIX jusqu’à ce que l’utilisateur démarre l’application. Il s’agit du type d’inscription que nous vous recommandons d’utiliser.

    - Le **blocage à l’ouverture de session** procède à l’inscription uniquement lorsque l’utilisateur se connecte. Nous le déconseillons, car il peut entraîner des temps de connexion plus longs pour les utilisateurs.

7.  Pour **État**, sélectionnez votre état par défaut.
    -  L’état **Actif** permet aux utilisateurs d’interagir avec le package.
    -  L’état **Inactif** amène Azure Virtual Desktop à ignorer le package et à ne pas le remettre aux utilisateurs.

8. Une fois que vous avez terminé, sélectionnez **Ajouter**.

## <a name="publish-msix-apps-to-an-app-group"></a>Publier des applications MSIX dans un groupe d’applications

Ensuite, vous devez publier les applications dans le package. Vous devez procéder ainsi pour les groupes d’applications de bureau et distants.

Si vous disposez déjà d’une image MSIX, passez directement à l’étape [Publier des applications MSIX dans un groupe d’applications](#publish-msix-apps-to-an-app-group). Si vous souhaitez tester des applications héritées, suivez les instructions de [Créer un package MSIX à partir d’un programme d’installation de bureau](/windows/msix/packaging-tool/create-app-package-msi-vm/) pour convertir l’application héritée en package MSIX.

Pour publier les applications :

1. Dans le fournisseur de ressources Azure Virtual Desktop, sélectionnez l’onglet **Groupes d’applications**.

2. Sélectionnez le groupe d’applications dans lequel vous souhaitez publier les applications.

   >[!NOTE]
   >Les applications MSIX peuvent être livrées avec l’attachement d’application MSIX aux groupes d’applications de bureau et distants.

3. Une fois que vous êtes dans le groupe d’applications, sélectionnez l’onglet **Applications**. La grille **Applications** affiche toutes les applications existantes dans le groupe d’applications.

4. Sélectionnez **+ Ajouter** pour ouvrir l’onglet **Ajouter une application**.

      > [!div class="mx-imgBorder"]
      > ![Capture d’écran de l’utilisateur sélectionnant + Ajouter pour ouvrir l’onglet Ajouter une application](media/select-add.png)

5. Pour **Source de l’application**, choisissez la source de votre application.
    - Si vous utilisez un groupe d’applications de bureau, choisissez **Package MSIX**.
      
      > [!div class="mx-imgBorder"]
      > ![Capture d’écran d’un client sélectionnant le package MSIX dans le menu déroulant de la source de l’application. « Package MSIX » est mis en évidence en rouge.](media/select-source.png)
    
    - Si vous utilisez un groupe d’applications distant, choisissez l’une des options suivantes :
        
        - Menu Démarrer
        - Chemin de l’application
        - Package MSIX

    - Pour **Nom de l’application**, entrez un nom descriptif pour l’application.

    Vous pouvez également configurer les fonctionnalités facultatives suivantes :
   
    - Pour **Nom d’affichage**, entrez un nouveau nom pour le package que vos utilisateurs verront.

    - Pour **Description**, entrez une brève description du package de l’application.

    - Si vous utilisez un groupe d’applications distant, vous pouvez également configurer les options suivantes :

        - **Chemin d’accès à l’icône**
        - **Index d’icône**
        - **Afficher dans le flux web**

6. Quand vous avez terminé, sélectionnez **Enregistrer**.

>[!NOTE]
>Lorsqu’un utilisateur est affecté à un groupe d’applications distant et à un groupe d’applications de bureau à partir du même pool d’hôtes, le groupe d’applications de bureau s’affiche dans le flux.

## <a name="assign-a-user-to-an-app-group"></a>Affecter un utilisateur à un groupe d’applications

Après avoir attribué des applications MSIX à un groupe d’applications, vous devez autoriser les utilisateurs à y accéder. Vous pouvez attribuer l’accès en ajoutant des utilisateurs ou des groupes d’utilisateurs à un groupe d’applications avec des applications MSIX publiées. Suivez les instructions décrites dans [Gérer des groupes d’applications avec le portail Azure](manage-app-groups.md) pour affecter vos utilisateurs à un groupe d’applications.

>[!NOTE]
>Les applications distantes avec attachement d’application MSIX peuvent disparaître du flux lorsque vous testez des applications distantes dans la préversion publique. Les applications ne s’affichent pas, car le pool d’hôtes que vous utilisez dans l’environnement d’évaluation est pris en charge par un service Broker pour les connexions Bureau à distance dans l’environnement de production. Dans la mesure où le service Broker pour les connexions Bureau à distance de l’environnement de production n’enregistre pas la présence des applications distantes avec attachement d’application MSIX, les applications ne figureront pas dans le flux.

## <a name="change-msix-package-state"></a>Modifier l’état du package MSIX

Ensuite, vous devez modifier l’état du package MSIX en **Actif** ou **Inactif**, en fonction de ce que vous souhaitez faire avec le package. Les packages actifs sont des packages avec lesquels vos utilisateurs peuvent interagir une fois qu’ils sont publiés. Les packages inactifs sont ignorés par Azure Virtual Desktop, de sorte que vos utilisateurs ne peuvent pas interagir avec les applications qui s’y trouvent.

### <a name="change-state-with-the-applications-list"></a>Modifier l’état avec la liste Applications

Pour modifier l’état du package avec la liste Applications :

1. Accédez à votre pool d’hôtes et sélectionnez **Packages MSIX**. Vous devez voir une liste de tous les packages MSIX existants dans le pool d’hôtes.

2. Sélectionnez les packages MSIX dont vous devez modifier l’état, puis sélectionnez **Modifier l’état**.

### <a name="change-state-with-update-package"></a>Modifier l’état avec un package de mise à jour

Pour modifier l’état du package avec un package de mise à jour :

1. Accédez à votre pool d’hôtes et sélectionnez **Packages MSIX**. Vous devez voir une liste de tous les packages MSIX existants dans le pool d’hôtes.

2. Sélectionnez le nom du package dont vous souhaitez modifier l’état dans la liste des packages MSIX. L’onglet **Mettre à jour le package** s’ouvre.

3. Cliquez sur le bouton bascule **État** pour passer à l’état **Inactif** ou **Actif**, puis sélectionnez **Enregistrer**.

## <a name="change-msix-package-registration-type"></a>Modifier le type d’inscription du package MSIX

Pour modifier le type d’inscription du package :

1. Sélectionnez **Packages MSIX**. Vous devez voir une liste de tous les packages MSIX existants dans le pool d’hôtes.

2. Sélectionnez **Nom du package** dans la **grille des packages MSIX** ; cela ouvre le panneau permettant de mettre à jour le package.

3. Activez ou désactivez le **type d’inscription** à l’aide du bouton **À la demande/blocage à l’ouverture de session** selon votre choix, puis sélectionnez **Enregistrer.**

## <a name="remove-an-msix-package"></a>Supprimer un package MSIX

Pour supprimer un package MSIX de votre pool d’hôtes :

1. Sélectionnez **Packages MSIX**.  Vous devez voir une liste de tous les packages MSIX existants dans le pool d’hôtes.

2. Sélectionnez les points de suspension à droite du nom du package à supprimer, puis sélectionnez **Supprimer**.

## <a name="remove-msix-apps"></a>Supprimer des applications MSIX

Pour supprimer des applications MSIX individuelles de votre package :

1. Accédez au pool d’hôtes et sélectionnez **Groupes d’applications**.

2. Sélectionnez le groupe d’applications dans lequel vous souhaitez supprimer des applications.

3. Ouvrez l’onglet **Applications**.

4. Sélectionnez l’application que vous souhaitez supprimer, puis sélectionnez **Supprimer**.

## <a name="next-steps"></a>Étapes suivantes

Posez vos questions concernant cette fonctionnalité à la [communauté technique d’Azure Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop).

Vous pouvez également formuler vos commentaires concernant Azure Virtual Desktop sur le [Hub de commentaires Azure Virtual Desktop](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).

Voici d’autres articles qui peuvent vous être utiles :

- [Glossaire Attachement d’application MSIX](app-attach-glossary.md)
- [FAS sur l’attachement d’application MSIX](app-attach-faq.md)
