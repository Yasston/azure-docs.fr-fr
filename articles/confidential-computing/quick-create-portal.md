---
title: 'Démarrage rapide : Créer une machine virtuelle d’informatique confidentielle Azure dans le portail Azure'
description: Commencez vos déploiements en apprenant à créer rapidement une machine virtuelle d’informatique confidentielle dans le portail Azure.
author: JBCook
ms.service: virtual-machines
ms.subservice: workloads
ms.workload: infrastructure
ms.topic: quickstart
ms.date: 06/13/2021
ms.author: JenCook
ms.openlocfilehash: 8fb93b7697e2dd9077995572fc91b6e82a7d8512
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/30/2021
ms.locfileid: "113107215"
---
# <a name="quickstart-deploy-an-azure-confidential-computing-vm-in-the-azure-portal"></a>Démarrage rapide : Déployer une machine virtuelle d’informatique confidentielle Azure dans le portail Azure

Découvrez comment commencer à utiliser l’informatique confidentielle Azure avec le portail Azure pour créer une machine virtuelle reposant sur Intel SGX. Vous pourrez ensuite exécuter des applications d’enclave.

Nous vous recommandons de suivre ce tutoriel si vous souhaitez déployer une machine virtuelle d’informatique confidentielle avec une configuration personnalisée. Sinon, suivez les [étapes de déploiement d’une machine virtuelle d’informatique confidentielle pour la Place de marché commerciale de Microsoft](quick-create-marketplace.md).


## <a name="prerequisites"></a>Prérequis

Si vous n’avez pas d’abonnement Azure, [créez un compte](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) avant de commencer.

> [!NOTE]
> Les comptes associés à un essai gratuit n’ont pas accès aux machines virtuelles utilisées dans ce tutoriel. Veuillez passer à un abonnement avec paiement à l’utilisation.


## <a name="sign-in-to-azure"></a>Connexion à Azure

