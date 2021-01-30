---
description: sys.fn_trace_getinfo (Transact-SQL)
title: sys.fn_trace_getinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_trace_getinfo
- fn_trace_getinfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- traces [SQL Server], status information
- status information [SQL Server], traces
- sys.fn_trace_getinfo function
- fn_trace_getinfo function
ms.assetid: 04b140fe-110a-47b8-98b5-e4c161beb6c9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dae691a749a91e00276dd74477b35f7916b76618
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205346"
---
# <a name="sysfn_trace_getinfo-transact-sql"></a>sys.fn_trace_getinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de un seguimiento especificado o de todas los seguimientos existentes.  
  
> **IMPORTANTE:** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use eventos extendidos en su lugar.    
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_trace_getinfo ( { trace_id | NULL | 0 | DEFAULT } )  
```  
  
## <a name="arguments"></a>Argumentos  
 *trace_id*  
 Es el identificador de seguimiento. *trace_id* es de **tipo int**.  Las entradas válidas son el número de identificación de un seguimiento, NULL, 0 o DEFAULT. NULL, 0 y DEFAULT son valores equivalentes en este contexto. Especifique NULL, 0 o DEFAULT para devolver información de todos los seguimientos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="tables-returned"></a>Tablas devueltas  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|traceid|**int**|IDENTIFICADOR del seguimiento.|  
|propiedad|**int**|Propiedad del seguimiento:<br /><br /> 1= Opciones de seguimiento. Para obtener más información, vea @options en [sp_trace_create &#40;&#41;de TRANSACT-SQL ](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md).<br /><br /> 2 = Nombre de archivo<br /><br /> 3 = Tamaño máximo<br /><br /> 4 = Hora de detención<br /><br /> 5 = Estado actual del seguimiento. 0 = detenido. 1 = en ejecución.|  
|value|**sql_variant**|Información acerca de la propiedad de seguimiento especificado.|  
  
## <a name="remarks"></a>Observaciones  
 Cuando se ha pasado el identificador de un seguimiento específico, fn_trace_getinfo devuelve información acerca del seguimiento. Si se pasa un Id. no válido, esta función devuelve un conjunto de filas vacío.  
  
 fn_trace_getinfo anexa una extensión .trc al nombre de un archivo de seguimiento incluido en su conjunto de resultados. Para obtener información acerca de cómo definir un seguimiento, vea [sp_trace_create &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md). Para obtener información similar acerca de los filtros de seguimiento, vea [sys.fn_trace_getfilterinfo &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md).  
  
 Para obtener un ejemplo completo del uso de procedimientos almacenados de seguimiento, vea [crear un seguimiento &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER TRACE en el servidor.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve información acerca de todos los seguimientos activos.  
  
```  
SELECT * FROM sys.fn_trace_getinfo(0) ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Crear un seguimiento &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_generateevent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sys.fn_trace_geteventinfo &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_gettable &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)  
  
  
