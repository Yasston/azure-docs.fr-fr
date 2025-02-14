---
title: Nouveautés d’Azure Sentinel
description: Cet article décrit les nouvelles fonctionnalités d’Azure Sentinel introduites au cours des derniers mois.
services: sentinel
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 05/13/2021
ms.openlocfilehash: 210be7199f5f28773c2dbbf913e4914918e7eeeb
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2021
ms.locfileid: "110087919"
---
# <a name="whats-new-in-azure-sentinel"></a>Nouveautés d’Azure Sentinel

Cet article liste les fonctionnalités récentes ajoutées à Azure Sentinel et les nouvelles fonctionnalités des services associés qui offrent une expérience utilisateur améliorée dans Azure Sentinel.

Si vous recherchez des éléments datant de plus de six mois, vous les trouverez dans l’[Archive des nouveautés d’Azure Sentinel](whats-new-archive.md). Pour plus d’informations sur les fonctionnalités précédentes fournies, consultez nos [blogs Tech Community](https://techcommunity.microsoft.com/t5/azure-sentinel/bg-p/AzureSentinelBlog/label-name/What's%20New).

> [!IMPORTANT]
> Les fonctionnalités indiquées sont disponibles en préversion. Les [Conditions d’utilisation supplémentaires des préversions Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) incluent des conditions légales supplémentaires qui s’appliquent aux fonctionnalités Azure en version bêta, en préversion ou pas encore disponibles dans la version en disponibilité générale.
> 

> [!TIP]
> Nos équipes de chasse des menaces Microsoft mettent à la disposition de la [communauté Azure Sentinel](https://github.com/Azure/Azure-Sentinel) des requêtes, playbooks, workbooks et notebooks, notamment des [requêtes de chasse](https://github.com/Azure/Azure-Sentinel) spécifiques que vos équipes peuvent adapter et utiliser.
>
> Vous pouvez également contribuer ! Rejoignez-nous dans la [communauté GitHub des chasseurs de menaces Azure Sentinel](https://github.com/Azure/Azure-Sentinel/wiki).
>

## <a name="may-2021"></a>Mai 2021

- [Solutions Azure Sentinel (préversion publique)](#azure-sentinel-solutions-public-preview)
- [Solution Continuous Threat Monitoring pour SAP (préversion publique)](#continuous-threat-monitoring-for-sap-solution-public-preview)
- [Intégrations Threat Intelligence (préversion publique)](#threat-intelligence-integrations-public-preview)
- [Fusion sur les alertes planifiées (préversion publique)](#fusion-over-scheduled-alerts-public-preview)
- [Anomalies SOC-ML (préversion publique)](#soc-ml-anomalies-public-preview)
- [Page Entité IP (préversion publique)](#ip-entity-page-public-preview)
- [Personnalisation d’activité (préversion publique)](#activity-customization-public-preview)
- [Tableau de bord de repérage (préversion publique)](#hunting-dashboard-public-preview)
- [Équipe d’incident - Collaborer dans Microsoft Teams (préversion publique)](#azure-sentinel-incident-team---collaborate-in-microsoft-teams-public-preview)
- [Classeur Confiance Zéro (TIC 3.0)](#zero-trust-tic30-workbook)

### <a name="azure-sentinel-solutions-public-preview"></a>Solutions Azure Sentinel (préversion publique)

Azure Sentinel offre désormais des [solutions](sentinel-solutions-catalog.md) à **contenu packagé** qui incluent des combinaisons d’un ou plusieurs connecteurs de données, classeurs, règles d’analyse, playbooks, requêtes de chasse, analyseurs, watchlists et d’autres composants pour Azure Sentinel.

Les solutions améliorent la détectabilité dans les produits, le déploiement en une seule étape et des scénarios de produit de bout en bout. Pour plus d’informations, consultez [Découvrir et déployer des solutions Azure Sentinel](sentinel-solutions-deploy.md).

### <a name="continuous-threat-monitoring-for-sap-solution-public-preview"></a>Solution Continuous Threat Monitoring pour SAP (préversion publique)

Les solutions Azure Sentinel incluent désormais **Continuous Threat Monitoring pour SAP**, ce qui vous permet de superviser les systèmes SAP à la recherche des menaces sophistiquées au sein des couches métier et d’application.

Le connecteur de données SAP diffuse 14 journaux d’application de l’ensemble du paysage du système SAP, et collecte des journaux tant à partir du langage de programmation ABAP (Advanced Business Application Programming) via des appels RFC NetWeaver, qu’à partir des données de stockage de fichiers via l’interface de contrôle OSSAP. Le connecteur de données SAP ajoute à Azure Sentinel la capacité de surveiller l’infrastructure SAP sous-jacente.

Pour ingérer des journaux SAP dans Azure Sentinel, le connecteur de données SAP Sentinel Azure doit être installé sur votre environnement SAP. Une fois le connecteur de données SAP déployé, déployez le contenu riche de sécurité de la solution SAP pour avoir un aperçu de l’environnement SAP de votre organisation et améliorer les fonctionnalités d’opération de sécurité associées.

Pour plus d’informations, consultez [Tutoriel : Déployer la solution Azure Sentinel pour SAP (préversion publique)](sap-deploy-solution.md).

### <a name="threat-intelligence-integrations-public-preview"></a>Intégrations Threat Intelligence (préversion publique)

Azure Sentinel vous offre différentes façons d’utiliser des flux de [renseignement sur les menaces](import-threat-intelligence.md) pour améliorer la capacité des analystes de la sécurité à détecter et hiérarchiser les menaces connues.

Vous pouvez maintenant utiliser l’un des nombreux nouveaux produits de la plateforme décisionnelle intégrée (TIP) disponibles, vous connecter aux serveurs TAXII pour tirer parti de toute source de renseignement sur les menaces compatible avec STIX, et également utiliser les solutions personnalisées qui peuvent communiquer directement avec [l’API Microsoft Graph Security tiIndicators](/graph/api/resources/tiindicator).

Vous pouvez également vous connecter à des sources de renseignement sur les menaces à partir de playbooks, afin d’enrichir les incidents avec des informations IT qui peuvent aider à effectuer des investigations et à exécuter des actions de réponse.

Pour plus d’informations, consultez [Intégration Threat Intelligence dans Azure Sentinel](threat-intelligence-integration.md).

### <a name="fusion-over-scheduled-alerts-public-preview"></a>Fusion sur les alertes planifiées (préversion publique)

Le moteur de corrélation de Machine Learning **Fusion** peut désormais détecter les attaques multiphases à l’aide d’alertes générées par un ensemble de [règles analytiques planifiées](tutorial-detect-threats-custom.md) dans ses corrélations, en plus des alertes importées à partir d’autres sources de données.

Pour plus d’informations, consultez [Détection avancée des attaques multiphases dans Azure Sentinel](fusion.md).

### <a name="soc-ml-anomalies-public-preview"></a>Anomalies SOC-ML (préversion publique)

Les anomalies basées sur le Machine Learning SOC-ML d’Azure Sentinel peuvent identifier un comportement inhabituel susceptible d’échapper à la détection.

SOC-ML utilise des modèles de règle analytique qui peuvent être mis en place et fonctionner immédiatement. Même si les anomalies n’indiquent pas nécessairement un comportement malveillant ou suspect, elles peuvent être utilisées pour améliorer la fidélité des détections, des investigations et de la recherche de menaces.

Pour plus d’informations, consultez [Utiliser des anomalies SOC-ML pour détecter les menaces dans Azure Sentinel](soc-ml-anomalies.md).

### <a name="ip-entity-page-public-preview"></a>Page Entité IP (préversion publique)

Azure Sentinel prend désormais en charge l’entité d’adresse IP, et vous pouvez désormais afficher les informations d’entité IP dans la nouvelle page Entité IP.

À l’instar des pages d’entité utilisateur et hôte, la page IP contient des informations générales sur l’adresse IP, une liste des activités auxquelles l’adresse IP a participé, et bien plus encore, vous offrant une banque d’informations encore plus riche pour améliorer votre investigation des incidents de sécurité.

Pour plus d’informations, consultez [Pages d’entité](identify-threats-with-entity-behavior-analytics.md#entity-pages).

### <a name="activity-customization-public-preview"></a>Personnalisation d’activité (préversion publique)

Concernant les pages d’entité, vous pouvez désormais créer des activités personnalisées pour vos entités, qui seront suivies et affichées sur leurs pages d’entité respectives, en plus des activités prêtes à l’emploi dont vous disposez déjà.

Pour plus d’informations, consultez [Personnaliser les activités sur les chronologies des pages d’entité](customize-entity-activities.md).

### <a name="hunting-dashboard-public-preview"></a>Tableau de bord de repérage (préversion publique)

Le panneau **Hunting** (Repérage) a été actualisé. Le nouveau tableau de bord vous permet d’exécuter toutes vos requêtes, ou un sous-ensemble sélectionné, en un seul clic.

Identifiez où commencer le repérage en examinant le nombre de résultats, les pics ou la modification du nombre de résultats sur une période de 24 heures. Vous pouvez également trier et filtrer par favoris, source de données, tactique et technique MITRE ATT&CK, résultats ou delta de résultat. Affichez les requêtes qui n’ont pas encore les sources de données nécessaires connectées et recevez des recommandations sur la façon d’activer ces requêtes.

Pour plus d’informations, consultez [Repérer les menaces avec Azure Sentinel](hunting.md).

### <a name="azure-sentinel-incident-team---collaborate-in-microsoft-teams-public-preview"></a>Équipe d’incident - Azure Sentinel - Collaborer avec Microsoft Teams (préversion publique)

Azure Sentinel prend désormais en charge une intégration directe avec Microsoft Teams, ce qui vous permet de collaborer en toute transparence au sein de l’organisation et avec des parties prenantes externes.

Directement à partir de l’incident dans Azure Sentinel, créez une nouvelle *équipe d’incident* à utiliser pour la communication et la coordination centralisées.

Les équipes d’incident sont particulièrement utiles lorsqu’elles sont utilisées en tant que pont de conférence dédié pour les incidents à gravité élevée en cours. Les organisations qui utilisent déjà Microsoft Teams pour la communication et la collaboration peuvent utiliser l’intégration Azure Sentinel pour intégrer des données de sécurité directement dans leurs conversations et leurs tâches quotidiennes.

Dans Microsoft Teams, l’onglet **page Incident** de la nouvelle équipe contient toujours les données les plus récentes et à jour d’Azure Sentinel, garantissant ainsi que les données les plus pertinentes sont disponibles pour vos équipes.

[ ![Page Incident dans Microsoft Teams.](media/collaborate-in-microsoft-teams/incident-in-teams.jpg) ](media/collaborate-in-microsoft-teams/incident-in-teams.jpg#lightbox)

Pour plus d’informations, consultez [Collaborer dans Microsoft Teams (préversion publique)](collaborate-in-microsoft-teams.md).

### <a name="zero-trust-tic30-workbook"></a>Classeur Confiance Zéro (TIC 3.0)

Le nouveau classeur Confiance Zéro (TIC3.0) d’Azure Sentinel fournit une visualisation automatisée des principes de [Confiance Zéro](/security/zero-trust/) croisée avec l’infrastructure [Trusted Internet Connections](https://www.cisa.gov/trusted-internet-connections) (TIC).

Nous savons que la conformité n’est pas seulement une exigence annuelle et que les organisations doivent surveiller les configurations au fil du temps. Le classeur Confiance Zéro d’Azure Sentinel utilise toute la gamme des offres de sécurité Microsoft dans Azure, Office 365, Teams, Intune, Windows Virtual Desktop et bien d’autres.

[ ![Classeur Confiance Zéro. ](media/zero-trust-workbook.gif) ](media/zero-trust-workbook.gif#lightbox)

**Le classeur Confiance Zéro** :

- Permet aux implémenteurs, analystes des opérations de sécurité, évaluateurs, décideurs de sécurité et de conformité, MSSP et autres d’avoir connaissance de la posture de sécurité des charges de travail cloud.
- Propose plus de 75 cartes de contrôle, alignées sur les fonctionnalités de sécurité TIC 3.0, avec des boutons d’interface utilisateur sélectionnables pour la navigation.
- Est conçu pour habiliter le personnel grâce à l’automatisation, l’intelligence artificielle, le Machine Learning, la génération de requêtes/alertes, les visualisations, les recommandations personnalisées et les références de documentation respectives.

Pour plus d’informations, consultez [Tutoriel : Visualisation et supervision des données](tutorial-monitor-your-data.md).

## <a name="april-2021"></a>Avril 2021

- [Connecteurs de données basés sur Azure Policy](#azure-policy-based-data-connectors)
- [Chronologie des incidents (préversion publique)](#incident-timeline-public-preview)

### <a name="azure-policy-based-data-connectors"></a>Connecteurs de données basés sur Azure Policy

Azure Policy vous permet d'appliquer un ensemble commun de paramètres de journaux de diagnostic à toutes les ressources (actuelles et futures) d'un type particulier dont vous souhaitez ingérer les journaux dans Azure Sentinel.

Dans le cadre des efforts que nous déployons pour mettre la puissance d'[Azure Policy](../governance/policy/overview.md) au service de la configuration de la collecte de données, nous proposons désormais en préversion publique un autre collecteur de données basé sur Azure Policy pour les [comptes de stockage Azure](connect-azure-storage-account.md).

En outre, deux de nos connecteurs qui étaient jusque-là en préversion, pour [Azure Key Vault](connect-azure-key-vault.md) et [Azure Kubernetes Service](connect-azure-kubernetes-service.md), sont désormais en disponibilité générale (GA), comme le connecteur [Azure SQL Database](connect-azure-sql-logs.md) qui les a précédé.

### <a name="incident-timeline-public-preview"></a>Chronologie des incidents (préversion publique)

Le premier onglet d’une page de détails d’incident est maintenant **Chronologie**, qui affiche une chronologie des alertes et des signets dans l’incident. La chronologie d’un incident peut vous aider à mieux comprendre l’incident et à retracer la chronologie de l’activité de l’attaquant parmi les alertes et les signets associés.

- Sélectionnez un élément dans la chronologie pour afficher ses détails, sans quitter le contexte de l’incident
- Filtrez le contenu de la chronologie pour afficher uniquement les alertes ou les signets, ou les éléments d’une gravité ou d’une tactique MITRE spécifique.
- Vous pouvez sélectionner le lien **ID d’alerte système** pour afficher l’intégralité de l’enregistrement, ou le lien **Événements** pour afficher les événements associés dans la zone **Journaux**.

Par exemple :

:::image type="content" source="media/tutorial-investigate-cases/incident-timeline.png" alt-text="Onglet Chronologie de l’incident":::

Pour plus d’informations, consultez [Didacticiel : Examiner les incidents avec Azure Sentinel](tutorial-investigate-cases.md).

## <a name="march-2021"></a>Mars 2021

- [Définir les classeurs pour qu’ils s’actualisent automatiquement en mode affichage](#set-workbooks-to-automatically-refresh-while-in-view-mode)
- [Nouvelles détections pour le pare-feu Azure](#new-detections-for-azure-firewall)
- [Règles d'automatisation et guides opérationnels déclenchés par un incident (préversion publique)](#automation-rules-and-incident-triggered-playbooks-public-preview) (y compris la toute nouvelle documentation des guides opérationnels)
- [Nouveaux enrichissements d'alerte : mappage d'entités amélioré et détails personnalisés (préversion publique)](#new-alert-enrichments-enhanced-entity-mapping-and-custom-details-public-preview)
- [Imprimer vos classeurs Azure Sentinel ou enregistrer au format PDF](#print-your-azure-sentinel-workbooks-or-save-as-pdf)
- [Filtres d’incident et préférences de tri désormais enregistrés dans votre session (préversion publique)](#incident-filters-and-sort-preferences-now-saved-in-your-session-public-preview)
- [Intégration des incidents Microsoft 365 Defender (préversion publique)](#microsoft-365-defender-incident-integration-public-preview)
- [Nouveaux connecteurs de service Microsoft avec Azure Policy](#new-microsoft-service-connectors-using-azure-policy)

### <a name="set-workbooks-to-automatically-refresh-while-in-view-mode"></a>Définir les classeurs pour qu’ils s’actualisent automatiquement en mode affichage

Les utilisateurs d’Azure Sentinel peuvent désormais utiliser la nouvelle [fonctionnalité d’Azure Monitor](https://techcommunity.microsoft.com/t5/azure-monitor/azure-workbooks-set-it-to-auto-refresh/ba-p/2228555) pour actualiser automatiquement les données de classeur pendant une session d’affichage.

Dans chaque classeur ou modèle de classeur, sélectionnez :::image type="icon" source="media/whats-new/auto-refresh-workbook.png" border="false"::: **Actualisation automatique** pour afficher vos options d’intervalle. Sélectionnez l’option que vous souhaitez utiliser pour la session d’affichage en cours, puis **Appliquer**.

- Les intervalles d’actualisation pris en charge sont compris entre **5 minutes** et **1 jour**.
- Par défaut, l’actualisation automatique est désactivée. Pour optimiser les performances, l’actualisation automatique est également désactivée chaque fois que vous fermez un classeur, et elle ne s’exécute pas en arrière-plan. Activez l’actualisation automatique en fonction des besoins la prochaine fois que vous ouvrirez le classeur.
- L’actualisation automatique est suspendue lorsque vous modifiez un classeur, et les intervalles d’actualisation automatique sont redémarrés chaque fois que vous revenez en mode d’affichage à partir du mode d’édition.

    Les intervalles sont également redémarrés si vous actualisez manuellement le classeur en sélectionnant le bouton :::image type="icon" source="media/whats-new/manual-refresh-button.png" border="false"::: **Actualiser**.

Pour plus d’informations, consultez [Tutoriel : Visualiser et surveiller vos données](tutorial-monitor-your-data.md) et la [documentation d’Azure Monitor](../azure-monitor/visualize/workbooks-overview.md).

### <a name="new-detections-for-azure-firewall"></a>Nouvelles détections pour le pare-feu Azure

Plusieurs détections prêtes à l’emploi pour le pare-feu Azure ont été ajoutées à la zone [Analytique](import-threat-intelligence.md#analytics-puts-your-threat-indicators-to-work-detecting-potential-threats) dans Azure Sentinel. Ces nouvelles détections permettent aux équipes de sécurité d’obtenir des alertes si des machines sur le réseau interne tentent d’interroger ou de se connecter à des noms de domaine Internet ou à des adresses IP associées à des IOC connus, comme défini dans la requête de règle de détection.

Les nouvelles détections sont les suivantes :

- [Balise réseau Solorigate](https://github.com/Azure/Azure-Sentinel/blob/master/Detections/MultipleDataSources/Solorigate-Network-Beacon.yaml)
- [Domaines et hachages GALLIUM connus](https://github.com/Azure/Azure-Sentinel/blob/master/Detections/MultipleDataSources/GalliumIOCs.yaml)
- [Known IRIDIUM IP (Adresse IP IRIDIUM connue)](https://github.com/Azure/Azure-Sentinel/blob/master/Detections/MultipleDataSources/IridiumIOCs.yaml)
- [Domaines/adresses IP de groupe Phosphorus connus](https://github.com/Azure/Azure-Sentinel/blob/master/Detections/MultipleDataSources/PHOSPHORUSMarch2019IOCs.yaml)
- [Domaines THALLIUM inclus dans DCU](https://github.com/Azure/Azure-Sentinel/blob/master/Detections/MultipleDataSources/ThalliumIOCs.yaml)
- [Hachages maldoc liés à ZINC connus](https://github.com/Azure/Azure-Sentinel/blob/master/Detections/MultipleDataSources/ZincJan272021IOCs.yaml)
- [Domaines de groupe STRONTIUM connus](https://github.com/Azure/Azure-Sentinel/blob/master/Detections/MultipleDataSources/STRONTIUMJuly2019IOCs.yaml)
- [NOBELIUM - Domaine et IOC IP - Mars 2021](https://github.com/Azure/Azure-Sentinel/blob/master/Detections/MultipleDataSources/NOBELIUM_DomainIOCsMarch2021.yaml)


Les détections pour les pare-feu Azure sont ajoutées en permanence à la galerie de modèles intégrée. Pour obtenir les détections les plus récentes pour le pare-feu Azure, sous **Modèles de règle**, filtrez les **Sources de données** par **Pare-feu Azure** :

:::image type="content" source="media/whats-new/new-detections-analytics-efficiency-workbook.jpg" alt-text="Nouvelles détections dans le classeur d’efficacité analytique":::

Pour plus d’informations, consultez [Nouvelles détections pour le pare-feu Azure dans Azure Sentinel](https://techcommunity.microsoft.com/t5/azure-network-security/new-detections-for-azure-firewall-in-azure-sentinel/ba-p/2244958).

### <a name="automation-rules-and-incident-triggered-playbooks-public-preview"></a>Règles d'automatisation et guides opérationnels déclenchés par un incident (préversion publique)

Les règles d’automatisation sont un nouveau concept dans Azure Sentinel, qui vous permet de gérer de manière centralisée l’automatisation de la gestion des incidents. En plus de vous permettre d’affecter des guides opérationnels aux incidents (pas seulement aux alertes comme auparavant), les règles d’automatisation vous permettent également d’automatiser les réponses pour plusieurs règles d’analyse à la fois, d’étiqueter, d’attribuer ou de fermer automatiquement des incidents sans nécessiter de guides opérationnels et de contrôler l’ordre des actions exécutées. Les règles d’automatisation simplifient l’utilisation de l’automatisation dans Azure Sentinel et vous permettent de simplifier des flux de travail complexes pour vos processus d’orchestration d’incident.

Pour en savoir plus, consultez cette [explication complète sur les règles d’automatisation](automate-incident-handling-with-automation-rules.md).

Comme indiqué ci-dessus, les règles peuvent désormais être activées avec le déclencheur d’incident en plus du déclencheur d’alerte. Le déclencheur d’incident fournit à vos guides opérationnels un plus grand ensemble d’entrées à utiliser (puisque l’incident inclut également toutes les données d’alerte et d’entité), ce qui vous donne encore plus de puissance et de flexibilité dans vos flux de travail de réponse. Les règles déclenchées par incident sont activées par l’appel à partir de règles d’automatisation.

En savoir plus sur les [fonctionnalités améliorées des guides opérationnels](automate-responses-with-playbooks.md) et sur la [création d’un flux de travail de réponse](tutorial-respond-threats-playbook.md) à l’aide de guides opérationnels avec des règles d’automatisation.

### <a name="new-alert-enrichments-enhanced-entity-mapping-and-custom-details-public-preview"></a>Nouveaux enrichissements d'alerte : mappage d'entités amélioré et détails personnalisés (préversion publique)

Enrichissez vos alertes de deux façons pour les rendre plus utilisables et plus instructifs.

Commencez par mettre le mappage de votre entité au niveau suivant. Vous pouvez désormais mapper presque 20 types d’entités, des utilisateurs, des hôtes et des adresses IP, aux fichiers et aux processus, aux boîtes aux lettres, aux ressources Azure et aux appareils IoT. Vous pouvez également utiliser plusieurs identificateurs pour chaque entité, afin de renforcer leur identification unique. Vous bénéficiez ainsi d’un jeu de données beaucoup plus riche dans vos incidents, en fournissant une corrélation plus large et une investigation plus puissante. [Découvrez la nouvelle façon de mapper des entités](map-data-fields-to-entities.md) dans vos alertes.

[Découvrez-en plus sur les entités](entities-in-azure-sentinel.md) et consultez la [liste complète des entités disponibles et de leurs identificateurs](entities-reference.md).

Stimulez davantage vos capacités d’investigation et de réponse en personnalisant vos alertes en détails de surface à partir de vos événements bruts. Améliorez la visibilité du contenu des événements dans vos incidents, en vous donnant une puissance et une flexibilité accrues pour répondre aux menaces de sécurité et les examiner. [Découvrez comment faire apparaître des détails personnalisés](surface-custom-details-in-alerts.md) dans vos alertes.



### <a name="print-your-azure-sentinel-workbooks-or-save-as-pdf"></a>Imprimer vos classeurs Azure Sentinel ou enregistrer-les au format PDF

Vous pouvez désormais imprimer des classeurs Azure Sentinel, ce qui vous permet également de les exporter vers des fichiers PDF et de les enregistrer localement ou de les partager.

Dans votre classeur, sélectionnez le menu options > :::image type="icon" source="media/whats-new/print-icon.png" border="false"::: **Imprimer le contenu**. Sélectionnez ensuite votre imprimante ou sélectionnez **Enregistrer au format PDF** si nécessaire.

:::image type="content" source="media/whats-new/print-workbook.png" alt-text="Imprimez votre classeur ou enregistrez-le au format PDF.":::

Pour plus d’informations, consultez [Tutoriel : Visualisation et supervision des données](tutorial-monitor-your-data.md).

### <a name="incident-filters-and-sort-preferences-now-saved-in-your-session-public-preview"></a>Filtres d’incident et préférences de tri désormais enregistrés dans votre session (préversion publique)

À présent, vos filtres et tris d’incident sont enregistrés dans toute votre session Azure Sentinel, même lors de la navigation vers d’autres zones du produit.
Tant que vous êtes toujours dans la même session, le fait de revenir à la zone [Incidents](tutorial-investigate-cases.md) dans Azure Sentinel affiche vos filtres et le tri comme vous l’avez laissé.

> [!NOTE]
> Les filtres d’incidents et le tri ne sont pas enregistrés après avoir quitté Azure Sentinel ou après avoir actualisé votre navigateur.

### <a name="microsoft-365-defender-incident-integration-public-preview"></a>Intégration des incidents Microsoft 365 Defender (préversion publique)

L’intégration d’incidents [Microsoft 365 Defender (M365D)](/microsoft-365/security/mtp/microsoft-threat-protection) d’Azure Sentinel vous permet de diffuser tous les incidents M365D dans Azure Sentinel et de les garder synchronisés entre les deux portails. Les incidents de M365D (anciennement Protection Microsoft contre les menaces) incluent l’ensemble des alertes, entités et informations pertinentes associées, ce qui vous offre suffisamment de contexte pour effectuer un triage et une investigation préliminaire dans Azure Sentinel. Une fois dans Sentinel, les incidents restent synchronisés de manière bidirectionnelle avec M365D, ce qui vous permet de tirer parti des avantages des deux portails dans votre investigation sur l’incident.

L’utilisation conjointe d’Azure Sentinel et de Microsoft 365 Defender vous offre le meilleur des deux mondes. Vous bénéficiez de tout l’éventail des insights offert par SIEM pour l’ensemble des ressources d’informations de votre organisation, ainsi que de toute la puissance d’investigation personnalisée qu’une solution XDR procure pour protéger vos ressources Microsoft 365, tout ceci coordonné et synchronisé pour des opérations SOC fluides.

Pour plus d’informations, consultez [Intégration de Microsoft 365 Defender avec Azure Sentinel](microsoft-365-defender-sentinel-integration.md).

### <a name="new-microsoft-service-connectors-using-azure-policy"></a>Nouveaux connecteurs de service Microsoft avec Azure Policy

[Azure Policy](../governance/policy/overview.md) est un service Azure qui vous permet d’utiliser des stratégies pour appliquer et contrôler les propriétés d’une ressource. L’utilisation de stratégies garantit que les ressources restent conformes à vos normes de gouvernance informatique.

Parmi les propriétés des ressources qui peuvent être contrôlées par les stratégies, citons la création et la gestion des journaux de diagnostic et d’audit. Azure Sentinel utilise désormais Azure Policy pour vous permettre d’appliquer un ensemble commun de paramètres de journaux de diagnostic à toutes les ressources (actuelles et futures) d’un type particulier dont vous souhaitez ingérer les journaux dans Azure Sentinel. Grâce à Azure Policy, vous n’avez plus besoin de définir les paramètres des journaux de diagnostics ressource par ressource.

Des connecteurs basés sur Azure Policy sont maintenant disponibles pour les services Azure suivants :
- [Azure Key Vault](connect-azure-key-vault.md) (préversion publique)
- [Azure Kubernetes Service](connect-azure-kubernetes-service.md) (préversion publique)
- [Serveurs/bases de données Azure SQL](connect-azure-sql-logs.md) (GA)

Les clients pourront toujours envoyer les journaux manuellement pour des instances spécifiques, et ne sont pas obligés d’utiliser le moteur de stratégie.

## <a name="february-2021"></a>Février 2021

- [Workbook CMMC (Cybersecurity Maturity Model Certification)](#cybersecurity-maturity-model-certification-cmmc-workbook)
- [Connecteurs de données tiers](#third-party-data-connectors)
- [Aperçus UEBA dans la page d’entités (préversion publique)](#ueba-insights-in-the-entity-page-public-preview)
- [Amélioration de la recherche d’incidents (préversion publique)](#improved-incident-search-public-preview)

### <a name="cybersecurity-maturity-model-certification-cmmc-workbook"></a>Workbook CMMC (Cybersecurity Maturity Model Certification)

Le workbook CMMC Azure Sentinel fournit un mécanisme permettant d’afficher les requêtes de journal alignées sur les contrôles CMMC dans le portefeuille Microsoft, notamment les offres de sécurité Microsoft, Office 365, Teams, Intune, Windows Virtual Desktop et bien plus encore.

Le workbook CMMC permet aux architectes de sécurité, aux ingénieurs, aux analystes d’opérations de sécurité, aux responsables et aux professionnels de l’informatique de bénéficier d’une visibilité de la conscience situationnelle de la posture de sécurité des charges de travail cloud. Il existe également des recommandations pour la sélection, la conception, le déploiement et la configuration des offres Microsoft afin qu’elles soient en phase avec les pratiques et exigences CMMC respectives.

Même si vous n’êtes pas obligé de vous conformer à la certification CMMC, le workbook CMMC est utile à la création de centres d’opérations de sécurité, au développement d’alertes, à la visualisation des menaces et à la prise en charge de la conscience situationnelle des charges de travail.

Accédez au workbook CMMC dans la zone **Workbooks** d’Azure Sentinel. Sélectionnez **Modèle**, puis recherchez **CMMC**.

:::image type="content" source="media/whats-new/cmmc-guide-toggle.gif" alt-text="Activez et désactivez le guide du workbook CMMC" lightbox="media/whats-new/cmmc-guide-toggle.gif":::


Pour plus d’informations, consultez :

- [Azure Sentinel Cybersecurity Maturity Model Certification (CMMC) Workbook](https://techcommunity.microsoft.com/t5/public-sector-blog/azure-sentinel-cybersecurity-maturity-model-certification-cmmc/ba-p/2110524)
- [Tutoriel : Visualiser et superviser vos données](tutorial-monitor-your-data.md)


### <a name="third-party-data-connectors"></a>Connecteurs de données tiers

Notre collection d’intégrations tierces continue de s’agrandir, avec 30 connecteurs ajoutés ces deux derniers mois. En voici la liste :

- [Agari Phishing Defense and Brand Protection](connect-agari-phishing-defense.md)
- [Akamai Security Events](connect-akamai-security-events.md)
- [Alsid for Active Directory](connect-alsid-active-directory.md)
- [Apache HTTP Server](connect-apache-http-server.md)
- [Aruba ClearPass](connect-aruba-clearpass.md)
- [Blackberry CylancePROTECT](connect-data-sources.md)
- [Broadcom Symantec DLP](connect-broadcom-symantec-dlp.md)
- [Cisco Firepower eStreamer](connect-data-sources.md)
- [Cisco Meraki](connect-cisco-meraki.md)
- [Cisco Umbrella](connect-cisco-umbrella.md)
- [Cisco Unified Computing System (UCS)](connect-cisco-ucs.md)
- [ESET Enterprise Inspector](connect-data-sources.md)
- [ESET Security Management Center](connect-data-sources.md)
- [Google Workspace (anciennement G Suite)](connect-google-workspace.md)
- [Imperva WAF Gateway](connect-imperva-waf-gateway.md)
- [Juniper SRX](connect-juniper-srx.md)
- [Netskope](connect-data-sources.md)
- [NXLog DNS Logs](connect-nxlog-dns.md)
- [NXLog Linux Audit](connect-nxlog-linuxaudit.md)
- [Onapsis Platform](connect-data-sources.md)
- [Proofpoint On Demand Email Security (POD)](connect-proofpoint-pod.md)
- [Qualys Vulnerability Management Knowledge Base](connect-data-sources.md)
- [Salesforce Service Cloud](connect-salesforce-service-cloud.md)
- [SonicWall Firewall](connect-data-sources.md)
- [Sophos Cloud Optix](connect-sophos-cloud-optix.md)
- [Squid Proxy](connect-squid-proxy.md)
- [Symantec Endpoint Protection](connect-data-sources.md)
- [Thycotic Secret Server](connect-thycotic-secret-server.md)
- [Trend Micro XDR](connect-data-sources.md)
- [VMware ESXi](connect-vmware-esxi.md)

### <a name="ueba-insights-in-the-entity-page-public-preview"></a>Aperçus UEBA dans la page d’entités (préversion publique)

Les pages de détails des entités Sentinel Azure contiennent un [volet Insights](identify-threats-with-entity-behavior-analytics.md#entity-insights) qui affiche des informations sur le comportement de l’entité et aident à identifier rapidement les anomalies et les menaces de sécurité.

Si [UEBA est activé](ueba-enrichments.md) et que vous avez sélectionné un délai d’au moins quatre jours, ce volet Insights inclut aussi désormais les nouvelles sections suivantes pour les insights UEBA :

|Section  |Description  |
|---------|---------|
|**Insights UEBA**     | Récapitule les activités anormales des utilisateurs : <br>- Parmi les zones géographiques, appareils et environnements<br>- Parmi les horizons de temps et de fréquence (par rapport à l’historique de l’utilisateur) <br>- Par rapport au comportement des pairs <br>- Par rapport au comportement de l’organisation     |
|**Pairs de l’utilisateur en fonction de l’appartenance au groupe de sécurité**     |   Liste les pairs de l’utilisateur en fonction de l’appartenance à des groupes de sécurité Azure AD, ce qui permet aux équipes des opérations de sécurité de disposer d’une liste d’autres utilisateurs qui partagent des autorisations similaires.  |
|**Autorisations d’accès utilisateur à l’abonnement Azure**     |     Affiche les autorisations d’accès de l’utilisateur aux abonnements Azure accessibles directement, ou par le biais de principaux de service/groupes Azure AD.   |
|**Indicateurs de menace associés à l’utilisateur**     |  Liste une collection de menaces connues associées aux adresses IP représentées dans les activités de l’utilisateur. Les menaces sont listées par type et famille de menaces, et sont enrichies par le service de renseignement sur les menaces de Microsoft.       |
|     |         |

### <a name="improved-incident-search-public-preview"></a>Amélioration de la recherche d’incidents (préversion publique)

Nous avons amélioré l’expérience de recherche d’incidents Azure Sentinel afin de vous permettre de naviguer plus rapidement parmi les incidents lors de l’investigation d’une menace spécifique.

Lors de la recherche d’incidents dans Azure Sentinel, vous pouvez désormais effectuer une recherche d’après les détails d’incident suivants :

- ID
- Titre
- Produit
- Propriétaire
- Tag

## <a name="january-2021"></a>Janvier 2021

- [Assistant de la règle d’analyse : Expérience d’édition des requêtes améliorée (préversion publique)](#analytics-rule-wizard-improved-query-editing-experience-public-preview)
- [Module PowerShell Az.SecurityInsights (préversion publique)](#azsecurityinsights-powershell-module-public-preview)
- [Connecteur de base de données SQL](#sql-database-connector)
- [Connecteur dynamique 365 (préversion publique)](#dynamics-365-connector-public-preview)
- [Commentaires d’incident améliorés](#improved-incident-comments)
- [Clusters Log Analytics dédiés](#dedicated-log-analytics-clusters)
- [Identités managées par applications logiques](#logic-apps-managed-identities)
- [Amélioration de l’optimisation des règles avec les graphiques d’aperçu des règles d’analytique](#improved-rule-tuning-with-the-analytics-rule-preview-graphs-public-preview)


### <a name="analytics-rule-wizard-improved-query-editing-experience-public-preview"></a>Assistant de règle Analytics : expérience d’édition des requêtes améliorée (préversion publique)

L’Assistant de règle d’analyse planifiée Azure Sentinel fournit désormais les améliorations suivantes pour l’écriture et la modification de requêtes :

-   Fenêtre d’édition extensible vous offrant plus d’espace à l’écran pour afficher votre requête.
-   Mise en surbrillance de mot clé dans le code de votre requête.
-   Prise en charge de l’autocomplétion étendue.
-   Validations de requêtes en temps réel. Les erreurs dans votre requête s’affichent désormais sous la forme d’un bloc rouge dans la barre de défilement, et d’un point rouge dans le nom d’onglet **Définir la logique de la règle**. En outre, il n’est pas possible d’enregistrer une requête comportant des erreurs.

Pour plus d’informations, consultez [Tutoriel : Créer des règles d’analytique personnalisées pour détecter des menaces](tutorial-detect-threats-custom.md).
### <a name="azsecurityinsights-powershell-module-public-preview"></a>Module PowerShell Az.SecurityInsights (préversion publique)

Azure Sentinel prend désormais en charge le nouveau module PowerShell [Az.SecurityInsights](https://www.powershellgallery.com/packages/Az.SecurityInsights/).

Le module **Az.SecurityInsights** prend en charge les cas d’utilisation Azure Sentinel courants, tels que l’interaction avec les incidents pour changer les états, la gravité, le propriétaire, etc., l’ajout de commentaires et d’étiquettes aux incidents et la création de signets.

Même si nous vous déconseillons d’utiliser des modèles [Azure Resource Manager (ARM)](../azure-resource-manager/templates/index.yml) pour votre pipeline CI/CD, le module **Az.SecurityInsights** est utile pour les tâches postérieures au déploiement et destiné à l’automatisation du SOC.  Par exemple, l’automatisation de votre SOC peut inclure des étapes pour configurer des connecteurs de données, créer des règles d’analyse ou ajouter des actions d’automatisation à des règles d’analytique.

Pour plus d’informations, notamment une liste complète et une description des applets de commande disponibles, des descriptions de paramètres et des exemples, consultez la [documentation PowerShell sur Az.SecurityInsights](/powershell/module/az.securityinsights/).

### <a name="sql-database-connector"></a>Connecteur de base de données SQL

Azure Sentinel fournit désormais un connecteur de base de données Azure SQL, qui vous permet de diffuser les journaux d’audit et de diagnostics de vos bases de données dans Azure Sentinel et de superviser en continu l’activité dans toutes vos instances.

Azure SQL est un moteur de base de données PaaS (Platform-as-a-Service) complètement managé qui prend en charge la plupart des fonctions de gestion de base de données telles que la mise à niveau, la mise à jour corrective, les sauvegardes et la surveillance, sans intervention de l’utilisateur.

Pour plus d’informations, consultez [Connecter les journaux de diagnostics et d’audit Azure SQL Database](connect-azure-sql-logs.md).

### <a name="dynamics-365-connector-public-preview"></a>Connecteur dynamique 365 (préversion publique)

Azure Sentinel fournit désormais un connecteur pour Microsoft Dynamics 365, qui vous permet de collecter les journaux d’activité des utilisateurs, des administrateurs et du support de vos applications Dynamics 365 dans Azure Sentinel. Vous pouvez utiliser ces données pour vous aider à auditer l’intégralité des actions de traitement des données qui ont lieu, et à les analyser afin d’identifier d’éventuelles failles de sécurité.

Pour plus d’informations, consultez [Connecter des journaux d’activité Dynamics 365 à Azure Sentinel](connect-dynamics-365.md).

### <a name="improved-incident-comments"></a>Commentaires d’incident améliorés

Les analystes utilisent les commentaires d’incident pour collaborer sur les incidents, en documentant les processus et les étapes manuellement ou dans le cadre d’un playbook. 

Notre expérience de commentaires d’incident améliorée vous permet de mettre en forme vos commentaires et de modifier ou de supprimer des commentaires existants.

Pour plus d’informations, consultez [Créer automatiquement des incidents à partir d’alertes de sécurité Microsoft](create-incidents-from-alerts.md).
### <a name="dedicated-log-analytics-clusters"></a>Clusters Log Analytics dédiés

Azure Sentinel prend désormais en charge les clusters Log Analytics dédiés en tant qu’option de déploiement. Nous vous recommandons d’envisager un cluster dédié si vous :

- **Ingérez plus de 1 To par jour** dans votre espace de travail Azure Sentinel
- **Avez plusieurs espaces de travail Azure Sentinel** dans votre inscription Azure

Les clusters dédiés vous permettent d’utiliser des fonctionnalités telles que les clés gérées par le client, le référentiel sécurisé, le chiffrement double et des requêtes plus rapides entre les espaces de travail quand un même cluster abrite plusieurs espaces de travail.

Pour plus d’informations, consultez [Clusters dédiés pour les journaux Azure Monitor](../azure-monitor/logs/logs-dedicated-clusters.md).

### <a name="logic-apps-managed-identities"></a>Identités managées par applications logiques

Azure Sentinel prend désormais en charge les identités managées pour le connecteur Azure Sentinel Logic Apps, ce qui vous permet d’accorder des autorisations directement à un playbook spécifique destiné à fonctionner sur Azure Sentinel au lieu de créer des identités supplémentaires.

- **Sans identité managée**, le connecteur Logic Apps nécessite une identité distincte avec un rôle RBAC Azure Sentinel pour s’exécuter sur Azure Sentinel. L’identité distincte peut être un utilisateur Azure AD ou un principal de service, tel qu’une application Azure AD inscrite.

- L’**activation de la prise en charge des identités managées dans votre application logique** inscrit cette dernière auprès d’Azure AD et fournit un ID d’objet. Utilisez l’ID d’objet dans Azure Sentinel pour affecter l’application logique avec un rôle RBAC Azure dans votre espace de travail Azure Sentinel. 

Pour plus d'informations, consultez les pages suivantes :

- [Authentification avec une identité managée dans Azure Logic Apps](../logic-apps/create-managed-service-identity.md)
- [Documentation du connecteur Azure Sentinel Logic Apps](/connectors/azuresentinel) 

### <a name="improved-rule-tuning-with-the-analytics-rule-preview-graphs-public-preview"></a>Amélioration de l’optimisation des règles avec les graphiques d’aperçu des règles d’analytique (préversion publique)

Azure Sentinel vous aide désormais à optimiser vos règles d’analytique, ce qui vous permet d’accroître leur précision et de réduire le bruit.

Après avoir modifié une règle d’analytique sous l’onglet **Définir la logique de la règle**, recherchez la zone **Simulation des résultats** à droite. 

Sélectionnez **Tester avec les données actuelles** pour qu’Azure Sentinel exécute une simulation des 50 dernières exécutions de votre règle d’analytique. Un graphique est créé pour indiquer le nombre moyen d’alertes que la règle aurait générées, en fonction des données d’événement brutes évaluées. 

Pour plus d’informations, consultez [Définir la logique de requête de règle et configurer les paramètres](tutorial-detect-threats-custom.md#define-the-rule-query-logic-and-configure-settings).

## <a name="december-2020"></a>Décembre 2020

- [80 nouvelles requêtes de chasse intégrées](#80-new-built-in-hunting-queries)
- [Améliorations de l’agent Log Analytics](#log-analytics-agent-improvements)

### <a name="80-new-built-in-hunting-queries"></a>80 nouvelles requêtes de chasse intégrées
 
Les requêtes de chasse intégrées d’Azure Sentinel permettent aux analystes du SOC de réduire les lacunes dans la couverture actuelle de la détection et de se lancer sur de nouvelles pistes de chasse.

Cette mise à jour pour Azure Sentinel comprend de nouvelles requêtes de chasse qui fournissent une couverture au sein de la matrice de framework MITRE ATT&CK :

- **Collection**
- **Commande et contrôle**
- **Accès aux informations d’identification**
- **Découverte**
- **Exécution**
- **Exfiltration**
- **Impact**
- **Accès initial**
- **Persistance**
- **Élévation des privilèges**

Les requêtes de chasse ajoutées sont conçues pour vous aider à détecter les activités suspectes dans votre environnement. Même si elles peuvent retourner des activités légitimes et des activités potentiellement malveillantes, elles peuvent être utiles pour guider votre chasse. 

Si, après avoir exécuté ces requêtes, vous êtes convaincu des résultats, vous souhaiterez peut-être les convertir en règles d’analytique ou ajouter les résultats de la chasse à des incidents existants ou nouveaux.

Toutes les requêtes ajoutées sont disponibles par le biais de la page Chasse avec Azure Sentinel. Pour plus d’informations, consultez [Repérer les menaces avec Azure Sentinel](hunting.md).

### <a name="log-analytics-agent-improvements"></a>Améliorations de l’agent Log Analytics

Les utilisateurs Azure Sentinel bénéficient des améliorations de l’agent Log Analytics suivantes :

- **Prise en charge d’un plus grand nombre de systèmes d’exploitation**, notamment CentOS 8, RedHat 8 et SUSE Linux 15
- **Prise en charge de Python 3** en plus de Python 2

Azure Sentinel utilise l’agent Log Analytics pour envoyer des événements à votre espace de travail, tels que les événements de sécurité Windows, les événements Syslog et les journaux CEF.

> [!NOTE]
> L’agent Log Analytics est parfois appelé agent OMS ou MMA (Microsoft Monitoring Agent). 
> 

Pour plus d’informations, consultez la [documentation Log Analytics](../azure-monitor/agents/log-analytics-agent.md) et les [notes de publication sur l’agent Log Analytics](https://github.com/microsoft/OMS-Agent-for-Linux/releases).

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
>[Intégrer Azure Sentinel](quickstart-onboard.md)

> [!div class="nextstepaction"]
>[Obtenir une visibilité des alertes](quickstart-get-visibility.md)
