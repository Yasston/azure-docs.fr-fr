---
title: Utilisation de la licence Microsoft Defender for Endpoint incluse dans Azure Security Center
description: Découvrez Microsoft Defender for Endpoint et déployez-le à partir d’Azure Security Center.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 06/10/2021
ms.author: memildin
ms.openlocfilehash: df7d3d880161895b6cc883a15f7adf2def839a53
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/14/2021
ms.locfileid: "112062259"
---
# <a name="protect-your-endpoints-with-security-centers-integrated-edr-solution-microsoft-defender-for-endpoint"></a>Protéger vos points de terminaison avec la solution EDR intégrée de Security Center : Microsoft Defender for Endpoint

Microsoft Defender for Endpoint est une solution holistique de sécurité des points de terminaison dans le cloud. Ses principales fonctionnalités sont les suivantes :

- Gestion et évaluation des vulnérabilités basées sur les risques 
- Réduction de la surface d’attaque
- Protection basée sur le comportement et gérée par le cloud
- Détection de point de terminaison et réponse (EDR)
- Investigation et correction automatiques
- Services de chasse gérés

> [!TIP]
> Lancé à l’origine sous le nom **Windows Defender ATP**, ce produit de détection de point de terminaison et réponse (EDR) a été renommé en 2019 en **Microsoft Defender ATP**.
>
> À l’occasion d’Ignite 2020, nous avons lancé la [suite Microsoft Defender XDR](https://www.microsoft.com/security/business/threat-protection) et ce composant EDR a été renommé **Microsoft Defender for Endpoint**.


## <a name="availability"></a>Disponibilité

| Aspect                          | Détails                                                                                                                                                                                                                                                                                                       |
|---------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| État de sortie :                  | Disponibilité générale (GA)                                                                                                                                                                                                                                                                                      |
| Prix :                        | Nécessite [Azure Defender pour les serveurs](defender-for-servers-introduction.md)                                                                                                                                                                                                                                             |
| Plateformes prises en charge :            |  • Machines Azure exécutant Windows<br> • Machines Azure Arc exécutant Windows|
| Versions de Windows prises en charge pour la détection :  |  • Windows Server 2019, 2016, 2012 R2 et 2008 R2 SP1<br> • [Windows Virtual Desktop (WVD)](../virtual-desktop/overview.md)<br> • [Windows 10 Entreprise multisession](../virtual-desktop/windows-10-multisession-faq.yml) (anciennement Enterprise for Virtual Desktops ou EVD)|
| Systèmes d’exploitation non pris en charge :  |  • Windows 10 (autre que EVD ou WVD)<br> • Linux|
| Rôles et autorisations obligatoires : | Pour activer/désactiver l'intégration : **Administrateur de la sécurité** ou **Propriétaire**<br>Pour afficher des alertes MDATP dans Security Center : **Lecteur de sécurité**, **Lecteur**, **Contributeur du groupe de ressources**, **Propriétaire du groupe de ressources**, **Administrateur de la sécurité**, **Propriétaire de l’abonnement** ou **Contributeur de l’abonnement**|
| Clouds :                         | ![Oui](./media/icons/yes-icon.png) Clouds commerciaux<br>![Oui](./media/icons/yes-icon.png) Gouvernement des États-Unis<br>![Non](./media/icons/no-icon.png) Chine Gov, autres Gov                                                        |
|                                 |                                                                                                                                                                                                                                                                                                               |

## <a name="microsoft-defender-for-endpoint-features-in-security-center"></a>Fonctionnalités de Microsoft Defender for Endpoint dans Security Center

Microsoft Defender for Endpoint fournit les éléments suivants :

- **Capteurs de détection des violations avancés**. Les capteurs de Defender for Endpoint pour machines Windows collectent un large éventail de signaux comportementaux.

- **Détection des violations basée sur des analyses dans le cloud**. Defender for Endpoint s’adapte rapidement aux menaces changeantes. Elle utilise l’analytique avancée et le Big Data. Il est amplifié par la puissance d’Intelligent Security Graph avec des signaux à travers Windows, Azure et Office pour détecter les menaces inconnues. Il fournit des alertes actionnables et vous permet de réagir rapidement.

- **Informations sur les menaces**. Defender for Endpoint génère des alertes quand il identifie les outils, les techniques et les procédures de l’attaquant. Il utilise les données générées par les chasseurs de menaces de Microsoft et les équipes de sécurité, complétées par les renseignements fournis par les partenaires.

En intégrant Defender for Endpoint à Security Center, vous bénéficierez des capacités supplémentaires suivantes :

- **Intégration automatisée**. Security Center active automatiquement le capteur Microsoft Defender for Endpoint pour tous les serveurs Windows supervisés par Security Center.

- **Volet unique**. La console Security Center affiche des alertes Microsoft Defender for Endpoint. Pour approfondir vos recherches, utilisez les pages du portail de Microsoft Defender for Endpoint, où vous verrez des informations supplémentaires telles que l’arborescence du processus d’alerte et le graphique d’incident. Vous pouvez également voir une chronologie détaillée de la machine, qui indique tous les comportements pour un historique pouvant s’étendre sur six mois.

    :::image type="content" source="./media/security-center-wdatp/microsoft-defender-security-center.png" alt-text="Centre de sécurité de Microsoft Defender for Endpoint" lightbox="./media/security-center-wdatp/microsoft-defender-security-center.png":::

## <a name="what-are-the-requirements-for-the-microsoft-defender-for-endpoint-tenant"></a>Quelles sont les exigences de locataire pour Microsoft Defender pour point de terminaison ?

Quand vous utilisez Azure Security Center pour superviser vos serveurs, un locataire Microsoft Defender for Endpoint est créé automatiquement. 

- **Emplacement :** Les données collectées par Defender for Endpoint sont stockées dans la zone géographique du locataire tel qu’il est identifié lors de l’approvisionnement. Les données des clients, sous forme pseudonymisée, peuvent également être stockées dans des systèmes de stockage et de traitement centralisés aux États-Unis. Une fois que vous avez configuré l’emplacement, vous ne pouvez plus le modifier. Si vous disposez de votre propre licence Microsoft Defender for Endpoint et qu’il vous faut déplacer vos données vers un autre emplacement, contactez le support Microsoft pour réinitialiser le locataire.
- **Déplacement des abonnements :** Si vous avez déplacé votre abonnement Azure entre locataires Azure, certaines étapes préparatoires manuelles sont requises avant que Security Center déploie Defender pour point de terminaison. Pour plus d’informations, [contactez le support technique Microsoft](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).


## <a name="enable-the-microsoft-defender-for-endpoint-integration"></a>Activation de l’intégration de Microsoft Defender for Endpoint

### <a name="prerequisites"></a>Prérequis

Vérifiez que votre machine est conforme aux conditions requises pour Defender pour point de terminaison :

1. Assurez-vous que la machine est connectée à Azure comme requis :

    - Pour les serveurs **Windows**, configurez les paramètres réseau décrits dans [Configurer les paramètres de proxy et de connectivité Internet de l’appareil](/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet)
    - Pour des machines **locales**, connectez-la à Azure Arc comme expliqué dans [Connecter des machines hybrides à des serveurs avec Azure Arc](../azure-arc/servers/learn/quick-enable-hybrid-vm.md).
    - Pour les machines **Windows Server 2019** et [Windows Virtual Desktop (WVD)](../virtual-desktop/overview.md), vérifiez que vos machines disposent de l’extension MicrosoftMonitoringAgent.
    
1. Activez **Azure Defender pour les serveurs**. Consultez [Démarrage rapide : Activer Azure Defender](enable-azure-defender.md).
1. Si vous avez déjà acheté et déployé des licences Microsoft Defender for Endpoint sur vos serveurs, supprimez-les à l’aide de la procédure décrite dans [Retirer des serveurs Windows](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints#offboard-windows-servers).
1. Si vous avez déplacé votre abonnement entre locataires Azure, certaines étapes préparatoires manuelles sont également requises. Pour plus d’informations, [contactez le support technique Microsoft](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).


### <a name="enable-the-integration"></a>Activer l’intégration
1. Dans le menu de Security Center, sélectionnez **Tarification et paramètres**, puis l’abonnement que vous souhaitez modifier.
1. Sélectionnez **Intégrations**.
1. Sélectionnez **Autoriser Microsoft Defender for Endpoint à accéder à mes données**, puis **Enregistrer**.

    :::image type="content" source="./media/security-center-wdatp/enable-integration-with-edr.png" alt-text="Activer l’intégration entre Azure Security Center et la solution EDR de Microsoft, Microsoft Defender for Endpoint":::

    Azure Security Center intégrera automatiquement vos serveurs à Microsoft Defender for Endpoint. L’intégration peut prendre jusqu’à 24 heures.


## <a name="access-the-microsoft-defender-for-endpoint-portal"></a>Accéder au portail Microsoft Defender for Endpoint

1. Vérifiez que le compte d’utilisateur dispose des autorisations nécessaires. Pour plus d’informations, consultez [Attribuer l'accès utilisateur au Centre de sécurité Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access).

1. Vérifiez si vous avez un proxy ou un pare-feu qui bloque le trafic anonyme. Le capteur Defender for Endpoint se connecte à partir du contexte système, de sorte que le trafic anonyme doit être autorisé. Pour garantir un accès sans entraves au portail Defender for Endpoint, suivez les instructions mentionnées dans [Activer l’accès aux URL du service dans le serveur proxy](/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server).

1. Ouvrez le [portail Centre de sécurité Microsoft Defender](https://securitycenter.windows.com/). En savoir plus sur les fonctionnalités et les icônes du portail dans [Vue d’ensemble du portail Centre de sécurité Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/portal-overview). 

## <a name="send-a-test-alert"></a>Envoyer une alerte de test

Pour générer une alerte de test Microsoft Defender for Endpoint inoffensive :

1. Créez un dossier « C:\test-MDATP-test ».
1. Utilisez Bureau à distance pour accéder à votre ordinateur.
1. Ouvrez une fenêtre de ligne de commande.
1. A l'invite, copiez et exécutez la commande suivante. La fenêtre d’invite de commandes se ferme automatiquement.

    ```powershell
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```
    :::image type="content" source="./media/security-center-wdatp/generate-edr-alert.png" alt-text="Une fenêtre d’invite de commandes avec la commande pour générer une alerte de test.":::

1. Si la commande réussit, une nouvelle alerte s’affiche sur le tableau de bord Azure Security Center et sur le portail Microsoft Defender for Endpoint. L’alerte peut mettre quelques minutes à s’afficher.
1. Pour examiner l’alerte dans Security Center, accédez à **Alertes de sécurité** > **Ligne de commande PowerShell suspecte**.
1. Dans la fenêtre d’enquête, sélectionnez le lien d’accès au portail Microsoft Defender for Endpoint.

    > [!TIP]
    > L’alerte est déclenchée avec la gravité **Informations**.

## <a name="faq---security-centers-integration-with-microsoft-defender-for-endpoint"></a>FAQ : Intégration de Security Center à Microsoft Defender pour point de terminaison

- [Qu’est-ce que l’extension «MDE.Windows» qui s’exécute sur ma machine ?](#whats-this-mdewindows-extension-running-on-my-machine)
- [Quelles sont les conditions de licence pour Microsoft Defender for Endpoint ?](#what-are-the-licensing-requirements-for-microsoft-defender-for-endpoint)
- [Si j’ai déjà une licence Microsoft Defender for Endpoint, puis-je bénéficier d’une remise pour Azure Defender ?](#if-i-already-have-a-license-for-microsoft-defender-for-endpoint-can-i-get-a-discount-for-azure-defender)
- [Comment basculer dessus à partir d’un outil EDR tiers ?](#how-do-i-switch-from-a-third-party-edr-tool)

### <a name="whats-this-mdewindows-extension-running-on-my-machine"></a>Qu’est-ce que l’extension «MDE.Windows» qui s’exécute sur ma machine ?

Auparavant, Microsoft Defender pour point de terminaison était approvisionné par l’agent Log Analytics. Lorsque [nous avons étendu la prise en charge de manière à inclure Windows Server 2019](release-notes.md#microsoft-defender-for-endpoint-integration-with-azure-defender-now-supports-windows-server-2019-and-windows-10-virtual-desktop-wvd-released-for-general-availability-ga), nous avons également ajouté une extension pour effectuer l’intégration automatique. 

Security Center déploie automatiquement l’extension MDE.Windows sur les machines exécutant :

- Windows Server 2019 
- Windows 10 Virtual Desktop (WVD)
- D’autres versions de Windows Server si Security Center ne reconnaît pas la version du système d’exploitation (par exemple, lorsqu’une image de machine virtuelle personnalisée est utilisée). Dans ce cas, Microsoft Defender pour point de terminaison continue d’être approvisionné par l’agent Log Analytics.

> [!TIP]
> Si vous supprimez l’extension MDE.Windows, elle ne supprime pas Microsoft Defender pour point de terminaison. Pour plus d’informations, consultez [Retirer des serveurs Windows](/microsoft-365/security/defender-endpoint/configure-server-endpoints?view=o365-worldwide).

### <a name="what-are-the-licensing-requirements-for-microsoft-defender-for-endpoint"></a>Quelles sont les conditions de licence pour Microsoft Defender for Endpoint ?
Defender for Endpoint est inclus sans coût supplémentaire avec **Azure Defender pour les serveurs**. Vous pouvez également l’acheter séparément pour 50 machines ou plus.

### <a name="if-i-already-have-a-license-for-microsoft-defender-for-endpoint-can-i-get-a-discount-for-azure-defender"></a>Si j’ai déjà une licence Microsoft Defender for Endpoint, puis-je bénéficier d’une remise pour Azure Defender ?
Si vous disposez déjà d’une licence pour Microsoft Defender for Endpoint, vous n’aurez pas à payer pour cette partie de votre licence Azure Defender.

Pour demander votre remise, contactez l’équipe de support de Security Center et fournissez l’ID d’espace de travail, la région et le nombre de licences Microsoft Defender for Endpoint qui sont appliquées sur les ordinateurs de l’espace de travail donné.

La remise est effective à compter de la date d’approbation et ne sera pas rétroactive.

### <a name="how-do-i-switch-from-a-third-party-edr-tool"></a>Comment basculer dessus à partir d’un outil EDR tiers ?
Pour plus d’informations sur le basculement à partir d’une solution de point de terminaison non Microsoft, accédez à la documentation Microsoft Defender for Endpoint : [Vue d’ensemble de la migration](/windows/security/threat-protection/microsoft-defender-atp/switch-to-microsoft-defender-migration).
  


## <a name="next-steps"></a>Étapes suivantes

- [Plateformes et fonctionnalités prises en charge par Azure Security Center](security-center-os-coverage.md)
- [Gestion des suggestions de sécurité dans Azure Security Center](security-center-recommendations.md) : découvrez la façon dont les suggestions vous aident à protéger vos ressources Azure.
