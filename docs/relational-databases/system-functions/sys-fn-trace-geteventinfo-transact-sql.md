---
description: sys.fn_trace_geteventinfo (Transact-SQL)
title: sys.fn_trace_geteventinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_trace_geteventinfo
- fn_trace_geteventinfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- events [SQL Server], status information
- fn_trace_geteventinfo function
- sys.fn_trace_geteventinfo function
- status information [SQL Server], events
ms.assetid: 5b1c858a-ca43-4e2b-9d67-8654daaf0cc5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9249fc380c2642f2e3415d6b6da0e6103360fa04
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201879"
---
# <a name="sysfn_trace_geteventinfo-transact-sql"></a>sys.fn_trace_geteventinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de un evento para el que se va a realizar un seguimiento.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use eventos extendidos en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn_trace_geteventinfo ( trace_id )  
```  
  
## <a name="arguments"></a>Argumentos  
 *trace_id*  
 Es el identificador de seguimiento. *trace_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
## <a name="tables-returned"></a>Tablas devueltas  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**EventID**|**int**|Id. del evento del que se hace el seguimiento.|  
|**columnid**|**int**|Números de identificación de todas las columnas recopiladas para cada evento.|  
  
## <a name="remarks"></a>Observaciones  
 Cuando se pasa el identificador de un seguimiento específico, **fn_trace_geteventinfo** devuelve información sobre ese seguimiento. Si se pasa un Id. no válido, esta función devuelve un conjunto de filas vacío.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER TRACE en el servidor.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve información sobre el seguimiento número 2.  
  
```  
SELECT * FROM fn_trace_geteventinfo(2) ;  
GO  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [Crear un seguimiento &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_generateevent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [sys.fn_trace_gettable &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)   
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)  
  
  
