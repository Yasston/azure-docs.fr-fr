---
title: Connecter des sources de données à Azure Sentinel | Microsoft Docs
description: Découvrez comment connecter des sources de données comme Microsoft 365 Defender (anciennement Protection Microsoft contre les menaces), Microsoft 365 et Office 365, Azure AD, ATP et Cloud App Security à Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2020
ms.author: yelevin
ms.openlocfilehash: d6b132fbb3aed541cc602537df1d40fa0d47702a
ms.sourcegitcommit: ef950cf37f65ea7a0f583e246cfbf13f1913eb12
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2021
ms.locfileid: "111421847"
---
# <a name="connect-data-sources"></a>Connecter des sources de données

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Une fois que vous avez activé Azure Sentinel, la première chose à faire est de connecter vos sources de données. Azure Sentinel est fourni avec plusieurs connecteurs pour les solutions Microsoft, disponibles, prêts à l’emploi et offrant une intégration en temps réel, notamment les solutions Microsoft 365 Defender (anciennement Protection Microsoft contre les menaces), les sources Microsoft 365 (dont Office 365), Azure AD, Microsoft Defender pour Identity (anciennement Azure ATP), Microsoft Cloud App Security, entre autres. En outre, il existe des connecteurs intégrés pour la connexion aux écosystèmes de sécurité élargis pour les solutions non Microsoft. Vous pouvez également utiliser le format CEF (Common Event Format), Syslog ou l’API REST pour connecter vos sources de données à Azure Sentinel.

1. Dans le menu, sélectionnez **Connecteurs de données**. Cette page vous permet de voir la liste complète des connecteurs Azure Sentinel et fournit leur état. Sélectionnez le connecteur que vous souhaitez connecter, puis sélectionnez **Open connector page** (Ouvrir la page du connecteur). 

   ![Galerie des connecteurs de données](./media/collect-data/collect-data-page.png)

1. Dans la page du connecteur, vérifiez que tous les prérequis ont été respectés, puis suivez les instructions pour connecter les données à Azure Sentinel. La synchronisation des journaux avec Azure Sentinel peut prendre un certain temps à démarrer. Une fois connecté, vous voyez un récapitulatif des données dans le graphe **Données reçues**, ainsi que l’état de connectivité des types de données.

   ![Configurer des connecteurs de données](./media/collect-data/opened-connector-page.png)
  
1. Cliquez sur l’onglet **Étapes suivantes** pour voir le contenu prêt à l’emploi qui est fourni par Azure Sentinel pour le type de données en question.

   ![Étapes suivantes pour les connecteurs](./media/collect-data/data-insights.png)
 

## <a name="data-connection-methods"></a>Méthodes de connexion de données

Les méthodes de connexion de données suivantes sont prises en charge par Azure Sentinel :

- **Intégration de service à service** :<br> Certains services sont connectés en mode natif, tels que les services Microsoft et AWS. Ces services tirent parti de la base Azure pour une intégration prête à l’emploi ; les solutions suivantes peuvent être connectées en quelques clics :
    - [Amazon Web Services - CloudTrail](connect-aws.md)
    - [Azure Active Directory](connect-azure-active-directory.md) - journaux d’audit et journaux de connexion
    - [Azure Active Directory Identity Protection](connect-azure-ad-Identity-protection.md)
    - [Activité Azure](connect-azure-activity.md)
    - [Azure DDoS Protection](connect-azure-ddos-protection.md)
    - Alertes [Azure Defender](connect-azure-security-center.md) à partir d’Azure Security Center
    - [Azure Defender pour IoT](connect-asc-iot.md) (anciennement Azure Security Center pour IoT)
    - [Pare-feu Azure](connect-azure-firewall.md)
    - [Azure Information Protection](connect-azure-information-protection.md)
    - [Azure Key Vault](connect-azure-key-vault.md)
    - [Azure Kubernetes Service (AKS)](connect-azure-kubernetes-service.md)
    - [Bases de données SQL Azure](connect-azure-sql-logs.md)
    - [Compte Stockage Azure](connect-azure-storage-account.md)
    - [Pare-feu d’application web (WAF) Azure](connect-azure-waf.md) (anciennement Microsoft WAF)
    - [Serveur de noms de domaine](connect-dns.md)
    - [Dynamics 365](connect-dynamics-365.md)
    - [Microsoft 365 Defender](connect-microsoft-365-defender.md) : inclut les incidents M365D et les données brutes Defender pour point de terminaison
    - [Microsoft Cloud App Security](connect-cloud-app-security.md)
    - [Microsoft Defender for Endpoint](connect-microsoft-defender-advanced-threat-protection.md) (anciennement Microsoft Defender Advanced Threat Protection)
    - [Microsoft Defender pour Identity](connect-azure-atp.md) (anciennement Azure Advanced Threat Protection)
    - [Microsoft Defender pour Office 365](connect-office-365-advanced-threat-protection.md) (anciennement Office 365 - Protection avancée contre les menaces)
    - [Office 365](connect-office-365.md) (incluant Teams)
    - [Pare-feu Windows](connect-windows-firewall.md)
    - (Windows) [Événements de sécurité](connect-windows-security-events.md)

