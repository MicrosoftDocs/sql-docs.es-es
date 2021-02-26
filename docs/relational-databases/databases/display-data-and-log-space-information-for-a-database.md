---
title: Representación del espacio de datos y registro de una base de datos
description: Obtenga información sobre cómo mostrar los datos y la información del espacio de registro de una base de datos en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], space
- status information [SQL Server], space
- displaying space information
- disk space [SQL Server], displaying
- databases [SQL Server], space used
- viewing space information
- space allocation [SQL Server], displaying
- data space [SQL Server]
ms.assetid: c7b99463-4bab-4e9b-9217-fcb0898dc757
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 648e33d685e7c97a7e900fd752402ed135fb9ecb
ms.sourcegitcommit: c821c2bdc383a84e45bbdc95ff6fbabf4f54901c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560940"
---
# <a name="display-data-and-log-space-information-for-a-database"></a>Mostrar la información del espacio ocupado por los datos y el registro de una base de datos
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  En este tema se describe cómo mostrar la información del espacio ocupado por los datos y el registro de una base de datos en [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 El permiso para ejecutar **sp_spaceused** se otorga al rol **public** . Solo los miembros del rol fijo de base de datos **db_owner** pueden especificar el parámetro **\@updateusage**.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-display-data-and-log-space-information-for-a-database"></a>Para mostrar la información del espacio ocupado por los datos y el registro de una base de datos  
  
1. En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y expándala.  
  
2. Expanda **Bases de datos**.  
  
3. Haga clic con el botón derecho en una base de datos, seleccione **Informes** e **Informes estándar** y, luego, haga clic en **Uso de disco**.  

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL

#### <a name="to-display-data-and-log-space-information-for-a-database-by-using-sp_spaceused"></a>Para mostrar la información del espacio ocupado por los datos y el registro de una base de datos mediante sp_spaceused
  
1. Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2. En la barra Estándar, haga clic en **Nueva consulta**.  
  
3. Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. Este ejemplo usa el procedimiento almacenado del sistema [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) para notificar información del espacio en disco para toda la base de datos: tablas e índices.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused;  
GO  
```  

#### <a name="to-display-data-space-used-by-object-and-allocation-unit-for-a-database"></a>Para mostrar el espacio de datos empleado por el objeto y la unidad de asignación relativa a una base de datos
  
1. Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2. En la barra Estándar, haga clic en **Nueva consulta**.  
  
3. Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. Este ejemplo consulta las [vistas del catálogo de objetos](../system-catalog-views/object-catalog-views-transact-sql.md) para notificar el uso del espacio en disco por tabla y dentro de cada tabla por cada [unidad de asignación](../pages-and-extents-architecture-guide.md#IAM).  
  
```sql  
SELECT
  t.object_id,
  OBJECT_NAME(t.object_id) ObjectName,
  sum(u.total_pages) * 8 Total_Reserved_kb,
  sum(u.used_pages) * 8 Used_Space_kb,
  u.type_desc,
  max(p.rows) RowsCount
FROM
  sys.allocation_units u
  join sys.partitions p on u.container_id = p.hobt_id
  join sys.tables t on p.object_id = t.object_id
GROUP BY
  t.object_id,
  OBJECT_NAME(t.object_id),
  u.type_desc
ORDER BY
  Used_Space_kb desc,
  ObjectName
```  

#### <a name="to-display-data-and-log-space-information-for-a-database-by-querying-sysdatabase_files"></a>Para mostrar la información del espacio ocupado por los datos y el registro de una base de datos mediante una consulta a sys.database_files  
  
1. Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2. En la barra Estándar, haga clic en **Nueva consulta**.  
  
3. Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se consulta la vista de catálogo [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) para devolver información específica sobre los archivos de datos y de registro de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name, type_desc, physical_name, size, max_size  
FROM sys.database_files ;  
GO  
  
```  
  
## <a name="see-also"></a>Consulte también

 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [Agregar archivos de datos o de registro a una base de datos](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)   
 [Eliminar archivos de datos o de registro de una base de datos](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)  
