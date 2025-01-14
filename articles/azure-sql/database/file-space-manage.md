---
title: Gestion de l'espace de fichiers dans Azure SQL Database
description: Cette page explique comment gérer l’espace de fichier avec bases de données uniques et mises en pool dans Azure SQL Database et fournit des exemples de code pour déterminer si vous devez réduire une base de données unique ou mise en pool, ainsi que pour effectuer une opération de réduction de base de données.
services: sql-database
ms.service: sql-database
ms.subservice: deployment-configuration
ms.custom: sqldbrb=1, devx-track-azurepowershell
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: jrasnick, sstein
ms.date: 05/28/2021
ms.openlocfilehash: fb5ee8b096f64faa47756642b4e94bae429fb879
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2021
ms.locfileid: "111591257"
---
# <a name="manage-file-space-for-databases-in-azure-sql-database"></a>Gérer l'espace de fichier des bases de données dans Azure SQL Database
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Cet article décrit les différents types d'espace de stockage disponibles pour les bases de données dans Azure SQL Database, ainsi que les étapes à suivre lorsque l'espace de fichier alloué doit être explicitement géré.

> [!NOTE]
> Cet article ne s'applique pas à Azure SQL Managed Instance.

## <a name="overview"></a>Vue d’ensemble

Avec Azure SQL Database, il existe des modèles de charge de travail dans lesquels l'allocation des fichiers de données sous-jacents aux bases de données peut dépasser le nombre de pages de données utilisées. Ce scénario peut se produire quand l’espace utilisé augmente et que des données sont ensuite supprimées. La raison en est que l’espace de fichiers alloué n’est pas récupéré automatiquement quand des données sont supprimées.

La surveillance de l’utilisation de l’espace de fichiers et la réduction des fichiers de données peuvent être nécessaires dans les scénarios suivants :

- Autoriser la croissance des données dans un pool élastique quand l’espace de fichiers alloué pour ses bases de données atteint la taille maximale du pool.
- Autoriser la réduction de la taille maximale d’une base de données unique ou d’un pool élastique.
- Autoriser la modification d’une base de données unique ou d’un pool élastique pour les faire passer à un niveau de service ou à un niveau de performance avec une taille maximale inférieure.

### <a name="monitoring-file-space-usage"></a>Surveillance de l’utilisation de l’espace de fichiers

La plupart des métriques d’espace de stockage affichées dans les API suivantes mesurent seulement la taille des pages de données utilisées :

- API de métriques basées sur Azure Resource Manager dont l’API [get-metrics](/powershell/module/az.monitor/get-azmetric) PowerShell
- T-SQL : [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)

Cependant, les API suivantes mesurent aussi la taille de l’espace alloué pour les bases de données et les pools élastiques :

