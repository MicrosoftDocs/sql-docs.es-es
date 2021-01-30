---
description: sys.trace_event_bindings (Transact-SQL)
title: sys.trace_event_bindings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.trace_event_bindings_TSQL
- trace_event_bindings
- sys.trace_event_bindings
- trace_event_bindings_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trace_event_bindings catalog view
ms.assetid: 22f534e1-4ed6-4b3e-9ead-1d1001a1b0f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8ee9cd803ba1899e51a6c7f04efbf8f896e16446
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208381"
---
# <a name="systrace_event_bindings-transact-sql"></a>sys.trace_event_bindings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista de catálogo **Sys.trace_event_bindings** contiene una lista de todas las posibles combinaciones de uso de eventos y columnas. Para cada evento que aparece en la columna **trace_event_id** , todas las columnas disponibles se enumeran en la columna **trace_column_id** . No todas las columnas disponibles se llenan cada vez que se produce un evento determinado. Estos valores no cambian para una versión concreta de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
 Para obtener una lista completa de los eventos de seguimiento admitidos, vea [SQL Server referencia](../../relational-databases/event-classes/sql-server-event-class-reference.md)de la clase de eventos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use vistas de catálogo de eventos extendidos en su lugar.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**trace_event_id**|**smallint**|Id. del evento de seguimiento. Esta columna también está en la vista de catálogo **Sys.trace_events** .|  
|**trace_column_id**|**smallint**|Id. de columna de seguimiento. Esta columna también está en la vista de catálogo **Sys.trace_columns** .|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Sys. Traces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-traces-transact-sql.md)   
 [sys.trace_categories &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-categories-transact-sql.md)   
 [sys.trace_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-columns-transact-sql.md)   
 [sys.trace_events &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-events-transact-sql.md)   
 [sys.trace_subclass_values &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-trace-subclass-values-transact-sql.md)  
  
  
