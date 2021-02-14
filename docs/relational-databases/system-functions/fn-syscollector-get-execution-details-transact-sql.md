---
description: fn_syscollector_get_execution_details (Transact-SQL)
title: fn_syscollector_get_execution_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_syscollector_get_execution_details_TSQL
- fn_syscollector_get_execution_details
dev_langs:
- TSQL
helpviewer_keywords:
- fn_syscollector_get_execution_details function
ms.assetid: d59ddf0c-72c0-4c57-bc83-aef260e4e105
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3cc8f5cd454fb92be15938858a221d2968baff59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350643"
---
# <a name="fn_syscollector_get_execution_details-transact-sql"></a>fn_syscollector_get_execution_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una parte del registro de [!INCLUDE[ssIS](../../includes/ssis-md.md)] (sysssislog) que coincide con el package_execution_id para el paquete determinado. La tabla contiene una fila por cada entrada de registro generada en tiempo de ejecución por los paquetes o sus tareas y contenedores.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn_syscollector_get_execution_details ( log_id )  
```  
  
## <a name="arguments"></a>Argumentos  
 *log_id*  
 Identificador único local para el registro de ejecución. *log_id* es de **tipo int**.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|id|**int**|Identificador único de la entrada de registro.|  
|event|**sysname**|Nombre del evento que genera la entrada de registro.|  
|computer|**nvarchar**|Equipo en el que se estaba ejecutando el paquete cuando se generó la entrada de registro.|  
|operator|**nvarchar**|Nombre de usuario de la persona o agente que ejecuta el paquete que genera la entrada de registro.|  
|source|**nvarchar**|Nombre del ejecutable que genera la entrada de registro.|  
|sourceid|**uniqueidentifier**|GUID del ejecutable que genera la entrada de registro.|  
|executionid|**uniqueidentifier**|GUID de la instancia de ejecución del ejecutable que genera la entrada de registro.|  
|starttime|**datetime**|Hora en que el paquete empezó a ejecutarse.|  
|endtime|**datetime**|Hora en que la ejecución del paquete se completó.|  
|datacode|**int**|Valor entero que identifica el evento asociado a la entrada de registro. El valor "0" indica que el evento no ha generado ningún identificador.|  
|databytes|**image**|Matriz de bytes que identifica un valor devuelto.|  
|message|**nvarchar**|Descripción del evento e información asociada a dicho evento.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso SELECT para **dc_operator**.  
  
## <a name="see-also"></a>Consulte también  
 [Habilitar el registro de paquetes en SQL Server Data Tools](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)  
  
  