- T-SQL : [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL : [sys.elastic_pool_resource_stats](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

### <a name="shrinking-data-files"></a>Réduction des fichiers de données

Azure SQL Database ne réduit pas automatiquement les fichiers de données pour récupérer l'espace alloué inutilisé en raison de l'impact potentiel sur les performances de la base de données.  Toutefois, les clients peuvent réduire les fichiers de données en libre service lorsqu’ils le souhaitent en suivant les étapes décrites à la rubrique [Récupérer l’espace alloué non utilisé](#reclaim-unused-allocated-space).

### <a name="shrinking-transaction-log-file"></a>Réduction du fichier journal de transactions

Contrairement aux fichiers de données, Azure SQL Database réduit automatiquement le fichier journal de transactions afin d’éviter une utilisation excessive de l’espace, qui peut entraîner des erreurs d’insuffisance d’espace. Il n’est généralement pas nécessaire pour les clients de réduire le fichier journal de transactions.

Dans les niveaux de service Premium et Critique pour l’entreprise, si le journal de transactions devient volumineux, il peut contribuer considérablement à la l’utilisation du stockage local jusqu’à la limite de [stockage local maximal](resource-limits-logical-server.md#storage-space-governance). Si l’utilisation du stockage local est proche de la limite, les clients peuvent choisir de réduire le journal des transactions à l’aide de la commande [DBCC SHRINKFILE](/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql), comme dans l’exemple suivant. Cela libère le stockage local dès la fin de la commande, sans attendre l’opération de réduction automatique périodique.

```tsql
DBCC SHRINKFILE (2);
```

## <a name="understanding-types-of-storage-space-for-a-database"></a>Appréhender les types d’espace de stockage d’une base de données

Il est essentiel d’appréhender les quantités d’espace de stockage suivantes pour gérer l’espace de fichier d’une base de données.

|Quantité pour une base de données|Définition|Commentaires|
|---|---|---|
|**Espace de données utilisé**|Quantité d’espace utilisée pour stocker les données de la base de données.|En général, l’espace utilisé augmente (diminue) lors des insertions (suppressions). Dans certains cas, l’espace utilisé ne change pas lors des insertions ou suppressions selon la quantité et le modèle des données impliquées dans l’opération et dans toute fragmentation éventuelle. Par exemple, la suppression d’une ligne dans chaque page de données ne diminue pas forcément l’espace utilisé.|
|**Espace de données alloué**|La quantité d’espace de fichiers formatés mise à disposition pour stocker les données de la base de données.|La quantité d’espace allouée augmente automatiquement, mais ne diminue jamais après les suppressions. Ce comportement garantit que les insertions ultérieures seront plus rapides, car l’espace n’aura pas besoin d’être reformaté.|
|**Espace de données alloué mais non utilisé**|La différence entre la quantité d’espace de données allouée et la quantité d’espace de données utilisée.|Cette quantité représente la quantité maximale d’espace libre qui peut être récupérée par la réduction des fichiers de données de la base de données.|
|**Taille maximale des données**|La quantité maximale d’espace qui peut être utilisée pour le stockage des données de la base de données.|La quantité d’espace de données allouée ne peut pas croître au-delà de la taille maximale des données.|

Le schéma suivant illustre la relation entre les différents types d’espace de stockage d’une base de données.

![relations et types d’espace de stockage](./media/file-space-manage/storage-types.png)

## <a name="query-a-single-database-for-storage-space-information"></a>Interroger une base de données unique pour des informations relatives à l’espace de stockage

Les requêtes suivantes peuvent être utilisées pour déterminer les quantités d’espace de stockage d’une base de données unique.  

### <a name="database-data-space-used"></a>Espace de données de base de données utilisé

Modifiez la requête suivante pour retourner la quantité d’espace de données de base de données utilisée.  Le résultat de la requête est exprimé en Mo.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC;
```

### <a name="database-data-space-allocated-and-unused-allocated-space"></a>Espace de données alloué et espace alloué non utilisé de la base de données

Utilisez la requête suivante pour retourner la quantité d’espace de données allouée de la base de données et la quantité d’espace alloué non utilisé.  Le résultat de la requête est exprimé en Mo.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB,
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS';
```

### <a name="database-data-max-size"></a>Taille maximale des données de la base de données

Modifiez la requête suivante pour retourner la taille maximale des données de la base de données.  Le résultat de la requête est exprimé en octets.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes;
```

## <a name="understanding-types-of-storage-space-for-an-elastic-pool"></a>Appréhender les types d’espace de stockage d’un pool élastique

Il est essentiel d’appréhender les quantités d’espace de stockage suivantes pour gérer l’espace de fichier d’un pool élastique.

|Quantité pour un pool élastique|Définition|Commentaires|
|---|---|---|
|**Espace de données utilisé**|L’espace de données total utilisé par toutes les bases de données dans le pool élastique.||
|**Espace de données alloué**|L’espace de données total alloué par toutes les bases de données dans le pool élastique.||
|**Espace de données alloué mais non utilisé**|La différence entre la quantité d’espace de données allouée et la quantité d’espace de données utilisée par toutes les bases de données dans le pool élastique.|Cette quantité représente la quantité maximale d’espace alloué au pool élastique qui peut être récupérée par la réduction des fichiers de données de la base de données.|
|**Taille maximale des données**|La quantité maximale d’espace de données qui peut être utilisée par le pool élastique pour toutes ses bases de données.|L’espace alloué au pool élastique ne doit pas dépasser la taille maximale du pool élastique.  Si cette condition se produit, l’espace alloué qui n’est pas utilisé peut être récupéré en réduisant les fichiers de données de la base de données.|

> [!NOTE]
> Le message d’erreur « Le pool élastique a atteint sa limite de stockage » indique que suffisamment d’espace a été alloué aux objets de base de données pour respecter la limite de stockage du pool élastique, mais qu’il peut y avoir de l’espace inutilisé dans l’allocation de l’espace de données. Envisagez d’augmenter la limite de stockage du pool élastique ou, comme solution à court terme, de libérer de l’espace de données à l’aide de la section [**Récupérer l’espace alloué non utilisé**](#reclaim-unused-allocated-space) ci-dessous. Vous devez également être conscient du possible impact négatif sur les performances que peut avoir la réduction des fichiers de base de données. Voir [**Reconstruire des index**](#rebuild-indexes) ci-dessous.

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Interroger un pool élastique pour des informations relatives à l’espace de stockage

Les requêtes suivantes peuvent être utilisées pour déterminer les quantités d’espace de stockage d’un pool élastique.  

### <a name="elastic-pool-data-space-used"></a>Espace de données du pool élastique utilisé

Modifiez la requête suivante pour retourner la quantité d’espace de données du pool élastique utilisée.  Le résultat de la requête est exprimé en Mo.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent / 100.0 * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC;
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Espace de données alloué et espace alloué non utilisé du pool élastique

Modifiez les exemples suivants pour retourner une table listant l’espace alloué et l’espace alloué non utilisé pour chaque base de données d’un pool élastique. La table trie les bases de données en commençant par celles qui ont la plus grande quantité d’espace alloué non utilisé jusqu’à celles qui en ont la plus petite quantité.  Le résultat de la requête est exprimé en Mo.  

Les résultats de requête permettant de déterminer l’espace alloué à chaque base de données dans le pool peuvent être cumulés pour déterminer l’espace total alloué au pool élastique. L’espace de pool élastique alloué ne doit pas dépasser la taille maximale du pool élastique.  

> [!IMPORTANT]
> Le module PowerShell Azure Resource Manager est toujours pris en charge par Azure SQL Database, mais tous les développements futurs sont destinés au module Az.Sql. Le module AzureRM continue à recevoir des résolutions de bogues jusqu’à au moins décembre 2020. Les arguments des commandes dans le module Az sont sensiblement identiques à ceux des modules AzureRm. Pour en savoir plus sur leur compatibilité, consultez [Présentation du nouveau module Az Azure PowerShell](/powershell/azure/new-azureps-module-az).

Le script PowerShell nécessite le module SQL Server PowerShell. Pour l’installer, consultez [Télécharger le module PowerShell](/sql/powershell/download-sql-server-ps-module).

```powershell
$resourceGroupName = "<resourceGroupName>"
$serverName = "<serverName>"
$poolName = "<poolName>"
$userName = "<userName>"
$password = "<password>"

# get list of databases in elastic pool
$databasesInPool = Get-AzSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $serverName -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# for each database in the elastic pool, get space allocated in MB and space allocated unused in MB
foreach ($database in $databasesInPool) {
    $sqlCommand = "SELECT DB_NAME() as DatabaseName, `
    SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, `
    SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB `
    FROM sys.database_files `
    GROUP BY type_desc `
    HAVING type_desc = 'ROWS'"
    $serverFqdn = "tcp:" + $serverName + ".database.windows.net,1433"
    $databaseStorageMetrics = $databaseStorageMetrics + 
        (Invoke-Sqlcmd -ServerInstance $serverFqdn -Database $database.DatabaseName `
            -Username $userName -Password $password -Query $sqlCommand)
}

# display databases in descending order of space allocated unused
Write-Output "`n" "ElasticPoolName: $poolName"
Write-Output $databaseStorageMetrics | Sort -Property DatabaseDataSpaceAllocatedUnusedInMB -Descending | Format-Table
```

La capture d’écran suivante est un exemple de sortie du script :

![espace de pool élastique alloué et exemple d’espace alloué non utilisé](./media/file-space-manage/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-data-max-size"></a>Taille maximale des données du pool élastique

Modifiez la requête T-SQL suivante pour retourner la dernière taille maximale enregistrée des données du pool élastique.  Le résultat de la requête est exprimé en Mo.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC;
```

## <a name="reclaim-unused-allocated-space"></a>Récupérer l’espace alloué non utilisé

> [!NOTE]
> Les commandes de réduction ont un impact sur les performances de la base de données pendant l’exécution et, si possible, doivent être exécutées pendant les périodes de faible utilisation.

### <a name="dbcc-shrink"></a>Réduire avec DBCC

Une fois que les bases de données ont été identifiées pour la récupération de l’espace alloué non utilisé, changez le nom de la base de données dans la commande suivante pour réduire les fichiers de données pour chaque base de données.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1');
```

Les commandes de réduction ont un impact sur les performances de la base de données pendant l’exécution et, si possible, doivent être exécutées pendant les périodes de faible utilisation.  

Vous devez également être conscient du possible impact négatif sur les performances que peut avoir la réduction des fichiers de base de données. Voir [**Reconstruire des index**](#rebuild-indexes) ci-dessous.

Pour plus d’informations sur cette commande, consultez [SHRINKDATABASE](/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).

### <a name="auto-shrink"></a>Réduction automatique

Vous pouvez aussi activer la réduction automatique pour une base de données.  La réduction automatique a pour effet de réduire la complexité de la gestion des fichiers, et a moins d’impact sur les performances des bases de données que les commandes `SHRINKDATABASE` ou `SHRINKFILE`. La réduction automatique peut être particulièrement utile pour gérer des pools élastiques avec de nombreuses bases de données qui connaissent une croissance et une réduction significatives de l’espace utilisé. Cependant, elle peut être moins efficace pour récupérer de l’espace de fichiers que `SHRINKDATABASE` et `SHRINKFILE`.

Par défaut, la réduction automatique est désactivée, ce qui est recommandé pour la plupart des bases de données. S’il est nécessaire d’activer la réduction automatique, il est recommandé de la désactiver une fois les objectifs de gestion de l’espace atteints, au lieu de la conserver indéfiniment. Pour plus d’informations, consultez [Considérations relatives à AUTO_SHRINK](/troubleshoot/sql/admin/considerations-autogrow-autoshrink#considerations-for-auto_shrink).

Pour activer la réduction automatique, exécutez la commande suivante dans votre base de données (et non pas dans la base de données master).

```sql
-- Enable auto-shrink for the current database.
ALTER DATABASE CURRENT SET AUTO_SHRINK ON;
```

Pour plus d’informations sur cette commande, consultez les options de [DATABASE SET](/sql/t-sql/statements/alter-database-transact-sql-set-options).

### <a name="rebuild-indexes"></a>Reconstruire des index

Une fois les fichiers de données réduits, les index peuvent devenir fragmentés et perdre en efficacité au niveau de l’optimisation des performances. En cas de dégradation des performances, vous pouvez envisager de reconstruire les index de la base de données. Pour plus d’informations sur la fragmentation et la maintenance des index, consultez [Optimiser la maintenance des index pour améliorer les performances des requêtes et réduire la consommation des ressources](/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Étapes suivantes

- Pour plus d’informations sur les tailles maximales de base de données, consultez :
  - [Limites du modèle d’achat vCore Azure SQL Database pour une base de données unique](resource-limits-vcore-single-databases.md)
  - [Limites de ressources pour des bases de données uniques suivant le modèle d’achat DTU](resource-limits-dtu-single-databases.md)
  - [Limites du modèle d’achat vCore Azure SQL Database pour les pools élastiques](resource-limits-vcore-elastic-pools.md)
  - [Limites de ressources pour des pools élastiques suivant le modèle d’achat DTU](resource-limits-dtu-elastic-pools.md)
- Pour plus d’informations sur la commande `SHRINKDATABASE`, consultez [SHRINKDATABASE](/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).
- Pour plus d’informations sur la fragmentation et la reconstruction d’index, consultez [Réorganiser et reconstruire des index](/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).
