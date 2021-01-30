---
description: sp_filestream_force_garbage_collection (Transact-SQL)
title: sp_filestream_force_garbage_collection (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_filestream_force_garbage_collection
- sp_filestream_force_garbage_collection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FILESTREAM [SQL Server]
- sp_filestream_force_garbage_collection
ms.assetid: 9d1efde6-8fa4-42ac-80e5-37456ffebd0b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0c234c5c03b00b25babbec0a4fbc197f235c966d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165247"
---
# <a name="sp_filestream_force_garbage_collection-transact-sql"></a>sp_filestream_force_garbage_collection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fuerza la ejecución del recolector de elementos no utilizados de FILESTREAM eliminando los archivos FILESTREAM innecesarios.  
  
 Un contenedor de FILESTREAM no se puede quitar hasta que el recolector de elementos no utilizados ha limpiado todos los archivos eliminados que contiene. El recolector de elementos no utilizados de FILESTREAM se ejecuta automáticamente. Sin embargo, si necesita quitar un contenedor antes de que se haya ejecutado el recolector de elementos no utilizados, puede usar sp_filestream_force_garbage_collection para ejecutar el recolector de elementos no utilizados manualmente.  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_filestream_force_garbage_collection
    [ @dbname = ]  'database_name'
    [ , [ @filename = ] 'logical_file_name' ]
```  
  
## <a name="arguments"></a>Argumentos  
 `[ @dbname = ]  'database_name'`  
 Indica el nombre de la base de datos en la que se debe ejecutar el recolector de elementos no utilizados.  
  
> [!NOTE]  
> `@dbname` es **sysname**. Si no se especifica, se supone que es la base de datos actual.  
  
 `[ @filename = ] 'logical_file_name'`  
 Especifica el nombre lógico del contenedor de FILESTREAM en el que se va a ejecutar el recolector de elementos no utilizados. `@filename` es opcional. Si no se especifica ningún nombre de archivo lógico, el recolector de elementos no utilizados limpia todos los contenedores de FILESTREAM de la base de datos especificada.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
  
| Value | Descripción |
| ----- | ----------- |   
|0|Operación correcta|  
|1|Error de operación|  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Value|Descripción|  
|-----------|-----------------|  
|*file_name*|Indica el nombre del contenedor de FILESTREAM|  
|*num_collected_items*|Indica el número de elementos de FILESTREAM (archivos o directorios) que el recolector de elementos eliminados ha eliminado en este contenedor.|  
|*num_marked_for_collection_items*|Indica el número de elementos de FILESTREAM (archivos o directorios) que se han marcado para la recolección de elementos no utilizados en este contenedor. Estos elementos todavía no se han eliminado, pero pueden ser válidos para su eliminación después de la fase de recolección de elementos no utilizados.|  
|*num_unprocessed_items*|Indica el número de elementos de FILESTREAM (archivos o directorios) que se podían haber eliminado y no se procesaron para la recolección de elementos no utilizados en este contenedor de FILESTREAM. Los elementos pueden no haberse procesado por diversas razones, por ejemplo:<br /><br /> Archivos que se tienen que anclar porque no se ha tomado la copia de seguridad de registros o CHECKPOINT.<br /><br /> Archivos del modelo de recuperación FULL o BULK_LOGGED.<br /><br /> Hay una transacción activa de ejecución prolongada.<br /><br /> No se ha ejecutado el trabajo del lector del registro de replicación. Consulte las notas del producto [almacenamiento FileStream en SQL Server 2008](/previous-versions/sql/sql-server-2008/hh461480(v=msdn.10)) para obtener más información.|  
|*last_collected_xact_seqno*|Devuelve el último número de secuencia de registro (LSN) correspondiente hasta el que el recolector de elementos no utilizados ha recolectado archivos del contenedor de FILESTREAM especificado.|  
  
## <a name="remarks"></a>Observaciones  
 Ejecuta explícitamente la tarea del recolector de elementos no utilizados de FILESTREAM hasta su finalización en la base de datos solicitada (y el contenedor de FILESTREAM). El proceso de recolección de elementos no utilizados quita los archivos que ya no se necesitan. El tiempo necesario para completar esta operación depende del tamaño de los datos de FILESTREAM en la base de datos o el contenedor, así como de la cantidad de actividad DML que se ha producido recientemente en los datos de FILESTREAM. Si bien esta operación se puede ejecutar mientras la base de datos está en línea, el rendimiento de la base de datos puede verse afectado durante la ejecución como consecuencia de diversas actividades de I/O que el proceso de recolección de elementos no utilizados lleva a cabo.  
  
> [!NOTE]  
>  Se recomienda que esta operación solo se ejecute cuando sea necesario y fuera las horas de actividad normales.  
  
Solo se pueden ejecutar simultáneamente varias invocaciones de este procedimiento almacenado en contenedores o bases de datos independientes.  

Debido a las operaciones en dos fases, el procedimiento almacenado se debe ejecutar dos veces para eliminar realmente los archivos FileStream subyacentes.  

La recolección de elementos no utilizados (GC) se basa en el truncamiento del registro. Por lo tanto, si los archivos se eliminaron recientemente en una base de datos mediante el modelo de recuperación completa, son GC-Ed solo después de que se realice una copia de seguridad de registros de esas partes del registro de transacciones y la parte del registro se marque como inactiva. En una base de datos que usa el modelo de recuperación simple, se produce un truncamiento del registro después de que se haya `CHECKPOINT` emitido en la base de datos.  


## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol de la base de datos db_owner.  
  
## <a name="examples"></a>Ejemplos  
 En los siguientes ejemplos se ejecuta el recolector de elementos no utilizados para contenedores de FILESTREAM de la base de datos `FSDB`.  
  
### <a name="a-specifying-no-container"></a>A. Sin especificar ningún contenedor  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB';  
```  
  
### <a name="b-specifying-a-container"></a>B. Especificando un contenedor  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB',
    @filename = N'FSContainer';  
```  
  
## <a name="see-also"></a>Consulte también  
[Secuencia de archivos](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetables](../../relational-databases/blob/filetables-sql-server.md)
<br>[Vistas de administración dinámica de secuencia de archivo y FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Vistas de catálogo de secuencia de archivo y FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[sp_kill_filestream_non_transacted_handles (Transact-SQL)](filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md)
  
