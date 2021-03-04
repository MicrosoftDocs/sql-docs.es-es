---
description: sys.dm_tran_transactions_snapshot (Transact-SQL)
title: sys.dm_tran_transactions_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_transactions_snapshot
- dm_tran_transactions_snapshot
- sys.dm_tran_transactions_snapshot_TSQL
- dm_tran_transactions_snapshot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_transactions_snapshot dynamic management view
ms.assetid: 03f64883-07ad-4092-8be0-31973348c647
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d200e13c95af3e7c4f1f19e58d8cebcfee1ebb25
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837211"
---
# <a name="sysdm_tran_transactions_snapshot-transact-sql"></a>sys.dm_tran_transactions_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una tabla virtual para la **sequence_number** de transacciones que están activas cuando se inicia cada transacción de instantánea. La información que devuelve esta vista le puede ayudar a realizar las siguientes acciones:  
  
-   Buscar el número de transacciones de instantáneas activas actualmente.  
  
-   Identificar modificaciones de datos pasadas por alto por una transacción de instantáneas determinada. Si existe una transacción activa cuando comienza una transacción de instantáneas, todas las modificaciones de datos de la primera, incluso después de confirmarla, se pasan por alto en la transacción de instantáneas.  
  
 Por ejemplo, considere la siguiente salida de **Sys.dm_tran_transactions_snapshot**:  
  
```  
transaction_sequence_num snapshot_id snapshot_sequence_num  
------------------------ ----------- ---------------------  
59                       0           57  
59                       0           58  
60                       0           57  
60                       0           58  
60                       0           59  
60                       3           57  
60                       3           58  
60                       3           59  
60                       3           60  
```  
  
 La columna `transaction_sequence_num` identifica el número de secuencia de transacción (XSN) de las transacciones de instantáneas actuales. El resultado muestra dos: `59` y `60`. La columna `snapshot_sequence_num` identifica el número de secuencia de las transacciones activas al iniciarse cada transacción de instantáneas.  
  
 El resultado muestra que la transacción de instantáneas XSN-59 comienza mientras se ejecutan dos transacciones activas, XSN-57 y XSN-58. Si XSN-57 o XSN-58 realizan modificaciones de datos, XSN-59 pasa por alto los cambios y utiliza versiones de fila para conservar una vista coherente de la base de datos desde el punto de vista transaccional.  
  
 La transacción de instantáneas XSN-60 pasa por alto las modificaciones efectuadas por XSN-57 y XSN-58 y también por XSN 59.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|Número de secuencia de transacción (XSN) de una transacción de instantáneas.|  
|**snapshot_id**|**int**|Id. de instantánea de cada instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] iniciada con lectura confirmada y que utiliza las versiones de fila. Este valor se utiliza para generar una vista de la base de datos transaccionalmente coherente que admita cada consulta que se ejecute en una lectura confirmada utilizando las versiones de fila.|  
|**snapshot_sequence_num**|**bigint**|Número de secuencia de una transacción que estaba activa cuando se inició la transacción de instantánea.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="remarks"></a>Observaciones  
 Cuando se inicia una transacción de instantáneas, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] registra todas las transacciones activas en ese momento determinado. **Sys.dm_tran_transactions_snapshot** notifica esta información para todas las transacciones de instantáneas activas.  
  
 Cada transacción se identifica mediante un número de secuencia que se asigna cuando se inicia la transacción. Las transacciones empiezan en el momento en que se ejecuta una instrucción BEGIN TRANSACTION o BEGIN WORK. No obstante, [!INCLUDE[ssDE](../../includes/ssde-md.md)] asigna el número de secuencia de la transacción con la ejecución de la primera instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] que tiene acceso a datos después de la instrucción BEGIN TRANSACTION o BEGIN WORK. Los números de secuencia de la transacción se incrementan de uno en uno.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con transacciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
