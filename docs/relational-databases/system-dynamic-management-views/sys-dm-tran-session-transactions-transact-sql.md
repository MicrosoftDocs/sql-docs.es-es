---
description: sys.dm_tran_session_transactions (Transact-SQL)
title: sys.dm_tran_session_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_tran_session_transactions
- sys.dm_tran_session_transactions
- sys.dm_tran_session_transactions_TSQL
- dm_tran_session_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_session_transactions dynamic management view
ms.assetid: c7157491-58c2-49fe-87d7-0c9723113adf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4c5eebed16415e9eb64deed24771ad6bdbc4e11c
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100340077"
---
# <a name="sysdm_tran_session_transactions-transact-sql"></a>sys.dm_tran_session_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve información de correlación para sesiones y transacciones asociadas.  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_tran_session_transactions**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|session_id|**int**|Id. de la sesión en que se ejecuta la transacción.|  
|transaction_id|**bigint**|Id. de la transacción.|  
|transaction_descriptor|**Binary(8**|Identificador de la transacción utilizado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al comunicarse con el controlador cliente.|  
|enlist_count|**int**|Número de solicitudes activas en la sesión de la transacción.|  
|is_user_transaction|**bit**|1 = Transacción iniciada por una solicitud de usuario.<br /><br /> 0 = Transacción de sistema.|  
|is_local|**bit**|1 = Transacción local.<br /><br /> 0 = Transacción distribuida o transacción de sesión enlazada dada de alta.|  
|is_enlisted|**bit**|1 = Transacción distribuida dada de alta.<br /><br /> 0 = No es una transacción distribuida dada de alta.|  
|is_bound|**bit**|1 = La transacción está activa en la sesión a través de sesiones enlazadas.<br /><br /> 0 = La transacción no está activa en la sesión a través de sesiones enlazadas.|  
|open_transaction_count||La cantidad de transacciones abiertas para cada sesión.|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="remarks"></a>Observaciones  
 Por medio de sesiones enlazadas y transacciones distribuidas, una transacción puede ejecutarse en más de una sesión. En tales casos, sys.dm_tran_session_transactions mostrará varias filas para el mismo valor de transaction_id, una por cada sesión en la que se ejecuta la transacción.  
  
 Al ejecutar varias solicitudes en modo de confirmación automática mediante el uso de conjuntos de resultados activos múltiples (MARS), puede tener más de una transacción activa en una sola sesión. En tales casos, sys.dm_tran_session_transactions mostrará varias filas para el mismo valor de session_id, una por cada transacción que se ejecuta en la sesión.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con transacciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


