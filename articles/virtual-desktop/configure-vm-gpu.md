---
title: Configurer le GPU pour Azure Virtual Desktop - Azure
description: Découvrez comment activer le rendu et le codage avec accélération GPU dans Azure Virtual Desktop.
author: gundarev
ms.topic: how-to
ms.date: 05/06/2019
ms.author: denisgun
ms.openlocfilehash: e55564ab1534b145958e128f58d50911ae9c51fa
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111746282"
---
# <a name="configure-graphics-processing-unit-gpu-acceleration-for-azure-virtual-desktop"></a>Configurer l’accélération GPU pour Azure Virtual Desktop

>[!IMPORTANT]
>Ce contenu s’applique à Azure Virtual Desktop avec des objets Azure Virtual Desktop pour Azure Resource Manager. Si vous utilisez Azure Virtual Desktop (classique) sans objets Azure Resource Manager, consultez [cet article](./virtual-desktop-fall-2019/configure-vm-gpu-2019.md).

Azure Virtual Desktop prend en charge le rendu et le codage avec accélération GPU pour des performances d’applications et une extensibilité améliorées. L’accélération GPU est particulièrement importante pour les applications graphiques.

Suivez les instructions de cet article pour créer une machine virtuelle Azure optimisée pour le GPU, ajoutez-la à votre pool d’hôtes et configurez-la pour utiliser l’accélération GPU à des fins de codage et de rendu. Cet article suppose que vous avez déjà un locataire Azure Virtual Desktop configuré.

## <a name="select-an-appropriate-gpu-optimized-azure-virtual-machine-size"></a>Sélectionner une taille de machine virtuelle Azure optimisée pour le GPU appropriée

Sélectionnez l’une des tailles de machine virtuelle Azure de la [série NV](../virtual-machines/nv-series.md), [NVv3](../virtual-machines/nvv3-series.md) ou [NVv4](../virtual-machines/nvv4-series.md). Ces tailles sont adaptées à la virtualisation des applications et des appareils de bureau et permettent à la plupart des applications et à l’interface utilisateur Windows d’être accélérées par GPU. Le bon choix pour votre pool d’hôtes dépend de plusieurs facteurs, notamment vos charges de travail d’applications spécifiques, la qualité souhaitée de l’expérience utilisateur et le coût. En général, les GPU plus grands et plus efficaces offrent une meilleure expérience utilisateur à une densité d’utilisateurs donnée, tandis que les tailles de GPU plus petites et fractionnaires permettent un contrôle plus précis de la qualité et du coût.

>[!NOTE]
>Les machines virtuelles de la série NC, NCv2, NCv3, ND et NDv2 d’Azure ne conviennent généralement pas aux hôtes de session Azure Virtual Desktop. Ces machines virtuelles sont adaptées à des outils de calcul ou de Machine Learning hautes performances spécialisés, comme ceux créés avec NVIDIA CUDA. Elles ne prennent pas en charge l’accélération par GPU pour la plupart des applications ni pour l’interface utilisateur Windows.

## <a name="create-a-host-pool-provision-your-virtual-machine-and-configure-an-app-group"></a>Créer un pool d’hôtes, provisionner votre machine virtuelle et configurer un groupe d’applications

Créez un nouveau pool d’hôtes à l’aide d’une machine virtuelle de la taille sélectionnée. Pour obtenir des instructions, consultez [Tutoriel : Créer un pool d’hôtes avec le portail Azure](./create-host-pools-azure-marketplace.md).

Azure Virtual Desktop prend en charge le rendu et le codage avec accélération GPU dans les systèmes d’opération suivants :

* Windows 10 version 1511 ou ultérieure
* Windows Server 2016 ou version ultérieure

Vous devez également configurer un groupe d’applications ou utilisez le groupe d’applications bureau par défaut (nommé « Groupe d’applications bureau ») qui est créé automatiquement lorsque vous créez un nouveau pool d’hôtes. Pour obtenir des instructions, consultez [Tutoriel : Gérer des groupes d’applications pour Azure Virtual Desktop](./manage-app-groups.md).

## <a name="install-supported-graphics-drivers-in-your-virtual-machine"></a>Installer des pilotes graphiques pris en charge dans votre machine virtuelle

