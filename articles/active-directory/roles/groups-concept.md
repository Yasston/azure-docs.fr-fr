---
title: Utiliser des groupes cloud pour gérer les attributions de rôles dans Azure Active Directory | Microsoft Docs
description: Affichez un aperçu des rôles Azure AD personnalisés pour la délégation de la gestion des identités. Gérez les attributions de rôles Azure dans le portail Azure, PowerShell ou l’API Graph.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: article
ms.date: 05/05/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d037f93b1381fcd7f1816104f6fd7b2e1200c852
ms.sourcegitcommit: 070122ad3aba7c602bf004fbcf1c70419b48f29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2021
ms.locfileid: "111438933"
---
# <a name="use-cloud-groups-to-manage-role-assignments-in-azure-active-directory-preview"></a>Utiliser des groupes cloud pour gérer les attributions de rôles dans Azure Active Directory (préversion)

Azure Active Directory (Azure AD) introduit une préversion publique dans laquelle vous pouvez attribuer un groupe cloud à des rôles intégrés Azure AD. Grâce à cette fonctionnalité, vous pouvez utiliser des groupes pour accorder un accès administrateur dans Azure AD avec le minimum d’effort de la part de vos Administrateurs généraux et Administrateurs de rôle privilégié.

Examinez cet exemple : Contoso a recruté des personnes pour toutes les zones géographiques afin de gérer et réinitialiser les mots de passe des employés de son organisation Azure AD. Plutôt que de demander à un Administrateur de rôle privilégié ou à un Administrateur général d’attribuer le rôle Administrateur du support technique à chaque personne individuellement, il peut créer un groupe Contoso_Helpdesk_Administrators et l’attribuer au rôle. Lorsque des personnes rejoignent le groupe, elles se voient attribuer le rôle indirectement. Votre workflow de gouvernance actuel peut ensuite prendre en charge le processus d’approbation et l’audit de l’appartenance du groupe pour s’assurer que seuls les utilisateurs légitimes sont membres du groupe et donc affectés au rôle Administrateur du support technique.

## <a name="how-this-feature-works"></a>Principe de la fonctionnalité

Créez un nouveau groupe Microsoft 365 ou un groupe de sécurité dont la propriété « isAssignableToRole » a la valeur « true ». Vous pouvez également activer cette propriété lors de la création d’un groupe dans le portail Azure en activant l’option **Des rôles Azure AD peuvent être attribués au groupe**. Dans les deux cas, vous pouvez affecter le groupe à un ou plusieurs rôles Azure AD de la même façon que vous attribuez des rôles aux utilisateurs. Un maximum de 300 groupes assignables à un rôle peuvent être créés dans une seule organisation (locataire) Azure AD.

Si vous ne souhaitez pas que les membres du groupe disposent d’un accès permanent au rôle, vous pouvez utiliser Azure AD Privileged Identity Management. Assignez un groupe comme membre éligible d’un rôle Azure AD. Chaque membre du groupe peut alors faire activer son affectation pour le rôle auquel le groupe est assigné. Il peut ensuite activer son attribution de rôle pour une durée déterminée.

> [!Note]
> Vous devez disposer d’une version mise à jour de Privileged Identity Management pour être en mesure d’assigner un groupe à un rôle Azure AD via PIM. Vous pourriez être sur une version antérieure de PIM parce que votre organisation Azure AD exploite l’API Privileged Identity Management. Veuillez contacter l’alias pim_preview@microsoft.com pour déplacer votre organisation et mettre à jour votre API. Pour plus d’informations, consultez [Fonctionnalités et rôles Azure AD dans PIM](../privileged-identity-management/azure-ad-roles-features.md).

## <a name="why-we-enforce-creation-of-a-special-group-for-assigning-it-to-a-role"></a>Avantages de la création d’un groupe spécial pour l’attribution d’un rôle

Si un rôle est attribué à un groupe, tout administrateur informatique qui peut gérer l’appartenance au groupe peut également gérer indirectement l’appartenance à ce rôle. Par exemple, supposons qu’un groupe Contoso_User_Administrators est affecté au rôle Administrateur d’utilisateurs. Un Administrateur Exchange qui peut modifier l’appartenance au groupe peut s’ajouter au groupe Contoso_User_Administrators et devenir ainsi Administrateur d’utilisateurs. Comme vous pouvez le voir, un administrateur peut élever ses privilèges d’une manière que vous n’aviez pas prévue.

Azure AD vous permet de protéger un groupe affecté à un rôle à l’aide d’une nouvelle propriété appelée isAssignableToRole pour les groupes. Seuls les groupes cloud dont la propriété isAssignableToRole a la valeur « true » au moment de la création peuvent être affectés à un rôle. Cette propriété est non modifiable ; une fois qu’un groupe a été créé avec cette propriété définie sur « true », il ne peut pas être modifié. Vous ne pouvez pas définir la propriété sur un groupe existant. Nous avons conçu la manière dont les groupes sont affectés aux rôles afin d’éviter des violations potentielles :

