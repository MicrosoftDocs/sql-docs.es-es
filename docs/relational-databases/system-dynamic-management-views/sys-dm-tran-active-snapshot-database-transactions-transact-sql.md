---
description: sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
title: sys.dm_tran_active_snapshot_database_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_active_snapshot_database_transactions_TSQL
- dm_tran_active_snapshot_database_transactions
- sys.dm_tran_active_snapshot_database_transactions
- dm_tran_active_snapshot_database_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_active_snapshot_database_transactions dynamic management view
ms.assetid: 55b83f9c-da10-4e65-9846-f4ef3c0c0f36
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7798fa7e5e93b4c0e5b5f227877cec536195513b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99134080"
---
# <a name="sysdm_tran_active_snapshot_database_transactions-transact-sql"></a>sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  En una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta vista de administración dinámica devuelve una tabla virtual de todas las transacciones activas que generan versiones de fila o que pueden tener acceso a ellas. Las transacciones se incluyen en función de una o varias de las siguientes condiciones:  
  
-   Si una de las opciones de base de datos ALLOW_SNAPSHOT_ISOLATION y READ_COMMITTED_SNAPSHOT (o ambas) está establecida en ON:  
  
    -   Existe una fila para cada transacción que se ejecute en el nivel de aislamiento de instantáneas, o bien en el nivel de aislamiento de lectura confirmada que utiliza versiones de fila.  
  
    -   Existe una fila para cada transacción que provoca la creación de una versión de fila en la base de datos actual. Por ejemplo, la transacción genera una versión de fila cuando actualiza o elimina una fila de la base de datos actual.  
  
-   Cuando se inicia un desencadenador, existe una fila para la transacción en la que se ejecuta el desencadenador.  
  
-   Cuando se ejecuta un procedimiento de indización en línea, existe una fila para la transacción que crea el índice.  
  
-   Cuando se habilita la sesión MARS (Conjuntos de resultados activos múltiples), existe una fila para cada transacción que tiene acceso a las versiones de fila.  
  
 Esta vista de administración dinámica no incluye transacciones del sistema.  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_tran_active_snapshot_database_transactions**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.dm_tran_active_snapshot_database_transactions  
