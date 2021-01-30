---
description: sys.dm_tran_top_version_generators (Transact-SQL)
title: sys.dm_tran_top_version_generators (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_tran_top_version_generators
- sys.dm_tran_top_version_generators
- dm_tran_top_version_generators_TSQL
- sys.dm_tran_top_version_generators_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_top_version_generators dynamic management view
ms.assetid: cec7809b-ba8a-4df9-b5bb-d4f651ff1a86
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 939913a431621bd21b6125dc93c4375ac1ea6ee1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99133445"
---
# <a name="sysdm_tran_top_version_generators-transact-sql"></a>sys.dm_tran_top_version_generators (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una tabla virtual para los objetos que producen la mayor parte de las versiones en el almacén de versiones. **Sys.dm_tran_top_version_generators** devuelve las longitudes de registro agregadas principales 256 agrupadas por los **database_id** y **rowset_id**. **Sys.dm_tran_top_version_generators** recupera datos consultando la tabla virtual **dm_tran_version_store** . **Sys.dm_tran_top_version_generators** es una vista ineficaz para ejecutarse porque esta vista consulta el almacén de versiones y el almacén de versiones puede ser muy grande. Es recomendable utilizar esta función para encontrar los principales consumidores del almacén de versiones.  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_tran_top_version_generators**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.dm_tran_top_version_generators  
```  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Id. de la base de datos.|  
|**rowset_id**|**bigint**|Id. del conjunto de filas.|  
|**aggregated_record_length_in_bytes**|**int**|Suma de las longitudes de los registros de cada **database_id** y **rowset_id par** en el almacén de versiones.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="remarks"></a>Observaciones  
 Dado que es posible que **Sys.dm_tran_top_version_generators** tenga que leer muchas páginas mientras examina todo el almacén de versiones, ejecutar **Sys.dm_tran_top_version_generators** puede interferir con el rendimiento del sistema.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se utiliza un escenario de prueba con cuatro transacciones simultáneas, identificadas con un número de secuencia de transacción (XSN), que se ejecutan en una base de datos con las opciones ALLOW_SNAPSHOT_ISOLATION y READ_COMMITTED_SNAPSHOT establecidas en ON. Se están ejecutando las siguientes transacciones:  
  
-   XSN-57 es una operación de actualización con aislamiento serializable.  
  
-   XSN-58 es igual que XSN-57.  
  
-   XSN-59 es una operación de selección con aislamiento de instantáneas.  
  
-   XSN-60 es igual que XSN-59.  
  
 Se ejecuta la siguiente consulta.  
  
```  
SELECT  
    database_id,  
    rowset_id,  
    aggregated_record_length_in_bytes  
  FROM sys.dm_tran_top_version_generators;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
database_id rowset_id            aggregated_record_length_in_bytes  
----------- -------------------- ---------------------------------  
9           72057594038321152    87  
9           72057594038386688    33  
```  
  
 En el resultado se muestra que todas las versiones son creadas por `database_id``9` y que las versiones generan a partir de dos tablas.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con transacciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


