---
description: sys.dm_exec_xml_handles (Transact-SQL)
title: sys.dm_exec_xml_handles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_xml_handles
- dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles_TSQL
- sys.dm_exec_xml_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_xml_handles dynamic management function
ms.assetid: a873ce0f-6955-417a-96a1-b2ef11a83633
author: pmasl
ms.author: pelopes
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3e8529f7626ffa25d86cbb73c704198082e32bcf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208054"
---
# <a name="sysdm_exec_xml_handles-transact-sql"></a>sys.dm_exec_xml_handles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Devuelve información sobre los identificadores activos abiertos por **sp_xml_preparedocument**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
dm_exec_xml_handles (session_id | 0 )  
```  
  
## <a name="arguments"></a>Argumentos  
 *session_id* | 0,1  
 Id. de la sesión. Si se especifica *session_id* , esta función devuelve información acerca de los identificadores XML en la sesión especificada.  
  
 Si se especifica 0, esta función devuelve información acerca de todos los identificadores XML de todas las sesiones.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Id. de la sesión que contiene este identificador del documento XML.|  
|**document_id**|**int**|IDENTIFICADOR de identificador de documento XML devuelto por **sp_xml_preparedocument**.|  
|**namespace_document_id**|**int**|IDENTIFICADOR de identificador interno usado para el documento de espacio de nombres asociado que se ha pasado como el tercer parámetro a **sp_xml_preparedocument**. Es NULL si no hay ningún documento de espacio de nombres.|  
|**sql_handle**|**varbinary (64)**|Identificador del texto del código SQL en el que se ha definido el identificador.|  
|**statement_start_offset**|**int**|Número de caracteres en el lote que se está ejecutando actualmente o en el procedimiento almacenado en el que se produce la llamada **sp_xml_preparedocument** . Se puede usar junto con el **sql_handle**, el **statement_end_offset** y la función de administración dinámica **Sys.dm_exec_sql_text** para recuperar la instrucción que se está ejecutando actualmente para la solicitud.|  
|**statement_end_offset**|**int**|Número de caracteres en el lote que se está ejecutando actualmente o en el procedimiento almacenado en el que se produce la llamada **sp_xml_preparedocument** . Se puede usar junto con el **sql_handle**, el **statement_start_offset** y la función de administración dinámica **Sys.dm_exec_sql_text** para recuperar la instrucción que se está ejecutando actualmente para la solicitud.|  
|**creation_time**|**datetime**|Marca de tiempo cuando se llamó a **sp_xml_preparedocument** .|  
|**original_document_size_bytes**|**bigint**|Tamaño del documento XML no analizado, en bytes.|  
|**original_namespace_document_size_bytes**|**bigint**|Tamaño del documento de espacio de nombres XML no analizado, en bytes. Es NULL si no hay ningún documento de espacio de nombres.|  
|**num_openxml_calls**|**bigint**|Número de llamadas OPENXML para este identificador de documento.|  
|**row_count**|**bigint**|Número de filas devueltas por todas las llamadas OPENXML anteriores para este identificador de documento.|  
|**dormant_duration_ms**|**bigint**|Milisegundos desde la última llamada OPENXML. Si no se ha llamado a OPENXML, devuelve milisegundos desde la llamada a **sp_xml_preparedocumen** t.|  
  
## <a name="remarks"></a>Observaciones  
 La duración de **sql_handles** utilizada para recuperar el texto SQL que ejecutó una llamada a **sp_xml_preparedocument** el plan almacenado en caché que se usa para ejecutar la consulta. Si el texto de la consulta no está disponible en la memoria caché, los datos no pueden recuperarse usando la información proporcionada en el resultado de la función. Esto puede ocurrir si está ejecutando muchos lotes grandes.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW SERVER STATE en el servidor para ver todas las sesiones o Id. de sesión que no son propiedad del autor de la llamada. El autor de la llamada siempre puede ver los datos de su propio Id. de sesión actual.      
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se seleccionan todos los identificadores activos.  
  
```  
SELECT * FROM sys.dm_exec_xml_handles(0);  
```  
  
## <a name="see-also"></a>Consulte también  
 <br>[Funciones y vistas de administración dinámica (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
 <br>[Funciones y vistas de administración dinámica relacionadas con la ejecución (Transact-SQL)](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
 <br>[sp_xml_preparedocument (Transact-SQL)](../system-stored-procedures/sp-xml-preparedocument-transact-sql.md)
 <br>[sp_xml_removedocument (Transact-SQL)](../system-stored-procedures/sp-xml-removedocument-transact-sql.md)


 
  
  
