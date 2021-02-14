---
description: sys.dm_os_performance_counters (Transact-SQL)
title: sys.dm_os_performance_counters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_performance_counters
- sys.dm_os_performance_counters_TSQL
- dm_os_performance_counters_TSQL
- sys.dm_os_performance_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_performance_counters dynamic management view
ms.assetid: a1c3e892-cd48-40d4-b6be-2a9246e8fbff
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4717c16b30d53e258ca1e18924b47b68c0b640f3
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342791"
---
# <a name="sysdm_os_performance_counters-transact-sql"></a>sys.dm_os_performance_counters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por contador de rendimiento que se mantiene en el servidor. Para obtener información acerca de cada contador de rendimiento, vea [usar objetos SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_performance_counters**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_name**|**nchar(128)**|Categoría a la que pertenece este contador.|  
|**counter_name**|**nchar(128)**|Nombre del contador. Para obtener más información acerca de un contador, es el nombre del tema que se va a seleccionar en la lista de contadores en [uso SQL Server objetos](../../relational-databases/performance-monitor/use-sql-server-objects.md). |  
|**instance_name**|**nchar(128)**|Nombre de la instancia específica del contador. A menudo contiene el nombre de la base de datos.|  
|**cntr_value**|**bigint**|Valor actual del contador.<br /><br /> **Nota:** En los contadores por segundo, este valor es acumulativo. El valor de la tarifa se debe calcular probando el valor en intervalos de tiempo distintos. La diferencia entre dos valores de ejemplo sucesivos es igual a la tarifa del intervalo de tiempo usado.|  
|**cntr_type**|**int**|Tipo de contador definido en la arquitectura de rendimiento de Windows. Para obtener más información sobre los tipos de contadores de rendimiento, consulte [tipos de contador de rendimiento de WMI](/windows/desktop/WmiSdk/wmi-performance-counter-types) en docs o la documentación de Windows Server.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="remarks"></a>Comentarios  
 Si la instancia de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede mostrar los contadores de rendimiento del sistema operativo Windows, utilice la consulta de [!INCLUDE[tsql](../../includes/tsql-md.md)] siguiente para confirmar que se han deshabilitado los contadores de rendimiento.  
  
```sql  
SELECT COUNT(*) FROM sys.dm_os_performance_counters;  
```  
  
Si el valor devuelto es 0 filas, significa que se han deshabilitado los contadores de rendimiento. A continuación, debe examinar el registro de instalación y buscar el error 3409, `Reinstall sqlctr.ini for this instance, and ensure that the instance login account has correct registry permissions.` lo que indica que los contadores de rendimiento no están habilitados. Los errores inmediatamente anteriores al error 3409 deben indicar la causa principal del error en la habilitación del contador de rendimiento. Para obtener más información acerca de los archivos de registro de instalación, vea [ver y leer archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  

Los contadores de rendimiento en los que el valor de la `cntr_type` columna es 65792, 272696320 y 537003264 muestran un valor de contador de instantáneas instantáneas.

Los contadores de rendimiento en los que el valor de la `cntr_type` columna es 272696576, 1073874176 y 1073939712 muestran los valores de los Contadores acumulativos en lugar de una instantánea instantánea. Por lo tanto, para obtener una lectura similar a una instantánea, debe comparar la diferencia entre dos puntos de colección.

## <a name="permission"></a>Permiso

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
 
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelven todos los contadores de rendimiento que muestran los valores del contador de instantáneas.  
  
```sql  
SELECT object_name, counter_name, instance_name, cntr_value, cntr_type  
FROM sys.dm_os_performance_counters
WHERE cntr_type = 65792 OR cntr_type = 272696320 OR cntr_type = 537003264;  
```  
  
## <a name="see-also"></a>Consulte también  
  [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.sysperfinfo &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  
  
