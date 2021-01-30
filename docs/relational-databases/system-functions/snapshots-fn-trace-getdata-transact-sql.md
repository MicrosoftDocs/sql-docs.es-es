---
description: snapshots.fn_trace_getdata (Transact-SQL)
title: snapshots.fn_trace_getdata (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- snapshots.fn_trace_getdata
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots.fn_trace_getdata function
ms.assetid: ac28ef48-f4f4-4bf2-ba22-d44e1be88172
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5ce872efff2eff679d02c56985783fd2d70b2082
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194494"
---
# <a name="snapshotsfn_trace_getdata-transact-sql"></a>snapshots.fn_trace_getdata (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esta función devuelve todos los eventos capturados para el seguimiento especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
snapshots.fn_trace_gettable ( trace_info_id, start_time, end_time )  
```  
  
## <a name="arguments"></a>Argumentos  
 *trace_info_id*  
 Identificador único de la clave principal en la tabla de snapshots.trace_info de la base de datos del almacén de administración de datos. *trace_info_id* es de **tipo int**.  
  
 *start_time*  
 Hora a la que empezó el seguimiento. *start_time* es **DateTime**.  
  
 *end_time*  
 Hora a la que finalizó el seguimiento. *end_time* es **DateTime**.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|\<All trace columns>|\<Varies>|Datos de seguimiento de la tabla snapshots.trace_data de la base de datos del almacén de administración de datos.<br /><br /> Puede obtener una lista de las columnas para el seguimiento especificado mediante la siguiente consulta:<br /><br /> `SELECT * FROM sys.trace_columns`<br /><br /> **Nota:** Las columnas devueltas por la función snapshots.fn_trace_gettable se corresponden con los valores de la columna Nombre de la vista del sistema sys.trace_columns. La única diferencia es que la función no devuelve la columna GroupID.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso SELECT para mdw_reader.  
  
## <a name="see-also"></a>Consulte también  
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)  
  
  
