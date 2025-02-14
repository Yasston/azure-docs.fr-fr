---
title: Accès conditionnel basé sur les risques de connexion – Azure Active Directory
description: Créer des stratégies d’accès conditionnel basé sur les risques de connexion dans Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/04/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 99d8fbf65cdfd4a56f4e7bec197131a1274b0beb
ms.sourcegitcommit: 6323442dbe8effb3cbfc76ffdd6db417eab0cef7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/28/2021
ms.locfileid: "110612798"
---
# <a name="conditional-access-sign-in-risk-based-conditional-access"></a>Accès conditionnel : Accès conditionnel basé sur les risques de connexion

La plupart des utilisateurs ont un comportement normal qui peut être suivi. Lorsqu’ils dévient de cette norme, il peut être risqué de les autoriser à se connecter uniquement. Vous pouvez bloquer ces utilisateurs ou simplement leur demander d’effectuer une authentification multifacteur pour prouver qu’ils sont vraiment ceux qu’ils prétendent être. 

Un risque de connexion reflète la probabilité qu’une requête d’authentification donnée soit rejetée par le propriétaire de l'identité. Les organisations disposant de licences Azure AD Premium P2 peuvent créer des stratégies d’accès conditionnel incorporant les [détections de risques de connexion d’Azure AD Identity Protection](../identity-protection/concept-identity-protection-risks.md#sign-in-risk).

Il existe deux emplacements où cette stratégie peut être configurée, l’Accès conditionnel et la Protection de l’identité. La configuration à l’aide d’une stratégie d’accès conditionnel est la méthode privilégiée qui fournit davantage de contexte, notamment des données de diagnostic améliorées, l’intégration du mode de création de rapports uniquement, la prise en charge API Graph et la possibilité d’utiliser d’autres attributs d’accès conditionnel dans la stratégie.

## <a name="enable-with-conditional-access-policy"></a>Activer avec la stratégie d’accès conditionnel

1. Connectez-vous au **portail Microsoft Azure** en tant qu’administrateur général, administrateur de sécurité ou administrateur de l’accès conditionnel.
1. Accédez à **Azure Active Directory** > **Sécurité** > **Accès conditionnel.**
1. Sélectionnez **Nouvelle stratégie**.
1. Donnez un nom à votre stratégie. Nous recommandons aux organisations de créer une norme explicite pour les noms de leurs stratégies.
1. Sous **Affectations**, sélectionnez **Utilisateurs et groupes**.
   1. Sous **Inclure**, sélectionnez **Tous les utilisateurs**.
   1. Sous **Exclure**, sélectionnez **Utilisateurs et groupes**, puis choisissez les comptes d’accès d’urgence ou de secours de votre organisation. 
   1. Sélectionnez **Terminé**.
1. Sous **Applications ou actions cloud** > **Inclure**, sélectionnez **Toutes les applications cloud**.
1. Dans **Conditions** > **Risque de connexion**, définissez **Configurer** sur **Oui**. Sous **Sélectionner le niveau de risque de connexion auquel cette stratégie s’applique** 
   1. Sélectionnez **Haut** et **Moyen**.
   1. Sélectionnez **Terminé**.
1. Sous **Contrôles d’accès** > **Accorder**, sélectionnez **Accorder l'accès**, **Requérir l’authentification multifacteur**, et sélectionnez **Sélectionner**.
1. Confirmez vos paramètres et réglez **Activer la stratégie** sur **Activé**.
1. Sélectionnez **Créer** pour créer votre stratégie.

## <a name="enable-through-identity-protection"></a>Activer avec Identity Protection

1. Connectez-vous au **portail Azure**.
1. Sélectionnez **Tous les services**, puis accédez à **Azure AD Identity Protection**.
1. Sélectionnez **Stratégie de connexion à risque**.
1. Sous **Attributions**, sélectionnez **Utilisateurs**.
   1. Sous **Inclure**, sélectionnez **Tous les utilisateurs**.
   1. Sous **Exclure**, sélectionnez **Sélectionner les utilisateurs exclus**, choisissez les comptes d’accès d’urgence ou de secours de votre organisation, puis sélectionnez **Sélectionner**.
   1. Sélectionnez **Terminé**.
1. Sous **Conditions**, sélectionnez **Risque de connexion**, puis choisissez **Moyen et supérieur**.
   1. Sélectionnez **Sélectionner**, puis **Terminé**.
1. Sous **Contrôles** > **Accès**, choisissez **Autoriser l’accès**, puis sélectionnez **Exiger une authentification multifacteur**.
   1. Sélectionnez **Sélectionner**.
1. Définissez **Appliquer la stratégie** sur **Activé**.
1. Sélectionnez **Enregistrer**.

## <a name="next-steps"></a>Étapes suivantes

[Stratégies d’accès conditionnel courantes](concept-conditional-access-policy-common.md)

[Accès conditionnel basé sur les risques d’utilisateur](howto-conditional-access-policy-risk-user.md)

[Déterminer l'impact à l'aide du mode Rapport seul de l'Accès conditionnel](howto-conditional-access-insights-reporting.md)

[Simuler le comportement de connexion à l’aide de l’outil What If pour l’accès conditionnel](troubleshoot-conditional-access-what-if.md)

[Qu’est-ce que Azure Active Directory Identity Protection ?](../identity-protection/overview-identity-protection.md)
