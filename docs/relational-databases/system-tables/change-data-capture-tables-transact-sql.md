---
description: Tablas de captura de datos de cambio (Transact-SQL)
title: Tablas de captura de datos modificados (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: a4372d0b-50ca-4e58-80f6-2ed3cb52a84a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8003b9d42055aa31cae82765a2f13663fe260860
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198274"
---
# <a name="change-data-capture-tables-transact-sql"></a>Tablas de captura de datos de cambio (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La captura de datos de cambio permite realizar un seguimiento de las tablas de tal manera que los cambios en el lenguaje de manipulación de datos (DML) y en el lenguaje de definición de datos (DDL) realizados en las tablas se puedan cargar incrementalmente en un almacenamiento de datos. Los temas de esta sección describen las tablas del sistema que almacenan las información que usan las operaciones de captura de datos modificados.  
  
## <a name="in-this-section"></a>En esta sección  
 [capture_instance CDC. <>_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md)  
 Devuelve una fila para cada cambio realizado en una columna capturada en la tabla de origen asociada.  
  
 [cdc.captured_columns](../../relational-databases/system-tables/cdc-captured-columns-transact-sql.md)  
 Devuelve una fila para cada columna de la que se ha realizado un seguimiento en una instancia de captura.  
  
 [cdc.change_tables](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md)  
 Devuelve una fila por cada tabla de cambios en la base de datos.  
  
 [cdc.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md)  
 Devuelve una fila para cada cambio del lenguaje de definición de datos (DDL) realizado en las tablas que se habilitan para la captura de datos de cambio.  
  
 [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md)  
 Devuelve una fila para cada transacción que tiene filas en una tabla de cambios. Esta tabla se utiliza para las asignaciones entre los valores de confirmación de número de secuencia de registro (LSN) y la hora de confirmación de la transacción.  
  
 [cdc.index_columns](../../relational-databases/system-tables/cdc-index-columns-transact-sql.md)  
 Devuelve una fila para cada columna de índice asociada a una tabla de cambios.  
  
 [dbo.cdc_jobs &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)  
 Devuelve los parámetros de configuración para los trabajos de agente de captura de datos del cambio.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de captura de datos modificados &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)   
 [Funciones de captura de datos modificados &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-data-capture-functions-transact-sql.md)  
  
  