Pour tirer parti des fonctionnalités GPU des machines virtuelles Azure de série N dans Azure Virtual Desktop, vous devez installer des pilotes graphiques appropriés. Suivez les instructions de la rubrique [Systèmes d’exploitation et pilotes pris en charge](../virtual-machines/sizes-gpu.md#supported-operating-systems-and-drivers) pour installer les pilotes. Seuls les pilotes distribués par Azure sont pris en charge.

* Pour les machines virtuelles Azure de série NV ou NVv3, seuls les pilotes NVIDIA GRID, et non les pilotes NVIDIA CUDA, prennent en charge l’accélération par GPU pour la plupart des applications et pour l’interface utilisateur Windows. Si vous choisissez d’installer les pilotes manuellement, veillez à installer les pilotes GRID. Si vous choisissez d’installer les pilotes à l’aide de l’extension de machine virtuelle Azure, les pilotes GRID sont automatiquement installés pour ces tailles de machine virtuelle.
* Pour les machines virtuelles Azure de série NVv4, installez les pilotes AMD fournis par Azure. Vous pouvez les installer automatiquement à l’aide de l’extension de machine virtuelle Azure, ou vous pouvez les installer manuellement.

Après l’installation du pilote, le redémarrage de la machine virtuelle est nécessaire. Utilisez les étapes de vérification dans les instructions ci-dessus pour confirmer que les pilotes graphiques ont été correctement installés.

## <a name="configure-gpu-accelerated-app-rendering"></a>Configurer le rendu d’application avec accélération GPU

Par défaut, le rendu des applications et des bureaux exécutés dans des configurations multi-session est effectué avec le processeur et n’exploite pas le GPU disponible. Configurer la stratégie de groupe pour l’hôte de session afin d’activer le rendu avec accélération GPU :

1. Connectez-vous au bureau de la machine virtuelle à l’aide d’un compte avec des privilèges d’administrateur local.
2. Ouvrez le menu Démarrer, puis tapez « gpedit.msc » pour ouvrir l’Éditeur de stratégie de groupe.
3. Naviguez dans l’arborescence vers **Configuration de l’ordinateur** > **Modèles d’administration** > **Composants Windows** > **Services Bureau à distance** > **Hôte de session Bureau à distance** > **Environnement de session à distance**.
4. Sélectionnez la stratégie **Utiliser les adaptateurs graphiques matériels pour toutes les sessions Services Bureau à distance** et définissez-la sur **Activé** pour activer le rendu GPU dans la session à distance.

## <a name="configure-gpu-accelerated-frame-encoding"></a>Configurer le codage d’image avec accélération GPU

Le Bureau à distance code tous les graphiques rendus par les applications et les bureaux (que le rendu ait été fait avec le GPU ou l’UC) pour la transmission aux clients Bureau à distance. Quand une partie de l’écran est fréquemment mise à jour, cette partie de l’écran est encodée à l’aide d’un codec vidéo (H.264/AVC). Par défaut, le Bureau à distance n’exploite pas les GPU disponibles pour ce codage. Configurez la stratégie de groupe pour l’hôte de session afin d’activer le codage d’image avec accélération GPU. Continuez les étapes ci-dessus :

>[!NOTE]
>L’encodage de trames avec accélération GPU n’est pas disponible dans les machines virtuelles de la série NVv4.

1. Sélectionnez la stratégie **Configurer le codage matériel H.264/AVC pour les connexions Bureau à distance** et définissez-la sur **Activé** pour activer le codage de matériel pour AVC/H.264 dans la session à distance.

    >[!NOTE]
    >Dans Windows Server 2016, définissez l’option **Préférer le codage matériel AVC** sur **Toujours essayer**.

2. Maintenant que les stratégies de groupe ont été modifiées, forcez une mise à jour de la stratégie de groupe. Ouvrez l’invite de commandes et tapez :

    ```cmd
    gpupdate.exe /force
    ```

3. Déconnectez-vous de la session Bureau à distance.

## <a name="configure-fullscreen-video-encoding"></a>Configurer l’encodage vidéo plein écran

Si vous utilisez souvent des applications qui produisent un contenu à fréquence d’images élevée, tel que la modélisation 3D, les applications CAO/FAO et vidéo, vous pouvez choisir d’activer un encodage vidéo plein écran pour une session à distance. Le profil vidéo en plein écran fournit une fréquence d’images supérieure et une meilleure expérience utilisateur pour ces applications au détriment de la bande passante réseau et des ressources de l’hôte de session et du client. Il est recommandé d’utiliser l’encodage de trames avec accélération GPU pour un encodage vidéo plein écran. Configurez la stratégie de groupe pour l’hôte de session afin d’activer le codage d’image en plein écran. Continuez les étapes ci-dessus :

1. Sélectionnez la stratégie **Prioriser le mode H.264/AVC 444 Graphics pour les connexions Bureau à distance** et définissez-la sur **Activé** pour forcer le codec H.264/AVC 444 dans la session à distance.
2. Maintenant que les stratégies de groupe ont été modifiées, forcez une mise à jour de la stratégie de groupe. Ouvrez l’invite de commandes et tapez :

    ```cmd
    gpupdate.exe /force
    ```

3. Déconnectez-vous de la session Bureau à distance.
## <a name="verify-gpu-accelerated-app-rendering"></a>Vérifier le rendu d’application avec accélération GPU

Pour vérifier que les applications utilisent le GPU pour le rendu, essayez l’une des opérations suivantes :

* Pour les machines virtuelles Azure dotées d’un GPU NVIDIA, utilisez l’utilitaire `nvidia-smi` comme décrit dans [Vérifier l’installation du pilote](../virtual-machines/windows/n-series-driver-setup.md#verify-driver-installation) pour vérifier l’utilisation du GPU lors de l’exécution de vos applications.
* Dans les versions de système d’exploitation prises en charge, vous pouvez utiliser le Gestionnaire des tâches pour vérifier l’utilisation du GPU. Sélectionnez le GPU dans l’onglet « Performances » pour voir si les applications utilisent le GPU.

## <a name="verify-gpu-accelerated-frame-encoding"></a>Vérifier le codage d’image avec accélération GPU

Pour vérifier que le Bureau à distance utilise le codage avec accélération GPU :

1. Connectez-vous au bureau de la machine virtuelle à l’aide du client Azure Virtual Desktop.
2. Lancer l’Observateur d’événements et accédez au nœud suivant : **Journaux des applications et services** > **Microsoft** > **Windows** > **RemoteDesktopServices-RdpCoreCDV** > **Opérationnel**
3. Pour déterminer si de codage avec accélération GPU est utilisé, recherchez l’ID d’événement 170. Si vous voyez « Encodeur matériel AVC activé : 1 » alors le codage GPU est utilisé.

## <a name="verify-fullscreen-video-encoding"></a>Vérifier l’encodage vidéo plein écran

Pour vérifier que le Bureau à distance utilise le codage en plein écran :

1. Connectez-vous au bureau de la machine virtuelle à l’aide du client Azure Virtual Desktop.
2. Lancer l’Observateur d’événements et accédez au nœud suivant : **Journaux des applications et services** > **Microsoft** > **Windows** > **RemoteDesktopServices-RdpCoreCDV** > **Opérationnel**
3. Pour déterminer si de codage en plein écran est utilisé, recherchez l’ID d’événement 162. Si vous voyez « AVC disponible : 1 Profil initial : 2048 » alors AVC 444 est utilisé.

## <a name="next-steps"></a>Étapes suivantes

Grâce à ces instructions, vous pouvez commencer à utiliser l’accélération GPU sur un hôte de session unique (une machine virtuelle). Considérations supplémentaires pour l’activation de l’accélération GPU sur un plus grand pool d’hôtes :

* Envisagez d’utiliser une [extension de machine virtuelle](../virtual-machines/extensions/overview.md) pour simplifier l’installation et les mises à jour des pilotes sur plusieurs machines virtuelles. Utilisez l’[extension du pilote GPU NVIDIA](../virtual-machines/extensions/hpccompute-gpu-windows.md) pour les machines virtuelles dotées de GPU NVIDIA et utilisez l’[extension du pilote GPU](../virtual-machines/extensions/hpccompute-amd-gpu-windows.md) AMD pour les machines virtuelles dotées de GPU AMD.
* Envisagez d’utiliser la stratégie de groupe Active Directory pour simplifier la configuration de la stratégie de groupe sur plusieurs machines virtuelles. Pour plus d’informations sur le déploiement de la stratégie de groupe dans le domaine Active Directory, consultez [Utilisation d’objets stratégie de groupe](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731212(v=ws.11)).