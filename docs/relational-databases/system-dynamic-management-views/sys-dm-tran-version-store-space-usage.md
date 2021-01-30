---
description: sys.dm_tran_version_store_space_usage (Transact-SQL)
title: sys.dm_tran_version_store_space_usage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_version_store_space_usage_TSQL
- sys.dm_tran_version_store_space_usage
- dm_tran_version_store_space_usage
- dm_tran_version_store_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_version_store_space_usage dynamic management view
ms.assetid: 7ab44517-0351-4f91-bdd9-7cf940f03c51
author: savjani
ms.author: pariks
manager: ajayj
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c36dce670a89883b436421ae26c36d54308f2457
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99132100"
---
# <a name="sysdm_tran_version_store_space_usage-transact-sql"></a>sys.dm_tran_version_store_space_usage (Transact-SQL)
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Devuelve una tabla que muestra el espacio total en tempdb utilizado por los registros del almacén de versiones para cada base de datos. **Sys.dm_tran_version_store_space_usage** es eficaz y no caro de ejecutarse, ya que no navega por los registros individuales del almacén de versiones y devuelve el espacio de almacén de versiones agregado consumido en tempdb por base de datos.
  
Cada registro con versión se almacena como datos binarios, junto con algún seguimiento o información de estado. Al igual que los registros en tablas de base de datos, los registros del almacén de versiones se almacenan en páginas de 8.192 bytes. Si un registro supera los 8.192 bytes, se dividirá en dos registros diferentes.  
  
Puesto que el registro de versiones se almacena como binario, no existen problemas con las diferentes intercalaciones de bases de datos distintas. Use **Sys.dm_tran_version_store_space_usage** para supervisar y planear el tamaño de tempdb según el uso del espacio del almacén de versiones de las bases de datos en una instancia de SQL Server.
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|IDENTIFICADOR de base de datos de la base de datos.|  
|**reserved_page_count**|**bigint**|Recuento total de las páginas reservadas en tempdb para los registros de almacén de versiones de la base de datos.|  
|**reserved_space_kb**|**bigint**|Espacio total utilizado en kilobytes en tempdb para los registros del almacén de versiones de la base de datos.|  
  
## <a name="permissions"></a>Permisos  
En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   

## <a name="examples"></a>Ejemplos  
La siguiente consulta se puede utilizar para determinar el espacio consumido en tempdb, por almacén de versiones de cada base de datos en una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instancia de. 
  
```sql  
SELECT 
  DB_NAME(database_id) as 'Database Name',
  reserved_page_count,
  reserved_space_kb 
FROM sys.dm_tran_version_store_space_usage;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Database Name            reserved_page_count reserved_space_kb  
------------------------ -------------------- -----------  
msdb                      0                    0             
AdventureWorks2016        10                   80             
AdventureWorks2016DW      0                    0             
WideWorldImporters        20                   160             
```
 
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con transacciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
