---
title: sys.dm_pdw_nodes_exec_query_plan (Transact-SQL) | Microsoft Docs
description: Vista de administración dinámica que devuelve el plan de presentación en formato XML para el lote especificado por el identificador del plan. Este plan especificado por el identificador del plan puede estar almacenado en caché o ejecutándose.
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: f78f40922db54563206bfe4c9fb4966958b41840
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99141200"
---
# <a name="syspdw_nodes_dm_exec_query_plan-transact-sql"></a>sys.pdw_nodes_dm_exec_query_plan (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Devuelve el plan de presentación en formato XML para el lote especificado por el identificador del plan. Este plan especificado por el identificador del plan puede estar almacenado en caché o ejecutándose.  

> [!note] 
> En Synapse SQL, la adición de espacios en blanco en una consulta constituye un cambio de consulta que hace que el hash de consulta se vuelva a calcular y no se reutilice el plan de ejecución almacenado en caché anterior.


## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|IDENTIFICADOR numérico único asociado al nodo.| 
|**DBID**|**smallint**|Identificador de la base de datos de contexto que estaba activa al compilarse la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondiente a este plan. En el caso de instrucciones SQL no planeadas y preparadas, el identificador de la base de datos donde se compilaron las instrucciones.<br /><br /> Esta columna acepta valores NULL.|  
|**objectid**|**int**|Identificador del objeto (por ejemplo, procedimiento almacenado o función definida por el usuario) de este plan de consulta. Para lotes "ad hoc" y preparados, esta columna es **null**.<br /><br /> Esta columna acepta valores NULL.|  
|**número**|**smallint**|Entero de procedimiento almacenado numerado. Para lotes "ad hoc" y preparados, esta columna es **null**.<br /><br /> Esta columna acepta valores NULL.| 
|**cifra**|**bit**|Indica si el procedimiento almacenado correspondiente está cifrado.<br /><br /> 0 = no cifrado<br /><br /> 1 = cifrado<br /><br /> La columna no acepta valores NULL.|  
|**query_plan**|**xml**|Contiene la representación del plan de presentación de tiempo de compilación del plan de ejecución de consultas especificado con *plan_handle*. El plan de presentación está en formato XML. Se genera un plan para cada lote que contiene, por ejemplo, instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] "ad hoc", llamadas a procedimientos almacenados y llamadas a funciones definidas por el usuario.<br /><br /> Esta columna acepta valores NULL.|  
  
## <a name="remarks"></a>Observaciones  
Se aplican los mismos comentarios en [Sys.dm_exec_query_plan](./sys-dm-exec-query-plan-transact-sql.md) .  
  
## <a name="permissions"></a>Permisos  
 Requiere el rol de servidor **sysadmin** o `VIEW SERVER STATE` el permiso en el servidor.  
  
## <a name="see-also"></a>Vea también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

 ## <a name="next-steps"></a>Pasos siguientes
 Para obtener más sugerencias sobre desarrollo, consulte [información general sobre el desarrollo de Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop).
