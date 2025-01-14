---
title: Authentification Azure Virtual Desktop – Azure
description: Méthodes d’authentification pour Azure Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/26/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: bb5341f1f80350ce14b99a6ca5cf27343bd20675
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111745382"
---
# <a name="supported-authentication-methods"></a>Méthodes d’authentification prises en charge

Dans cet article, nous vous fournirons un aperçu des types d’authentification que vous pouvez utiliser dans Azure Virtual Desktop.

## <a name="session-host-authentication"></a>Authentification de l’hôte de session

Azure Virtual Desktop prend en charge NT LAN Manager (NTLM) et Kerberos pour l’authentification de l’hôte de session. Toutefois, pour utiliser Kerberos, le client doit obtenir des tickets de sécurité Kerberos auprès d’un service de centre de distribution de clés (KDC, Key Distribution Center) fonctionnant sur un contrôleur de domaine. Pour obtenir des tickets, le client a besoin d’une ligne de vue directe sur le contrôleur de domaine. Vous pouvez obtenir une ligne de vue directe en utilisant le réseau de votre entreprise. Vous pouvez également utiliser une connexion VPN à votre réseau d’entreprise ou configurer un [serveur proxy KDC](key-distribution-center-proxy.md).

Voici les méthodes de connexion actuellement prises en charge :

- Client Windows Desktop
    - Nom d’utilisateur et mot de passe
    - Carte à puce
    - Windows Hello Entreprise (approbation de certificat uniquement)
- Client Windows Store
    - Nom d’utilisateur et mot de passe
- Client web
    - Nom d’utilisateur et mot de passe
- Android
    - Nom d’utilisateur et mot de passe
- iOS
    - Nom d’utilisateur et mot de passe
- macOS
    - Nom d’utilisateur et mot de passe

>[!NOTE]
>La carte à puce et Windows Hello Entreprise peuvent uniquement utiliser Kerberos pour se connecter. La connexion avec Kerberos nécessite une ligne de mire sur le contrôleur de domaine ou un [serveur proxy KDC](key-distribution-center-proxy.md).

## <a name="hybrid-identity"></a>Identité hybride

Azure Virtual Desktop prend en charge les [identités hybrides](../active-directory/hybrid/whatis-hybrid-identity.md) par le biais d’Azure Active Directory (AD), notamment celles fédérées à l’aide des services de fédération Active Directory (AD FS). Comme les utilisateurs doivent pouvoir être découverts via Azure AD, Azure Virtual Desktop ne prend pas en charge les déploiements autonomes d’Active Directory avec AD FS.

## <a name="single-sign-on-sso"></a>Authentification unique (SSO)

Azure Virtual Desktop prend en charge [l’authentification unique à l’aide des services de fédération Active Directory (AD FS)](configure-adfs-sso.md) pour les clients Windows et web.

Sinon, la seule façon d’éviter d’être invité à entrer vos informations d’identification pour l’hôte de la session est de les enregistrer dans le client. Nous vous recommandons de le faire uniquement avec des appareils sécurisés pour empêcher d’autres utilisateurs d’accéder à vos ressources.

## <a name="next-steps"></a>Étapes suivantes

Vous êtes curieux de connaître d’autres moyens de sécuriser votre déploiement ? Consultez les [meilleures pratiques relatives à la sécurité](security-guide.md).
