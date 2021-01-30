---
description: sys.dm_exec_cursors (Transact-SQL)
title: sys.dm_exec_cursors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_cursors_TSQL
- dm_exec_cursors
- dm_exec_cursors_TSQL
- sys.dm_exec_cursors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_cursors dynamic management function
ms.assetid: f520b63c-36af-40f1-bf71-6901d6331d3d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 30e47feb0650e5e345c78f431f27bd0a953b31bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99106395"
---
# <a name="sysdm_exec_cursors-transact-sql"></a>sys.dm_exec_cursors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de los cursores abiertos en diversas bases de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
dm_exec_cursors (session_id | 0 )  
```  
  
## <a name="arguments"></a>Argumentos  
 *session_id* | 0,1  
 Id. de la sesión. Si se especifica *session_id* , esta función devuelve información acerca de los cursores de la sesión especificada.  
  
 Si se especifica 0, esta función devuelve información acerca de todos los cursores de todas las sesiones.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Id. de la sesión que alberga este cursor.|  
|**cursor_id**|**int**|Id. del objeto de cursor.|  
|**name**|**nvarchar(256)**|Nombre del cursor tal como lo ha definido el usuario.|  
|**properties**|**nvarchar(256)**|Especifica las propiedades del cursor. Los valores de las propiedades siguientes se concatenan para formar el valor de esta columna:<br />Interfaz de declaración<br />Tipo de cursor <br />Simultaneidad de cursor<br />Alcance del cursor<br />Nivel de anidamiento de cursor<br /><br /> Por ejemplo, el valor devuelto en esta columna podría ser "TSQL &#124; Dynamic &#124; optimistic &#124; global (0)".|  
|**sql_handle**|**varbinary (64)**|Identificador del texto del lote que declaró el cursor.|  
|**statement_start_offset**|**int**|Número de caracteres en el lote que se está ejecutando actualmente o procedimiento almacenado en el que se inicia la instrucción que se está ejecutando actualmente. Se puede usar junto con el **sql_handle**, el **statement_end_offset** y la función de administración dinámica [Sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) para recuperar la instrucción que se está ejecutando actualmente para la solicitud.|  
|**statement_end_offset**|**int**|Número de caracteres en el lote que se está ejecutando actualmente o procedimiento almacenado en el que finaliza la instrucción que se está ejecutando actualmente. Se puede usar junto con el **sql_handle**, el **statement_start_offset** y la función de administración dinámica **Sys.dm_exec_sql_text** para recuperar la instrucción que se está ejecutando actualmente para la solicitud.|  
|**plan_generation_num**|**bigint**|Número de secuencia que se puede usar para distinguir entre instancias de los planes después de una nueva compilación.|  
|**creation_time**|**datetime**|Marca de tiempo a la que se creó este cursor.|  
|**is_open**|**bit**|Especifica si el cursor está abierto.|  
|**is_async_population**|**bit**|Especifica si el subproceso en segundo plano aún está llenando asincrónicamente un cursor KEYSET o STATIC.|  
|**is_close_on_commit**|**bit**|Especifica si el cursor se declaró mediante CURSOR_CLOSE_ON_COMMIT.<br /><br /> 1 = El cursor se cerrará cuando finalice la transacción.|  
|**fetch_status**|**int**|Devuelve el estado de la última recopilación del cursor. Este es el último valor devuelto @FETCH_STATUS .|  
|**fetch_buffer_size**|**int**|Devuelve información acerca del tamaño del búfer de lectura.<br /><br /> 1 = Cursores Transact-SQL. Se puede establecer en un valor mayor para los cursores API.|  
|**fetch_buffer_start**|**int**|Para cursores FAST_FORWARD y DYNAMIC, devuelve 0 si el cursor no está abierto o si se ha colocado antes de la primera fila. De lo contrario, devuelve -1.<br /><br /> Para cursores STATIC y KEYSET, devuelve 0 si el cursor no está abierto y -1 si se ha colocado después de la última fila.<br /><br /> De lo contrario, devuelve el número de fila en la que se ha colocado.|  
|**ansi_position**|**int**|Posición del cursor en el búfer de lectura.|  
|**worker_time**|**bigint**|Tiempo empleado, en microsegundos, por los trabajadores que ejecutan este cursor.|  
|**Read**|**bigint**|Número de lecturas realizadas por el cursor.|  
|**graba**|**bigint**|Número de escrituras realizadas por el cursor.|  
|**dormant_duration**|**bigint**|Milisegundos desde que se inició la última consulta (abierta o capturada) en este cursor.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="remarks"></a>Observaciones  
 En la tabla siguiente se proporciona información acerca de la interfaz de la declaración del cursor y se incluyen los valores posibles para la columna de propiedades.  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|API|El cursor se declaró mediante una de las API de acceso a datos (ODBC, OLEDB).|  
|TSQL|El cursor se declaró mediante la sintaxis Transact-SQL DECLARE CURSOR.|  
  
 En la tabla siguiente se proporciona información acerca del tipo de cursor y se incluyen los valores posibles para la columna de propiedades.  
  
|Tipo|Descripción|  
|----------|-----------------|  
|Keyset|El cursor se ha declarado como de conjunto de claves.|  
|Dinámica|El cursor se ha declarado como dinámico.|  
|Instantánea|El cursor se ha declarado como instantánea o estático.|  
|Fast_Forward|El cursor se ha declarado como de avance rápido.|  
  
 En la tabla siguiente se proporciona información acerca de la simultaneidad de cursor y se incluyen los valores posibles para la columna de propiedades.  
  
|Simultaneidad|Description|  
|-----------------|-----------------|  
|Solo lectura|El cursor se ha declarado como de solo lectura.|  
|Scroll Locks|El cursor utiliza bloqueos de desplazamiento.|  
|Optimistic|El cursor utiliza control de simultaneidad optimista.|  
  
 En la tabla siguiente se proporciona información acerca del alcance del cursor y se incluyen los valores posibles para la columna de propiedades.  
  
|Ámbito|Descripción|  
|-----------|-----------------|  
|Local|Especifica que el alcance del cursor es local para el proceso por lotes, procedimiento almacenado o desencadenador en que se creó el cursor.|  
|Global|Especifica que el alcance del cursor es global para la conexión.|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-detecting-old-cursors"></a>A. Detectar cursores antiguos  
 En este ejemplo se devuelve información acerca de los cursores que llevan abiertos en el servidor más tiempo del especificado de 36 horas.  
  
```  
SELECT creation_time, cursor_id, name, c.session_id, login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s ON c.session_id = s.session_id   
WHERE DATEDIFF(hh, c.creation_time, GETDATE()) > 36;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con la ejecución &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
  

