---
description: sys.dm_repl_schemas (Transact-SQL)
title: sys.dm_repl_schemas (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_repl_schemas_TSQL
- dm_repl_schemas
- sys.dm_repl_schemas_TSQL
- sys.dm_repl_schemas
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_schemas dynamic management function
ms.assetid: 6f5fefff-8492-4360-bd5b-a97287367914
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5c2791dbc87997b3e2afcde966ed82b39384798d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99137772"
---
# <a name="sysdm_repl_schemas-transact-sql"></a>sys.dm_repl_schemas (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de columnas de tablas publicadas por la replicación.  
  
 
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**artcache_schema_address**|**varbinary(8**|Dirección de memoria de la estructura del esquema en caché de la tabla de artículos publicada.|  
|**tabid**|**bigint**|Id. de la tabla replicada.|  
|**indexid**|**smallint**|Id. de un índice clúster en la tabla publicada.|  
|**idSch**|**bigint**|Id. del esquema de tabla.|  
|**tabschema**|**nvarchar (510)**|Nombre del esquema de tabla.|  
|**ccTabschema**|**smallint**|Longitud en caracteres del esquema de tabla.|  
|**tabname**|**nvarchar (510)**|Nombre de la tabla publicada.|  
|**ccTabname**|**smallint**|Longitud en caracteres del nombre de la tabla publicada.|  
|**rowsetid_delete**|**bigint**|Id. de la fila eliminada.|  
|**rowsetid_insert**|**bigint**|Id. de la fila insertada.|  
|**num_pk_cols**|**int**|Número de columnas de clave principal.|  
|**pcitee**|**binario (8000)**|Puntero a la estructura de expresión de consulta usada para evaluar la columna calculada.|  
|**re_numtextcols**|**int**|Número de columnas de objetos binarios grandes en la tabla replicada.|  
|**re_schema_lsn_begin**|**binario (8000)**|Inicio del número de secuencia del registro (LSN) del registro de versiones del esquema.|  
|**re_schema_lsn_end**|**binario (8000)**|Fin del LSN del registro de versiones del esquema.|  
|**re_numcols**|**int**|Número de columnas publicadas.|  
|**re_colid**|**int**|Identificador de columna en el publicador.|  
|**re_awcName**|**nvarchar (510)**|Nombre de la columna publicada.|  
|**re_ccName**|**smallint**|Número de caracteres en el nombre de columna.|  
|**re_pk**|**tinyint**|Especifica si la columna publicada forma parte de una clave principal.|  
|**re_unique**|**tinyint**|Especifica si la columna publicada forma parte de un índice único.|  
|**re_maxlen**|**smallint**|Longitud máxima de la columna publicada.|  
|**re_prec**|**tinyint**|Precisión de la columna publicada.|  
|**re_scale**|**tinyint**|Escala de la columna publicada.|  
|**re_collatid**|**bigint**|Id. de intercalación de la columna publicada.|  
|**re_xvtype**|**smallint**|Tipo de la columna publicada.|  
|**re_offset**|**smallint**|Desplazamiento de la columna publicada.|  
|**re_bitpos**|**tinyint**|Posición de bit de la columna publicada, en el vector de bytes.|  
|**re_fNullable**|**tinyint**|Especifica si la columna publicada admite valores NULL.|  
|**re_fAnsiTrim**|**tinyint**|Especifica si se utiliza el recorte ANSI en la columna publicada.|  
|**re_computed**|**smallint**|Especifica si la columna publicada es una columna calculada.|  
|**se_rowsetid**|**bigint**|Id. del conjunto de filas.|  
|**se_schema_lsn_begin**|**binario (8000)**|Inicio del LSN del registro de versiones del esquema.|  
|**se_schema_lsn_end**|**binario (8000)**|Fin del LSN del registro de versiones del esquema.|  
|**se_numcols**|**int**|Número de columnas.|  
|**se_colid**|**int**|Id. de la columna en el suscriptor.|  
|**se_maxlen**|**smallint**|Longitud máxima de la columna.|  
|**se_prec**|**tinyint**|Precisión de la columna.|  
|**se_scale**|**tinyint**|Escala de la columna.|  
|**se_collatid**|**bigint**|Id. de intercalación de la columna.|  
|**se_xvtype**|**smallint**|Tipo de la columna.|  
|**se_offset**|**smallint**|Desplazamiento de la columna.|  
|**se_bitpos**|**tinyint**|Posición de bit de la columna, en el vector de bytes.|  
|**se_fNullable**|**tinyint**|Especifica si la columna admite valores NULL.|  
|**se_fAnsiTrim**|**tinyint**|Especifica si se utiliza el recorte ANSI en la columna.|  
|**se_computed**|**smallint**|Especifica si la columna es una columna calculada.|  
|**se_nullBitInLeafRows**|**int**|Especifica si el valor de la columna es NULL.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW DATABASE STATE en la base de datos de publicaciones para llamar a **dm_repl_schemas**.  
  
## <a name="remarks"></a>Observaciones  
 La instancia solo se devuelve para objetos de la base de datos replicada que está cargada actualmente en la caché del artículo de replicación.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con la replicación &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)  
  
  

