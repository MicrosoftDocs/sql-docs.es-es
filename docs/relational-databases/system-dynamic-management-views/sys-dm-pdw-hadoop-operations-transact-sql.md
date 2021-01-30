---
description: sys.dm_pdw_hadoop_operations (Transact-SQL)
title: sys.dm_pdw_hadoop_operations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5d2337d4-e2c7-48de-9c26-cdc7e6eb5d55
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 92b2ca358d1c1ef012af00159d94f30b0ce035a3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142415"
---
# <a name="sysdm_pdw_hadoop_operations-transact-sql"></a>sys.dm_pdw_hadoop_operations (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene una fila por cada trabajo de asignación y reducción que se inserta en Hadoop como parte de la ejecución de una [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] consulta en una tabla de Hadoop externa. Cada trabajo de asignación y reducción representa uno de los predicados de la consulta. Solo se usa cuando el predicado aplicación está habilitado para consultas en tablas externas de Hadoop.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|IDENTIFICADOR de esta operación de Hadoop externa.|Igual que el identificador de [sys.dm_pdw_exec_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|Índice del paso de consulta que hace referencia a esta operación de Hadoop.|Igual que step_index en [sys.dm_pdw_request_steps &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql.md).|  
|operation_type|**nvarchar(255)**|Describe el tipo de operación externa.|' Operación de Hadoop externa '|  
|operation_name|**nvarchar(4000)**|El ID. de trabajo de un trabajo de asignación y reducción. Esto lo devuelve Hadoop después [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] de que envíe el trabajo.||  
|map_progress|**float**|Porcentaje de datos de entrada que el trabajo de asignación ha consumido hasta ahora.|Un número de punto flotante entre, incluido, 0 y 100.|  
|reduce_progress|**int**|Porcentaje del trabajo de reducción que se ha completado..|Un número de punto flotante entre, incluido, 0 y 100.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)  
  
