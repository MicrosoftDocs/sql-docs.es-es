---
description: cdc.lsn_time_mapping (Transact-SQL)
title: cdc.lsn_time_mapping (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- cdc.lsn_time_mapping
- cdc.lsn_time_mapping_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.lsn_time_mapping
ms.assetid: 1cb7aedc-48a4-486e-9b91-d30c4bd4084e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6cf48f0c68e5e376b186e7f51361a479c9d1e67c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203639"
---
# <a name="cdclsn_time_mapping-transact-sql"></a>cdc.lsn_time_mapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila para cada transacción que tiene filas en una tabla de cambios. Esta tabla se utiliza para las asignaciones entre los valores de confirmación de número de secuencia de registro (LSN) y la hora de confirmación de la transacción. También se pueden registrar entradas en las que no se han modificado entradas de tablas. De este modo, la tabla podrá grabar el procesamiento completo del LSN en períodos de baja o nula actividad de cambio.  
  
 Se recomienda que no consulte directamente las tablas del sistema. En su lugar, ejecute el [sys.fn_cdc_map_lsn_to_time &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-map-lsn-to-time-transact-sql.md) y sys.fn_cdc_map_time_to_lsn &#40;funciones del sistema [de transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md) .  
    
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**start_lsn**|**binary(10)**|LSN de la transacción confirmada.|  
|**tran_begin_time**|**datetime**|Hora de inicio de la transacción asociada con el LSN.|  
|**tran_end_time**|**datetime**|Hora de finalización de la transacción.|  
|**tran_id**|**varbinary (10)**|Id. de la transacción.|  
  
## <a name="see-also"></a>Consulte también  
 [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [cdc.&#60;capture_instance&#62;_CT &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md)  
  
  
