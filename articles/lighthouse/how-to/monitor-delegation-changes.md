---
title: Superviser les changements de délégation dans votre locataire gérant
description: Découvrez comment superviser toute l’activité de la délégation Azure Lighthouse sur votre locataire gérant.
ms.date: 05/11/2021
ms.topic: how-to
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 2472e7519a6c45ff62fb0c0280f0886b4aa0ff4d
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/14/2021
ms.locfileid: "112077697"
---
# <a name="monitor-delegation-changes-in-your-managing-tenant"></a>Superviser les changements de délégation dans votre locataire gérant

En tant que fournisseur de services, vous souhaiterez peut-être savoir quand les abonnements clients ou les groupes de ressources sont délégués à votre locataire par le biais d’[Azure Lighthouse](../overview.md) ou quand des ressources déléguées sont supprimées.

Dans le locataire gérant, le [journal d’activité Azure](../../azure-monitor/essentials/platform-logs-overview.md) effectue le suivi de l’activité de délégation au niveau du locataire. Cette activité journalisée comprend les délégations ajoutées ou supprimées de tous les locataires clients.

Cette rubrique décrit les autorisations nécessaires pour superviser l’activité de délégation à votre locataire (parmi tous vos clients). Elle comprend également un exemple de script qui illustre une méthode d’interrogation et de création de rapports sur ces données.

> [!IMPORTANT]
> Toutes ces étapes doivent être effectuées dans votre locataire gérant, plutôt que dans les locataires clients.
>
> Même si nous faisons référence aux fournisseurs de services et aux clients dans cette rubrique, les [entreprises gérant plusieurs locataires](../concepts/enterprise.md) peuvent utiliser les mêmes processus.

## <a name="enable-access-to-tenant-level-data"></a>Activer l’accès aux données au niveau du locataire