```  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**transaction_id**|**bigint**|Número de identificación único asignado para la transacción. El Id. de transacción se utiliza principalmente para identificar la transacción en las operaciones de bloqueo.|  
|**transaction_sequence_num**|**bigint**|Número de secuencia de la transacción. Se trata de un número de secuencia único que se asigna a una transacción cuando se inicia. Las transacciones que no generan registros de versiones y no utilizan recorridos de instantáneas no reciben un número de secuencia de la transacción.|  
|**commit_sequence_num**|**bigint**|Número de secuencia que indica cuándo finaliza (se confirma o se detiene) la transacción. Para las transacciones activas, el valor es NULL.|  
|**is_snapshot**|**int**|0 = No es una transacción de aislamiento de instantánea.<br /><br /> 1 = Es una transacción de aislamiento de instantánea.|  
|**session_id**|**int**|Id. de la sesión que ha iniciado la transacción.|  
|**first_snapshot_sequence_num**|**bigint**|Número más bajo de secuencia de la transacción de las transacciones que estaban activas cuando se obtuvo la instantánea. Cuando se ejecuta, una transacción de instantánea realiza una instantánea de todas las transacciones activas en ese momento. En las transacciones que no son de instantánea, en esta columna se muestra 0.|  
|**max_version_chain_traversed**|**int**|Longitud máxima de la cadena de versiones que se recorre para buscar la versión coherente desde el punto de vista de las transacciones.|  
|**average_version_chain_traversed**|**real**|Promedio de versiones de fila en las cadenas de versiones que se recorren.|  
|**elapsed_time_seconds**|**bigint**|Tiempo transcurrido desde que la transacción obtuvo su número de secuencia de la transacción.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="remarks"></a>Observaciones  
 **Sys.dm_tran_active_snapshot_database_transactions** notifica las transacciones que tienen asignado un número de secuencia de la transacción (xsn). Este XSN se asigna cuando la transacción tiene acceso por primera vez al almacén de versiones. En una base de datos habilitada para el aislamiento de instantáneas o el aislamiento de lectura confirmada que utiliza las versiones de fila, los ejemplos muestran cuándo se asigna un valor XSN a una transacción:  
  
-   Si una transacción se ejecuta con el nivel de aislamiento serializable, se asigna un XSN cuando la transacción ejecuta por primera vez una instrucción, como una operación UPDATE, que provoca la creación de una versión de fila.  
  
-   Si una transacción se ejecuta con el aislamiento de instantáneas, se asigna un XSN cuando se ejecuta cualquier instrucción de lenguaje de manipulación de datos (DML), incluida una operación SELECT.  
  
 Los números de secuencia de la transacción se incrementan en serie para cada transacción que se inicia en una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se utiliza un escenario de prueba con cuatro transacciones simultáneas, identificadas con un número de secuencia de transacción (XSN), que se ejecutan en una base de datos con las opciones ALLOW_SNAPSHOT_ISOLATION y READ_COMMITTED_SNAPSHOT establecidas en ON. Se están ejecutando las siguientes transacciones:  
  
-   XSN-57 es una operación de actualización con aislamiento serializable.  
  
-   XSN-58 es igual que XSN-57.  
  
-   XSN-59 es una operación de selección con aislamiento de instantáneas.  
  
-   XSN-60 es igual que XSN-59.  
  
 Se ejecuta la siguiente consulta.  
  
```  
SELECT   
    transaction_id,  
    transaction_sequence_num,  
    commit_sequence_num,  
    is_snapshot session_id,  
    first_snapshot_sequence_num,  
    max_version_chain_traversed,  
    average_version_chain_traversed,  
    elapsed_time_seconds  
  FROM sys.dm_tran_active_snapshot_database_transactions;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_id  transaction_sequence_num  commit_sequence_num  
--------------  ------------------------  -------------------  
9295            57                        NULL  
9324            58                        NULL  
9387            59                        NULL  
9400            60                        NULL  
  
is_snapshot  session_id   first_snapshot_sequence_num  
-----------  -----------  ---------------------------  
0            54           0  
0            53           0  
1            52           57  
1            51           57  
  
max_version_chain_traversed  average_version_chain_traversed  
---------------------------  -------------------------------  
0                            0  
0                            0  
1                            1  
1                            1  
  
elapsed_time_seconds  
--------------------  
419  
397  
359  
333  
```  
  
 La siguiente información evalúa los resultados de **Sys.dm_tran_active_snapshot_database_transactions**:  
  
-   XSN-57: dado que esta transacción no se ejecuta con aislamiento de instantánea, el `is_snapshot` valor y `first_snapshot_sequence_num` son `0` . `transaction_sequence_num` muestra que se ha asignado un número de secuencia de la transacción a esta transacción, ya que una o ambas de las opciones de base de datos ALLOW_SNAPSHOT_ISOLATION o READ_COMMITTED_SNAPSHOT están establecidas en ON.  
  
-   XSN-58: esta transacción no se ejecuta con aislamiento de instantáneas y se aplica la misma información que en XSN-57.  
  
-   XSN-59: ésta es la primera transacción activa que se ejecuta con aislamiento de instantáneas. Esta transacción lee datos confirmados antes de XSN-57, como se indica mediante `first_snapshot_sequence_num`. La salida de esta transacción también muestra que la cadena de versiones máxima que se recorre para una fila es `1` y que ha recorrido un promedio de `1` versión para cada fila a la que se tiene acceso. Esto significa que las transacciones XSN-57, XSN-58 y XSN-60 no han modificado filas ni realizado confirmaciones.  
  
-   XSN-60: ésta es la segunda transacción que se ejecuta con aislamiento de instantáneas. La salida muestra la misma información que XSN-59.  
  
## <a name="see-also"></a>Consulte también  
 [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con transacciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


