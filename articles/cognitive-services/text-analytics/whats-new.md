---
title: Nouveautés de l’API Analyse de texte
titleSuffix: Text Analytics - Azure Cognitive Services
description: Cet article fournit des informations sur les nouvelles versions et fonctionnalités de l’API Analyse de texte Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/12/2021
ms.author: aahi
ms.custom: references_regions
ms.openlocfilehash: f79c9cb7381f2325de2efca5e20b37c60ab72013
ms.sourcegitcommit: d2738669a74cda866fd8647cb9c0735602642939
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/13/2021
ms.locfileid: "113652468"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Nouveautés de l’API Analyse de texte

L’API Analyse de texte est mise à jour de manière continue. Pour vous informer des développements récents, cet article vous fournit des informations sur les nouvelles versions et fonctionnalités.

## <a name="july-2021"></a>Juillet 2021

### <a name="ga-release-updates"></a>Mises à jour de la version en disponibilité générale

* Disponibilité générale de [Analyse de texte pour le médical](how-tos/text-analytics-for-health.md?tabs=ner) pour les conteneurs et pour l’API hébergée (/health).
* Disponibilité générale pour [Exploration des opinions](how-tos/text-analytics-how-to-sentiment-analysis.md?tabs=version-3-1#opinion-mining).
* Disponibilité générale pour [Extraction et rédaction des informations d’identification personnelle](how-tos/text-analytics-how-to-entity-linking.md?tabs=version-3-1#personally-identifiable-information-pii).
* Disponibilité générale pour [Point de terminaison asynchrone (`/analyze`)](how-tos/text-analytics-how-to-call-api.md?tabs=synchronous#using-the-api-asynchronously).
* Exemples de [guides de démarrage rapide](quickstarts/client-libraries-rest-api.md) mis à jour avec le nouveau SDK. 

## <a name="june-2021"></a>Juin 2021

### <a name="general-api-updates"></a>Mises à jour générales de l’API

* Nouveau modèle-version `2021-06-01` pour l’extraction d’expressions clés basée sur des transformateurs. Il offre :
  * Prise en charge de 10 langues (Latin et CJC). 
  * Extraction améliorée d’expressions clés.
* La version du modèle `2021-06-01` pour [Reconnaissance d’entité nommée](how-tos/text-analytics-how-to-entity-linking.md) v3.x, qui fournit 
  * Amélioration de la qualité de l’intelligence artificielle et extension de la prise en charge linguistique à la catégorie d’entité *Compétence*. 
  * Ajout de la prise en charge de l’espagnol, du français, de l’allemand, de l’italien et du portugais pour la catégorie d’entité *Compétence*.
* Opération asynchrone (/analyze) et Analyse de texte pour le médical (préversion non contrôlée) disponibles dans toutes les régions. 

### <a name="text-analytics-for-health-updates"></a>Analyse de texte pour des mises à jour de l’intégrité

* Vous n’avez plus besoin de demander l’accès à la préversion d’Analyse de texte pour l’intégrité.
* Nouvelle version de modèle `2021-05-15` pour le point de terminaison `/health` et le conteneur local fournit
    * 5 nouveaux types d’entités : `ALLERGEN`, `CONDITION_SCALE`, `COURSE`, `EXPRESSION` et `MUTATION_TYPE`,
    * 14 nouveaux types de relations,
    * Détection d’assertion étendue aux nouveaux types d’entités, et
    * Prise en charge de la liaison pour le type d’entité ALLERGEN
* Nouvelle image pour le conteneur Analyse de texte pour le médical avec l’étiquette `3.0.016230002-onprem-amd64` et la version de modèle `2021-05-15`. Ce conteneur est disponible en téléchargement auprès de Microsoft Container Registry.
 
## <a name="may-2021"></a>Mai 2021

* [Réponse à une question personnalisée](../qnamaker/custom-question-answering.md) (précédemment QnA Maker) est à présent accessible à l’aide d’une ressource Analyse de texte. 

### <a name="general-api-updates"></a>Mises à jour générales de l’API

* Publication de la nouvelle API v3.1-preview.5, qui inclut ce qui suit : 
  * L’[API Analyse](how-tos/text-analytics-how-to-call-api.md?tabs=asynchronous) asynchrone prend maintenant en charge l’analyse des sentiments et l’exploration des opinions.
  * Le nouveau paramètre de requête `LoggingOptOut` est à présent disponible pour les clients qui souhaitent refuser l’enregistrement du texte d’entrée pour les rapports d’incident.  Pour en savoir plus sur ce paramètre, consultez l’article sur la [confidentialité des données](/legal/cognitive-services/text-analytics/data-privacy?context=/azure/cognitive-services/text-analytics/context/context).
* L’Analyse de texte pour l’intégrité et les opérations asynchrones d’analyse sont à présent disponibles dans toutes les régions

## <a name="march-2021"></a>Mars 2021

### <a name="general-api-updates"></a>Mises à jour générales de l’API
* Mise en production de la nouvelle API v 3.1-préversion. 4, qui comprend 
   * Modifications dans le corps de la réponse JSON d’exploration de données : 
      * `aspects` est maintenant `targets` et `opinions` est maintenant `assessments`. 
   * Modifications du corps de réponse JSON de l’API web hébergée de l’Analyse de texte pour l’intégrité : 
      * Le nom booléen `isNegated` d’un objet entité détecté pour la négation est déconseillé et remplacé par la détection d’assertion.
      * Une nouvelle propriété appelée `role` fait maintenant partie de la relation extraite entre un attribut et une entité, ainsi que la relation entre des entités.  Cela ajoute une spécificité au type de relation détecté.
   * La liaison d'entités est maintenant disponible en tant que tâche asynchrone dans le point de terminaison `/analyze`.
   * Un nouveau paramètre `pii-categories` est désormais disponible dans le point de terminaison `/pii`.
      * Ce paramètre vous permet de spécifier les entités PII sélectionnées, ainsi que celles qui ne sont pas prises en charge par défaut pour la langue d’entrée.
* Bibliothèques de client mises à jour, qui incluent les opérations d’analyse asynchrone et d’analyse de texte pour la santé. Vous trouverez des exemples sur GitHub :

    * [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics)
    * [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/)
    * [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)
    * [JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics/samples/v5/javascript)
    
> [!div class="nextstepaction"]
> [En savoir plus sur l’API d’Analyse de texte v3.1-Préversion.4](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-4/operations/Languages)

### <a name="text-analytics-for-health-updates"></a>Analyse de texte pour des mises à jour de l’intégrité

* Nouvelle version de modèle `2021-03-01` pour le point de terminaison `/health` et le conteneur local fournit
    * Un changement de nom du type d’entité `Gene` pour `GeneOrProtein`.
    * Nouveau type d’entité `Date`.
    * Détection d’assertion qui remplace la détection de négation (disponible uniquement dans l’API v 3.1-préversion. 4).
    * Nouvelle propriété préférée `name` pour les entités liées normalisée à partir de différents systèmes d’ontologies et de codage (disponibles uniquement dans l’API v 3.1-préversion. 4). 
* Une nouvelle image conteneur avec la balise `3.0.015490002-onprem-amd64` avec la nouvelle version du modèle `2021-03-01` a été mise en production dans le référentiel du conteneur en préversion. 
    * Cette image conteneur ne sera plus disponible en téléchargement à partir de `containerpreview.azurecr.io` à compter du 26 avril 2021.
* Une nouvelle image conteneur Analyse de texte pour l’intégrité avec ces mêmes modèle et version est désormais disponible à l’adresse `mcr.microsoft.com/azure-cognitive-services/textanalytics/healthcare`. À compter du 26 avril, vous pourrez uniquement télécharger le conteneur à partir de ce référentiel.

> [!div class="nextstepaction"]
> [En savoir plus sur l’API Analyse de texte pour la santé](how-tos/text-analytics-for-health.md)

### <a name="text-analytics-resource-portal-update"></a>Mise à jour du portail de ressources de l’Analyse de texte
* Les **enregistrements de texte traités** sont désormais disponibles en tant que métrique dans la section **Analyse** de votre ressource d’Analyse de texte dans le Portail Azure.  

## <a name="february-2021"></a>Février 2021

* La version de modèle `2021-01-15` pour le point de terminaison PII dans [Reconnaissance d’entité nommée](how-tos/text-analytics-how-to-entity-linking.md) v3.1-preview.x, qui fournit 
  * Une prise en charge étendue de 9 nouvelles langues
  * Une amélioration de la qualité de l’IA des catégories d’entités nommées pour les langues prises en charge.
* Les niveaux tarifaires S0 à S4 sont supprimés le 8 mars 2021. Si vous disposez d’une ressource Analyse de texte existante avec un niveau tarifaire compris entre S0 et S4, vous devez la mettre à jour pour utiliser le [niveau tarifaire](how-tos/text-analytics-how-to-call-api.md#change-your-pricing-tier) Standard (S).
* Le [conteneur Détection de langue](how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment) est désormais en disponibilité générale.
* La version 2.1 de l’API est en cours de retrait. 

## <a name="january-2021"></a>Janvier 2021

* La version du modèle `2021-01-15` pour [Reconnaissance d’entité nommée](how-tos/text-analytics-how-to-entity-linking.md) v3.x, qui fournit 
  * Prise en charge linguistique étendue pour [plusieurs catégories d’entités générales](named-entity-types.md). 
  * Amélioration de la qualité de l’IA des catégories d’entités générales pour toutes les langues v3 prises en charge. 

* La version du modèle `2021-01-05` pour la [détection de la langue](how-tos/text-analytics-how-to-language-detection.md), qui fournit une [prise en charge linguistique](language-support.md?tabs=language-detection) supplémentaire.

Ces versions de modèle ne sont pas disponibles actuellement dans la région USA Est. 

> [!div class="nextstepaction"]
> [En savoir plus sur le nouveau modèle NER](https://azure.microsoft.com/updates/text-analytics-ner-improved-ai-quality)

## <a name="december-2020"></a>Décembre 2020

* Détails de la [mise à jour de la tarification](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/) de l’API Analyse de texte.

## <a name="november-2020"></a>Novembre 2020

* [Nouveau point de terminaison](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Analyze) avec l’API Analyse de texte v3.1-preview.3 pour la nouvelle [API d’analyse asynchrone](how-tos/text-analytics-how-to-call-api.md?tabs=analyze), qui prend en charge le traitement par lots des opérations NER, PII et d’extraction d’expressions clés.
* [Nouveau point de terminaison](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Health) avec l’API Analyse de texte v3.1-preview.3 pour la nouvelle API hébergée [Analyse de texte pour la santé](how-tos/text-analytics-for-health.md) asynchrone avec prise en charge du traitement par lots.
* Les deux nouvelles fonctionnalités listées ci-dessus sont disponibles uniquement dans les régions suivantes : `West US 2`, `East US 2`, `Central US`, `North Europe` et `West Europe`.
* Le portugais brésilien `pt-BR` est maintenant pris en charge dans [Analyse des sentiments](how-tos/text-analytics-how-to-sentiment-analysis.md) v3.x, à partir de la version de modèle `2020-04-01`. Il s’ajoute à la prise en charge existante de `pt-PT` pour le portugais.
* Bibliothèques de client mises à jour, qui incluent les opérations d’analyse asynchrone et d’analyse de texte pour la santé. Vous trouverez des exemples sur GitHub :

    * [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics)
    * [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/)
    * [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)
    * 
> [!div class="nextstepaction"]
> [En savoir plus sur l’API Analyse de texte v3.1-Preview.3](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Languages)

## <a name="october-2020"></a>Octobre 2020

* Prise en charge de la langue Hindi pour la fonctionnalité Analyse des sentiments v3.x, à partir de la version `2020-04-01` du modèle. 
* La version de modèle `2020-09-01` pour le point de terminaison /languages de la v3 améliore la détection de la langue et renforce la précision.
* Disponibilité de la v3 dans les régions Inde Centre et Émirats arabes unis Nord.

## <a name="september-2020"></a>Septembre 2020

### <a name="general-api-updates"></a>Mises à jour générales de l’API

* Publication d’une nouvelle URL pour la préversion publique de l’API Analyse de texte v3.1 permettant la prise en charge des mises à jour des points de terminaison suivants de l’API Reconnaissance d'entité nommée v3 : 
    * Le point de terminaison `/pii` comprend désormais la nouvelle propriété `redactedText` dans la réponse JSON où les entités PII détectées dans le texte d’entrée sont remplacées par un `*` pour chaque caractère de ces entités.
    * Le point de terminaison `/linking` comprend désormais la propriété `bingID` dans la réponse JSON pour les entités liées.
* Les points de terminaison de l’API Analyse de texte (préversion) suivants ont été retirés le 4 septembre 2020 :
    * v2.1-preview
    * v3.0-preview
    * v3.0-preview.1
    
> [!div class="nextstepaction"]
> [En savoir plus sur l’API Analyse de texte v3.1-Preview.2](quickstarts/client-libraries-rest-api.md)

### <a name="text-analytics-for-health-container-updates"></a>Mises à jour du conteneur Analyse de texte pour la santé

Les mises à jour suivantes sont spécifiques à la version du mois de septembre du conteneur Analyse de texte pour la santé uniquement.
* Une nouvelle image conteneur avec la balise `1.1.013530001-amd64-preview` avec la nouvelle version `2020-09-03` du modèle a été publiée dans le référentiel du conteneur en préversion. 
* Cette version de modèle apporte des améliorations en matière de reconnaissance d’entités, de détection d’abréviation et de latence.

> [!div class="nextstepaction"]
> [En savoir plus sur l’API Analyse de texte pour la santé](how-tos/text-analytics-for-health.md)

## <a name="august-2020"></a>Août 2020

### <a name="general-api-updates"></a>Mises à jour générales de l’API

* Version de modèle `2020-07-01` pour les points de terminaison v3 `/keyphrases`, `/pii` et `/languages`, qui ajoute ce qui suit :
    * Des [catégories d’entités](named-entity-types.md?tabs=personal) supplémentaires spécifiques à un gouvernement ou un pays pour la Reconnaissance d’entité nommée.
    * Prise en charge du norvégien et du turc dans Analyse des sentiments v3.
* Une erreur HTTP 400 est désormais retournée pour les demandes d’API v3 qui dépassent les [limites de données](concepts/data-limits.md) publiées. 
* Les points de terminaison qui retournent un offset prennent désormais en charge le paramètre facultatif `stringIndexType`, qui ajuste les valeurs `offset` et `length` retournées pour qu’elles correspondent à un schéma d’index de chaîne [ pris en charge](concepts/text-offsets.md).

### <a name="text-analytics-for-health-container-updates"></a>Mises à jour du conteneur Analyse de texte pour la santé

Les mises à jour suivantes sont spécifiques à la version du mois d’août de l’Analyse de texte pour le conteneur d’intégrité uniquement.

* Nouvelle version de modèle pour l’Analyse de texte pour l’intégrité : `2020-07-24`
* Nouvelle URL pour l’envoi d’Analyse de texte pour des demandes de contrôle d’intégrité : `http://<serverURL>:5000/text/analytics/v3.2-preview.1/entities/health` (Veuillez noter qu’un effacement du cache du navigateur sera nécessaire pour pouvoir utiliser l’application web de démonstration incluse dans cette nouvelle image de conteneur)

Les propriétés suivantes de la réponse JSON ont changé :

* `type` a été renommé en `category` 
* `score` a été renommé en `confidenceScore`
* Les entités dans le champ `category` de la sortie JSON sont maintenant en casse Pascal. Les entités suivantes ont été renommées :
    * `EXAMINATION_RELATION` a été renommé en `RelationalOperator`.
    * `EXAMINATION_UNIT` a été renommé en `MeasurementUnit`.
    * `EXAMINATION_VALUE` a été renommé en `MeasurementValue`.
    * `ROUTE_OR_MODE` a été renommé en `MedicationRoute`.
    * L’entité relationnelle `ROUTE_OR_MODE_OF_MEDICATION` a été renommée en `RouteOfMedication`.

Les entités suivantes ont été ajoutées :

* NER
    * `AdministrativeEvent`
    * `CareEnvironment`
    * `HealthcareProfession`
    * `MedicationForm` 

* Extraction de relations
    * `DirectionOfCondition`
    * `DirectionOfExamination`
    * `DirectionOfTreatment`

> [!div class="nextstepaction"]
> [En savoir plus sur l’Analyse de texte pour le conteneur d’intégrité](how-tos/text-analytics-for-health.md)

## <a name="july-2020"></a>Juillet 2020 

### <a name="text-analytics-for-health-container---public-gated-preview"></a>Analyse de texte pour le conteneur d’intégrité – Préversion publique contrôlée

L’Analyse de texte pour le conteneur d’intégrité est désormais en préversion publique contrôlée, ce qui vous permet d’extraire des informations à partir de texte en anglais non structuré dans des documents cliniques tels que les formulaires d’admission des patients, les notes du médecin, les documents de recherche et les bilans de sortie d’hospitalisation. Actuellement, vous ne serez pas facturé pour l’utilisation de l’Analyse de texte pour le conteneur d’intégrité.

Le conteneur offre les fonctionnalités suivantes :

* Reconnaissance d’entité nommée
* Extraction des relations
* Liaison d’entités
* Négation

## <a name="may-2020"></a>Mai 2020

### <a name="text-analytics-api-v3-general-availability"></a>Disponibilité générale de l’API Analyse de texte v3

L’API Analyse de texte v3 est désormais en disponibilité générale avec les mises à jour suivantes :

* Version de modèle `2020-04-01`
* Nouvelles [limites de données](concepts/data-limits.md) pour chaque fonctionnalité
* Mise à jour de la [prise en charge linguistique](language-support.md) pour [Analyse des sentiments v3](how-tos/text-analytics-how-to-sentiment-analysis.md)
* Point de terminaison distinct pour la liaison d’entités 
* Nouvelle catégorie d’entité « Address » dans [Reconnaissance d’entité nommée (NER) v3](how-tos/text-analytics-how-to-entity-linking.md).
* Nouvelles sous-catégories dans NER v3 :
   * Location - Geographical
   * Location - Structural
   * Organization - Stock Exchange
   * Organization - Medical
   * Organization - Sports
   * Event - Cultural
   * Event - Natural
   * Event - Sports

Les propriétés suivantes de la réponse JSON ont été ajoutées :
   * `SentenceText` dans Analyse des sentiments
   * `Warnings` pour chaque document 

Les noms des propriétés suivantes dans la réponse JSON ont été changés, le cas échéant :

* `score` a été renommé en `confidenceScore`
    * `confidenceScore` a une précision à deux décimales. 
* `type` a été renommé en `category`
* `subtype` a été renommé en `subcategory`

> [!div class="nextstepaction"]
> [En savoir plus sur l’API Analyse de texte v3](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Languages)

### <a name="text-analytics-api-v31-public-preview"></a>Préversion publique de l’API Analyse de texte v3.1
   * Nouvelle fonctionnalité Analyse des sentiments - [Exploration des opinions](how-tos/text-analytics-how-to-sentiment-analysis.md#opinion-mining)
   * Nouveau filtre de domaine (`PII`) Personnel pour les informations médicales protégées (`PHI`).

> [!div class="nextstepaction"]
> [En savoir plus sur la préversion de l’API Analyse de texte v3.1](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-1/operations/Languages)

## <a name="february-2020"></a>Février 2020

### <a name="sdk-support-for-text-analytics-api-v3-public-preview"></a>Prise en charge du Kit de développement logiciel (SDK) pour l’API Analyse de texte v3 (Préversion publique)

Dans le cadre de la [version unifiée du Kit de développement logiciel (SDK) Azure](https://techcommunity.microsoft.com/t5/azure-sdk/january-2020-unified-azure-sdk-release/ba-p/1097290), le Kit de développement logiciel (SDK) de l’API Analyse de texte v3 est désormais disponible en préversion publique pour les langages de programmation suivants :
   * [C#](./quickstarts/client-libraries-rest-api.md?pivots=programming-language-csharp&tabs=version-3)
   * [Python](./quickstarts/client-libraries-rest-api.md?pivots=programming-language-python&tabs=version-3)
   * [JavaScript (Node.js)](./quickstarts/client-libraries-rest-api.md?pivots=programming-language-javascript&tabs=version-3)
   * [Java](./quickstarts/client-libraries-rest-api.md?pivots=programming-language-java&tabs=version-3)
   
> [!div class="nextstepaction"]
> [En savoir plus sur le Kit de développement logiciel (SDK) de l’API Analyse de texte v3](./quickstarts/client-libraries-rest-api.md?tabs=version-3)

### <a name="named-entity-recognition-v3-public-preview"></a>Préversion publique de la reconnaissance d’entité nommée v3

D’autres types d’entités sont désormais disponibles dans le service Reconnaissance d’entité nommée (NER) v3 en préversion publique, car nous développons la détection des entités d’informations générales et personnelles trouvées dans le texte. Cette mise à jour introduit le [modèle version](concepts/model-versioning.md) `2020-02-01`, qui inclut :

* Reconnaissance des types suivants d’entités générales (en anglais uniquement) :
    * PersonType
    * Produit
    * Événement
    * Geopolitical Entity (GPE) comme sous-type de Location
    * Compétence

* Reconnaissance des types suivants d’entités d’informations personnelles (en anglais uniquement) :
    * Personne
    * Organisation
    * Age comme sous-type de Quantity
    * Date comme sous-type de DateTime
    * E-mail 
    * Phone Number (États-Unis uniquement)
    * URL
    * Adresse IP

### <a name="october-2019"></a>2 octobre 2019

#### <a name="named-entity-recognition-ner"></a>Reconnaissance d’entité nommée (NER)

* Un [nouveau point de terminaison](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesRecognitionPii) pour reconnaître les types d’entités d’informations personnelles (en anglais uniquement)

* Points de terminaison distincts pour la [reconnaissance d’entité](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesRecognitionGeneral) et la [liaison d’entité](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesLinking).

* [Modèle version](concepts/model-versioning.md) `2019-10-01`, qui inclut :
    * Détection et catégorisation élargies des entités trouvées dans le texte. 
    * Reconnaissance des nouveaux types d’entités suivants :
        * Numéro de téléphone
        * Adresse IP

La liaison d’entités prend en charge l’anglais et l’espagnol. Le support du langage NER varie selon le type d’entité.

#### <a name="sentiment-analysis-v3-public-preview"></a>Analyse des sentiments v3 - Préversion publique

* Un [nouveau point de terminaison](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/Sentiment) pour l’analyse des sentiments.
* [Modèle version](concepts/model-versioning.md) `2019-10-01`, qui inclut :

    * Améliorations significatives en matière d’exactitude et de détail du score et de la catégorisation du texte de l’API.
    * Étiquetage automatique pour différents sentiments dans le texte.
    * Analyse des sentiments et sortie au niveau du document et de la phrase. 

Elle prend en charge l’anglais (`en`), le japonnais (`ja`), le chinois simplifié (`zh-Hans`),  le chinois traditionnel (`zh-Hant`), le français (`fr`), l’italien (`it`), l’espagnol (`es`), le néerlandais (`nl`), la portugais (`pt`) et l’allemand (`de`) et est disponible dans les régions suivantes : `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` et `West US 2`. 

> [!div class="nextstepaction"]
> [En savoir plus sur Analyse des sentiments v3](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features)

## <a name="next-steps"></a>Étapes suivantes

* [Qu’est-ce que l’API Analyse de texte ?](overview.md)  
* [Exemples de scénarios utilisateur](text-analytics-user-scenarios.md)
* [Analyse des sentiments](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Détection de la langue](how-tos/text-analytics-how-to-language-detection.md)
* [Reconnaissance d’entités](how-tos/text-analytics-how-to-entity-linking.md)
* [Extraction de phrases clés](how-tos/text-analytics-how-to-keyword-extraction.md)
