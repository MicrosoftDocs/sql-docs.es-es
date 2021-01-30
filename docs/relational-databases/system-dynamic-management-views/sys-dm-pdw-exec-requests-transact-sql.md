---
description: sys.dm_pdw_exec_requests (Transact-SQL)
title: sys.dm_pdw_exec_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/05/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 390225cc-23e8-4051-a5f6-221e33e4c0b4
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 44dc108960c3fe8cda9e632802185f0472eb0373
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142684"
---
# <a name="sysdm_pdw_exec_requests-transact-sql"></a>sys.dm_pdw_exec_requests (Transact-SQL)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene información acerca de todas las solicitudes que se encuentran actualmente o que están activas recientemente en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Muestra una fila por solicitud o consulta.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|Clave para esta vista. IDENTIFICADOR numérico único asociado a la solicitud.|Único en todas las solicitudes del sistema.|  
|session_id|**nvarchar(32)**|IDENTIFICADOR numérico único asociado a la sesión en la que se ejecutó esta consulta. Vea [sys.dm_pdw_exec_sessions &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).||  
|status|**nvarchar(32)**|Estado actual de la solicitud.|"Running", "Suspended", "completed", "Canceled", "Failed".|  
|submit_time|**datetime**|Hora a la que se envió la solicitud para su ejecución.|**Fecha** y hora válida, menor o igual que la hora actual y start_time.|  
|start_time|**datetime**|Hora a la que se inició la ejecución de la solicitud.|NULL para las solicitudes en cola; de lo contrario, la **fecha y** hora válidas es menor o igual que la hora actual.|  
|end_compile_time|**datetime**|Hora a la que el motor completó la compilación de la solicitud.|NULL para las solicitudes que aún no se han compilado; de lo contrario, un **valor DateTime** válido menor que start_time y menor o igual que la hora actual.|
|end_time|**datetime**|Hora a la que se completó la ejecución de la solicitud, se produjo un error o se canceló.|Null para las solicitudes en cola o activas; de lo contrario, una **fecha y** hora válida menor o igual que la hora actual.|  
|total_elapsed_time|**int**|Tiempo transcurrido en la ejecución desde que se inició la solicitud, en milisegundos.|Entre 0 y la diferencia entre submit_time y end_time.</br></br> Si total_elapsed_time supera el valor máximo de un entero, total_elapsed_time seguirá siendo el valor máximo. Esta condición generará la advertencia "se ha superado el valor máximo".</br></br> El valor máximo en milisegundos es el mismo que 24,8 días.|  
|etiqueta|**nvarchar(255)**|Cadena de etiqueta opcional asociada a algunas instrucciones de consulta SELECT.|Cualquier cadena que contenga "a-z", "A-Z", "0-9", "_".|  
|error_id|**nvarchar (36)**|IDENTIFICADOR único del error asociado a la solicitud, si existe.|Vea [sys.dm_pdw_errors &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md); se establece en NULL si no se produjo ningún error.|  
|database_id|**int**|Identificador de la base de datos utilizada por el contexto explícito (por ejemplo, USE DB_X).|Vea ID en [Sys. databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).|  
|.|**nvarchar(4000)**|Contiene el texto completo de la solicitud enviado por el usuario.|Cualquier consulta o texto de solicitud válido. Las consultas que tienen más de 4000 bytes se truncan.|  
|resource_class|**nvarchar (20)**|Grupo de cargas de trabajo que se usa para esta solicitud. |Clases de recursos estáticos</br>staticrc10</br>staticrc20</br>staticrc30</br>staticrc40</br>staticrc50</br>staticrc60</br>staticrc70</br>staticrc80</br>            </br>Clases de recursos dinámicos</br>SmallRC</br>MediumRC</br>LargeRC</br>XLargeRC|
|importance|**nvarchar(128)**|La importancia del establecimiento de la solicitud ejecutada a las.  Esta es la importancia relativa de una solicitud en este grupo de cargas de trabajo y entre grupos de cargas de trabajo para los recursos compartidos.  La importancia especificada en el clasificador invalida la configuración de importancia del grupo de cargas de trabajo.</br>Se aplica a: Azure Synapse Analytics|NULL</br>low</br>below_normal</br>normal (valor predeterminado)</br>above_normal</br>high|
|group_name|**sysname** |En el caso de las solicitudes que usan recursos, group_name es el nombre del grupo de cargas de trabajo en el que se ejecuta la solicitud.  Si la solicitud no emplea recursos, group_name es NULL.</br>Se aplica a: Azure Synapse Analytics|
|classifier_name|**sysname**|Para las solicitudes que usan recursos, el nombre del clasificador utilizado para asignar recursos e importancia.||
|resource_allocation_percentage|**decimal (5, 2)**|La cantidad de recursos asignados a la solicitud.</br>Se aplica a: Azure Synapse Analytics|
|result_cache_hit|**int**|Detalla si una consulta completada ha utilizado caché de conjunto de resultados.  </br>Se aplica a: Azure Synapse Analytics| 1 = acierto de caché de conjunto de resultados </br> 0 = error de caché de conjunto de resultados </br> Valores enteros negativos = motivos por los que no se ha usado el almacenamiento en caché del conjunto de resultados.  Vea la sección Comentarios para obtener más información.|
|client_correlation_id|**nvarchar(255)**|Nombre opcional definido por el usuario para una sesión de cliente.  Para establecer para una sesión, llame a sp_set_session_context ' client_correlation_id ', ' <CorrelationIDName> '.  Ejecute `SELECT SESSION_CONTEXT(N'client_correlation_id')` para recuperar su valor.|
||||

