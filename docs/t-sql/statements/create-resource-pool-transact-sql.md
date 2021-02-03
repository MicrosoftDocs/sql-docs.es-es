---
description: CREATE RESOURCE POOL (Transact-SQL)
title: CREATE RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE RESOURCE POOL
- RESOURCE POOL
- CREATE_RESOURCE_POOL_TSQL
- RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE RESOURCE POOL
ms.assetid: 82712505-c6f9-4a65-a469-f029b5a2d6cd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a55efe97292550be8d0150514c56c3eb478f39ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188510"
---
# <a name="create-resource-pool-transact-sql"></a>CREATE RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Crea un grupo de recursos de servidor del regulador de recursos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un grupo de recursos de servidor representa un subconjunto de los recursos físicos (memoria, CPU y E/S) de una instancia del motor de base de datos. El Regulador de recursos permite que un administrador de bases de datos distribuya los recursos del servidor entre los grupos de recursos, hasta un máximo de 64 grupos. El regulador de recursos no está disponible en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintaxis  
```syntaxsql
CREATE RESOURCE POOL pool_name  
[ WITH  
    (  
        [ MIN_CPU_PERCENT = value ]  
        [ [ , ] MAX_CPU_PERCENT = value ]   
        [ [ , ] CAP_CPU_PERCENT = value ]   
        [ [ , ] AFFINITY {SCHEDULER =  
                  AUTO 
                | ( <scheduler_range_spec> )   
                | NUMANODE = ( <NUMA_node_range_spec> )
                } ]   
        [ [ , ] MIN_MEMORY_PERCENT = value ]  
        [ [ , ] MAX_MEMORY_PERCENT = value ]  
        [ [ , ] MIN_IOPS_PER_VOLUME = value ]  
        [ [ , ] MAX_IOPS_PER_VOLUME = value ]  
    )   
]  
[;]  
  
<scheduler_range_spec> ::=  
{ SCHED_ID | SCHED_ID TO SCHED_ID }[,...n]  
  
<NUMA_node_range_spec> ::=  
{ NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID }[,...n]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*pool_name*  
Es el nombre definido por el usuario para identificar el grupo de recursos de servidor. *pool_name* es alfanumérico, puede tener hasta 128 caracteres, debe ser único dentro de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y debe cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
MIN_CPU_PERCENT =*value*  
Especifica el ancho banda de la CPU promedio garantizado para todas las solicitudes en el grupo de recursos de servidor cuando hay contención de CPU. *valor* es un entero con un valor predeterminado de 0. El intervalo permitido para *value* es de 0 a 100.  
  
MAX_CPU_PERCENT =*value*  
Especifica el ancho de banda de CPU promedio máximo que recibirán todas las solicitudes del grupo de recursos de servidor cuando hay contención de CPU. *value* es un entero con un valor predeterminado de 100. El intervalo permitido para *value* es de 1 a 100.  
  
CAP_CPU_PERCENT =*value*   
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
Especifica un límite máximo de ancho de banda de CPU que recibirán todas las solicitudes del grupo de recursos de servidor. Limita el nivel de ancho de banda máximo de CPU para que coincida con el valor especificado. *value* es un entero con un valor predeterminado de 100. El intervalo permitido para *value* es de 1 a 100.  
  
AFFINITY {SCHEDULER = AUTO | ( \<scheduler_range_spec> ) | NUMANODE = (\<NUMA_node_range_spec>)}      
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
Adjunte el grupo de recursos de servidor a los programadores específicos. El valor predeterminado es AUTO.  
  
AFFINITY SCHEDULER = **(** \<scheduler_range_spec> **)** asigna el grupo de recursos a las programaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que indican los id. proporcionados. Estos identificadores se asignan a los valores de la columna scheduler_id de [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). 
  
Cuando se usa AFFINITY NUMANODE = **(** \<NUMA_node_range_spec> **)** , se establece afinidad entre el grupo de recursos y los programadores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se asignan a las CPU físicas correspondientes al nodo o al intervalo de nodos NUMA especificado. Puede usar la siguiente consulta de [!INCLUDE[tsql](../../includes/tsql-md.md)] para detectar la asignación entre la configuración física de NUMA y los identificadores de programador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
```sql  
SELECT osn.memory_node_id AS [numa_node_id], sc.cpu_id, sc.scheduler_id  
FROM sys.dm_os_nodes AS osn  
INNER JOIN sys.dm_os_schedulers AS sc   
    ON osn.node_id = sc.parent_node_id   
    AND sc.scheduler_id < 1048576;  
