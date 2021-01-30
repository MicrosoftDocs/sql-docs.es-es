---
description: sys.fn_virtualfilestats (Transact-SQL)
title: sys.fn_virtualfilestats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_virtualfilestats_TSQL
- fn_virtualfilestats
dev_langs:
- TSQL
helpviewer_keywords:
- I/O [SQL Server], statistics
- fn_virtualfilestats function
- sys.fn_virtualfilestats function
- statistical information [SQL Server], I/O
ms.assetid: 96b28abb-b059-48db-be2b-d60fe127f6aa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 755ec3806e72ab535588c6da15075ebf0beb4043
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187392"
---
# <a name="sysfn_virtualfilestats-transact-sql"></a>sys.fn_virtualfilestats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve las estadísticas de E/S de los archivos de las bases de datos, incluidos los archivos de registro. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esta información también está disponible en la vista de administración dinámica [Sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) .  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn_virtualfilestats ( { database_id | NULL } , { file_id | NULL } )  
```  
  
## <a name="arguments"></a>Argumentos  
 *database_id* | ACEPTA  
 Es el ID. de la base de datos. *database_id* es de tipo **int** y no tiene ningún valor predeterminado. Especifique NULL para devolver información de todas las bases de datos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *file_id* | ACEPTA  
 Identificador del archivo. *file_id* es de **tipo int** y no tiene ningún valor predeterminado. Especifique NULL para devolver información de todos los archivos de la base de datos.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DbId**|**smallint**|Id. de la base de datos.|  
|**FileId**|**smallint**|Identificador de archivo.|  
|**Indicaciones**|**bigint**|Marca de tiempo de la base de datos en la que se obtuvieron los datos. **int** en versiones anteriores a [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] . |  
|**NumberReads**|**bigint**|Número de operaciones de lectura realizadas en el archivo.|  
|**BytesRead**|**bigint**|Número de bytes leídos emitidos en el archivo.|  
|**IoStallReadMS**|**bigint**|Tiempo total, en milisegundos, que los usuarios han esperado para que finalicen las E/S de lectura en el archivo.|  
|**NumberWrites**|**bigint**|Número de operaciones de escritura realizadas en el archivo.|  
|**BytesWritten**|**bigint**|Número de bytes escritos en el archivo.|  
|**IoStallWriteMS**|**bigint**|Tiempo total, en milisegundos, que los usuarios han esperado para que finalicen las E/S de escritura en el archivo.|  
|**IoStallMS**|**bigint**|Suma de **IoStallReadMS** y **IoStallWriteMS**.|  
|**FileHandle**|**bigint**|Valor del identificador de archivos.|  
|**BytesOnDisk**|**bigint**|Tamaño físico del archivo (recuento de bytes) en disco.<br /><br /> En el caso de los archivos de base de datos, es el mismo valor que el **tamaño** en **Sys.database_files**, pero se expresa en bytes en lugar de en páginas.<br /><br /> En los archivos dispersos de instantáneas de base de datos, se trata del espacio que utiliza el sistema operativo para el archivo.|  
  
## <a name="remarks"></a>Observaciones  
 **fn_virtualfilestats** es una función con valores de tabla del sistema que proporciona información estadística, como el número total de operaciones de e/s realizadas en un archivo. Puede utilizarse esta función como ayuda para realizar un seguimiento del tiempo que los usuarios tienen que esperar para leer un archivo o escribir en él. La función también ayuda a identificar los archivos que tienen mucha actividad de E/S.  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-statistical-information-for-a-database"></a>A. Mostrar información estadística de una base de datos  
 En el siguiente ejemplo se muestra información estadística del archivo con Id. 1 de la base de datos con un Id. de `1`.  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(1, 1);  
GO  
```  
  
### <a name="b-displaying-statistical-information-for-a-named-database-and-file"></a>B. Mostrar información estadística de una base de datos y un archivo con nombre  
 En el siguiente ejemplo se muestra información estadística del archivo de registro de la base de datos de ejemplo [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La función del sistema `DB_ID` se usa para especificar el parámetro *database_id* .  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="c-displaying-statistical-information-for-all-databases-and-files"></a>C. Mostrar información estadística de todas las bases de datos y todos los archivos  
 En el siguiente ejemplo se muestra información estadística de todos los archivos de todas las bases de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(NULL,NULL);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [DB_ID &#40;&#41;de Transact-SQL ](../../t-sql/functions/db-id-transact-sql.md)   
 [FILE_IDEX &#40;Transact-SQL&#41;](../../t-sql/functions/file-idex-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
