---
title: Découvrez comment auditer le contenu des machines virtuelles
description: Découvrez comment Azure Policy utilise le client Guest Configuration pour auditer les paramètres à l’intérieur des machines virtuelles.
ms.date: 05/01/2021
ms.topic: conceptual
ms.openlocfilehash: 80de6651d59b26b596633b8ba775c774dcfea62e
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2021
ms.locfileid: "111970343"
---
# <a name="understand-azure-policys-guest-configuration"></a>Comprendre la configuration d’invité d’Azure Policy

Azure Policy peut vérifier les paramètres à l’intérieur d’une machine, tant pour les machines s’exécutant dans Azure que dans [Arc Connected Machines](../../../azure-arc/servers/overview.md). La validation est effectuée par le client et l’extension de configuration d’invité. L’extension, via le client, valide des paramètres tels que :

- La configuration du système d’exploitation
- La configuration ou la présence de l’application
- Paramètres d'environnement

À l’heure actuelle, la plupart des définitions de stratégie pour Azure Policy Guest Configuration auditent uniquement les paramètres à l’intérieur de la machine. Elles n’appliquent pas de configurations. L’exception est une stratégie intégrée [référencée ci-dessous](#applying-configurations-using-guest-configuration).

[Un guide vidéo de ce document est disponible](https://youtu.be/Y6ryD3gTHOs).

## <a name="enable-guest-configuration"></a>Activer la configuration d’invité

Pour vérifier l’état des machines dans votre environnement, y compris les machines dans Azure et Arc Connected Machines, examinez les informations suivantes.

## <a name="resource-provider"></a>Fournisseur de ressources

Avant de pouvoir utiliser la configuration d’invité, vous devez inscrire le fournisseur de ressources.
Si l’affectation d’une stratégie de configuration d’invité est effectuée via le portail, ou si l’abonnement est inscrit dans Azure Security Center, le fournisseur de ressources est inscrit automatiquement. Vous pouvez l’inscrire manuellement via le [portail](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), [Azure PowerShell](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell) ou [Azure CLI](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-cli).

## <a name="deploy-requirements-for-azure-virtual-machines"></a>Configuration requise pour le déploiement de machines virtuelles Azure

Pour vérifier les paramètres à l’intérieur d’une machine, une [extension de machine virtuelle](../../../virtual-machines/extensions/overview.md) est activée et la machine doit disposer d’une identité managée par le système. L’extension télécharge l’attribution de stratégie applicable et la définition de configuration correspondante. L’identité est utilisée pour authentifier la machine lorsqu’elle lit et écrit dans le service de configuration d’invité. L’extension n’est pas requise pour Arc Connected Machines, car elle est incluse dans l’agent Arc Connected Machines.

> [!IMPORTANT]
> L’extension Guest Configuration et une identité managée sont requises pour l’audit des machines virtuelles Azure. Pour déployer l’extension à l’échelle, attribuez l’initiative de stratégie suivante :
>
> `Deploy prerequisites to enable Guest Configuration policies on virtual machines`

### <a name="limits-set-on-the-extension"></a>Limites définies sur l’extension

Pour limiter l’impact de l’extension sur les applications qui s’exécutent à l’intérieur de la machine, la configuration d’invité ne peut pas dépasser plus de 5 % de la capacité du processeur. Cette limitation existe à la fois pour les définitions intégrées et personnalisées. Il en va de même pour le service de configuration invité de l’agent Arc Connected Machines.

### <a name="validation-tools"></a>Outils de validation

À l’intérieur de la machine, le client de configuration d’invité utilise des outils locaux pour exécuter l’audit.

Le tableau suivant affiche une liste des outils locaux utilisés sur chaque système d’exploitation pris en charge. Pour le contenu intégré, la configuration d’invité gère automatiquement le chargement de ces outils.

|Système d’exploitation|Outil de validation|Notes|
|-|-|-|
|Windows|[PowerShell Desired State Configuration](/powershell/scripting/dsc/overview/overview) v2| Chargé dans un dossier utilisé uniquement par Azure Policy. Aucun conflit avec Windows PowerShell DSC. PowerShell Core n’est pas ajouté au chemin d’accès système.|
|Linux|[Chef InSpec](https://www.chef.io/inspec/) | Installe Chef InSpec version 2.2.61 dans l’emplacement par défaut et est ajouté au chemin d’accès système. Des dépendances pour le package InSpec, y compris Ruby et Python, sont également installées. |

### <a name="validation-frequency"></a>Fréquence de validation

Le client de configuration d'invité vérifie les affectations d’invités nouvelles ou modifiées toutes les 5 minutes. Une fois l’affectation d’invité reçue, les paramètres de cette configuration sont revérifiés à intervalle de 15 minutes. Les résultats sont envoyés au fournisseur de ressources de configuration d’invité dès la fin de l’audit.
Lorsqu'un [déclencheur d’évaluation](../how-to/get-compliance-data.md#evaluation-triggers) de stratégie intervient, l'état de la machine est consigné dans le fournisseur de ressources de configuration d'invité. Avec cette mise à jour, Azure Policy évalue alors les propriétés Azure Resource Manager. Une évaluation Azure Policy à la demande récupère la valeur la plus récente du fournisseur de ressources de configuration d'invité. Cela étant, elle ne déclenche pas de nouvel audit de la configuration de la machine. L’état est écrit simultanément dans Azure Resource Graph.

## <a name="supported-client-types"></a>Types de clients pris en charge

Les définitions de stratégie Guest Configuration sont incluses dans les nouvelles versions. Les versions antérieures des systèmes d’exploitation disponibles dans Place de marché Azure sont exclues si le client Guest Configuration n’est pas compatible. Le tableau suivant affiche une liste des systèmes d’exploitation pris en charge sur des images Azure.
« .x » est utilisé pour représenter les nouvelles versions mineures des distributions Linux.

|Serveur de publication|Nom|Versions|
|-|-|-|
|Canonical|Serveur Ubuntu|14.04 - 20.x|
|Credativ|Debian|8 - 10.x|
|Microsoft|Windows Server|2012 - 2019|
|Microsoft|Client Windows|Windows 10|
|OpenLogic|CentOS|7.3 - 8.x|
|Red Hat|Red Hat Enterprise Linux|7.4 - 8.x|
|SUSE|SLES|12 SP3-SP5, 15.x|

Les images de machine virtuelle personnalisées sont prises en charge par les définitions de stratégie Guest Configuration, dans la mesure où il s’agit d’un des systèmes d’exploitation répertoriés dans le tableau ci-dessus.

## <a name="network-requirements"></a>Configuration requise pour le réseau

Dans Azure, les machines virtuelles peuvent utiliser leur carte réseau locale ou une liaison privée pour communiquer avec le service Guest Configuration.

Les machines Azure Arc se connectent à l’aide de l’infrastructure réseau locale pour atteindre les services Azure et signaler l’état de conformité.

### <a name="communicate-over-virtual-networks-in-azure"></a>Communiquer via des réseaux virtuels dans Azure

Pour communiquer avec le fournisseur de ressources de configuration d’invité dans Azure, les machines nécessitent un accès sortant vers des centres de données Azure sur le port **443**. Si un réseau dans Azure n’autorise pas le trafic sortant, configurez des exceptions à l’aide de règles du [Groupe de sécurité réseau](../../../virtual-network/manage-network-security-group.md#create-a-security-rule). L’[étiquette de service](../../../virtual-network/service-tags-overview.md) « AzureArcInfrastructure » peut être utilisée pour référencer le service Guest Configuration plutôt que de gérer manuellement la [liste des plages d’adresses IP](https://www.microsoft.com/en-us/download/details.aspx?id=56519) pour les centres de données Azure.

### <a name="communicate-over-private-link-in-azure"></a>Communiquer via une liaison privée dans Azure

Les machines virtuelles peuvent utiliser une [liaison privée](../../../private-link/private-link-overview.md) pour la communication avec le service Guest Configuration. Appliquez la balise avec le nom `EnablePrivateNetworkGC` et la valeur `TRUE` pour activer cette fonctionnalité. La balise peut être appliquée avant ou après l’application des définitions de stratégie Guest Configuration à la machine.

Le trafic est acheminé à l’aide de l’[IP publique virtuelle](../../../virtual-network/what-is-ip-address-168-63-129-16.md) d’Azure pour établir un canal sécurisé et authentifié avec les ressources de la plateforme Azure.

### <a name="azure-arc-connected-machines"></a>Machines connectées à Azure Arc

Les nœuds situés en dehors d’Azure et qui sont connectés par le biais d’Azure Arc requièrent une connexion au service Guest Configuration. Des détails sur la configuration requise du réseau et du proxy sont fournis dans la [documentation Azure Arc](../../../azure-arc/servers/overview.md).

Pour les serveurs connectés à Arc dans des centres de données privés, autorisez le trafic à l’aide des modèles suivants :

- Port : seul le port TCP 443 est nécessaire pour l’accès Internet sortant
- URL globale : `*.guestconfiguration.azure.com`

## <a name="managed-identity-requirements"></a>Exigences relatives à l’identité managée

Les définitions de stratégie de l’initiative _Déployer les prérequis pour activer les stratégies Guest Configuration sur les machines virtuelles_ activent une identité managée affectée par le système, s’il n’en n’existe pas. L’initiative comporte deux définitions de stratégie qui gèrent la création d’identités. Les conditions IF dans les définitions de stratégie garantissent un comportement correct en fonction de l’état actuel de la ressource de la machine dans Azure.

Si la machine n’a pas d’identité managée, la stratégie actuelle est la suivante : [Ajouter une identité managée affectée par le système pour activer les attributions Guest Configuration sur les machines virtuelles sans identité](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3cf2ab00-13f1-4d0c-8971-2ac904541a7e)

Si la machine a actuellement une identité système affectée par l’utilisateur, la stratégie actuelle est la suivante : [Ajouter une identité managée affectée par le système pour activer les attributions Guest Configuration sur les machines virtuelles qui ont une identité affectée par l’utilisateur](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F497dff13-db2a-4c0f-8603-28fa3b331ab6)

## <a name="guest-configuration-definition-requirements"></a>Exigences de définition de la configuration d’invité

Les définitions de stratégie Guest Configuration utilisent l’effet **AuditIfNotExists**. Lorsque la définition est assignée, un service back-end gère automatiquement le cycle de vie de toutes les spécifications dans le fournisseur de ressources Azure `Microsoft.GuestConfiguration`.

Les définitions de stratégie **AuditIfNotExists** ne retournent pas de résultats de conformité tant que toutes les spécifications ne sont pas satisfaites sur la machine. Les spécifications sont décrites dans la section [Configuration requise pour le déploiement de machines virtuelles Azure](#deploy-requirements-for-azure-virtual-machines).

> [!IMPORTANT]
> Dans une version précédente de Guest Configuration, une initiative était nécessaire pour combiner les définitions **DeployIfNoteExists** et **AuditIfNotExists**. Les définitions **DeployIfNotExists** ne sont plus nécessaires. Les définitions et les initiatives sont étiquetées `[Deprecated]` mais les attributions existantes continuent à fonctionner. Pour plus d’informations, consultez le billet de blog : [Modification importante publiée pour les stratégies d’audit de Guest Configuration](https://techcommunity.microsoft.com/t5/azure-governance-and-management/important-change-released-for-guest-configuration-audit-policies/ba-p/1655316)

### <a name="what-is-a-guest-assignment"></a>Qu’est-ce qu’une affectation d’invité ?

Lorsqu’une instance Azure Policy est affectée, si elle se trouve dans la catégorie « Configuration d’invité », des métadonnées sont incluses pour décrire une affectation d’invité. Vous pouvez considérer une affectation d’invité comme un lien entre un ordinateur et un scénario Azure Policy. Par exemple, l’extrait de code suivant associe la configuration de base Windows Azure avec une version minimale `1.0.0` à n’importe quel ordinateur dans l’étendue de la stratégie.
Par défaut, l’affectation d’invité effectue uniquement un audit de l’ordinateur.

```json
"metadata": {
    "category": "Guest Configuration",
    "guestConfiguration": {
        "name": "AzureWindowsBaseline",
        "version": "1.*"
    }
//additional metadata properties exist
```

Les affectations d’invités sont créées automatiquement pour chaque ordinateur par le service de configuration d’invité. Le type de ressource, est `Microsoft.GuestConfiguration/guestConfigurationAssignments`. Azure Policy utilise la propriété **complianceStatus** de la ressource Attribution d’invité pour signaler l’état de conformité. Pour plus d’informations, consultez [Obtention de données de conformité](../how-to/get-compliance-data.md).

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Audit des paramètres du système d’exploitation conformément aux lignes de base du secteur

Une initiative dans Azure Policy audite les paramètres de système d’exploitation d’après une « ligne de base ». La définition, _\[Préversion\] : Les machines Windows doivent répondre aux exigences de la base de référence de sécurité Azure_ comprend un ensemble de règles basées sur une stratégie de groupe Active Directory.

La plupart des paramètres sont disponibles en tant que tels. Les paramètres vous permettent de personnaliser ce qui est audité.
Alignez la stratégie sur vos besoins ou mappez la stratégie à des informations tierces, telles que les normes réglementaires sectorielles.

Certains paramètres prennent en charge une plage de valeurs entières. Par exemple, le paramètre Antériorité maximale du mot de passe peut auditer le paramètre Stratégie de groupe effectif. Une plage de « 1,70 » confirme que les utilisateurs doivent modifier leur mot de passe au moins tous les 70 jours, mais pas plus d’une fois par jour.

Si vous attribuez la stratégie à l’aide d’un modèle Resource Manager, utilisez un fichier de paramètres pour gérer les exceptions. Archivez les fichiers dans un système de contrôle de version tel que Git. Les commentaires sur les modifications de fichiers fournissent une preuve de la raison pour laquelle une affectation est une exception à la valeur attendue.

#### <a name="applying-configurations-using-guest-configuration"></a>Application de configurations à l’aide de Guest Configuration

Seule la définition _Configurer le fuseau horaire sur les machines Windows_ apporte des modifications à la machine en configurant le fuseau horaire. Les définitions de stratégie personnalisées pour la configuration des paramètres à l’intérieur des machines ne sont pas prises en charge.

Lorsque vous attribuez des définitions qui commencent par _Configurer_, vous devez également attribuer la définition _Déployer les composants requis pour activer la stratégie de configuration d’invité sur des machines virtuelles Windows_. Vous pouvez combiner ces définitions dans une initiative.

> [!NOTE]
> La stratégie intégrée de fuseau horaire est la seule définition qui prend en charge la configuration des paramètres à l’intérieur des machines ; les définitions de stratégie personnalisées qui configurent les paramètres à l’intérieur des machines ne sont pas prises en charge.

#### <a name="assigning-policies-to-machines-outside-of-azure"></a>Attribution de stratégies à des machines en dehors d’Azure

Les définitions de stratégie Audit disponibles pour Guest Configuration incluent le type de ressource **Microsoft.HybridCompute/machines**. Toutes les machines intégrées à [Azure Arc pour serveurs](../../../azure-arc/servers/overview.md) qui relèvent du champ d’application de l’attribution de stratégies sont automatiquement incluses.

## <a name="availability"></a>Disponibilité

Les clients qui conçoivent une solution hautement disponible doivent tenir compte des exigences de planification de la redondance pour les [machines virtuelles](../../../virtual-machines/availability.md), car les attributions d’invités sont des extensions des ressources de machine dans Azure. Lorsque les ressources d’attributions d’invités sont approvisionnées dans une région Azure [jumelée](../../../best-practices-availability-paired-regions.md), tant qu’au moins une région jumelée est disponible, les rapports d’attributions d’invités sont disponibles. Si la région Azure n’est pas jumelée et devient indisponible, il n’est pas possible d’accéder aux rapports d’une attribution d’invité tant que la région n’est pas restaurée.

Lorsque vous envisagez une architecture pour les applications hautement disponibles, en particulier lorsque les machines virtuelles sont approvisionnées dans des [groupes à haute disponibilité](../../../virtual-machines/availability.md#availability-sets) derrière une solution d’équilibrage de charge pour fournir une haute disponibilité, il est recommandé d’affecter les mêmes définitions de stratégie avec les mêmes paramètres à toutes les machines de la solution. Si possible, une attribution de stratégie unique couvrant toutes les machines offrirait la surcharge administrative la moins importante.

Pour les machines protégées par [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md), assurez-vous que les machines d’un site secondaire se trouvent dans l’étendue des attributions d’Azure Policy pour les mêmes définitions en utilisant les mêmes valeurs de paramètres que les machines du site principal.

## <a name="troubleshooting-guest-configuration"></a>Résolution des problèmes de configuration invité

Pour plus d’informations sur la résolution des problèmes de configuration invité, consultez [Résolution des problèmes Azure Policy](../troubleshoot/general.md).

### <a name="multiple-assignments"></a>Affectations multiples

Actuellement, les définitions de stratégie Guest Configuration prennent en charge l’attribution d’une seule affectation d’invité par machine, même si l’attribution de stratégie utilise des paramètres différents.

### <a name="client-log-files"></a>Fichiers journaux du client

L’extension de configuration d’invité écrit les fichiers journaux aux emplacements suivants :

Windows : `C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log`

Linux

- Machine virtuelle Azure : `/var/lib/GuestConfig/gc_agent_logs/gc_agent.log`
- Machine virtuelle Azure : `/var/lib/GuestConfig/arc_policy_logs/gc_agent.log`

### <a name="collecting-logs-remotely"></a>Collecte des journaux à distance

La première étape de résolution des problèmes des configurations ou modules Guest Configuration doit être d’utiliser la cmdlet `Test-GuestConfigurationPackage` suivant la procédure pour [créer une stratégie d’audit Guest Configuration personnalisée pour Windows](../how-to/guest-configuration-create.md#step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows).
Si cela ne réussit pas, la collecte des journaux des clients peut vous aider à diagnostiquer les problèmes.

#### <a name="windows"></a>Windows

Capturez des informations de fichiers journaux à l’aide de la [commande Run de la machine virtuelle Azure](../../../virtual-machines/windows/run-command.md). L’exemple de script PowerShell suivant peut être utile.

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$logPath = 'C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log'
Select-String -Path $logPath -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

Capturez des informations de fichiers journaux à l’aide de la [commande Run de la machine virtuelle Azure](../../../virtual-machines/linux/run-command.md). L’exemple de script Bash suivant peut être utile.

```bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
logPath=/var/lib/GuestConfig/gc_agent_logs/gc_agent.log
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' $logPath | tail
```

### <a name="client-files"></a>Fichiers client

Le client de configuration invité télécharge des packages de contenu sur un ordinateur et extrait le contenu.
Pour vérifier quel contenu a été téléchargé et stocké, consultez les emplacements de dossier indiqués ci-dessous.

Windows : `c:\programdata\guestconfig\configuration`

Linux : `/var/lib/GuestConfig/Configuration`

## <a name="guest-configuration-samples"></a>Exemples de la configuration d’invité

Des exemples de stratégie intégrée de configuration d’invité sont disponibles dans les emplacements suivants :

- [Définitions de stratégie intégrées - Configuration d’invité](../samples/built-in-policies.md#guest-configuration)
- [Initiatives intégrées - Configuration d’invité](../samples/built-in-initiatives.md#guest-configuration)
- [Exemples Azure Policy - Dépôt GitHub](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Guest%20Configuration)

### <a name="video-overview"></a>Présentation vidéo

La présentation suivante d’Azure Policy Guest Configuration provient de ITOps Talks 2021.

[Gestion des bases de référence dans des environnements de serveur hybrides à l’aide d’Azure Policy Guest Configuration](https://techcommunity.microsoft.com/t5/itops-talk-blog/ops114-governing-baselines-in-hybrid-server-environments-using/ba-p/2109245)

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment afficher les détails de chaque paramètre à partir de la [vue de conformité de la configuration d’invité](../how-to/determine-non-compliance.md#compliance-details-for-guest-configuration)
- Consultez des exemples à la page [Exemples Azure Policy](../samples/index.md).
- Consultez la [Structure de définition Azure Policy](./definition-structure.md).
- Consultez la page [Compréhension des effets de Policy](./effects.md).
- Découvrez comment [créer des stratégies par programmation](../how-to/programmatically-create.md).
- Découvrez comment [obtenir des données de conformité](../how-to/get-compliance-data.md).
- Découvrez comment [corriger des ressources non conformes](../how-to/remediate-resources.md).
- Pour en savoir plus sur les groupes d’administration, consultez [Organiser vos ressources avec des groupes d’administration Azure](../../management-groups/overview.md).
