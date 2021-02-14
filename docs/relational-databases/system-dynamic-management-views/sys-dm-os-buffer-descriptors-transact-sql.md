---
description: sys.dm_os_buffer_descriptors (Transact-SQL)
title: sys.dm_os_buffer_descriptors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_buffer_descriptors_TSQL
- dm_os_buffer_descriptors_TSQL
- sys.dm_os_buffer_descriptors
- dm_os_buffer_descriptors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_buffer_descriptors dynamic management view
ms.assetid: 012aab95-8888-4f35-9ea3-b5dff6e3f60f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83162825d5c4598c8c9f5fecae10feed03364abd
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344322"
---
# <a name="sysdm_os_buffer_descriptors-transact-sql"></a>sys.dm_os_buffer_descriptors (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información acerca de todas las páginas de datos que están actualmente en el grupo de búferes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La salida de esta vista se puede utilizar para determinar la distribución de páginas de la base de datos en el grupo de búferes según la base de datos, el objeto o el tipo. En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], esta vista de administración dinámica también devuelve información sobre las páginas de datos del archivo de la extensión del grupo de búferes. Para obtener más información, consulte la [extensión del grupo de búferes](../../database-engine/configure-windows/buffer-pool-extension.md).  
  
 Cuando se lee una página de datos del disco, esta se copia en el grupo de búferes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y se almacena en caché para volver a utilizarla. Cada página de datos almacenada en caché tiene un descriptor de búfer. Los descriptores de búfer únicamente identifican cada página de datos que está almacenada actualmente en memoria caché en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sys.dm_os_buffer_descriptors devuelve páginas en memoria caché para todas las bases de datos de usuario y de sistema. Esto incluye las páginas que están asociadas a la base de datos Resource.  
  
> **Nota:** Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_buffer_descriptors**.  

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|database_id|**int**|Identificador de la base de datos asociada con la página en el grupo de búferes. Acepta valores NULL.|  
|file_id|**int**|Identificador del archivo que almacena la imagen permanente de la página. Acepta valores NULL.|  
|page_id|**int**|IDENTIFICADOR de la página dentro del archivo. Acepta valores NULL.|  
|page_level|**int**|Nivel de índice de la página. Acepta valores NULL.|  
|allocation_unit_id|**bigint**|Identificador de la unidad de asignación de la página. Este valor se puede utilizar para combinar sys.allocation_units. Acepta valores NULL.|  
|page_type|**nvarchar(60)**|Tipo de la página, como: página de datos o página de índice. Acepta valores NULL.|  
|row_count|**int**|Número de filas de la página. Acepta valores NULL.|  
|free_space_in_bytes|**int**|Cantidad, en bytes, de espacio disponible en la página. Acepta valores NULL.|  
|is_modified|**bit**|1 = La página se ha modificado después de leerse en el disco. Acepta valores NULL.|  
|numa_node|**int**|Nodo de acceso no uniforme a memoria para el búfer. Acepta valores NULL.|  
|read_microsec|**bigint**|El tiempo real (en microsegundos) necesario para leer la página en el búfer. Este número se restablece cuando se reutiliza el búfer. Acepta valores NULL.|  
|is_in_bpool_extension|**bit**|1 = la página está en la extensión del grupo de búferes. Acepta valores NULL.|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
   
## <a name="remarks"></a>Comentarios  
 sys.dm_os_buffer_descriptors devuelve páginas que utiliza la base de datos de recursos. sys.dm_os_buffer_descriptors no devuelve información acerca de las páginas liberadas o robadas, o de las páginas que tenían errores cuando se leyeron.  
  
|De|En|Activado|Relación|  
|----------|--------|--------|------------------|  
|sys.dm_os_buffer_descriptors|sys.databases|database_id|varios a uno|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.allocation_units|allocation_unit_id|varios a uno|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.database_files|file_id|varios a uno|  
|sys.dm_os_buffer_descriptors|sys.dm_os_buffer_pool_extension_configuration|file_id|varios a uno|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-cached-page-count-for-each-database"></a>A. Devolver el recuento de páginas almacenadas en caché de cada base de datos  
 En el ejemplo siguiente se devuelve el recuento de páginas cargadas para cada base de datos.  
  
```  
SELECT COUNT(*)AS cached_pages_count  
    ,CASE database_id   
        WHEN 32767 THEN 'ResourceDb'   
        ELSE db_name(database_id)   
        END AS database_name  
FROM sys.dm_os_buffer_descriptors  
GROUP BY DB_NAME(database_id) ,database_id  
ORDER BY cached_pages_count DESC;  
```  
  
### <a name="b-returning-cached-page-count-for-each-object-in-the-current-database"></a>B. Devolver el recuento de páginas almacenadas en caché para cada objeto de la base de datos actual  
 En el ejemplo siguiente se devuelve el recuento de páginas cargadas para cada objeto en la base de datos actual.  
  
```  
SELECT COUNT(*)AS cached_pages_count   
    ,name ,index_id   
FROM sys.dm_os_buffer_descriptors AS bd   
    INNER JOIN   
    (  
        SELECT object_name(object_id) AS name   
            ,index_id ,allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.hobt_id   
                    AND (au.type = 1 OR au.type = 3)  
        UNION ALL  
        SELECT object_name(object_id) AS name     
            ,index_id, allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.partition_id   
                    AND au.type = 2  
    ) AS obj   
        ON bd.allocation_unit_id = obj.allocation_unit_id  
WHERE database_id = DB_ID()  
GROUP BY name, index_id   
ORDER BY cached_pages_count DESC;  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 
 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [Base de datos Resource](../../relational-databases/databases/resource-database.md)   
 [sys.dm_os_buffer_pool_extension_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)  
  
  