Pour accéder aux données du journal d’activité au niveau du locataire, un compte doit se voir attribuer le rôle intégré [Lecteur d’analyse](../../role-based-access-control/built-in-roles.md#monitoring-reader) Azure au niveau de l’étendue racine (/). Cette affectation doit être effectuée par un utilisateur qui a le rôle Administrateur général avec accès élevé supplémentaire.

### <a name="elevate-access-for-a-global-administrator-account"></a>Élever l’accès d’un compte Administrateur général

Pour attribuer un rôle au niveau de l’étendue racine (/), vous devez avoir le rôle Administrateur général avec un accès élevé. Cet accès élevé doit être ajouté uniquement quand vous devez effectuer l’attribution de rôle, puis supprimé quand vous avez terminé.

Pour obtenir des instructions détaillées sur l’ajout et la suppression de l’élévation, consultez [Élever l’accès pour gérer tous les abonnements et groupes d’administration Azure](../../role-based-access-control/elevate-access-global-admin.md).

Une fois que vous avez élevé votre accès, votre compte dispose du rôle Administrateur de l’accès utilisateur dans Azure au niveau de l’étendue racine. Cette attribution de rôle vous permet de voir toutes les ressources et d’attribuer des accès dans n’importe quel abonnement ou groupe d’administration de l’annuaire ainsi que d’effectuer des attributions de rôle au niveau de l’étendue racine.

### <a name="assign-the-monitoring-reader-role-at-root-scope"></a>Attribuer le rôle Lecteur d’analyse au niveau de l’étendue racine

Une fois que vous avez élevé votre accès, vous pouvez attribuer les autorisations appropriées à un compte afin qu’il puisse interroger les données du journal d’activité au niveau du locataire. Ce compte doit se voir attribuer le rôle intégré [Lecteur d’analyse](../../role-based-access-control/built-in-roles.md#monitoring-reader) Azure au niveau de l’étendue racine de votre locataire gérant.

> [!IMPORTANT]
> L’octroi d’une attribution de rôle au niveau de l’étendue racine signifie que les mêmes autorisations s’appliquent à toutes les ressources du locataire. Comme il s’agit d’un niveau d’accès étendu, vous pouvez [attribuer ce rôle à un compte de principal de service et utiliser ce compte pour interroger les données](#use-a-service-principal-account-to-query-the-activity-log). Vous pouvez également attribuer le rôle Lecteur d’analyse au niveau de l’étendue racine à des utilisateurs individuels ou à des groupes d’utilisateurs pour qu’ils puissent [consulter les informations de délégation directement dans le portail Azure](#view-delegation-changes-in-the-azure-portal). Si vous procédez ainsi, sachez qu’il s’agit d’un niveau d’accès large qui doit être limité au moins d’utilisateurs possible.

Utilisez l’une des méthodes suivantes pour effectuer l’attribution au niveau de l’étendue racine.

#### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

New-AzRoleAssignment -SignInName <yourLoginName> -Scope "/" -RoleDefinitionName "Monitoring Reader"  -ObjectId <objectId> 
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az role assignment create --assignee 00000000-0000-0000-0000-000000000000 --role "Monitoring Reader" --scope "/"
```

### <a name="remove-elevated-access-for-the-global-administrator-account"></a>Supprimer l’accès élevé du compte Administrateur général

Une fois que vous avez attribué le rôle Lecteur d’analyse au niveau de l’étendue racine au compte souhaité, veillez à [supprimer l’accès élevé](../../role-based-access-control/elevate-access-global-admin.md#remove-elevated-access) du compte Administrateur général, car ce niveau d’accès n’est plus nécessaire.

## <a name="view-delegation-changes-in-the-azure-portal"></a>Afficher les modifications de délégation dans le Portail Azure

Les utilisateurs qui ont été affectés au rôle Lecteur d’analyse au niveau de l’étendue racine peuvent consulter les modifications de délégation directement dans le portail Azure.

1. Accédez à la page **Mes clients**, puis sélectionnez **Journal d’activité** dans le menu de navigation de gauche.
1. Assurez-vous qu’**Activité du répertoire** est sélectionné dans le filtre près du haut de l’écran.

Une liste des modifications de délégation s’affiche. Vous pouvez sélectionner **Modifier les colonnes** pour afficher ou masquer les éléments **État**, **Catégorie d’événement**, **Heure**, **Horodatage**, **Abonnement**, **Événement initié par**, **Groupe de ressources**, **Type de ressource** et les valeurs des **Ressources**.

:::image type="content" source="../media/delegation-activity-portal.jpg" alt-text="Capture d’écran des modifications de délégation dans le portail Azure.":::

## <a name="use-a-service-principal-account-to-query-the-activity-log"></a>Utiliser un compte de principal de service pour interroger le journal d’activité

Comme le rôle Lecteur d’analyse au niveau de l’étendue racine est un niveau d’accès étendu, vous pouvez attribuer ce rôle à un compte de principal de service et utiliser ce compte pour interroger les données à l’aide du script ci-dessous.

> [!IMPORTANT]
> Actuellement, les locataires ayant une grande activité de délégation peuvent rencontrer des erreurs lors de l’interrogation de ces données.

Lorsque vous utilisez un compte de principal de service pour interroger le journal d’activité, nous vous recommandons les meilleures pratiques suivantes :

- [Créez un compte de principal de service](../../active-directory/develop/howto-create-service-principal-portal.md) dédié uniquement à cette fonction, au lieu d’attribuer ce rôle à un principal de service existant utilisé pour d’autres tâches d’automatisation.
- Assurez-vous que ce principal de service n’a pas accès aux ressources client déléguées.
- [Utilisez un certificat pour l’authentification](../../active-directory/develop/howto-create-service-principal-portal.md#authentication-two-options) et [stockez-le de manière sécurisée dans Azure Key Vault](../../key-vault/general/security-features.md).
- Limitez les utilisateurs habilités à agir pour le compte du principal de service.

Une fois que vous avez créé un compte de principal de service avec un accès Lecteur d’analyse au niveau de l’étendue racine de votre locataire gérant, vous pouvez l’utiliser pour interroger l’activité de délégation et créer des rapports sur celle-ci dans votre locataire.

[Ce script Azure PowerShell](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/monitor-delegation-changes) peut être utilisé pour interroger le dernier jour d'activité et détecter tout ajout ou suppression de délégation (ou tentative ayant échoué). Il interroge les données du [journal d’activité du locataire](/rest/api/monitor/TenantActivityLogs/List), puis construit les valeurs suivantes pour créer des rapports sur les délégations ajoutées ou supprimées :

- **DelegatedResourceId** : ID de l’abonnement ou du groupe de ressources délégué
- **CustomerTenantId** : ID du locataire client
- **CustomerSubscriptionId** : ID d’abonnement qui a été délégué ou qui contient le groupe de ressources qui a été délégué
- **CustomerDelegationStatus** : changement d’état de la ressource déléguée (réussite ou échec)
- **EventTimeStamp** : date et heure auxquelles le changement de délégation a été journalisé

Lors de l’interrogation de ces données, gardez à l’esprit les points suivants :

- Si plusieurs groupes de ressources sont délégués dans un déploiement unique, des entrées distinctes sont retournées pour chaque groupe de ressources.
- Les modifications apportées à une délégation précédente (par exemple, la mise à jour de la structure d’autorisation) sont journalisées en tant que délégation ajoutée.
- Comme indiqué plus haut, un compte doit avoir le rôle intégré Lecteur d’analyse Azure au niveau de l’étendue racine (/) pour pouvoir accéder à ces données au niveau du locataire.
- Vous pouvez utiliser ces données dans vos propres workflows et rapports. Par exemple, vous pouvez utiliser l’[API Collecte de données HTTP (préversion publique)](../../azure-monitor/logs/data-collector-api.md) pour journaliser les données dans Azure Monitor à partir d’un client d’API REST, puis utiliser des [groupes d’actions](../../azure-monitor/alerts/action-groups.md) pour créer des notifications ou des alertes.

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Azure Lighthouse: Query Tenant Activity Log for registered/unregistered delegations for the last 1 day

$GetDate = (Get-Date).AddDays((-1))

$dateFormatForQuery = $GetDate.ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ssZ")

# Getting Azure context for the API call
$currentContext = Get-AzContext

# Fetching new token
$azureRmProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = [Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient]::new($azureRmProfile)
$token = $profileClient.AcquireAccessToken($currentContext.Tenant.Id)

$listOperations = @{
    Uri     = "https://management.azure.com/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&`$filter=eventTimestamp ge '$($dateFormatForQuery)'"
    Headers = @{
        Authorization  = "Bearer $($token.AccessToken)"
        'Content-Type' = 'application/json'
    }
    Method  = 'GET'
}
$list = Invoke-RestMethod @listOperations

# First link can be empty - and point to a next link (or potentially multiple pages)
# While you get more data - continue fetching and add result
while($list.nextLink){
    $list2 = Invoke-RestMethod $list.nextLink -Headers $listOperations.Headers -Method Get
    $data+=$list2.value;
    $list.nextLink = $list2.nextlink;
}

$showOperations = $data;

if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/register/action") {
    $registerOutputs = $showOperations | Where-Object -FilterScript { $_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/register/action" }
    foreach ($registerOutput in $registerOutputs) {
        $eventDescription = $registerOutput.description | ConvertFrom-Json;
    $registerOutputdata = [pscustomobject]@{
        Event                    = "An Azure customer has registered delegated resources to your Azure tenant";
        DelegatedResourceId      = $eventDescription.delegationResourceId; 
        CustomerTenantId         = $eventDescription.subscriptionTenantId;
        CustomerSubscriptionId   = $eventDescription.subscriptionId;
        CustomerDelegationStatus = $registerOutput.status.value;
        EventTimeStamp           = $registerOutput.eventTimestamp;
        }
        $registerOutputdata | Format-List
    }
}
if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/unregister/action") {
    $unregisterOutputs = $showOperations | Where-Object -FilterScript { $_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/unregister/action" }
    foreach ($unregisterOutput in $unregisterOutputs) {
        $eventDescription = $registerOutput.description | ConvertFrom-Json;
    $unregisterOutputdata = [pscustomobject]@{
        Event                    = "An Azure customer has unregistered delegated resources from your Azure tenant";
        DelegatedResourceId      = $eventDescription.delegationResourceId;
        CustomerTenantId         = $eventDescription.subscriptionTenantId;
        CustomerSubscriptionId   = $eventDescription.subscriptionId;
        CustomerDelegationStatus = $unregisterOutput.status.value;
        EventTimeStamp           = $unregisterOutput.eventTimestamp;
        }
        $unregisterOutputdata | Format-List
    }
}
else {
    Write-Output "No new delegation events for tenant: $($currentContext.Tenant.TenantId)"
}
```

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment [intégrer des clients à Azure Lighthouse](onboard-customer.md).
- Découvrez [Azure Monitor](../../azure-monitor/index.yml) et le [journal d’activité Azure](../../azure-monitor/essentials/platform-logs-overview.md).
- Consultez l’exemple de classeur [Journaux d’activité par domaine](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/workbook-activitylogs-by-domain) pour savoir comment afficher les journaux d’activité Azure des différents abonnements avec une option permettant un filtrage par nom de domaine.