- **Solutions externes via API** : Certaines sources de données sont connectées à l’aide d’API fournies par la source de données connectée. En général, la plupart des technologies de sécurité fournissent un ensemble d’API par le biais duquel les journaux des événements peuvent être récupérés. Les API se connectent à Azure Sentinel, collectent des types de données spécifiques et les envoient à Azure Log Analytics. Les appliances connectées via une API sont les suivantes :
    
    - [Agari Phishing Defense and Brand Protection](connect-agari-phishing-defense.md)
    - [Alcide kAudit](connect-alcide-kaudit.md)
    - [Pare-feu d’applications web Barracuda](connect-barracuda.md)
    - [Pare-feu Barracuda CloudGen](connect-barracuda-cloudgen-firewall.md)
    - [BETTER Mobile Threat Defense](connect-better-mtd.md)
    - [Beyond Security beSECURE](connect-besecure.md)
    - [Cisco Umbrella](connect-cisco-umbrella.md)
    - [Citrix Analytics (Sécurité)](connect-citrix-analytics.md)
    - [F5 BIG-IP](connect-f5-big-ip.md)
    - [Forcepoint DLP](connect-forcepoint-dlp.md)
    - [Google Workspace (anciennement G Suite)](connect-google-workspace.md)
    - [Journaux DNS NXLog (Windows)](connect-nxlog-dns.md)
    - [NXLog LinuxAudit](connect-nxlog-linuxaudit.md)
    - [Okta SSO](connect-okta-single-sign-on.md)
    - [Orca Security](connect-orca-security-alerts.md)
    - [Journaux Perimeter 81](connect-perimeter-81-logs.md)
    - [Proofpoint On Demand (POD) Email Security](connect-proofpoint-pod.md)
    - [Proofpoint TAP](connect-proofpoint-tap.md)
    - [Qualys VM](connect-qualys-vm.md)
    - [Salesforce Service Cloud](connect-salesforce-service-cloud.md)
    - [Sophos Cloud Optix](connect-sophos-cloud-optix.md)
    - [Squadra Technologies secRMM](connect-squadra-secrmm.md)
    - [Symantec ICDX](connect-symantec.md)
    - [VMware Carbon Black Cloud Endpoint Standard](connect-vmware-carbon-black.md)
    - [Zimperium](connect-zimperium-mtd.md)