```  
  
MIN_MEMORY_PERCENT =*value*    
Especifica la cantidad mínima de memoria reservada para este grupo de recursos de servidor que no se puede compartir con otros grupos de recursos de servidor. *value* es un entero con un valor predeterminado de 0. El intervalo permitido para *value* está comprendido entre 0 y 100.  
  
MAX_MEMORY_PERCENT =*value*    
Especifica la memoria total del servidor que puede ser utilizada por las solicitudes en este grupo de recursos de servidor. *value* es un entero con un valor predeterminado de 100. El intervalo permitido para *value* es de 1 a 100.  
  
MIN_IOPS_PER_VOLUME =*value*    
**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
Especifica el número mínimo de operaciones de E/S por segundo (IOPS) por volumen de disco que se deben reservar para el grupo de recursos de servidor. El intervalo permitido para *value* es de 0 a 2^31-1 (2.147.483.647). Especifique 0 si no desea indicar un umbral mínimo para el grupo. El valor predeterminado es 0.  
  
MAX_IOPS_PER_VOLUME =*value*    
**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
Especifica el número máximo de operaciones de E/S por segundo (IOPS) por volumen de disco que se deben permitir para el grupo de recursos de servidor. El intervalo permitido para *value* es de 0 a 2^31-1 (2.147.483.647). Especifique 0 para establecer un umbral ilimitado para el grupo. El valor predeterminado es 0.  
  
Si `MAX_IOPS_PER_VOLUME` se establece en 0 para un grupo, este no está regulado y puede tomar todas las IOPS del sistema, incluso aunque otros grupos tengan establecido un valor MIN_IOPS_PER_VOLUME. En este caso, se recomienda establecer el valor de `MAX_IOPS_PER_VOLUME` para este grupo en un número alto (por ejemplo, el valor máximo 2^31-1) si desea que se regule la E/S de este grupo.  
  
## <a name="remarks"></a>Observaciones  
`MIN_IOPS_PER_VOLUME` y `MAX_IOPS_PER_VOLUME` especifican el mínimo y el máximo de escrituras o escrituras por segundo. Estas lecturas o escrituras pueden ser de cualquier tamaño y no indican un rendimiento mínimo o máximo.  
  
Los valores de `MAX_CPU_PERCENT` y `MAX_MEMORY_PERCENT` deben ser mayores o igual que los valores de `MIN_CPU_PERCENT` y `MIN_MEMORY_PERCENT`, respectivamente.  
  
`CAP_CPU_PERCENT` difiere de `MAX_CPU_PERCENT` en que las cargas de trabajo asociadas al grupo pueden utilizar la capacidad de CPU existente por encima del valor de `MAX_CPU_PERCENT` si está disponible, pero sin sobrepasar el valor de `CAP_CPU_PERCENT`.  
  
El porcentaje total de la CPU para cada componente afín (programadores o nodos NUMA) no debe superar el 100 por ciento.  
  
## <a name="permissions"></a>Permisos  
Requiere el permiso `CONTROL SERVER`.  
  
## <a name="examples"></a>Ejemplos  
### <a name="1-shows-how-to-create-a-resource-pool"></a>1. Mostrar cómo crear un grupo de recursos

En este ejemplo se creó un grupo de recursos denominado "bigPool". Este grupo utiliza los valores predeterminados del regulador de recursos.  
  
```sql  
CREATE RESOURCE POOL bigPool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
### <a name="2-set-the-cap_cpu_percent-to-a-hard-cap-and-set-affinity-scheduler"></a>2. Establecer CAP_CPU_PERCENT en un límite máximo y establecer AFFINITY SCHEDULER

Establezca CAP_CPU_PERCENT en un límite máximo de 30 por ciento y establezca AFFINITY SCHEDULER en un intervalo de 0 a 63, 128 a 191. 
  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
```sql  
CREATE RESOURCE POOL PoolAdmin  
WITH (  
     MIN_CPU_PERCENT = 10,  
     MAX_CPU_PERCENT = 20,  
     CAP_CPU_PERCENT = 30,  
     AFFINITY SCHEDULER = (0 TO 63, 128 TO 191),  
     MIN_MEMORY_PERCENT = 5,  
     MAX_MEMORY_PERCENT = 15  
      );  
```  
  
### <a name="3-set-min_iops_per_volume-and-max_iops_per_volume"></a>3. Establecer MIN_IOPS_PER_VOLUME y MAX_IOPS_PER_VOLUME   

Establezca MIN_IOPS_PER_VOLUME en 20 y MAX_IOPS_PER_VOLUME en 100. Estos valores controlan las operaciones de lectura y escritura de E/S física disponibles para el grupo de recursos de servidor.  
  
**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.  
  
```sql  
CREATE RESOURCE POOL PoolAdmin  
WITH (  
    MIN_IOPS_PER_VOLUME = 20,  
    MAX_IOPS_PER_VOLUME = 100  
      );  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)     
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)     
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)     
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)     
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)     
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)     
 [Grupo de recursos de servidor del regulador de recursos](../../relational-databases/resource-governor/resource-governor-resource-pool.md)     
 [Crear un grupo de recursos de servidor](../../relational-databases/resource-governor/create-a-resource-pool.md)    
  