1. Connectez-vous au [portail Azure](https://portal.azure.com/).

1. En haut, sélectionnez **Créer une ressource**.

1. Dans le volet **Place de marché**, sélectionnez **Compute** sur la gauche.

1. Recherchez et sélectionnez **Machine virtuelle**.

    ![Déployer une machine virtuelle](media/quick-create-portal/compute-virtual-machine.png)

1. Dans la page d’arrivée Machine virtuelle, sélectionnez **Créer**.


## <a name="configure-a-confidential-computing-virtual-machine"></a>Configurer une machine virtuelle d’informatique confidentielle

1. Sous l’onglet **De base**, sélectionnez votre **Abonnement** et votre **Groupe de ressources**.

1. Dans **Nom de la machine virtuelle**, indiquez le nom de votre nouvelle machine virtuelle.

1. Tapez ou sélectionnez les valeurs suivantes :

   * **Région** : sélectionnez la région Azure qui vous convient.

        > [!NOTE]
        > Les machines virtuelles d’informatique confidentielle fonctionnent uniquement sur du matériel spécialisé disponible dans des régions spécifiques. Pour connaître les régions prenant en charge les machines virtuelles de la série DCsv2, consultez la [liste des produits disponibles par région](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

1. Configurez l’image du système d’exploitation à utiliser pour votre machine virtuelle.

    * **Choisir une image** : pour ce tutoriel, sélectionnez Ubuntu 18.04 LTS. Vous pouvez également sélectionner Windows Server 2019, Windows Server 2016 ou Ubuntu 16.04 LTS. Dans ce cas, vous serez redirigé en conséquence dans ce tutoriel.
    
    * **Activer l’image de 2e génération** : les machines virtuelles d’informatique confidentielle s’exécutent uniquement sur des images de [2e génération](../virtual-machines/generation-2.md). Vérifiez que l’image que vous sélectionnez est une image de 2e génération. Cliquez sur l’onglet **Avancé** au-dessus de l’emplacement où vous configurez la machine virtuelle. Faites défiler vers le bas jusqu’à la section intitulée « Génération de machine virtuelle ». Sélectionnez Génération 2, puis revenez à l’onglet **Options de base**.
    

        ![Options avancées, onglet](media/quick-create-portal/advanced-tab-virtual-machine.png)


        ![Génération de machine virtuelle](media/quick-create-portal/gen2-virtual-machine.png)

    * **Revenir à la configuration de base** : utilisez les options de navigation dans la partie supérieure pour revenir à l’onglet **Options de base**.

1. Sélectionnez **changer la taille** pour choisir une machine virtuelle dotée de capacités d’informatique confidentielle dans le sélecteur de taille. Dans le sélecteur de taille de la machine virtuelle, cliquez sur **Effacer tous les filtres**. Choisissez **Ajouter un filtre**, sélectionnez **Famille** comme type de filtre, puis sélectionnez uniquement **Informatique confidentielle**.

    ![Machines virtuelles de la série DCsv2](media/quick-create-portal/dcsv2-virtual-machines.png)

    > [!TIP]
    > Vous devriez voir les tailles **DC1s_v2**, **DC2s_v2**, **DC4s_V2** et **DC8_v2**. Seules ces tailles de machines virtuelles prennent actuellement en charge l’informatique confidentielle. [Plus d’informations](virtual-machine-solutions.md)

1. Renseignez les informations suivantes :

   * **Type d'authentification** : Sélectionnez **Clé publique SSH** si vous créez une machine virtuelle Linux. 

        > [!NOTE]
        > Vous pouvez choisir d’utiliser une clé publique SSH ou un mot de passe pour l’authentification. L’utilisation d’une clé SSH est plus sécurisée. Pour savoir comment générer une clé SSH, consultez [Créer des clés SSH sur Linux et Mac pour les machines virtuelles Linux dans Azure](../virtual-machines/linux/mac-create-ssh-keys.md).

    * **Nom d’utilisateur** : indiquez le nom d’administrateur pour la machine virtuelle.

    * **Clé publique SSH** : le cas échéant, entrez votre clé publique RSA.
    
    * **Mot de passe** : le cas échéant, entrez votre mot de passe pour l’authentification.

    * **Ports d’entrée publics** : choisissez **Autoriser les ports sélectionnés**, puis sélectionnez **SSH (22)** et **HTTP (80)** dans la liste **Sélectionner les ports d’entrée publics**. Si vous déployez une machine virtuelle Windows, sélectionnez **HTTP (80)** et **RDP (3389)** .  

    >[!Note]
    > Autoriser les ports RDP/SSH n’est pas recommandé pour les déploiements de production.  

     ![Ports entrants](media/quick-create-portal/inbound-port-virtual-machine.png)


1. Apportez les modifications souhaitées sous l’onglet **Disques**.

   * Si vous avez choisi une machine virtuelle **DC1s_v2**, **DC2s_v2** ou **DC4s_V2**, optez pour un type de disque **SSD Standard** ou **SSD Premium**. 
   * Si vous avez choisi une machine virtuelle **DC8_v2**, optez pour **SSD Standard** comme type de disque.

1. Apportez les modifications souhaitées aux paramètres des onglets suivants ou conservez les paramètres par défaut.

    * **Mise en réseau**
    * **Gestion**
    * **Configuration de l’invité**
    * **Balises**

1. Sélectionnez **Revoir + créer**.

1. Dans le volet **Vérifier + créer**, sélectionnez **Créer**.

> [!NOTE]
> Si vous avez déployé une machine virtuelle Linux, passez à la section suivante de ce tutoriel. Si vous avez déployé une machine virtuelle Windows, [suivez cette procédure pour vous connecter à votre machine virtuelle Windows](../virtual-machines/windows/connect-logon.md), puis [installez le SDK OE sur Windows](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Windows.md).


## <a name="connect-to-the-linux-vm"></a>Se connecter à la machine virtuelle Linux

Si vous utilisez déjà un interpréteur de commandes BASH, connectez-vous à la machine virtuelle Azure à l’aide de la commande **SSH**. Dans la commande suivante, remplacez le nom d’utilisateur et l’adresse IP de la machine virtuelle pour vous connecter à votre machine virtuelle Linux.

```bash
ssh azureadmin@40.55.55.555
```

Vous trouverez l’adresse IP publique de votre machine virtuelle dans le portail Azure, sous la section Vue d’ensemble de votre machine virtuelle.

:::image type="content" source="media/quick-create-portal/public-ip-virtual-machine.png" alt-text="Adresse IP dans le portail Azure":::

Si vous êtes sous Windows et que vous n’avez pas d’interpréteur de commandes BASH, installez un client SSH, tel que PuTTY.

1. [Téléchargez et installez PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Exécutez PuTTY.

1. À l’écran de configuration de PuTTY, entrez l’adresse IP publique de votre machine virtuelle.

1. Sélectionnez **Open** (Ouvrir) et entrez votre nom d’utilisateur et votre mot de passe lorsque vous y êtes invité.

Pour en savoir plus la connexion aux machines virtuelles Linux, consultez [Création d’une machine virtuelle Linux sur Azure à l’aide du portail](../virtual-machines/linux/quick-create-portal.md).

> [!NOTE]
> Si vous voyez une alerte de sécurité PuTTY relative à la clé d’hôte du serveur non mise en cache dans le Registre, choisissez parmi les options suivantes. Si vous faites confiance à cet ordinateur hôte, sélectionnez **Yes** (Oui) pour ajouter la clé à la mémoire cache de PuTTy et poursuivre la connexion. Si vous souhaitez vous connecter une seule fois, sans ajouter la clé dans le cache, sélectionnez **No** (Non). Si vous ne faites pas confiance à cet ordinateur hôte, sélectionnez **Cancel** (Annuler) pour abandonner la connexion.

## <a name="intel-sgx-drivers"></a>Pilotes Intel SGX

> [!NOTE]
> Les pilotes Intel SGX font déjà partie des images de la galerie Azure Ubuntu et Windows. Aucune installation spéciale des pilotes n’est requise. Vous pouvez également mettre à jour les pilotes existants fournis dans les images en consultant la [liste des pilotes Intel SGX DCAP](https://01.org/intel-software-guard-extensions/downloads).

## <a name="optional-testing-enclave-apps-built-with-open-enclave-sdk-oe-sdk"></a>Facultatif : test des applications enclave créées avec le SDK Open Enclave SDK (OE SDK) <a id="Install"></a>

Suivez les instructions pas à pas pour installer le [SDK OE](https://github.com/openenclave/openenclave) sur votre machine virtuelle de la série DCsv2 exécutant une image Ubuntu 18.04 LTS Gen 2. 

Si votre machine virtuelle s’exécute sur Ubuntu 18.04 LTS Gen 2, vous devez suivre les [instructions d’installation pour Ubuntu 18.04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_18.04.md).

## <a name="clean-up-resources"></a>Nettoyer les ressources

Dès que vous n’en avez plus besoin, vous pouvez supprimer le groupe de ressources, la machine virtuelle et toutes les ressources associées. 

Sélectionnez le groupe de ressources de la machine virtuelle, puis sélectionnez **Supprimer**. Confirmez le nom du groupe de ressources pour terminer la suppression des ressources.

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez déployé une machine virtuelle d’informatique confidentielle et installé le SDK Open Enclave. Pour plus d’informations sur les machines virtuelles d’informatique confidentielle sur Azure, consultez [Solutions sur les machines virtuelles](virtual-machine-solutions.md). 

Découvrez comment créer des applications d’informatique confidentielle en accédant aux exemples du SDK Open Enclave sur GitHub. 

> [!div class="nextstepaction"]
> [Création d’exemples de SDK Open Enclave](https://github.com/openenclave/openenclave/blob/master/samples/README.md)
