---
description: sys.dm_tran_version_store (Transact-SQL)
title: sys.dm_tran_version_store (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_version_store_TSQL
- sys.dm_tran_version_store
- dm_tran_version_store
- dm_tran_version_store_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_version_store dynamic management view
ms.assetid: 7ab44517-0351-4f91-bdd9-7cf940f03c51
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83b1b28ad73a3fb3c8535f297f34fe9f95936e61
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100347978"
---
# <a name="sysdm_tran_version_store-transact-sql"></a>sys.dm_tran_version_store (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una tabla virtual que muestra todos los registros de versión del almacén de versiones. **Sys.dm_tran_version_store** es ineficaz para ejecutarse porque consulta todo el almacén de versiones y el almacén de versiones puede ser muy grande.  
  
 Cada registro de versión se almacena como datos binarios junto con alguna información de estado o seguimiento. Al igual que los registros en tablas de base de datos, los registros del almacén de versiones se almacenan en páginas de 8.192 bytes. Si un registro supera los 8.192 bytes, se dividirá en dos registros diferentes.  
  
 Puesto que el registro de versiones se almacena como binario, no existen problemas con las diferentes intercalaciones de bases de datos distintas. Utilice **Sys.dm_tran_version_store** para buscar las versiones anteriores de las filas en representación binaria tal como existen en el almacén de versiones.  
  
  
## <a name="syntax"></a>Syntax  
  
```  
sys.dm_tran_version_store  
```  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|Número de secuencia de la transacción que genera la versión de registro.|  
|**version_sequence_num**|**bigint**|Número de secuencia del registro de versión. Este valor es único en la transacción que genera la versión.|  
|**database_id**|**int**|Id. de la base de datos del registro de versiones.|  
|**rowset_id**|**bigint**|Id. del conjunto de filas del registro.|  
|**status**|**tinyint**|Indica si un registro de versiones se ha dividido en dos registros. Si el valor es 0, el registro está almacenado en una página. Si el valor es 1, el registro está dividido en dos registros almacenados en dos páginas diferentes.|  
|**min_length_in_bytes**|**smallint**|Longitud mínima del registro en bytes.|  
|**record_length_first_part_in_bytes**|**smallint**|Longitud de la primera parte del registro de versiones en bytes.|  
|**record_image_first_part**|**varbinary(8000)**|Imagen binaria de la primera parte del registro de versiones.|  
|**record_length_second_part_in_bytes**|**smallint**|Longitud de la segunda parte del registro de versiones en bytes.|  
|**record_image_second_part**|**varbinary(8000)**|Imagen binaria de la segunda parte del registro de versiones.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se utiliza un escenario de prueba con cuatro transacciones simultáneas, identificadas con un número de secuencia de transacción (XSN), que se ejecutan en una base de datos con las opciones ALLOW_SNAPSHOT_ISOLATION y READ_COMMITTED_SNAPSHOT establecidas en ON. Se están ejecutando las siguientes transacciones:  
  
-   XSN-57 es una operación de actualización con aislamiento serializable.  
  
-   XSN-58 es igual que XSN-57.  
  
-   XSN-59 es una operación de selección con aislamiento de instantáneas.  
  
-   XSN-60 es igual que XSN-59.  
  
 Se ejecuta la siguiente consulta.  
  
```  
SELECT  
    transaction_sequence_num,  
    version_sequence_num,  
    database_id rowset_id,  
    status,  
    min_length_in_bytes,  
    record_length_first_part_in_bytes,  
    record_image_first_part,  
    record_length_second_part_in_bytes,  
    record_image_second_part  
  FROM sys.dm_tran_version_store;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_sequence_num version_sequence_num database_id  
------------------------ -------------------- -----------  
57                      1                    9             
57                      2                    9             
57                      3                    9             
58                      1                    9             
  
rowset_id            status min_length_in_bytes  
-------------------- ------ -------------------  
72057594038321152    0      12                   
72057594038321152    0      12                   
72057594038321152    0      12                   
72057594038386688    0      16                   
  
record_length_first_part_in_bytes  
---------------------------------  
29                                 
29                                 
29                                 
33                                 
  
record_image_first_part                                               
--------------------------------------------------------------------  
0x50000C0073000000010000000200FCB000000001000000270000000000          
0x50000C0073000000020000000200FCB000000001000100270000000000          
0x50000C0073000000030000000200FCB000000001000200270000000000          
0x500010000100000002000000030000000300F800000000000000002E0000000000  
  
record_length_second_part_in_bytes record_image_second_part  
---------------------------------- ------------------------  
0                                  NULL  
0                                  NULL  
0                                  NULL  
0                                  NULL  
```  
  
 En la salida se muestra que XSN-57 ha creado tres versiones de fila de una tabla y que XSN-58 ha creado una versión de fila de otra tabla.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con transacciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
