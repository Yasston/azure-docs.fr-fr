---
title: Expédition autogérée Microsoft Azure Data Box Disk | Microsoft Docs sur les données
description: Décrit le flux de travail d’expédition autogérée pour les appareils Azure Data Box Disk
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 06/22/2021
ms.author: alkohli
ms.openlocfilehash: 66ff9018371de309b61824895492335e0d3bb763
ms.sourcegitcommit: 096e7972e2a1144348f8d648f7ae66154f0d4b39
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2021
ms.locfileid: "112517651"
---
# <a name="use-self-managed-shipping-for-azure-data-box-disk-in-the-azure-portal"></a>Utilisez l’expédition autogérée pour Azure Data Box Disk dans le portail Azure

Cet article décrit les tâches d’expédition autogérées pour la commande, l’enlèvement et le dépôt d’Azure Data Box Disk. Vous pouvez gérer Data Box Disk à l’aide du portail Azure.

## <a name="prerequisites"></a>Prérequis

L’expédition autogérée est disponible comme option lorsque vous [commandez Azure Data Box Disk](data-box-disk-deploy-ordered.md). L’expédition autogérée est disponible uniquement dans les régions suivantes :

* Gouvernement américain
* Royaume-Uni
* Europe de l’Ouest
* Australie
* Japon
* Singapour
* Corée du Sud
* Afrique du Sud
* Inde (préversion)
* Brésil

## <a name="use-self-managed-shipping"></a>Utiliser l’expédition autogérée

Quand passez une commande de Data Box Disk, vous pouvez choisir l’option d’expédition autogérée.

1. Dans votre commande Azure Data Box Disk, sous **Détails du contact**, sélectionnez **+ Ajouter une adresse de livraison**.

   ![Capture d’écran de l’Assistant Commande montrant l’étape Coordonnées avec l’option Ajouter une adresse d’expédition mise en évidence.](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-1.png)

2. Lorsque vous choisissez le type de livraison, sélectionnez l’option **expédition autogérée**. Cette option est disponible uniquement si vous êtes dans une des régions prises en charge, comme cela est décrit dans les prérequis.

3. Après avoir fourni votre adresse de livraison, vous devez la valider et terminer votre commande.

   ![Capture d’écran de la boîte de dialogue Ajouter une adresse de livraison avec les options Expédier avec et l’option Ajouter une adresse d’expédition en évidence.](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-2.png)

4. Une fois que l’appareil a été préparé et que vous avez reçu une notification par e-mail, vous pouvez planifier un prélèvement. Dans votre commande Azure Data Box Disk, accédez à **Vue d’ensemble** puis sélectionnez **Planifier l’enlèvement**.

   ![Commande d’un appareil Data Box à enlever](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-01b.png)

5. Suivez les instructions dans **Planifier la collecte pour Azure**. Avant de pouvoir obtenir votre code d’autorisation, vous devez envoyer un courrier électronique à [adbops@microsoft.com](mailto:adbops@microsoft.com) pour planifier l’enlèvement de l’appareil à partir du centre de données de votre région.

   ![Planifier l’enlèvement](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-02c.png)

   **Instructions pour le Brésil :** Si vous planifiez un enlèvement d’appareil au Brésil, spécifiez les informations suivantes dans votre e-mail. Le centre de données planifiera l’enlèvement après réception d’un document `Nota Fiscal`, ce qui peut prendre jusqu’à 4 jours ouvrables.

   ```
   Subject: Request Azure Data Box Disk pickup for order: <ordername>

   - Order name
   - Company name
   - Company legal name (if different) 
   - CNPJ (Business Tax ID, format: 00.000.000/0000-00) or CPF (Individual Tax ID, format: 000.000.000-00)
   - Address 
   - Country 
   - Phone number 
   - Contact name of the person who will pick up the Data Box Disk (A government-issued photo ID will be required to validate the contact’s identity upon arrival.)   
   ```

