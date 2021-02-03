---
description: ALTER RESOURCE POOL (Transact-SQL)
title: ALTER RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_RESOURCE_POOL_TSQL
- ALTER RESOURCE POOL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER RESOURCE POOL
ms.assetid: 9c1c4cfb-0e3b-4f01-bf57-3fce94c7d1d4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0f33cb191bee4b8856983f946a768c5abdd38726
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204043"
---
# <a name="alter-resource-pool-transact-sql"></a>ALTER RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia la configuración del grupo de recursos de servidor del regulador de recursos existente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER RESOURCE POOL { pool_name | "default" }  
[WITH  
    ( [ MIN_CPU_PERCENT = value ]  
    [ [ , ] MAX_CPU_PERCENT = value ]   
    [ [ , ] CAP_CPU_PERCENT = value ]   
    [ [ , ] AFFINITY {
                        SCHEDULER = AUTO 
                      | ( <scheduler_range_spec> ) 
                      | NUMANODE = ( <NUMA_node_range_spec> )
                      }]   
    [ [ , ] MIN_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_MEMORY_PERCENT = value ]   
    [ [ , ] MIN_IOPS_PER_VOLUME = value ]  
    [ [ , ] MAX_IOPS_PER_VOLUME = value ]  
)]   
[;]  
  
<scheduler_range_spec> ::=  
{SCHED_ID | SCHED_ID TO SCHED_ID}[,...n]  
  
<NUMA_node_range_spec> ::=  
{NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID}[,...n]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 { *pool_name* |  **"default"** }  
 Es el nombre de un grupo de recursos de servidor definido por el usuario ya existente o el grupo de recursos de servidor predeterminado creado al instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 "default" debe estar encerrado entre comillas ("") o corchetes ([]) si se utiliza con ALTER RESOURCE POOL para evitar el conflicto con DEFAULT, que es una palabra reservada del sistema. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
>  Todos los grupos de cargas de trabajo y de recursos predefinidos usan nombres en minúsculas, como "predeterminado". Debe tenerse esto en cuenta en los servidores que usan una intercalación que distingue entre mayúsculas y minúsculas. En los servidores que usan una intercalación que no distingue entre mayúsculas y minúsculas, como SQL_Latin1_General_CP1_CI_AS, los nombres "predeterminado" y "Predeterminado" son equivalentes.  
  
 MIN_CPU_PERCENT =*value*  
 Especifica el ancho banda de la CPU promedio garantizado para todas las solicitudes en el grupo de recursos de servidor cuando hay contención de CPU. *valor* es un entero con un valor predeterminado de 0. El intervalo permitido para *value* es de 0 a 100.  
  
 MAX_CPU_PERCENT =*value*  
 Especifica el promedio máximo de ancho banda de CPU que recibirán todas las solicitudes en el grupo de recursos de servidor cuando hay contención de CPU. *value* es un entero con un valor predeterminado de 100. El intervalo permitido para *value* es de 1 a 100.  
  
 CAP_CPU_PERCENT =*value*  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Especifica la capacidad máxima de CPU de destino para las solicitudes del grupo de recursos. *value* es un entero con un valor predeterminado de 100. El intervalo permitido para *value* es de 1 a 100.  
  
> [!NOTE]  
>  Debido a la naturaleza estadística de la gobernanza de la CPU, es posible que observe picos ocasionales que superen el valor especificado en CAP_CPU_PERCENT.  
  
 AFFINITY {SCHEDULER = AUTO | (Scheduler_range_spec) | NUMANODE = (NUMA_node_range_spec)}  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Adjunte el grupo de recursos de servidor a los programadores específicos. El valor predeterminado es AUTO.  
  
 AFFINITY SCHEDULER = (Scheduler_range_spec) asigna al grupo de recursos a las programaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] representadas por los identificadores proporcionados. Estos identificadores se asignan a los valores de la columna scheduler_id de [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).  
  
 Cuando usa AFFINITY NAMANODE = (NUMA_node_range_spec), se establece afinidad entre el grupo de recursos y a los programadores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se asignan a las CPU físicas correspondientes al nodo NUMA o al intervalo de nodos especificado. Puede usar la siguiente consulta de Transact-SQL para detectar la asignación entre la configuración física de NUMA y los identificadores de programador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT osn.memory_node_id AS [numa_node_id], sc.cpu_id, sc.scheduler_id  
FROM sys.dm_os_nodes AS osn  
INNER JOIN sys.dm_os_schedulers AS sc 
   ON osn.node_id = sc.parent_node_id 
      AND sc.scheduler_id < 1048576;  