## <a name="remarks"></a>Observaciones 
 Para obtener información acerca de las filas máximas retenidas en esta vista, consulte la sección de metadatos en el tema [límites de capacidad](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .

El valor entero negativo de la columna result_cache_hit es un valor de mapa de bits de todos los motivos aplicados por los que el conjunto de resultados de una consulta no se puede almacenar en caché.  Esta columna puede ser la [| (OR bit a bit)](../../t-sql/language-elements/bitwise-or-transact-sql.md) producto de uno o varios de los siguientes valores:  
  
|Value            |Descripción  |  
|-----------------|-----------------|  
|**1**|Acierto de caché de conjunto de resultados|  
|**0x00** (**0**)|Error de caché de conjunto de resultados|  
|-**0x01** (**-1**)|El almacenamiento en caché del conjunto de resultados está deshabilitado en la base de datos.|  
|-**0x02** (**-2**)|El almacenamiento en caché del conjunto de resultados está deshabilitado en la sesión. | 
|-**0x04** (**-4**)|El almacenamiento en caché del conjunto de resultados está deshabilitado debido a que no hay orígenes de datos para la consulta.|  
|-**0x08** (**-8**)|El almacenamiento en caché del conjunto de resultados está deshabilitado debido a predicados de seguridad de nivel de fila.|  
|-**0x10** (**-16**)|El almacenamiento en caché del conjunto de resultados está deshabilitado debido al uso de la tabla del sistema, la tabla temporal o la tabla externa en la consulta.|  
|-**0x20** (**-32**)|El almacenamiento en caché del conjunto de resultados está deshabilitado porque la consulta contiene constantes de tiempo de ejecución, funciones definidas por el usuario o funciones no deterministas.|  
|-**0x40**(**-64**)|El almacenamiento en caché del conjunto de resultados está deshabilitado porque el tamaño estimado del conjunto de resultados es >10 GB.|  
|-**0x80**(**-128**) |El almacenamiento en caché del conjunto de resultados está deshabilitado porque el conjunto de resultados contiene filas con un tamaño grande (>64 KB).|  
|-**0x100**(**-256**) |El almacenamiento en caché del conjunto de resultados está deshabilitado debido al uso del enmascaramiento dinámico de datos granular.|  

## <a name="permissions"></a>Permisos

 Requiere el permiso VIEW SERVER STATE.  
  
## <a name="security"></a>Seguridad

 sys.dm_pdw_exec_requests no filtra los resultados de la consulta según los permisos específicos de la base de datos. Los inicios de sesión con el permiso VIEW SERVER STATE pueden obtener resultados de consulta de resultados para todas las bases de datos  
  
>[!WARNING]  
>Un atacante puede usar sys.dm_pdw_exec_requests para recuperar información acerca de objetos de base de datos específicos con solo tener el permiso VIEW SERVER STATE y no tiene permiso específico de la base de datos.  
  
## <a name="see-also"></a>Consulte también

 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)
