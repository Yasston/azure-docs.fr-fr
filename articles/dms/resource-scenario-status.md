---
title: État du scénario de migration d’une base de données
titleSuffix: Azure Database Migration Service
description: Obtenez davantage d’informations sur l’état des scénarios de migration pris en charge par Azure Database Migration Service.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: troubleshooting
ms.date: 07/08/2020
ms.openlocfilehash: 237de7e75007de85054501ed76b253a6eedf4346
ms.sourcegitcommit: 70ce9237435df04b03dd0f739f23d34930059fef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2021
ms.locfileid: "111528367"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>État des scénarios de migration pris en charge par Azure Database Migration Service

Azure Database Migration Service est conçu pour prendre en charge différents scénarios de migration (paires source/cible), aussi bien pour le mode hors connexion (migration ponctuelle) que le mode en ligne (synchronisation continue). Azure Database Migration Service couvre de plus en plus de scénarios au fil du temps. De nouveaux scénarios sont régulièrement ajoutés. Cet article identifie les scénarios de migration qui sont actuellement pris en charge par Azure Database Migration Service et l’état de chaque scénario (préversion privée, préversion publique ou disposition générale).

## <a name="offline-versus-online-migrations"></a>Migrations hors connexion et en ligne

Avec Azure Database Migration Service, vous pouvez opter pour une migration hors connexion ou en ligne. Avec les migrations *hors connexion*, les temps d’arrêt de l’application commencent en même temps que la migration. Pour limiter le temps d’arrêt au temps nécessaire pour basculer vers le nouvel environnement une fois la migration terminée, utilisez une migration *en ligne*. Nous vous recommandons de tester une migration hors connexion pour déterminer si le temps d’arrêt est acceptable ; dans le cas contraire, privilégiez une migration en ligne.

## <a name="migration-scenario-status"></a>État du scénario de migration

L’état des scénarios de migration pris en charge par Azure Database Migration Service varie dans le temps. En règle générale, les scénarios sont d’abord publiés en **préversion privée**. Au terme de la préversion privée, le scénario passe à l’état de **préversion publique**. Les utilisateurs d’Azure Database Migration Service peuvent essayer des scénarios de migration en préversion directement à partir de l’interface utilisateur. Aucune inscription n’est nécessaire.  Toutefois, il se peut que les scénarios de migration en préversion publique ne soient pas disponibles dans toutes les régions et subissent des modifications supplémentaires avant la version finale. Au terme de la préversion publique, le scénario passe à l’état de **disponibilité générale**. La disponibilité générale est l’état de la version finale : les fonctionnalités sont complètes et accessibles à tous les utilisateurs.

## <a name="migration-scenario-support"></a>Prise en charge du scénario de migration

Les tableaux suivants indiquent les scénarios de migration qui sont pris en charge lorsqu’Azure Database Migration Service est utilisé.

> [!NOTE]
> Si un scénario apparaît dans la liste ci-dessous, mais qu’il ne s’affiche pas dans l’interface utilisateur, veuillez envoyer un courrier pour [Poser des questions sur les migrations de base de données Azure](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

### <a name="offline-one-time-migration-support"></a>Prise en charge de la migration hors connexion (migration ponctuelle)

Le tableau suivant présente la prise en charge d’Azure Database Migration Service pour les migrations hors connexion.

| Cible  | Source | Support | Statut |
| ------------- | ------------- |:-------------:|:-------------:|
| **Base de données Azure SQL** | SQL Server | ✔ | GA |
|   | RDS SQL | X |  |
|   | Oracle | X |  |
| **Base de données Azure SQL MI** | SQL Server | ✔ | GA |
|   | RDS SQL | X |  |
|   | Oracle | X |   |
| **Machine virtuelle Azure SQL** | SQL Server | ✔ | GA |
|   | Oracle | X |   |
| **Azure Cosmos DB** | MongoDB | ✔ | GA |
| **Azure DB pour MySQL – Serveur unique** | MySQL | ✔ | Version préliminaire publique  |
|   | RDS MySQL | ✔ | Version préliminaire publique  |
|   | Azure DB pour MySQL* | ✔ | Version préliminaire publique  |
| **Azure DB pour MySQL – Serveur flexible** | MySQL | ✔ | Version préliminaire publique  |
|   | RDS MySQL | ✔ | Version préliminaire publique  |
|   | Azure DB pour MySQL* | ✔ | Version préliminaire publique  |
| **Azure DB pour PostgreSQL - Serveur unique** | PostgreSQL | X |
|  | RDS PostgreSQL | X |   |
| **Azure DB pour PostgreSQL – Serveur flexible** | PostgreSQL | X |
|  | RDS PostgreSQL | X |   |
| **Azure DB pour PostgreSQL - Hyperscale (Citus)** | PostgreSQL | X |
|  | RDS PostgreSQL | X |   |

### <a name="online-continuous-sync-migration-support"></a>Prise en charge de la migration en ligne (synchronisation continue)

Le tableau suivant présente la prise en charge d’Azure Database Migration Service pour les migrations en ligne.

| Cible  | Source | Support | Statut |
| ------------- | ------------- |:-------------:|:-------------:|
| **Base de données Azure SQL** | SQL Server | X |  |
|   | RDS SQL | X |  |
|   | Oracle | X |  |
| **Base de données Azure SQL MI** | SQL Server | ✔ | GA |
|   | RDS SQL | X |  |
|   | Oracle | X |  |
| **Machine virtuelle Azure SQL** | SQL Server | X |   |
|   | Oracle  | X |  |
| **Azure Cosmos DB** | MongoDB | ✔ | GA |
| **Base de données Azure pour MySQL** | MySQL | ✔ | GA |
|   | RDS MySQL | ✔ | GA |
| **Azure DB pour PostgreSQL - Serveur unique** | PostgreSQL | ✔ | GA |
|   | Azure DB pour PostgreSQL - Serveur unique* | ✔ | GA |
|   | RDS PostgreSQL | ✔ | GA |
| **Azure DB pour PostgreSQL – Serveur flexible** | PostgreSQL | ✔ | GA |
|   | Azure DB pour PostgreSQL - Serveur unique* | ✔ | GA |
|   | RDS PostgreSQL | ✔ | GA |
| **Azure DB pour PostgreSQL - Hyperscale (Citus)** | PostgreSQL | ✔ | GA |
|   | RDS PostgreSQL | ✔ | GA |

> [!NOTE]
> Si votre base de données source se trouve déjà dans Azure PaaS (soit Azure DB pour MySQL ou Azure DB pour PostgreSQL), choisissez le moteur correspondant lorsque vous créez votre activité de migration. Par exemple, si vous effectuez une migration d’Azure DB pour MySQL – Serveur unique vers Azure DB pour MySQL – Serveur flexible, sélectionnez MySQL comme moteur source pendant le scénario de création. Si vous migrez d’Azure DB pour PostgreSQL – Serveur unique vers Azure DB pour PostgreSQL – Serveur flexible, choisissez PostgreSQL comme moteur source pendant le scénario de création. 

## <a name="next-steps"></a>Étapes suivantes

Pour une présentation d’Azure Database Migration Service et de la mise à disponibilité régionale, consultez l’article [Qu’est-ce qu’Azure Database Migration Service ?](dms-overview.md).
