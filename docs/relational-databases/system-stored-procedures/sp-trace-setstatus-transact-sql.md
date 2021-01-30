---
description: sp_trace_setstatus (Transact-SQL)
title: sp_trace_setstatus (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_setstatus_TSQL
- sp_trace_setstatus
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setstatus
ms.assetid: 29e7a7d7-b9c1-414a-968a-fc247769750d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 69090ce81bf7a70d28cf12959c412d88f270cfdd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209712"
---
# <a name="sp_trace_setstatus-transact-sql"></a>sp_trace_setstatus (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica el estado actual del seguimiento especificado.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use eventos extendidos en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_trace_setstatus [ @traceid = ] trace_id , [ @status = ] status  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @traceid = ] trace_id` Es el ID. del seguimiento que se va a modificar. *trace_id* es de **tipo int** y no tiene ningún valor predeterminado. El usuario emplea este valor *trace_id* para identificar, modificar y controlar el seguimiento. Para obtener información acerca de cómo recuperar el *trace_id*, vea [Sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md).  
  
`[ @status = ] status` Especifica la acción que se va a implementar en el seguimiento. *status* es de **tipo int** y no tiene ningún valor predeterminado.  
  
 En la tabla siguiente se muestra una lista de los estados que podrían especificarse.  
  
|Estado|Descripción|  
|------------|-----------------|  
|**0**|Detiene el seguimiento especificado.|  
|**1**|Inicia el seguimiento especificado.|  
|**2**|Cierra el seguimiento especificado y elimina su definición del servidor.|  
  
> [!NOTE]  
>  Para poder cerrar un seguimiento, primero debe detenerse. Para poder ver un seguimiento, antes debe detenerse y cerrarse.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 En la tabla siguiente se describen los valores del código que los usuarios pueden obtener después de completar el procedimiento almacenado.  
  
|Código devuelto|Descripción|  
|-----------------|-----------------|  
|**0**|Sin errores.|  
|**1**|Error desconocido.|  
|**8**|El estado especificado no es válido.|  
|**9**|El identificador de seguimiento especificado no es válido.|  
|**13**|Memoria insuficiente Se devuelve cuando no hay memoria suficiente para realizar la acción especificada.|  
  
 Si el seguimiento ya está en el estado especificado, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devolverá **0**.  
  
## <a name="remarks"></a>Observaciones  
 Los parámetros de todos los procedimientos almacenados de seguimiento de SQL (**sp_trace_xx**) tienen un tipo estricto. Si no se llama a estos parámetros con los tipos de datos de parámetros de entrada correctos, según se especifica en la descripción del argumento, el procedimiento almacenado devolverá un error.  
  
 Para obtener un ejemplo de cómo usar los procedimientos almacenados de seguimiento, vea [Crear un seguimiento &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener permiso ALTER TRACE.  
  
## <a name="see-also"></a>Consulte también  
 [sys.fn_trace_geteventinfo &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sp_trace_generateevent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [Seguimiento de SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
