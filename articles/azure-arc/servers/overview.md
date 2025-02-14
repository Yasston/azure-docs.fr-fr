---
title: Vue d'ensemble des serveurs avec Azure Arc
description: Apprenez à utiliser les serveurs avec Azure Arc afin de gérer les serveurs hébergés en dehors d'Azure comme une ressource Azure.
keywords: Azure Automation, DSC, PowerShell, Desired State Configuration, Update Management, Change Tracking, inventaire, runbooks, Python, graphique, hybride
ms.date: 05/26/2021
ms.topic: overview
ms.openlocfilehash: 2cf70cbf20d024d92a3a2025ca6b659ffdd8bffa
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2021
ms.locfileid: "112294676"
---
# <a name="what-is-azure-arc-enabled-servers"></a>Qu’est-ce qu’un serveur avec Azure Arc ?

Les serveurs avec Azure Arc vous permettent de gérer vos machines virtuelles et vos serveurs physiques Windows et Linux hébergés *en dehors* d’Azure sur votre réseau d’entreprise ou un autre fournisseur de cloud. Cette expérience de gestion est conçue pour être cohérente avec la manière dont vous gérez les machines virtuelles Azure natives. Quand une machine hybride est connectée à Azure, elle devient une machine connectée et est traitée comme une ressource dans Azure. Chaque machine connectée possède un ID de ressource, est incluse dans un groupe de ressources et tire parti des constructions Azure standard, comme Azure Policy et l’application d’étiquettes. Les fournisseurs de services qui gèrent l'infrastructure locale d'un client peuvent gérer ses machines hybrides (comme ils le font déjà avec les ressources Azure natives) dans différents environnements clients à l'aide d'[Azure Lighthouse](../../lighthouse/how-to/manage-hybrid-infrastructure-arc.md) avec Azure Arc.

Pour bénéficier de cette expérience avec vos machines hybrides hébergées en dehors d’Azure, vous devez installer Azure Connected Machine sur chaque machine que vous envisagez de connecter à Azure. Cet agent ne fournit aucune autre fonctionnalité et ne remplace pas l’agent Azure [Log Analytics](../../azure-monitor/agents/log-analytics-agent.md). L’agent Log Analytics pour Windows et Linux est nécessaire quand vous souhaitez superviser de manière proactive le système d’exploitation et les charges de travail en cours d’exécution sur la machine, gérer le système d’exploitation à l’aide de runbooks Automation ou de solutions comme Update Management ou utiliser d’autres services Azure tels qu’[Azure Security Center](../../security-center/security-center-introduction.md).

>[!NOTE]
> [L’agent Azure Monitor](../../azure-monitor/agents/azure-monitor-agent-overview.md) (AMA), actuellement en préversion, ne remplace pas Connected Machine Agent. Il remplace en revanche l’agent Log Analytics, l’extension Diagnostics et l’agent Telegraf pour les ordinateurs Windows et Linux. Pour plus d’informations, consultez la documentation Azure Monitor sur le nouvel agent.

## <a name="supported-scenarios"></a>Scénarios pris en charge

