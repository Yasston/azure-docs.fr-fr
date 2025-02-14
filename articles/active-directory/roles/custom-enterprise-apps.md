---
title: Créer des rôles personnalisés pour gérer les applications d’entreprise dans Azure Active Directory
description: Créez et attribuez des rôles Azure AD personnalisés pour l’accès aux applications d’entreprise dans Azure Active Directory.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: how-to
ms.date: 05/14/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: db22b44e032261d138d74e34340dca6fcaf75779
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2021
ms.locfileid: "110092869"
---
# <a name="create-custom-roles-to-manage-enterprise-apps-in-azure-active-directory"></a>Créer des rôles personnalisés pour gérer les applications d’entreprise dans Azure Active Directory

Cet article explique comment créer un rôle personnalisé avec des autorisations pour gérer les attributions d’applications d’entreprise pour les utilisateurs et groupes dans Azure Active Directory (Azure AD). Pour connaître les éléments des attributions de rôles et la signification des termes tels que le sous-type, l’autorisation et le jeu de propriétés, consultez [Vue d’ensemble des rôles personnalisés](custom-overview.md).

## <a name="prerequisites"></a>Prérequis

- Licence Azure AD Premium P1 ou P2
- Administrateur de rôle privilégié ou administrateur général
- Module AzureADPreview lors de l’utilisation de PowerShell
- Consentement de l’administrateur lors de l’utilisation de l’Explorateur graphique pour l’API Microsoft Graph

Pour plus d’informations, consultez [Configuration requise pour l’utilisation de PowerShell ou de l’Explorateur graphique](prerequisites.md).

## <a name="enterprise-app-role-permissions"></a>Autorisations des rôles des applications d’entreprise

Deux autorisations d’applications d’entreprise sont décrites dans cet article. Tous les exemples utilisent l’autorisation de mise à jour.

* Pour lire les attributions d’utilisateurs et de groupes au niveau de l’étendue, accordez l’autorisation `microsoft.directory/servicePrincipals/appRoleAssignedTo/read`
* Pour gérer les attributions d’utilisateurs et de groupes au niveau de l’étendue, accordez l’autorisation `microsoft.directory/servicePrincipals/appRoleAssignedTo/update`

L’octroi de l’autorisation de mise à jour permet au destinataire de gérer les attributions d’utilisateurs et de groupes aux applications d’entreprise. L’étendue des attributions d’utilisateurs et/ou de groupes peut être accordée pour une seule application ou pour toutes les applications. Si elle est accordée au niveau de l’organisation, le destinataire peut gérer les attributions pour toutes les applications. Si elle est accordée au niveau de l’application, le destinataire peut uniquement gérer les attributions pour l’application spécifiée.

L’octroi de l’autorisation de mise à jour s’effectue en deux étapes :

1. Créer un rôle personnalisé avec l’autorisation `microsoft.directory/servicePrincipals/appRoleAssignedTo/update`
1. Octroyez aux utilisateurs ou groupes des autorisations pour gérer les attributions d’utilisateurs et de groupes aux applications d’entreprise. Vous pouvez alors définir l’étendue au niveau de l’organisation ou d’une application unique.

## <a name="azure-portal"></a>Portail Azure

### <a name="create-a-new-custom-role"></a>Créer un rôle personnalisé

>[!NOTE]
> Les rôles personnalisés sont créés et gérés au niveau de l’organisation, et sont uniquement disponibles à partir de la page Vue d’ensemble de l’organisation.

