---
description: sys.dm_db_missing_index_columns (Transact-SQL)
title: sys.dm_db_missing_index_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_missing_index_columns_TSQL
- sys.dm_db_missing_index_columns_TSQL
- sys.dm_db_missing_index_columns
- dm_db_missing_index_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_columns dynamic management function
- missing indexes feature [SQL Server], sys.dm_db_missing_index_columns dynamic management function
ms.assetid: 3b24e5ed-0c79-47e5-805c-a0902d0aeb86
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3905f5c31da774eac8e5df59404aa84913c7b173
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208285"
---
# <a name="sysdm_db_missing_index_columns-transact-sql"></a>sys.dm_db_missing_index_columns (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información sobre las columnas de la tabla de la base de datos que no tienen índice, sin incluir los índices espaciales. **Sys.dm_db_missing_index_columns** es una función de administración dinámica.  

## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.dm_db_missing_index_columns(index_handle)  
```  
  
## <a name="arguments"></a>Argumentos  
 *index_handle*  
 Un entero que identifica de forma única un índice que falta. Puede obtenerse a partir de los siguientes objetos de administración dinámica:  
  
 [sys.dm_db_missing_index_details &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)  
  
 [sys.dm_db_missing_index_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**column_id**|**int**|Identificador de la columna.|  
|**column_name**|**sysname**|Nombre de la columna de la tabla.|  
|**column_usage**|**VARCHAR (20)**|Forma en que la consulta utiliza la columna. Los valores posibles y sus descripciones son:<br /><br /> IGUALDAD: la columna contribuye a un predicado que expresa igualdad, con el formato: <br />                        *tabla. columna*  =  *constant_value*<br /><br /> Desigualdad: la columna contribuye a un predicado que expresa desigualdad, por ejemplo, un predicado con el formato: *TABLE. Column*  >  *constant_value*. Cualquier operador de comparación distinto de "=" expresa desigualdad.<br /><br /> INCLUDE: la columna no se utiliza para evaluar un predicado, pero se usa por otro motivo, por ejemplo, para cubrir una consulta.|  
  
## <a name="remarks"></a>Observaciones  
 La información que devuelve **sys.dm_db_missing_index_columns** se actualiza cuando el optimizador de consultas optimiza una consulta, pero no se almacena. La información sobre índices que faltan solo se conserva hasta que se reinicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los administradores de bases de datos deben realizar copias de seguridad de forma periódica de la información de índices que faltan si desean conservarla después de reciclar el servidor.  
  
## <a name="transaction-consistency"></a>Coherencia de las transacciones  
 Si una transacción crea o quita una tabla, las filas que contienen información de índices que faltan sobre los objetos quitados se eliminan de este objeto de administración dinámica para mantener la coherencia de la transacción.  
  
## <a name="permissions"></a>Permisos  
 Los usuarios deben disponer del permiso VIEW SERVER STATE o de cualquier permiso que implique el permiso VIEW SERVER STATE para consultar esta función de administración dinámica.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se ejecuta una consulta en la tabla `Address` y, después, otra con la vista de administración dinámica `sys.dm_db_missing_index_columns` para devolver las columnas de la tabla a las que les falta un índice.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT City, StateProvinceID, PostalCode  
FROM Person.Address  
WHERE StateProvinceID = 9;  
GO  
SELECT mig.*, statement AS table_name,  
    column_id, column_name, column_usage  
FROM sys.dm_db_missing_index_details AS mid  
CROSS APPLY sys.dm_db_missing_index_columns (mid.index_handle)  
INNER JOIN sys.dm_db_missing_index_groups AS mig ON mig.index_handle = mid.index_handle  
ORDER BY mig.index_group_handle, mig.index_handle, column_id;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_db_missing_index_details &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
  
  
