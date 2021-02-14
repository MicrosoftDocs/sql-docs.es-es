---
description: sys.dm_os_memory_objects (Transact-SQL)
title: sys.dm_os_memory_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_objects
- sys.dm_os_memory_objects
- sys.dm_os_memory_objects_TSQL
- dm_os_memory_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_objects dynamic management view
ms.assetid: 5688bcf8-5da9-4ff9-960b-742b671d7096
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fd0f5daa6ba1b167aa1f56dce70dabe5e92e4247
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100338891"
---
# <a name="sysdm_os_memory_objects-transact-sql"></a>sys.dm_os_memory_objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve los objetos de memoria asignados actualmente por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Puede usar **Sys.dm_os_memory_objects** para analizar el uso de memoria y para identificar posibles pérdidas de memoria.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**memory_object_address**|**varbinary(8**|Dirección del objeto de memoria. No admite valores NULL.|  
|**parent_address**|**varbinary(8**|Dirección del objeto de memoria primario. Acepta valores NULL.|  
|**pages_allocated_count**|**int**|**Se aplica a**: desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Número de páginas asignadas por este objeto. No admite valores NULL.|  
|**pages_in_bytes**|**bigint**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Cantidad de memoria en bytes que esta instancia del objeto de memoria asigna. No admite valores NULL.|  
|**creation_options**|**int**|Solo para uso interno. Acepta valores NULL.|  
|**bytes_used**|**bigint**|Solo para uso interno. Acepta valores NULL.|  
|**type**|**nvarchar(60)**|Tipo de objeto de memoria.<br /><br /> Indica algún componente al que pertenece este objeto de memoria, o la función del objeto de memoria. Acepta valores NULL.|  
|**name**|**varchar(128)**|Solo para uso interno. Acepta valores NULL.|  
|**memory_node_id**|**smallint**|Identificador de un nodo de memoria que utiliza este objeto de memoria. No admite valores NULL.|  
|**creation_time**|**datetime**|Solo para uso interno. Acepta valores NULL.|  
|**max_pages_allocated_count**|**int**|**Se aplica a**: desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Número máximo de páginas asignadas por este objeto de memoria. No admite valores NULL.|  
|**page_size_in_bytes**|**int**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Tamaño de las páginas asignadas por este objeto, en bytes. No admite valores NULL.|  
|**max_pages_in_bytes**|**bigint**|Cantidad máxima de memoria que este objeto de memoria nunca usó. No admite valores NULL.|  
|**page_allocator_address**|**varbinary(8**|Dirección de memoria del asignador de la página. No admite valores NULL. Para obtener más información, vea [sys.dm_os_memory_clerks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**creation_stack_address**|**varbinary(8**|Solo para uso interno. Acepta valores NULL.|  
|**sequence_num**|**int**|Solo para uso interno. Acepta valores NULL.|  
|**partition_type**|**int**|**Válido para** : [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> El tipo de partición:<br /><br /> 0: objeto de memoria no particionable<br /><br /> 1-objeto de memoria con particiones, actualmente sin particiones<br /><br /> 2-objeto de memoria con particiones, particionado por nodo NUMA. En un entorno con un solo nodo NUMA, equivale a 1.<br /><br /> 3-objeto de memoria con particiones, con particiones por CPU.|  
|**contention_factor**|**real**|**Válido para** : [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> Valor que especifica la contención en este objeto de memoria, donde 0 significa que no hay contención. El valor se actualiza cada vez que se realiza un número especificado de asignaciones de memoria que reflejan la contención durante ese período. Solo se aplica a los objetos de memoria seguros para subprocesos.|  
|**waiting_tasks_count**|**bigint**|**Válido para** : [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> Número de esperas en este objeto de memoria. Este contador se incrementa siempre que se asigna memoria de este objeto de memoria. El incremento es el número de tareas que actualmente esperan el acceso a este objeto de memoria. Solo se aplica a los objetos de memoria seguros para subprocesos. Este es un mejor valor de esfuerzo sin una garantía de corrección.|  
|**exclusive_access_count**|**bigint**|**Válido para** : [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] y versiones posteriores.<br /><br /> Especifica la frecuencia de acceso exclusivo a este objeto de memoria. Solo se aplica a los objetos de memoria seguros para subprocesos.  Este es un mejor valor de esfuerzo sin una garantía de corrección.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
 **partition_type**, **contention_factor**, **waiting_tasks_count** y **exclusive_access_count** todavía no se han implementado en [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] .  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="remarks"></a>Comentarios  
 Los objetos de memoria son montones. Proporcionan asignaciones con una granularidad más fina que las que proporcionan los distribuidores de memoria. Los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizan objetos de memoria en lugar de distribuidores de memoria. Los objetos de memoria utilizan la interfaz del asignador de la página del distribuidor de memoria para asignar páginas. Los objetos de memoria no utilizan interfaces de memoria virtual o compartida. Según los patrones de asignación, los componentes pueden crear diferentes tipos de objetos de memoria para asignar regiones de tamaño arbitrario.  
  
 El tamaño de página típico de un objeto de memoria es de 8 KB. No obstante, los objetos de memoria incrementales pueden tener tamaños de página que van desde 512 bytes hasta 8 KB.  
  
> [!NOTE]  
>  El tamaño de página no tiene una asignación máxima. En su lugar, el tamaño de página es la granularidad de la asignación que admite una asignador de la página y que se implementa mediante un distribuidor de memoria. Puede solicitar asignaciones mayores de 8 KB de los objetos de memoria.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve la cantidad de memoria asignada por cada tipo de objeto de memoria.  
  
```  
SELECT SUM (pages_in_bytes) as 'Bytes Used', type   
FROM sys.dm_os_memory_objects  
GROUP BY type   
ORDER BY 'Bytes Used' DESC;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
  [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)  
  
  