- **Solutions externes via un agent** : Sentinel Azure peut être connecté par un agent à toute autre source de données capable d’effectuer une diffusion en continu de journal en temps réel à l’aide du protocole Syslog.

    La plupart des appliances utilisent le protocole Syslog pour envoyer des messages d’événement incluant le journal proprement dit et des données sur le journal. Le format des journaux varie, mais la plupart des appliances prennent en charge la mise en forme basée sur la norme CEF pour les données de journaux. 

    L’agent Azure Sentinel, qui est en réalité l’agent Log Analytics, convertit les journaux au format CEF dans un format que Log Analytics peut ingérer. Selon le type d’appliance, l’agent est installé directement sur l’appliance ou sur un redirecteur de journaux Linux dédié. L’agent pour Linux reçoit des événements du démon Syslog en utilisant le protocole UDP mais, si une machine Linux est supposée collecter un volume important d’événements Syslog, ceux-ci sont envoyés, en utilisant le protocole TCP, du démon Syslog à l’agent, puis de celui-ci à Log Analytics.

    - **Pare-feu, proxys et points de terminaison - CEF :**
        - [AI Vectra Detect](connect-ai-vectra-detect.md)
        - [Akamai Security Events](connect-akamai-security-events.md)
        - [Aruba ClearPass](connect-aruba-clearpass.md)
        - [Broadcom Symantec DLP](connect-broadcom-symantec-dlp.md)
        - [Check Point](connect-checkpoint.md)
        - [Cisco ASA](connect-cisco.md)
        - [Citrix WAF](connect-citrix-waf.md)
        - [CyberArk Enterprise Password Vault](connect-cyberark.md)
        - [ExtraHop Reveal(x)](connect-extrahop.md)
        - [F5 ASM](connect-f5.md)
        - [Produits Forcepoint](connect-forcepoint-casb-ngfw.md)
        - [Fortinet](connect-fortinet.md)
        - [Illusive Networks AMS](connect-illusive-attack-management-system.md)
        - [Imperva WAF Gateway](connect-imperva-waf-gateway.md)
        - [One Identity Safeguard](connect-one-identity.md)
        - [Palo Alto Networks](connect-paloalto.md)
        - [Thycotic Secret Server](connect-thycotic-secret-server.md)
        - [Trend Micro Deep Security](connect-trend-micro.md)
        - [Trend Micro TippingPoint](connect-trend-micro-tippingpoint.md)
        - [WireX Network Forensics Platform](connect-wirex-systems.md)
        - [Zscaler](connect-zscaler.md)
        - [Autres appliances basées sur le format CEF](connect-common-event-format.md)
    - **Pare-feu, proxys et points de terminaison - Syslog :**
        - [Alsid for Active Directory](connect-alsid-active-directory.md)
        - [Cisco Meraki](connect-cisco-meraki.md)
        - [Cisco Unified Computing System (UCS)](connect-cisco-ucs.md)
        - [Infoblox NIOS](connect-infoblox.md)
        - [Juniper SRX](connect-juniper-srx.md)
        - [Pulse Connect Secure](connect-pulse-connect-secure.md)
        - [Sophos XG](connect-sophos-xg-firewall.md)
        - [Squid Proxy](connect-squid-proxy.md)
        - [Symantec Proxy SG](connect-symantec-proxy-sg.md)
        - [Symantec VIP](connect-symantec-vip.md)
        - [VMware ESXi](connect-vmware-esxi.md)
        - [Autres appliances basées sur Syslog](connect-syslog.md)
    - [Apache HTTP Server](connect-apache-http-server.md)
    - Solutions DLP
    - [Fournisseurs de renseignement sur les menaces](connect-threat-intelligence.md)
    - [Machines DNS](connect-dns.md) : agent installé directement sur la machine DNS
    - [Machines virtuelles Azure Stack](connect-azure-stack.md)
    - Serveurs Linux
    - Autres clouds
    
## <a name="agent-connection-options"></a>Options de connexion de l’agent<a name="agent-options"></a>

Pour connecter votre appliance externe à Azure Sentinel, l’agent doit être déployé sur une machine dédiée (machine virtuelle ou ordinateur local) pour prendre en charge la communication entre l’appliance et Azure Sentinel. Vous pouvez déployer l’agent manuellement ou automatiquement. Le déploiement automatique n’est disponible que si votre machine dédiée est une nouvelle machine virtuelle que vous créez dans Azure. 

![CEF dans Azure](./media/connect-cef/cef-syslog-azure.png)

Vous pouvez également déployer l’agent manuellement sur une machine virtuelle Azure existante, sur une machine virtuelle dans un autre cloud, ou sur un ordinateur local.

![CEF local](./media/connect-cef/cef-syslog-onprem.png)

## <a name="map-data-types-with-azure-sentinel-connection-options"></a>Mapper des types de données avec les options de connexion Azure Sentinel


