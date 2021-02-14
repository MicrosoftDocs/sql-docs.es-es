---
description: sys.dm_db_missing_index_details (Transact-SQL)
title: sys.dm_db_missing_index_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_details
- dm_db_missing_index_details
- sys.dm_db_missing_index_details_TSQL
- dm_db_missing_index_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- missing indexes feature [SQL Server], sys.dm_db_missing_index_details dynamic management view
- sys.dm_db_missing_index_details dynamic management view
ms.assetid: ced484ae-7c17-4613-a3f9-6d8aba65a110
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6327baee24be8526cb0756495fe425e4103f35c8
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343080"
---
# <a name="sysdm_db_missing_index_details-transact-sql"></a>sys.dm_db_missing_index_details (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información detallada sobre los índices que faltan, excluidos los índices espaciales.  
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], las vistas de administración dinámica no pueden exponer información que impactaría a la contención de la base de datos ni acerca de otras bases de datos a las que el usuario tenga acceso. Para evitar exponer esta información, se filtran todas las filas que contienen datos que no pertenecen al inquilino conectado.  

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**index_handle**|**int**|Identifica un índice que falta específico. El identificador es único en todo el servidor. **index_handle** es la clave de esta tabla.|  
|**database_id**|**smallint**|Identifica la base de datos en la que reside la tabla en la que falta un índice.|  
|**object_id**|**int**|Identifica la tabla en la que falta el índice.|  
|**equality_columns**|**nvarchar(4000)**|Lista de columnas separadas por comas que contribuyen a predicados de igualdad de la forma:<br /><br /> *tabla. columna*  = *constant_value*|  
|**inequality_columns**|**nvarchar(4000)**|Lista de columnas separadas por comas que contribuyen a predicados de desigualdad; por ejemplo, a predicados de la forma:<br /><br /> *tabla. columna*  >  *constant_value*<br /><br /> Cualquier operador de comparación distinto de "=" expresa desigualdad.|  
|**included_columns**|**nvarchar(4000)**|Lista de columnas de cobertura separadas por comas requeridas por la consulta. Para obtener más información sobre cómo abarcar o incluir columnas, vea [crear índices con columnas incluidas](../../relational-databases/indexes/create-indexes-with-included-columns.md).<br /><br /> En el caso de los índices optimizados para memoria (hash y optimizado para memoria no agrupado), omita **included_columns**. Todas las columnas de la tabla se incluyen en cada índice optimizado para memoria.|  
|**instrucción**|**nvarchar(4000)**|Nombre de la tabla en la que falta el índice.|  
  
## <a name="remarks"></a>Comentarios  
 La información devuelta por **sys.dm_db_missing_index_details** se actualiza cuando se optimiza una consulta mediante el optimizador de consultas y no se guarda. La información sobre índices que faltan solo se conserva hasta que se reinicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los administradores de bases de datos deben realizar copias de seguridad de forma periódica de la información de índices que faltan si desean conservarla después de reciclar el servidor.  
  
 Para determinar a cuáles de los grupos de índices que faltan pertenece un índice que falta específico, puede consultar la vista de administración dinámica **sys.dm_db_missing_index_groups** mediante una combinación de igualdad con **sys.dm_db_missing_index_details** basada en la columna **index_handle**.  

  >[!NOTE]
  >El conjunto de resultados de esta DMV está limitado a 600 filas. Cada fila contiene un índice que falta. Si tiene más de 600 índices que faltan, debe tratar los índices que faltan existentes para que pueda ver los más recientes. 
  
## <a name="using-missing-index-information-in-create-index-statements"></a>Utilizar información de índices que faltan en instrucciones CREATE INDEX  
 Para convertir la información devuelta por **Sys.dm_db_missing_index_details** en una instrucción CREATE index para los índices optimizados para memoria y basados en disco, las columnas de igualdad deben colocarse delante de las columnas de desigualdad, y juntas deben crear la clave del índice. Las columnas incluidas deben agregarse a la instrucción CREATE INDEX mediante la cláusula INCLUDE. Para determinar un orden efectivo para las columnas de igualdad, ordénelas en función de su selectividad, mostrando primero las columnas más selectivas (en la parte izquierda de la lista de columnas).  
  
 Para obtener más información acerca de los índices optimizados para memoria, consulte [índices de Memory-Optimized tablas](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).  
  
## <a name="transaction-consistency"></a>Coherencia de las transacciones  
 Si una transacción crea o quita una tabla, las filas que contienen información de índices que faltan sobre los objetos quitados se eliminan de este objeto de administración dinámica para mantener la coherencia de la transacción.  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="see-also"></a>Consulte también  
 [sys.dm_db_missing_index_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
  
  
