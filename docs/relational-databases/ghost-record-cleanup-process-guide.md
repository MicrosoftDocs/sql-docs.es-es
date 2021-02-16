---
title: Guía de procesos de limpieza de registros fantasma | Microsoft Docs
description: Obtenga información sobre el proceso de limpieza de registros fantasma, un proceso en segundo plano que elimina los registros de las páginas que se han marcado para su eliminación en SQL Server.
ms.custom: ''
ms.date: 05/02/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- ghost cleanup
- ghost records
- ghost clean up process
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e228afb793e69ab7b9d36fae31e838f6d1bca15a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059230"
---
# <a name="ghost-cleanup-process-guide"></a>Guía de procesos de limpieza de registros fantasma

El proceso de limpieza de registros fantasma es un proceso en segundo plano de un único subproceso que elimina los registros de las páginas que se han marcado para su eliminación. En la ilustración siguiente se muestra una visión general de este proceso.

## <a name="ghost-records"></a>Registros fantasma

Los registros que se eliminan de un nivel hoja de una página de índice no se quitan físicamente de la página; en su lugar, el registro se marca como "para eliminar" o *fantasma*. Esto significa que la fila se conserva en la página pero se cambia una parte del encabezado de fila para indicar que la fila es realmente un fantasma. Esto sirve para optimizar el rendimiento durante una operación de eliminación. Los fantasmas son necesarios para el bloqueo de nivel de fila, pero también para el aislamiento de instantánea en el que es necesario mantener las versiones anteriores de las filas.

## <a name="ghost-record-cleanup-task"></a>Tarea de limpieza de registros fantasma

Los registros que están marcados para eliminación, o como *fantasma*, se limpian mediante el proceso de limpieza de registros fantasma en segundo plano. Este proceso en segundo plano se ejecuta en algún momento después de confirmar la transacción de eliminación y elimina físicamente los registros fantasma de las páginas. El proceso de limpieza de registros fantasma se ejecuta automáticamente en un intervalo (cada 5 segundos para SQL Server 2012 + y cada 10 segundos para SQL Server 2008/2008 R2) y comprueba si alguna página se ha marcado con registros fantasma. Si encuentra alguna, continúa y elimina los registros que están marcados para eliminación, o como *fantasma*, con un alcance máximo de 10 páginas con cada ejecución.

Cuando un registro es fantasma, la base de datos se marca con entradas fantasma, y el proceso de limpieza de registros fantasma solo examinará esas bases de datos. El proceso de limpieza de registros fantasma también marcará la base de datos como "sin registros fantasma" una vez que se hayan eliminado todos los registros fantasma, y omitirá esta base de datos la próxima vez que se ejecute. El proceso también omitirá las bases de datos en las que no pueda usar un bloqueo compartido y lo intentará de nuevo la próxima vez que se ejecute.

La consulta siguiente puede identificar el número de registros fantasma que existen en una sola base de datos. 

 ```sql
 SELECT sum(ghost_record_count) total_ghost_records, db_name(database_id) 
 FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, 'SAMPLED')
 group by database_id
 order by total_ghost_records desc
```

## <a name="disable-the-ghost-cleanup"></a>Deshabilitar la limpieza de registros fantasma

En sistemas de carga alta con muchas operaciones de eliminación, el proceso de limpieza de registros fantasma puede provocar un problema de rendimiento al conservar las páginas en el grupo de búferes y generar E/S. Por tanto, este proceso se puede deshabilitar mediante la [marca de seguimiento 661](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Pero la deshabilitación del proceso tiene implicaciones en el rendimiento.

Deshabilitar el proceso de limpieza de registros fantasma puede hacer que la base de datos crezca de forma innecesaria y se produzcan problemas de rendimiento. Como el proceso de limpieza de registros fantasma quita los registros que se marcan como fantasma, deshabilitar el proceso dejará estos registros en la página, lo que impide que SQL Server vuelva a usar este espacio. Esto obliga a SQL Server a agregar datos a las páginas nuevas en su lugar, lo que da lugar a archivos de base de datos sobredimensionados y también [divisiones de página](indexes/specify-fill-factor-for-an-index.md). Las divisiones de páginas generan problemas de rendimiento al crear planes de ejecución y al realizar operaciones de análisis. 

Una vez que se deshabilita el proceso de limpieza de registros fantasma, es necesario realizar alguna acción para quitar los registros fantasmas. Una opción consiste en ejecutar una regeneración de índice, que desplazará los datos en las páginas. Otra opción consiste en ejecutar manualmente [sp_clean_db_free_space](system-stored-procedures/sp-clean-db-free-space-transact-sql.md) (para limpiar todos los archivos de datos de base de datos) o [sp_clean_db_file_free_space](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md) (para limpiar un único archivo de datos de base de datos), lo que eliminará los registros fantasma.

 >[!warning]
 > Por lo general, no se recomienda deshabilitar el proceso de limpieza de registros fantasma. Si se hace, se debe probar minuciosamente en un entorno controlado antes de implementarlo de forma permanente en un entorno de producción.


## <a name="next-steps"></a>Pasos siguientes  
[Deshabilitar el proceso de limpieza de registros fantasma](https://support.microsoft.com/help/920093/tuning-options-for-sql-server-when-running-in-high-performance-workloa)
<br>[Quitar los registros fantasma de un único archivo de base de datos](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)
<br>[Quitar los registros fantasma de todos los archivos de datos de la base de datos](system-stored-procedures/sp-clean-db-free-space-transact-sql.md)