| **Type de données** | **Comment se connecter** | **Connecteur de données ?** | **Commentaires** |
|------|---------|-------------|------|
| AWSCloudTrail | [Connecter AWS](connect-aws.md) | &#10003; | |
| AzureActivity | [Connecter les journaux d’activité Azure](connect-azure-activity.md) et [Vue d’ensemble des journaux d’activité](../azure-monitor/essentials/platform-logs-overview.md)| &#10003; | |
| AuditLogs | [Connecter Azure AD](connect-azure-active-directory.md)  | &#10003; | |
| SigninLogs | [Connecter Azure AD](connect-azure-active-directory.md)  | &#10003; | |
| AzureFirewall |[Diagnostics Azure](../firewall/firewall-diagnostics.md) | &#10003; | |
| InformationProtectionLogs_CL  | [Rapports Azure Information Protection](/azure/information-protection/reports-aip)<br>[Connecter Azure Information Protection](connect-azure-information-protection.md)  | &#10003; | Cette opération utilise généralement la fonction **InformationProtectionEvents** en plus du type de données. Pour plus d’informations, consultez [Guide pratique pour modifier les rapports et créer des requêtes personnalisées](/azure/information-protection/reports-aip#how-to-modify-the-reports-and-create-custom-queries).|
| AzureNetworkAnalytics_CL  | [Schéma Traffic Analytics](../network-watcher/traffic-analytics.md) [Traffic Analytics](../network-watcher/traffic-analytics.md)  | | |
| CommonSecurityLog  | [Connecter CEF](connect-common-event-format.md)  | &#10003; | |
| OfficeActivity | [Connecter Office 365](connect-office-365.md) | &#10003; | |
| SecurityEvents | [Connecter les événements de sécurité Windows](connect-windows-security-events.md)  | &#10003; | Pour les classeurs des protocoles non sécurisés, consultez le blog relatif à la [configuration des classeurs des protocoles non sécurisés](./quickstart-get-visibility.md#use-built-in-workbooks)  |
| syslog | [Connecter Syslog](connect-syslog.md) | &#10003; | |
| Pare-feu d’applications web (WAF) Microsoft (AzureDiagnostics) |[Connecter un pare-feu d’applications web Microsoft](./connect-azure-waf.md) | &#10003; | |
| SymantecICDx_CL | [Connecter Symantec](connect-symantec.md) | &#10003; | |
| ThreatIntelligenceIndicator  | [Connecter Threat Intelligence](connect-threat-intelligence.md)  | &#10003; | |
| VMConnection <br> ServiceMapComputer_CL<br> ServiceMapProcess_CL|  [Azure Monitor Service Map](../azure-monitor/vm/service-map.md)<br>[Intégration d’insights de machine virtuelle Azure Monitor](../azure-monitor/vm/vminsights-enable-overview.md) <br> [Activer les insights de machine virtuelle Azure Monitor](../azure-monitor/vm/vminsights-enable-overview.md) <br> [Utilisation de l’intégration d’une machine virtuelle unique](../azure-monitor/vm/vminsights-enable-portal.md)<br>  [Utilisation de l’intégration par le biais d’une stratégie](../azure-monitor/vm/vminsights-enable-policy.md)| &#10007; | Classeur d’insights de machine virtuelle  |
| DnsEvents | [Connecter DNS](connect-dns.md) | &#10003; | |
| W3CIISLog | [Connecter les journaux IIS](../azure-monitor/agents/data-sources-iis-logs.md)  | &#10007; | |
| WireData | [Connecter Wire Data](../azure-monitor/insights/wire-data.md) | &#10007; | |
| WindowsFirewall | [Connecter le Pare-feu Windows](connect-windows-firewall.md) | &#10003; | |
| SecurityAlert AADIP  | [Connecter Azure AD Identity Protection](connect-azure-ad-identity-protection.md)  | &#10003; | |
| SecurityAlert AATP  | [Connecter Microsoft Defender pour Identity](connect-azure-atp.md) (anciennement Azure ATP) | &#10003; | |
| SecurityAlert ASC  | [Connecter les alertes Azure Defender](connect-azure-security-center.md) à partir d’Azure Security Center  | &#10003; | |
| SecurityAlert MCAS  | [Connecter Microsoft Cloud App Security](connect-cloud-app-security.md)  | &#10003; | |
| SecurityAlert | | | |
| Sysmon (événement) | [Connecter Sysmon](https://azure.microsoft.com/blog/detecting-in-memory-attacks-with-sysmon-and-azure-security-center)<br> [Connecter les événements Windows](../azure-monitor/agents/data-sources-windows-events.md) <br> [Obtenir l’analyseur Sysmon](https://github.com/Azure/Azure-Sentinel/blob/master/Parsers/Sysmon/Sysmon-v10.42-Parser.txt)| &#10007; | La collection Sysmon n’est pas installée par défaut sur les machines virtuelles. Pour plus d’informations sur l’installation de l’agent Sysmon, consultez [Sysmon](/sysinternals/downloads/sysmon). |
| ConfigurationData  | [Automatiser l’inventaire des machines virtuelles](../automation/change-tracking/overview.md)| &#10007; | |
| ConfigurationChange  | [Automatiser le suivi des machines virtuelles](../automation/change-tracking/overview.md) | &#10007; | |
| F5 BIG-IP | [Connecter F5 BIG-IP](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel)  | &#10003; | |
| McasShadowItReporting  |  | &#10007; | |
| Barracuda_CL | [Connecter Barracuda](connect-barracuda.md) | &#10003; | |


## <a name="next-steps"></a>Étapes suivantes

- Pour utiliser Azure Sentinel, vous devez disposer d’un abonnement à Microsoft Azure. Si vous n’avez pas d’abonnement, vous pouvez vous inscrire à un [essai gratuit](https://azure.microsoft.com/free/).
- Découvrez comment [intégrer vos données à Azure Sentinel](quickstart-onboard.md) et [obtenir une visibilité de vos données et des menaces potentielles](quickstart-get-visibility.md).