Lorsque vous connectez votre machine à des serveurs avec Azure Arc, vous pouvez effectuer les tâches de surveillance et de gestion de la configuration suivantes :
- Affecter des [configurations invité Azure Policy](../../governance/policy/concepts/guest-configuration.md) à l’aide de la même expérience que lors de l’attribution de stratégie pour des machines virtuelles Azure. De nos jours, la plupart des stratégies de configuration d’invité ne s’appliquent pas aux configurations : elles auditent seulement les paramètres à l’intérieur de la machine. Pour comprendre le coût de l’utilisation de stratégies Guest Configuration dans Azure Policy avec des serveurs Arc, consultez le [guide des tarifs](https://azure.microsoft.com/pricing/details/azure-policy/) d’Azure Policy.

- Signalez les changements de configuration relatifs aux logiciels installés, aux services Microsoft, au registre et aux fichiers Windows ainsi qu’aux démons Linux sur des serveurs surveillés à l’aide de la fonctionnalité [Suivi des modifications et inventaire](../../automation/change-tracking/overview.md) d’Azure Automation et de [Supervision d’intégrité de fichier dans Azure Security Center](../../security-center/security-center-file-integrity-monitoring.md), et à l’aide d’[Azure Defender pour serveurs](../../security-center/defender-for-servers-introduction.md) pour les serveurs.

- Supervisez les performances du système d’exploitation invité de votre machine connectée, et découvrez les composants de l’application pour superviser leurs processus et dépendances avec d’autres ressources que l’application communique à l’aide de [VM Insights](../../azure-monitor/vm/vminsights-overview.md).

- Simplifiez le déploiement avec d’autres services Azure tels qu’Azure Automation [State Configuration](../../automation/automation-dsc-overview.md) et un espace de travail Azure Monitor Log Analytics à l’aide des [extensions de machine virtuelle Azure](manage-vm-extensions.md) prises en charge pour votre machine Windows ou Linux non Azure. Cela comprend l’exécution de la configuration après déploiement ou de l’installation de logiciels à l’aide de l’extension de script personnalisé.

- Utilisez la fonctionnalité [Update Management](../../automation/update-management/overview.md) d’Azure Automation pour gérer les mises à jour du système d’exploitation de vos serveurs Windows et Linux.

    > [!NOTE]
    > À ce stade, l’activation d’Update Management, directement à partir d’un serveur Azure Arc, n’est pas prise en charge. Pour connaître les conditions requises et la façon de l’activer pour votre serveur, consultez [Activer Update Management à partir de votre compte Automation](../../automation/update-management/enable-from-automation-account.md).

- Incluez vos serveurs non Azure pour la détection avancée des menaces, et surveillez de manière proactive les menaces de sécurité potentielles à l'aide d'[Azure Security Center](../../security-center/security-center-introduction.md) ou d’[Azure Defender](../../security-center/azure-defender.md).

- Protégez les serveurs non Azure avec [Microsoft Defender pour point de terminaison](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint), inclus dans [Azure defender](../../security-center/azure-defender.md), à des fins de détection des menaces, de gestion des vulnérabilités et de surveillance proactive des menaces de sécurité potentielles.

Les données de journal collectées et stockées dans un espace de travail Log Analytics à partir de la machine hybride contiennent désormais des propriétés spécifiques de la machine, telles qu’un ID de ressource. Cela peut être utilisé pour prendre en charge l’accès au journal de [contexte de ressource](../../azure-monitor/logs/design-logs-deployment.md#access-mode).

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>Régions prises en charge

Pour obtenir la liste définitive des régions prises en charge dotées de serveurs avec Azure Arc, consultez la page [Produits Azure par région](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc).

Dans la plupart des cas, l’emplacement que vous sélectionnez au moment de créer le script d’installation doit être la région Azure géographiquement la plus proche de l’emplacement de votre ordinateur. Les données au repos sont stockées dans la zone géographique Azure englobant la région que vous spécifiez, ce qui peut aussi affecter votre choix de région si vous avez des exigences concernant la résidence des données. Si la région Azure à laquelle votre machine se connecte subit une panne, la machine connectée n’est pas affectée, mais les opérations de gestion effectuées avec Azure risquent de ne pas aboutir. En cas de panne régionale et si vous avez plusieurs emplacements qui prennent en charge un service géographiquement redondant, l’idéal est de connecter les machines de chaque emplacement à une région Azure distincte.

Les métadonnées suivantes concernant la machine connectée sont collectées et stockées dans la région où la ressource machine Azure Arc est configurée :

- Nom et version du système d’exploitation
- Nom de l'ordinateur
- Nom de domaine complet (FQDN) de l’ordinateur
- Version de l’agent Machine connectée

Par exemple, si la machine est inscrite auprès d’Azure Arc dans la région USA Est, ces données sont stockées dans la région USA.

### <a name="supported-environments"></a>Environnements pris en charge

Les serveurs avec Arc prennent en charge la gestion des serveurs physiques et des machines virtuelles hébergés *en dehors* d’Azure. Pour plus d’informations sur les environnements de cloud hybride hébergeant des machines virtuelles qui sont pris en charge, consultez [Prérequis de l’agent Connected Machine](agent-overview.md#supported-environments).

> [!NOTE]
> Les serveurs avec Arc ne sont pas conçus ni pris en charge pour permettre la gestion des machines virtuelles s’exécutant dans Azure.

### <a name="agent-status"></a>État de l’agent

L’agent Connected Machine envoie des messages de pulsation au service de façon régulière (toutes les 5 minutes). Si le service cesse de recevoir ces messages de pulsation d’une machine, cette machine est considérée comme étant hors connexion, et l’état dans le portail est automatiquement remplacé par **Déconnectée** au bout de 15 à 30 minutes. À la prochaine réception d’un message de pulsation de l’agent Connected Machine, son état devient automatiquement **Connecté**.

## <a name="next-steps"></a>Étapes suivantes

Avant d'évaluer ou d'activer des serveurs avec Azure Arc sur plusieurs machines hybrides, consultez [Vue d'ensemble d'Azure Connected Machine Agent](agent-overview.md) pour en savoir plus sur les exigences et les détails techniques relatifs à l'agent, ainsi que sur les méthodes de déploiement.
