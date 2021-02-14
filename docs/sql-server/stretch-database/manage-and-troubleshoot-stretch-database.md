---
description: Administración y solución de problemas de Stretch Database
title: Supervisión y solución de problemas
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, managing
- Stretch Database, troubleshooting
- managing Stretch Database
- troubleshooting Stretch Database
ms.assetid: 6334db3e-9297-44df-8d53-211187a95520
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 346bc660bc1fa1fd9a6e75a91caa182ff25e6c53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081109"
---
# <a name="manage-and-troubleshoot-stretch-database"></a>Administración y solución de problemas de Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Para administrar y solucionar problemas de Stretch Database, use las herramientas y los métodos que se describen en este artículo.  
## <a name="manage-local-data"></a>Administrar los datos locales  
  
###  <a name="get-info-about-local-databases-and-tables-enabled-for-stretch-database"></a><a name="LocalInfo"></a> Obtener información sobre las bases de datos y tablas locales habilitadas para Stretch Database  
 Abra las vistas de catálogo **sys.databases** y **sys.tables** para ver información sobre las bases de datos y las tablas de SQL Server habilitadas para Stretch. Para obtener más información, vea [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) y [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md).  
 
 Para ver el espacio que emplea una tabla habilitada para Stretch en SQL Server, ejecute la siguiente instrucción.
 
 ```sql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
   
## <a name="manage-data-migration"></a>Administrar la migración de datos  
  
### <a name="check-the-filter-function-applied-to-a-table"></a>Comprobar la función de filtro aplicada a una tabla  
 Abra la vista de catálogo **sys.remote_data_archive_tables** y compruebe el valor de la columna **filter_predicate** para identificar la función que usa Stretch Database para seleccionar las filas que se van a migrar. Si el valor es null, la tabla completa es elegible para la migración. Para obtener más información, vea [sys.remote_data_archive_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md) y [Select rows to migrate by using a filter function](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md) (Seleccionar las filas que se van a migrar mediante una función de filtro).  
  
###  <a name="check-the-status-of-data-migration"></a><a name="Migration"></a> Comprobar el estado de la migración de datos  
 Seleccione **Tareas | Stretch | Monitor** en una base de datos de SQL Server Management Studio para supervisar la migración de datos en el monitor de Stretch Database. Para obtener más información, vea [Supervisión y solución de problemas de migración de datos &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
 También puede abrir la vista de administración dinámica **sys.dm_db_rda_migration_status** para ver el número de lotes y filas de datos que se han migrado.  
  
###  <a name="troubleshoot-data-migration"></a><a name="Firewall"></a> Solución de problemas de migración de datos  
 Para obtener sugerencias de solución de problemas, vea [Supervisión y solución de problemas de migración de datos &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
## <a name="manage-remote-data"></a>Administrar los datos remotos  
  
###  <a name="get-info-about-remote-databases-and-tables-used-by-stretch-database"></a><a name="RemoteInfo"></a> Obtener información sobre las bases de datos y tablas remotas que usa Stretch Database  
 Abra las vistas de catálogo **sys.remote_data_archive_databases** y **sys.remote_data_archive_tables** para ver la información de las bases de datos y las tablas remotas en las que se almacenan los datos migrados. Para obtener más información, vea [sys.remote_data_archive_databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-databases.md) y [sys.remote_data_archive_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md).  
 
Para ver el espacio que emplea una tabla habilitada para Stretch en Azure, ejecute la siguiente instrucción.
 
 ```sql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Eliminar los datos migrados  
Si quiere eliminar los datos que ya se han migrado a Azure, siga los pasos descritos en [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md).  
  
## <a name="manage-table-schema"></a>Administrar el esquema de tabla

### <a name="dont-change-the-schema-of-the-remote-table"></a>No cambie el esquema de la tabla remota  
 No cambie el esquema de una tabla remota de Azure que esté asociada a una tabla de SQL Server configurada para Stretch Database. En concreto, no modifique el nombre o el tipo de datos de una columna. La característica Stretch Database da por supuestas varias cosas sobre el esquema de la tabla remota en relación con el esquema de la tabla de SQL Server. Si cambia el esquema remoto, Stretch Database deja de funcionar para la tabla modificada.  

### <a name="reconcile-table-columns"></a>Conciliar las columnas de tabla  
Si ha eliminado por error las columnas de la tabla remota, ejecute **sp_rda_reconcile_columns** para agregar columnas a la tabla remota que hay en la tabla de SQL Server habilitada para Stretch, pero no en la tabla remota. Para obtener más información, vea [sys.sp_rda_reconcile_columns](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-columns-transact-sql.md).  
  
  > [!IMPORTANT]
  > Cuando **sp_rda_reconcile_columns** vuelve a crear las columnas que eliminó por error de la tabla remota, no restaura los datos que había antes en las columnas eliminadas.
  
**sp_rda_reconcile_columns** no elimina las columnas de la tabla remota que hay en la tabla remota, pero no en la tabla de SQL Server habilitada para Stretch. Si hay columnas en la tabla remota de Azure que ya no existen en la tabla de SQL Server habilitada para Stretch, estas columnas adicionales no impiden que Stretch Database funcione con normalidad. También puede quitar estas columnas adicionales de forma manual.  
 