6. Une fois que vous avez planifié l’enlèvement de votre appareil, vous pouvez afficher votre code d’autorisation dans **Planifier l’enlèvement pour Azure**.

   ![Capture d’écran de la boîte de dialogue Panifier la récupération pour Azure avec la zone de texte Code d’autorisation pour la récupération mise en évidence.](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-01b.png)

   Prenez note de ce code d’autorisation. La personne chargée de l’enlèvement de l’appareil devra l’avoir en sa possession.

   Conformément aux exigences de sécurité, au moment de la planification de l’enlèvement, il est nécessaire de fournir le nom et les détails de la personne qui effectuera l’enlèvement. Vous ou le point de contact devez avoir une pièce d’identité avec photo approuvée par le gouvernement qui sera validée par le centre de données.

7. Procédez à l’enlèvement du Data Box Disk au centre de données à l’heure prévue.

   La personne chargée de l’enlèvement de l’appareil doit fournir ce qui suit :

   * Une copie de la confirmation par e-mail fournie par Microsoft Operations autorisant la visite du centre de données.

   * Le code d’autorisation. Le numéro de référence pour un enlèvement ou un dépôt est unique et validé dans le centre de données.

   * Une photo d’identité approuvée par le gouvernement. L’identité sera validée au centre de données, et le nom et les coordonnées de la personne chargée de l’enlèvement de l’appareil doivent être fournis lors de la planification de l’enlèvement.

   > [!NOTE]
   > Si vous manquez un rendez-vous planifié, vous devrez planifier un nouveau rendez-vous.

8. Votre commande passe automatiquement à l’état **Récupérée** une fois l’appareil enlevé au centre de données.

   ![Picked up (Récupérée)](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-ready-disk-01b.png)

9. Après avoir enlevé l’appareil, vous pouvez copier les données vers le ou les Data Box Disk sur votre site. Une fois la copie des données terminée, vous pouvez préparer l’expédition du Data Box Disk.

   Une fois la copie des données terminée, contactez le service des opérations pour planifier un rendez-vous pour le dépôt. Vous devrez partager les informations concernant la personne qui se rendra au centre de données pour déposer les disques. Le centre de données devra également vérifier le code d’autorisation au moment du dépôt. Vous trouverez le code d’autorisation pour le dépôt dans le portail Azure, sous **Planifier le dépôt**.

   > [!NOTE]
   > Ne partagez pas le code d’autorisation par e-mail. Il ne doit être vérifié qu’au centre de données lors du dépôt.

   **Instructions pour le Brésil :** Pour planifier un retour de l’appareil au Brésil, envoyez un e-mail à [adbops@microsoft.com](mailto:adbops@microsoft.com) avec les informations suivantes :

   ```
   Subject: Request Azure Data Box Disk drop-off for order: <ordername>

   - Order name
   - Contact name of the person who will drop off the Data Box Disk (A government-issued photo ID will be required to validate the contact’s identity upon arrival.) 
   - Inbound Nota Fiscal (A copy of the inbound Nota Fiscal will be required at drop-off.)   
   ```

10. Une fois que vous avez reçu une date de rendez-vous pour le dépôt, la commande doit se trouver à l’état **Prêt pour la réception au centre de données Azure** dans le portail Azure.

    ![Capture d’écran de la boîte de dialogue Ajouter une adresse d’expédition avec les options Expédier avec et le bouton Ajouter l’adresse d’expédition mis en évidence.](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-dropoff-02b.png)

11. Une fois que votre pièce d’identité et votre code d’autorisation ont été vérifiés et que vous avez déposé l’appareil dans le centre de données, l’état de la commande passe à **Reçue**.

    ![Réception terminée](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-received-01a.png)

11. Une fois l’appareil reçu, la copie des données se poursuit. Lorsque la copie est finie, la commande est terminée.

## <a name="next-steps"></a>Étapes suivantes

* [Démarrage rapide : Déployer Azure Data Box Disk à l’aide du portail Azure](data-box-disk-quickstart-portal.md)