```  
  
 MIN_MEMORY_PERCENT =*value*  
 Especifica la cantidad mínima de memoria reservada para este grupo de recursos de servidor que no se puede compartir con otros grupos de recursos de servidor. *valor* es un entero con un valor predeterminado de 0. El intervalo permitido para *value* es de 0 a 100.  
  
 MAX_MEMORY_PERCENT =*value*  
 Especifica la memoria total del servidor que puede ser utilizada por las solicitudes en este grupo de recursos de servidor. *value* es un entero con un valor predeterminado de 100. El intervalo permitido para *value* es de 1 a 100.  
  
 MIN_IOPS_PER_VOLUME =*value*  
 **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
 Especifica el número mínimo de operaciones de E/S por segundo (IOPS) por volumen de disco que se deben reservar para el grupo de recursos de servidor. El intervalo permitido para *value* es de 0 a 2^31-1 (2.147.483.647). Especifique 0 si no desea indicar un umbral mínimo para el grupo.  
  
 MAX_IOPS_PER_VOLUME =*value*  
 **Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
 Especifica el número máximo de operaciones de E/S por segundo (IOPS) por volumen de disco que se deben permitir para el grupo de recursos de servidor. El intervalo permitido para *value* es de 0 a 2^31-1 (2.147.483.647). Especifique 0 para establecer un umbral ilimitado para el grupo. El valor predeterminado es 0.  
  
 Si MAX_IOPS_PER_VOLUME se establece en 0 para un grupo, este no está regulado y puede tomar todas las IOPS del sistema, incluso aunque otros grupos tengan establecido un valor MIN_IOPS_PER_VOLUME. En este caso, se recomienda establecer el valor de MAX_IOPS_PER_VOLUME para este grupo en un número alto (por ejemplo, el valor máximo 2^31-1) si desea que se regule la E/S de este grupo.  
  
## <a name="remarks"></a>Observaciones  
 MAX_CPU_PERCENT y MAX_MEMORY_PERCENT deben ser mayores o iguales que MIN_CPU_PERCENT y MIN_MEMORY_PERCENT, respectivamente.  
  
 MAX_CPU_PERCENT puede usar la capacidad de CPU por encima del valor de MAX_CPU_PERCENT si está disponible. Aunque puede haber picos periódicos por encima de CAP_CPU_PERCENT, las cargas de trabajo no pueden exceder CAP_CPU_PERCENT durante períodos de tiempo prolongados, incluso cuando se dispone de capacidad de CPU adicional.  
  
 El porcentaje total de la CPU para cada componente afín (programadores o nodos NUMA) no debe superar el 100 %.  
  
 Si va a ejecutar instrucciones de DDL, se recomienda familiarizarse primero con los estados del regulador de recursos. Para obtener más información, vea [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  
  
 Cuando se cambia un plan que afecta al valor, el valor nuevo solo se aplica a los planes previamente almacenados en caché después de ejecutar DBCC FREEPROCCACHE (*pool_name*), donde *pool_name* es el nombre de un grupo de recursos de Resource Governor.  
  
-   Si se va a cambiar AFFINITY de varios programadores a uno solo, la ejecución de DBCC FREEPROCCACHE no es necesaria porque los planes paralelos se pueden ejecutar en serie. Pero puede que no sea tan eficaz como un plan compilado como un plan en serie.  
  
-   Si se va a cambiar AFFINITY de un programador a varios, la ejecución de DBCC FREEPROCCACHE no es necesaria. Pero los planes en serie no se pueden ejecutar en paralelo, así que el borrado de la memoria caché respectiva permite compilar potencialmente nuevos planes mediante paralelismo.  
  
> [!CAUTION]  
>  El borrado de los planes almacenados en caché de un grupo de recursos asociado a más de un grupo de cargas de trabajo afecta a todos los grupos de cargas de trabajo con el grupo de recursos definido por el usuario identificado por *pool_name*.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL SERVER.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente mantiene toda la configuración predeterminada del grupo de recursos de servidor en el grupo `default`, salvo `MAX_CPU_PERCENT`, que se cambia a `25`.  
  
```sql  
ALTER RESOURCE POOL "default"  
WITH  
     ( MAX_CPU_PERCENT = 25);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
 En el ejemplo siguiente, `CAP_CPU_PERCENT` establece el límite máximo en 80 % y `AFFINITY SCHEDULER` se establece según un valor individual de 8 y un intervalo de 12 a 16.  
  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
```sql  
ALTER RESOURCE POOL Pool25  
WITH(   
     MIN_CPU_PERCENT = 5,  
     MAX_CPU_PERCENT = 10,       
     CAP_CPU_PERCENT = 80,  
     AFFINITY SCHEDULER = (8, 12 TO 16),   
     MIN_MEMORY_PERCENT = 5,  
     MAX_MEMORY_PERCENT = 15  
);  
  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