## <a name="manage-performance-and-costs"></a>Administrar el rendimiento y los costos  
  
### <a name="troubleshoot-query-performance"></a>Solución de problemas del rendimiento de las consultas  
  Se espera que las consultas que incluyen tablas habilitadas para Stretch se realicen más lentamente que antes de que las tablas se habilitaran para Stretch. Si se reduce significativamente el rendimiento de las consultas, vea los siguientes problemas posibles.  
  
-   ¿Se encuentra el servidor de Azure en una región geográfica diferente de la de SQL Server? Configure el servidor de Azure para que se encuentre en la misma región geográfica que SQL Server para reducir la latencia de red.  
  
-   Las condiciones de red podrían haberse degradado. Póngase en contacto con el administrador de red para obtener información sobre los problemas o las interrupciones recientes.  
  
### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Aumentar el nivel de rendimiento de Azure para operaciones con uso intensivo de recursos, como la indexación  
 Al compilar, recompilar o reorganizar el índice de una tabla grande configurada para Stretch Database y se prevean numerosas consultas de los datos migrados en Azure durante este tiempo, considere la posibilidad de aumentar el nivel de rendimiento de la correspondiente base de datos remota de Azure mientras dure la operación. Para obtener más información sobre los niveles de rendimiento y los precios, vea [Precios de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>No se puede pausar el servicio de SQL Server Stretch Database en Azure  
 Asegúrese de seleccionar el nivel de rendimiento y el precio adecuados. Si aumenta temporalmente el nivel de rendimiento de una operación que consume muchos recursos, restáurelo al nivel anterior una vez finalizada la operación. Para obtener más información sobre los niveles de rendimiento y los precios, vea [Precios de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
   
 ## <a name="change-the-scope-of-queries"></a>Cambiar el ámbito de las consultas  
 De forma predeterminada, las consultas efectuadas en tablas habilitadas para Stretch devuelven datos locales y remotos. Puede cambiar el ámbito de todas las consultas efectuadas por todos los usuarios o de una sola consulta efectuada por un administrador.  
   
 ### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Cambiar el ámbito de todas las consultas efectuadas por todos los usuarios  
 Para cambiar el ámbito de todas las consultas efectuadas por todos los usuarios, ejecute el procedimiento almacenado **sys.sp_rda_set_query_mode**. Puede reducir el ámbito para consultar únicamente los datos locales, deshabilitar todas las consultas o restaurar la configuración predeterminada. Para obtener más información, vea [sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md).  
   
 ### <a name="change-the-scope-of-queries-for-a-single-query-by-an-administrator"></a><a name="queryHints"></a>Modificación del ámbito de las consultas de una única consulta que lleva a cabo un administrador  
 Para cambiar el ámbito de una sola consulta efectuada por un miembro del rol db_owner, agregue la sugerencia de consulta **WITH ( REMOTE_DATA_ARCHIVE_OVERRIDE = *valor* )** a la instrucción SELECT. La sugerencia de consulta REMOTE_DATA_ARCHIVE_OVERRIDE puede tener los siguientes valores:  
 -   **LOCAL_ONLY**. Solo se consultan los datos locales.  
   
 -   **REMOTE_ONLY**. Solo se consultan los datos remotos.  
   
 -   **STAGE_ONLY**. Solo se consultan los datos de la tabla en los que Stretch Database almacena provisionalmente las filas aptas para la migración y retiene las filas migradas durante el período especificado después de la migración. Esta sugerencia de consulta es la única manera de consultar la tabla de almacenamiento provisional.  
  
Por ejemplo, la siguiente consulta solo devuelve los resultados locales.  
  
 ```sql  
USE <Stretch-enabled database name>;
GO
SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
GO
```  
   
 ## <a name="make-administrative-updates-and-deletes"></a><a name="adminHints"></a>Efectuar actualizaciones y eliminaciones administrativas  
 De forma predeterminada, no se pueden actualizar o eliminar de una tabla habilitada para Stretch filas que son aptas para la migración ni filas que ya se han migrado. Cuando tiene que solucionar un problema, un miembro del rol db_owner puede ejecutar una operación UPDATE o DELETE si agrega la sugerencia de consulta **WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = *valor* )** a la instrucción. La sugerencia de consulta REMOTE_DATA_ARCHIVE_OVERRIDE puede tener los siguientes valores:  
 -   **LOCAL_ONLY**. Solo se actualizan o eliminan los datos locales.  
   
 -   **REMOTE_ONLY**. Solo se actualizan o eliminan los datos remotos.  
   
 -   **STAGE_ONLY**. Solo se actualizan o eliminan los datos de la tabla en los que Stretch Database almacena provisionalmente las filas aptas para la migración y retiene las filas migradas durante el período especificado después de la migración.  
  
## <a name="see-also"></a>Consulte también  
 [Supervisión y solución de problemas de migración de datos &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md)   
[Copia de seguridad de bases de datos habilitadas para Stretch (Stretch Database)](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
[Restore Stretch-enabled databases (Stretch Database) (Restauración de bases de datos habilitadas para Stretch (Stretch Database))](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
  
