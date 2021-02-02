---
description: sys.dm_db_page_info (Transact-SQL)
title: sys.dm_db_page_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/18/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_page_info
- sys.dm_db_page_info_TSQL
- dm_db_page_info
- dm_db_page_info_TSQL
- dbcc page
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_page_info dynamic management view
author: bluefooted
ms.author: pamela
manager: amitban
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 10756b0297cdc731468ea8983c5378d5b380fef9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236015"
---
# <a name="sysdm_db_page_info-transact-sql"></a>sys.dm_db_page_info (Transact-SQL)

[!INCLUDE[tsql-appliesto-ssver15-asdb-xxxx-xxx](../../includes/tsql-appliesto-ssver15-asdb-xxxx-xxx.md)]

Devuelve información sobre una página de una base de datos.  La función devuelve una fila que contiene la información de encabezado de la página, incluidos `object_id` , `index_id` y `partition_id` .  Esta función reemplaza la necesidad de usar `DBCC PAGE` en la mayoría de los casos.

> [!NOTE]
> `sys.dm_db_page_info` Actualmente solo se admite en [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.


## <a name="syntax"></a>Sintaxis   
```  
sys.dm_db_page_info ( DatabaseId, FileId, PageId, Mode )  
``` 

## <a name="arguments"></a>Argumentos  
*DatabaseId* | NULL | PREDETERMINADA     
Es el ID. de la base de datos. *DatabaseId* es **smallint**. Una entrada válida es el número de identificación de una base de datos. El valor predeterminado es NULL; sin embargo, si se envía un valor NULL para este parámetro, se producirá un error.
 
*FileId* | NULL | PREDETERMINADA   
Identificador del archivo. *FileId* es de **tipo int**.  Entrada válida es el número de identificación de un archivo en la base de datos especificada por *DatabaseId*. El valor predeterminado es NULL; sin embargo, si se envía un valor NULL para este parámetro, se producirá un error.

*PageId* | NULL | PREDETERMINADA   
Es el identificador de la página.  *PageId* es de **tipo int**.  Entrada válida es el número de identificación de una página en el archivo especificado por *FileId*. El valor predeterminado es NULL; sin embargo, si se envía un valor NULL para este parámetro, se producirá un error.

*Modo* | NULL | PREDETERMINADA   
Determina el nivel de detalle de la salida de la función. ' LIMITED ' devolverá valores NULL para todas las columnas de Descripción; ' Detailed ' rellenará las columnas de descripción.  El valor predeterminado es ' LIMITED '.

## <a name="table-returned"></a>Tabla devuelta  

|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|database_id |int |Identificador de base de datos |
|file_id |int |Id. de archivo |
|page_id |int |Identificador de página |
|page_header_version |int |Versión del encabezado de página |
|page_type |int |Tipo de página |
|page_type_desc |nvarchar (64) |Descripción del tipo de página |
|page_type_flag_bits |nvarchar (64) |Escribir bits de marca en encabezado de página |
|page_type_flag_bits_desc |nvarchar (64) |Tipo de marca bits Descripción en encabezado de página |
|page_flag_bits |nvarchar (64) |Marcar bits en encabezado de página |
|page_flag_bits_desc |nvarchar(256) |Descripción de bits de marca en encabezado de página |
|page_lsn |nvarchar (64) |Número de secuencia de registro/marca de tiempo |
|page_level |int |Nivel de la página en el índice (hoja = 0) |
|object_id |int |IDENTIFICADOR del objeto propietario de la página |
|index_id |int |IDENTIFICADOR del índice (0 para las páginas de datos del montón) |
|partition_id |bigint |IDENTIFICADOR de la partición |
|alloc_unit_id |bigint |IDENTIFICADOR de la unidad de asignación |
|is_encrypted |bit |Bit que indica si la página está cifrada o no |
|has_checksum |bit |Bit que indica si la página tiene o no un valor de suma de comprobación |
|suma de comprobación |int |Almacena el valor de suma de comprobación que se usa para detectar daños en los datos |
|is_iam_pg |bit |Bit que indica si la página es o no una página IAM  |
|is_mixed_ext |bit |Bit que indica si se asigna en una extensión mixta |
|has_ghost_records |bit |Bit que indica si la página contiene registros fantasma <br> Un registro fantasma es aquel que se ha marcado para su eliminación, pero que aún no se ha quitado.|
|has_version_records |bit |Bit que indica si la página contiene registros de versión que se usan para [acelerar la recuperación de bases de datos](../backup-restore/restore-and-recovery-overview-sql-server.md#adr) |
|pfs_page_id |int |ID. de página de la página PFS correspondiente |
|pfs_is_allocated |bit |Bit que indica si la página está marcada como asignada en la página PFS correspondiente |
|pfs_alloc_percent |int |Porcentaje de asignación indicado por el byte PFS correspondiente |
|pfs_status |nvarchar (64) |Byte PFS |
|pfs_status_desc |nvarchar (64) |Descripción del byte PFS |
|gam_page_id |int |ID. de página de la página GAM correspondiente |
|gam_status |bit |Bit que indica si se ha asignado en GAM |
|gam_status_desc |nvarchar (64) |Descripción del bit de estado de GAM |
|sgam_page_id |int |ID. de página de la página SGAM correspondiente |
|sgam_status |bit |Bit que indica si se ha asignado en SGAM |
|sgam_status_desc |nvarchar (64) |Descripción del bit de estado de SGAM |
|diff_map_page_id |int |ID. de página de la página de mapa de bits diferencial correspondiente |
|diff_status |bit |Bit que indica si se ha cambiado el estado de diferencia |
|diff_status_desc |nvarchar (64) |Descripción del bit de estado de diferencia |
|ml_map_page_id |int |ID. de página de la página de mapa de bits de registro mínima correspondiente |
|ml_status |bit |Bit que indica si la página tiene un registro mínimo |
|ml_status_desc |nvarchar (64) |Descripción del bit de estado de registro mínimo |
|prev_page_file_id |SMALLINT |ID. de archivo de página anterior |
|prev_page_page_id |int |ID. de página de página anterior |
|next_page_file_id |SMALLINT |IDENTIFICADOR de archivo de página siguiente |
|next_page_page_id |int |IDENTIFICADOR de página de la página siguiente |
|fixed_length |SMALLINT |Longitud de filas de tamaño fijo |
|slot_count |SMALLINT |Número total de ranuras (usadas y sin usar) <br> En el caso de una página de datos, este número es equivalente al número de filas. |
|ghost_rec_count |SMALLINT |Número de registros marcados como fantasma en la página <br> Un registro fantasma es aquel que se ha marcado para su eliminación, pero que aún no se ha quitado. |
|free_bytes |SMALLINT |Número de bytes libres en la página |
|free_data_offset |int |Desplazamiento de espacio disponible al final del área de datos |
|reserved_bytes |SMALLINT |Número de bytes libres reservados por todas las transacciones (si es montón) <br> Número de filas fantasma (si es hoja del índice) |
|reserved_bytes_by_xdes_id |SMALLINT |Espacio aportado por m_xdesID a m_reservedCnt <br> Solo con fines de depuración |
|xdes_id |nvarchar (64) |Última transacción aportada por m_reserved <br> Solo con fines de depuración |
||||

## <a name="remarks"></a>Notas
La `sys.dm_db_page_info` función de administración dinámica devuelve información de página como `page_id` , `file_id` , `index_id` , `object_id` etc. que se encuentra en un encabezado de página. Esta información es útil para la solución de problemas y la depuración de diversos problemas de rendimiento (contención de bloqueos y bloqueos temporales) y daños.

`sys.dm_db_page_info` se puede usar en lugar de la `DBCC PAGE` instrucción en muchos casos, pero solo devuelve la información del encabezado de página, no el cuerpo de la página. `DBCC PAGE` seguirá siendo necesario para los casos de uso en los que se requiera todo el contenido de la página.

## <a name="using-in-conjunction-with-other-dmvs"></a>Usar junto con otras DMV
Uno de los casos de uso más importantes de `sys.dm_db_page_info` es combinarlo con otras DMV que exponen información de página.  Para facilitar este caso de uso, se ha agregado una nueva columna denominada `page_resource` que expone la información de la página en un formato hexadecimal de 8 bytes. Esta columna se ha agregado a `sys.dm_exec_requests` y `sys.sysprocesses` y se agregará a otras DMV en el futuro según sea necesario.

Una nueva función, `sys.fn_PageResCracker` , toma `page_resource` como entrada y genera una sola fila que contiene `database_id` , `file_id` y `page_id` .  Esta función se puede utilizar después para facilitar combinaciones entre `sys.dm_exec_requests` o `sys.sysprocesses` y `sys.dm_db_page_info` .

## <a name="permissions"></a>Permisos  
Requiere el `VIEW DATABASE STATE` permiso en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-all-the-properties-of-a-page"></a>A. Mostrar todas las propiedades de una página
La siguiente consulta devuelve una fila con toda la información de la página para `database_id` una `file_id` combinación determinada,, con el `page_id` modo predeterminado (' Limited ')

```sql
SELECT *  
FROM sys.dm_db_page_info (5, 1, 15, DEFAULT)
```

### <a name="b-using-sysdm_db_page_info-with-other-dmvs"></a>B. Usar sys.dm_db_page_info con otras DMV 

La siguiente consulta devuelve una fila por `wait_resource` exposición `sys.dm_exec_requests` cuando la fila contiene un valor distinto de NULL. `page_resource`

```sql
SELECT page_info.* 
FROM sys.dm_exec_requests AS d  
CROSS APPLY sys.fn_PageResCracker (d.page_resource) AS r  
CROSS APPLY sys.dm_db_page_info(r.db_id, r.file_id, r.page_id, 'LIMITED') AS page_info
```

## <a name="see-also"></a>Consulte también  
[Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)     
[sys.fn_PageResCracker](../../relational-databases/system-functions/sys-fn-pagerescracker-transact-sql.md)


