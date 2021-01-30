---
description: sys.fn_MSxe_read_event_stream (Transact-SQL)
title: sys.fn_MSxe_read_event_stream (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_MSxe_read_event_stream_TSQL
- sys.fn_MSxe_read_event_stream_TSQL
- sys.fn_MSxe_read_event_stream
- fn_MSxe_read_event_stream
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_MSxe_read_event_stream
- fn_MSxe_read_event_stream
ms.assetid: 5edb1162-625a-41e0-8ec9-1edc8ab9a74a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dc8eff18237f4796d36812cf0afee363effcbf99
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201929"
---
# <a name="sysfn_msxe_read_event_stream-transact-sql"></a>sys.fn_MSxe_read_event_stream (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Los eventos extendidos proporcionan una función con valores de tabla (TVF) para ser usada internamente por la interfaz de usuario (IU) de eventos extendidos. La tabla no proporciona datos utilizables por el cliente.  
  
 Para ver los datos de evento, utilice lo siguiente:  
  
-   Interfaz de usuario de nueva sesión de eventos extendidos.  
  
-   Función con valores de tabla (TVF) fn_xe_file_target_read_file Para obtener más información, vea [sys.fn_xe_file_target_read_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_MSxe_read_event_stream ( session_name)  
```  
  
## <a name="arguments"></a>Argumentos  
 *Session_name*  
 Nombre de una sesión que se está ejecutando en el servidor.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|type|**Integer (4)**|El tipo de evento. No admite valores NULL.|  
|datos|**Image (16)**|Datos de la imagen de evento. Acepta valores NULL.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de eventos extendidos](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Vistas de catálogo de eventos extendidos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)  
  
  
