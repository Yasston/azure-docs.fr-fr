---
title: Chiffrements du chiffrement pris en charge pour Azure FXT Edge Filer
description: Liste des normes de chiffrement utilisées par les clusters FXT Edge Filer.
author: ekpgh
ms.author: v-erkel
ms.service: fxt-edge-filer
ms.topic: conceptual
ms.date: 05/20/2021
ms.openlocfilehash: b395e0cd2358ef22a223a58bcf8041fa8052b9cd
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/03/2021
ms.locfileid: "111377468"
---
# <a name="supported-encryption-standards-for-azure-fxt-edge-filer"></a>Normes de chiffrement prises en charge pour Azure FXT Edge Filer

Ce document décrit les normes de chiffrement nécessaires à Azure FXT Edge Filer. Ces normes sont implémentées à partir de la version 5.1.1.2 du système d’exploitation.

Ces normes s’appliquent à [Avere vFXT pour Azure](../avere-vfxt/index.yml) ainsi qu’à Azure FXT Edge Filer.

Tout système d’administration ou d’infrastructure qui se connecte au cache Azure FXT Edge Filer ou à des nœuds individuels doit respecter ces normes.

(Les machines clientes montent le cache avec NFS, par conséquent ces exigences de chiffrement ne s’appliquent pas. Utilisez d’autres mesures appropriées pour garantir leur sécurité.)

## <a name="tls-standard"></a>Standard TLS

* TLS1.2 doit être activé
* SSL V2 et V3 doivent être désactivés

TLS1.0 et TLS1.1 peuvent être utilisés si cela est absolument nécessaire pour la compatibilité descendante avec les magasins d’objets privés, mais il est préférable de mettre à niveau votre stockage privé vers des normes de sécurité modernes. Contactez les services du Support technique Microsoft pour plus d’informations.

## <a name="permitted-cipher-suites"></a>Suites de chiffrement autorisées

Azure FXT Edge Filer permet de négocier les suites de chiffrement TLS suivantes :

* ECDHE-ECDSA-AES128-GCM-SHA256
* ECDHE-ECDSA-AES256-GCM-SHA384
* ECDHE-RSA-AES128-GCM-SHA256
* ECDHE-RSA-AES256-GCM-SHA384
* ECDHE-ECDSA-AES128-SHA256
* ECDHE-ECDSA-AES256-SHA384
* ECDHE-RSA-AES128-SHA256
* ECDHE-RSA-AES256-SHA384

L’interface HTTPS d’administration de cluster (utilisée pour l’interface graphique utilisateur web du Panneau de configuration et les connexions RPC d’administration) prend uniquement en charge les suites de chiffrement ci-dessus et le protocole TLS1.2. Aucun autre protocole ni suite de chiffrement n’est pris en charge lors de la connexion à l’interface d’administration.

## <a name="ssh-server-access"></a>Accès au serveur SSH

Ces normes s’appliquent au serveur SSH qui est intégré à ces produits.

Le serveur SSH n’autorise pas la connexion à distance en tant que « racine » superutilisateur. Si l’accès SSH à distance est exigé sur instruction du Support technique Microsoft, connectez-vous en tant qu’utilisateur « administrateur » SSH, qui dispose d’un interpréteur de commandes restreint.

Les suites de chiffrement SSH suivantes sont disponibles sur le serveur SSH du cluster. Assurez-vous que tous les clients qui utilisent le protocole SSH pour se connecter au cluster disposent d’un logiciel à jour qui satisfait à ces normes.

### <a name="ssh-encryption-standards"></a>Normes de chiffrement SSH

| Type | Valeurs prises en charge |
|--|--|
| Chiffrements | aes256-gcm@openssh.com</br> aes128-gcm@openssh.com</br> aes256-ctr</br> aes128-ctr |
| MACs | hmac-sha2-512-etm@openssh.com</br> hmac-sha2-256-etm@openssh.com</br> hmac-sha2-512</br> hmac-sha2-256 |
| algorithmes KEX | ecdh-sha2-nistp521</br> ecdh-sha2-nistp384</br> ecdh-sha2-nistp256</br> diffie-hellman-group-exchange-sha256 |