- Seuls les Administrateurs généraux et les Administrateurs de rôle privilégié peuvent créer un groupe attribuable à un rôle (avec la propriété « isAssignableToRole » activée).
- Il ne peut pas s’agir d’un groupe dynamique Azure AD ; autrement dit, il doit avoir le type d’appartenance « Assigned ». L’alimentation automatisée de groupes dynamiques peut entraîner l’ajout d’un compte indésirable au groupe et don son affectation au rôle.
- Par défaut, seuls les Administrateurs généraux et les Administrateurs de rôle privilégié peuvent gérer l’appartenance à un groupe attribuable à un rôle, mais vous pouvez déléguer la gestion des groupes attribuables à un rôle en leur ajoutant des propriétaires de groupe.
- Pour empêcher une élévation des privilèges, seul un Administrateur de rôle privilégié ou un Administrateur général peut modifier les informations d’identification ou réinitialiser l’authentification multifacteur pour les membres et propriétaires d’un groupe attribuable à un rôle.
- Aucune imbrication. Un groupe ne peut pas être ajouté en tant que membre d’un groupe assignable à un rôle.

## <a name="limitations"></a>Limites

Les scénarios suivants ne sont pas pris en charge pour le moment :  

- Affecter des groupes locaux à des rôles Azure AD (intégrés ou personnalisés)

## <a name="known-issues"></a>Problèmes connus

- *Clients titulaires d’une licence Azure AD P2 uniquement :* n’affectez pas un groupe actif à un rôle à la fois par le biais d’Azure AD et de Privileged Identity Management (PIM). En particulier, n’affectez pas un rôle à un groupe assignable à un rôle lors de sa création *et* à l’aide de PIM plus tard. Cela entraînera des problèmes où les utilisateurs ne pourront pas voir leurs attributions de rôles actives dans PIM, ainsi que l’impossibilité de supprimer ces attributions PIM. Les attributions éligibles ne sont pas visées dans ce scénario. Si vous tentez d’effectuer cette affectation, vous pourriez constater des comportements inattendus, par exemple :
  - L’heure de fin de l’attribution de rôle peut s’afficher de manière incorrecte.
  - Dans le portail PIM, la rubrique **Mes rôles** ne peut afficher qu’une seule attribution de rôle, quel que soit le nombre de méthodes par lesquelles l’affectation est accordée (par le biais d’un ou de plusieurs groupes, directement ou non).
- *Clients titulaires d’une licence Azure AD P2 uniquement :* même après avoir supprimé le groupe, celui-ci apparaît toujours comme membre éligible du rôle dans l’interface utilisateur PIM. Fonctionnellement, il n’y a aucun problème ; il s’agit simplement d’un problème de cache dans le portail Azure.  
- Utilisez le nouveau [Centre d'administration Exchange](https://admin.exchange.microsoft.com/) pour les attributions de rôles via l'appartenance de groupe. L'ancien Centre d'administration Exchange ne prend pas encore en charge cette fonctionnalité. Les cmdlets Exchange PowerShell fonctionnent normalement.
- Le portail Azure Information Protection (le portail classique) ne reconnaît pas encore l’appartenance au rôle par le biais d’un groupe. Vous pouvez [migrer vers la plateforme unifiée d’étiquetage de confidentialité](/azure/information-protection/configure-policy-migrate-labels), puis utiliser le centre Security & Compliance d’Office 365 pour gérer les rôles à l’aide des affectations de groupe.
- L’[Apps Admin Center](https://config.office.com/) ne prend pas encore en charge cette fonctionnalité. Attribuez les utilisateurs directement au rôle Administrateur d’applications Office.
- Le [Centre de conformité Microsoft 365](https://compliance.microsoft.com/) ne prend pas encore en charge cette fonctionnalité. Affectez des utilisateurs directement aux rôles Azure AD appropriés pour utiliser ce portail.

Nous travaillons à corriger ces problèmes.

## <a name="license-requirements"></a>Conditions de licence :

L'utilisation de cette fonctionnalité nécessite une licence Azure AD Premium P1. En outre, l’utilisation de Privileged Identity Management à des fins d’activation de rôle juste-à-temps requiert une licence Azure AD Premium P2. Pour trouver la licence appropriée à vos besoins, consultez [Comparaison des fonctionnalités mises à la disposition générale des éditions Gratuite et Premium](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="next-steps"></a>Étapes suivantes

- [Créer un groupe assignable à un rôle](groups-create-eligible.md)
- [Attribuer un rôle à un groupe assignable à un rôle](groups-assign-role.md)