1. Connectez-vous au [Centre d’administration Azure AD](https://aad.portal.azure.com).
1. Sélectionnez **Azure Active Directory**, **Rôles et administrateurs**, puis **Nouveau rôle personnalisé**.

    ![Ajouter un rôle personnalisé à partir de la liste des rôles dans Azure AD](./media/custom-enterprise-apps/new-custom-role.png)

1. Sous l’onglet **De base**, indiquez « Gérer les attributions d’utilisateurs et de groupes » pour le nom du rôle et « Octroyer des autorisations pour gérer les attributions d’utilisateurs et de groupes » pour la description de rôle, puis sélectionnez **Suivant**.

    ![Indiquer le nom et la description du rôle personnalisé](./media/custom-enterprise-apps/role-name-and-description.png)

1. Sous l'onglet **Autorisations**, entrez « microsoft.directory/servicePrincipals/appRoleAssignedTo/update » dans la zone de recherche, puis activez les cases à cocher en regard des autorisations souhaitées, puis sélectionnez **Suivant**.

    ![Ajouter les autorisations au rôle personnalisé](./media/custom-enterprise-apps/role-custom-permissions.png)

1. Sur l’onglet **Vérifier + Créer**, vérifiez les permissions, puis sélectionnez **Créer**.

    ![Vous pouvez maintenant créer le rôle personnalisé.](./media/custom-enterprise-apps/role-custom-create.png)

### <a name="assign-the-role-to-a-user-using-the-azure-portal"></a>Attribuer le rôle à un utilisateur à l’aide du portail Azure

1. Connectez-vous au [Centre d’administration Azure AD](https://aad.portal.azure.com).
1. Sélectionnez **Azure Active Directory**, puis **Rôles et administrateurs**.
1. Sélectionnez le rôle **Octroyer des autorisations pour gérer les attributions d’utilisateurs et de groupes**.

    ![Ouvrir Rôles et administrateurs et rechercher le rôle personnalisé](./media/custom-enterprise-apps/select-custom-role.png)

1. Sélectionnez **Ajouter une attribution**, sélectionnez l’utilisateur souhaité, puis cliquez sur **Sélectionner** pour ajouter une attribution de rôle à cet utilisateur.

    ![Ajouter une attribution pour le rôle personnalisé à l’utilisateur](./media/custom-enterprise-apps/assign-user-to-role.png)

#### <a name="assignment-tips"></a>Conseils relatifs aux attributions

* Pour octroyer des autorisations aux destinataires afin de gérer les utilisateurs et l’accès au groupe pour toutes les applications d’entreprise à l’échelle de l’organisation, commencez par la liste **Roles et administrateurs** de la page **Vue d’ensemble** Azure AD de votre organisation.
* Pour octroyer des autorisations aux destinataires afin de gérer les utilisateurs et l’accès au groupe pour une application d’entreprise spécifique, accédez à cette application dans Azure AD et ouvrez la liste **Rôles et administrateurs** de cette application. Sélectionnez le nouveau rôle personnalisé et terminez l’attribution d’utilisateurs ou de groupe. Les destinataires peuvent uniquement gérer l’accès des utilisateurs et des groupes pour l’application spécifique.
* Pour tester l’attribution de votre rôle personnalisé, connectez-vous en tant que destinataire et ouvrez la page **Utilisateurs et groupes** de l’application afin de vérifier que l’option **Ajouter un utilisateur** est activée.

    ![Vérifier les autorisations des utilisateurs](./media/custom-enterprise-apps/verify-user-permissions.png)

## <a name="powershell"></a>PowerShell

Pour plus d’informations, consultez [Créer et attribuer un rôle personnalisé](custom-create.md) et [Affecter des rôles personnalisés avec une étendue de ressources à l’aide de PowerShell](custom-assign-powershell.md).

### <a name="create-a-custom-role"></a>Créer un rôle personnalisé

Créez un nouveau rôle à l’aide du script PowerShell suivant :

```PowerShell
# Basic role information
$description = "Manage user and group assignments"
$displayName = "Can manage user and group assignments for Applications"
$templateId = (New-Guid).Guid

# Set of permissions to grant
$allowedResourceAction = @("microsoft.directory/servicePrincipals/appRoleAssignedTo/update")
$resourceActions = @{'allowedResourceActions'= $allowedResourceAction}
$rolePermission = @{'resourceActions' = $resourceActions}
$rolePermissions = $rolePermission

# Create new custom admin role
$customRole = New-AzureADMSRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="assign-the-custom-role"></a>Attribuer le rôle personnalisé

Attribuez le rôle à l’aide de ce script PowerShell.

```powershell
# Get the user and role definition you want to link
$user = Get-AzureADUser -Filter "userPrincipalName eq 'chandra@example.com'"
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Manage user and group assignments'"

# Get app registration and construct resource scope for assignment.
$appRegistration = Get-AzureADApplication -Filter "displayName eq 'My Filter Photos'"
$resourceScope = '/' + $appRegistration.objectId

# Create a scoped role assignment
$roleAssignment = New-AzureADMSRoleAssignment -ResourceScope $resourceScope -RoleDefinitionId $roleDefinition.Id -PrincipalId $user.objectId
```

## <a name="microsoft-graph-api"></a>API Microsoft Graph

Créer un rôle personnalisé de l’exemple fourni dans l’API Microsoft Graph Pour plus d’informations, consultez [Créer et attribuer un rôle personnalisé](custom-create.md) et [Attribuer des rôles d’administrateur personnalisés à l’aide de l’API Microsoft Graph](custom-assign-graph.md).

Requête HTTP pour créer le rôle personnalisé.

```HTTP
POST
https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitionsIsEnabled $true
{
    "description":"Can manage user and group assignments for Applications.",
    "displayName":" Manage user and group assignments",
    "isEnabled":true,
    "rolePermissions":
    [
        {
            "resourceActions":
            {
                "allowedResourceActions":
                [
                    "microsoft.directory/servicePrincipals/appRoleAssignedTo/update"
                ]
            },
            "condition":null
        }
    ],
    "templateId":"<PROVIDE NEW GUID HERE>",
    "version":"1"
}
```

### <a name="assign-the-custom-role-using-the-microsoft-graph-api"></a>Assigner le rôle personnalisé à l’aide de l’API Microsoft Graph

L’attribution de rôle combine un ID de principal de sécurité (qui peut être un utilisateur ou un principal de service), un ID de définition de rôle et une étendue de ressource de Azure AD. Pour plus d’informations sur les éléments d’une attribution de rôle, consultez [Vue d’ensemble des rôles personnalisés](custom-overview.md)

Requête HTTP pour assigner un rôle personnalisé.

```HTTP
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments

{
    "principalId":"<PROVIDE OBJECTID OF USER TO ASSIGN HERE>",
    "roleDefinitionId":"<PROVIDE OBJECTID OF ROLE DEFINITION HERE>",
    "resourceScopes":["/"]
}
```

## <a name="next-steps"></a>Étapes suivantes

* [Explorer les autorisations de rôle personnalisé disponibles pour les applications d’entreprise](custom-enterprise-app-permissions.md)
