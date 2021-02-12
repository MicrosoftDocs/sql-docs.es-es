---
description: Pausa y reanudación de la migración de datos (Stretch Database)
title: Pausa y reanudación de la migración de datos
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, pausing and resuming
- pausing Stretch Database
- resuming Stretch Database
ms.assetid: 65d6a990-b295-41b2-97f9-7b6bf3000e4d
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 5521919a31e84a9ade57bddc6a0716f00d1fc98b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031677"
---
# <a name="pause-and-resume-data-migration-stretch-database"></a>Pausa y reanudación de la migración de datos (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Para pausar o reanudar la migración de datos en Azure, seleccione **Stretch** para una tabla en SQL Server Management Studio y después seleccione **Pausar** para pausar la migración de datos o **Reanudar** para reanudar la migración de datos. También puede usar Transact-SQL para pausar o reanudar la migración de datos.  
  
 Pause la migración de datos en tablas individuales para solucionar problemas en el servidor local o para maximizar el ancho de banda de red disponible.  

## <a name="pause-data-migration"></a>Pausa de la migración de datos  
  
### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Uso de SQL Server Management Studio para pausar la migración de datos  
  
1.  En el Explorador de objetos de SQL Server Management Studio, seleccione la tabla habilitada para Stretch para la que desea pausar la migración de datos.  
  
2.  Haga clic con el botón derecho y seleccione **Stretch**; después, seleccione **Pausar**.  
  
### <a name="use-transact-sql-to-pause-data-migration"></a>Use Transact-SQL para pausar la migración de datos  
 Ejecute el siguiente comando.  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO 
```  
  
## <a name="resume-data-migration"></a>Reanudación de la migración de datos  
  
### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Uso de SQL Server Management Studio para reanudar la migración de datos  
  
1.  En el Explorador de objetos de SQL Server Management Studio, seleccione la tabla habilitada para Stretch para la que desea reanudar la migración de datos.  
  
2.  Haga clic con el botón derecho y seleccione **Stretch**; después, seleccione **Reanudar**.  
  
### <a name="use-transact-sql-to-resume-data-migration"></a>Use Transact-SQL para reanudar la migración de datos  
 Ejecute el siguiente comando.  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```  

## <a name="check-whether-migration-is-active-or-paused"></a>Comprobar si la migración está activa o en pausa

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Usar SQL Server Management Studio para comprobar si la migración está activa o en pausa
En SQL Server Management Studio, abra **Supervisión de base de datos de Stretch** y compruebe el valor de la columna **Estado de migración** . Para obtener más información, vea [Supervisión y solución de problemas de migración de datos](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Usar Transact-SQL para comprobar si la migración está activa o en pausa
Consulte la vista de catálogo **sys.remote_data_archive_tables** y compruebe el valor de la columna **is_migration_paused** . Para obtener más información, vea [sys.remote_data_archive_tables](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md).

## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[Supervisión y solución de problemas de migración de datos](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) 
  
