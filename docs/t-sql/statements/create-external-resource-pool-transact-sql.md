---
description: CREATE EXTERNAL RESOURCE POOL (Transact-SQL)
title: CREATE EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- CREATE EXTERNAL RESOURCE POOL
- CREATE EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE POOL
- EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE
- EXTERNAL_RESOURCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL RESOURCE POOL statement
ms.assetid: 8cc798ad-c395-461c-b7ff-8c561c098808
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 20277ddde605ff45e00a913e0e5366e35f25fe43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179145"
---
# <a name="create-external-resource-pool-transact-sql"></a>CREATE EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Crea un grupo externo para definir los recursos de los procesos externos. Un grupo de recursos de servidor representa un subconjunto de los recursos físicos (memoria y CPU) de una instancia del motor de base de datos. Un Regulador de recursos puede distribuir los recursos del servidor entre los grupos de recursos, hasta un máximo de 64 grupos.

::: moniker range="=sql-server-2016"
En [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] en [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], el grupo externo rige `rterm.exe`, `BxlServer.exe` y otros procesos generados por ellos.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
En [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)], el grupo externo rige `rterm.exe`, `python.exe`, `BxlServer.exe` y otros procesos generados por ellos.
::: moniker-end
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
 

## <a name="syntax"></a>Sintaxis  
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"

```syntaxsql
CREATE EXTERNAL RESOURCE POOL pool_name  
[ WITH (  
    [ MAX_CPU_PERCENT = value ]  
    [ [ , ] MAX_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_PROCESSES = value ]   
    )   
]  
[ ; ]  
  
<CPU_range_spec> ::=    
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]  
```  
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```syntaxsql
CREATE EXTERNAL RESOURCE POOL pool_name  
[ WITH (  
    [ MAX_CPU_PERCENT = value ]  
    [ [ , ] AFFINITY CPU =    
            {  
                AUTO   
              | ( <cpu_range_spec> )   
              | NUMANODE = ( <NUMA_node_id> )   
            } ]   
    [ [ , ] MAX_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_PROCESSES = value ]   
    )   
]  
[ ; ]  
  
<CPU_range_spec> ::=    
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]  
```  
::: moniker-end

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*pool_name*  
Es el nombre definido por el usuario para identificar el grupo de recursos externos. *pool_name* es alfanumérico y puede tener hasta 128 caracteres. Este argumento debe ser únicos en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
MAX_CPU_PERCENT =*value*  
El promedio máximo de ancho de banda de CPU que pueden recibir todas las solicitudes en el grupo de recursos externos cuando haya contención de CPU. *value* es un valor entero. El intervalo permitido para *value* es de 1 a 100.


MAX_MEMORY_PERCENT =*value*  
Especifica la memoria total del servidor que puede ser usada por las solicitudes en este grupo de recursos externos. *value* es un valor entero. El intervalo permitido para *value* es de 1 a 100.

MAX_PROCESSES =*value*  
El número máximo de procesos permitidos para el grupo de recursos externos. 0 = umbral ilimitado para el grupo, que estará enlazado solamente por recursos del equipo.
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
MAX_CPU_PERCENT =*value*  
El promedio máximo de ancho de banda de CPU que pueden recibir todas las solicitudes en el grupo de recursos externos cuando haya contención de CPU. *value* es un valor entero. El intervalo permitido para *value* es de 1 a 100.

AFFINITY {CPU = AUTO | ( <CPU_range_spec>) | NUMANODE = (\<NUMA_node_range_spec>)} Adjunte el grupo de recursos externos a CPU específicas.

AFFINITY CPU = **(** <CPU_range_spec> **)** asigna el grupo de recursos externos a las CPU de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificadas por los CPU_ID dados.

Cuando usa AFFINITY NUMANODE = **(\<NUMA_node_range_spec> **)** , se establece una afinidad entre el grupo de recursos externos y las CPU físicas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondientes al nodo o al intervalo de nodos NUMA especificado. 

MAX_MEMORY_PERCENT =*value*  
Especifica la memoria total del servidor que puede ser usada por las solicitudes en este grupo de recursos externos. *value* es un valor entero. El intervalo permitido para *value* es de 1 a 100.

MAX_PROCESSES =*value*  
El número máximo de procesos permitidos para el grupo de recursos externos. 0 = umbral ilimitado para el grupo, que estará enlazado solamente por recursos del equipo.
::: moniker-end

## <a name="remarks"></a>Observaciones

El [!INCLUDE[ssDE](../../includes/ssde-md.md)] implementa el grupo de recursos al ejecutar la instrucción [ALTER RESOURCE GOVERNOR RECONFIGURE](../../t-sql/statements/alter-resource-governor-transact-sql.md).

Para obtener información general sobre los grupos de recursos, vea [Grupo de recursos de servidor del regulador de recursos](../../relational-databases/resource-governor/resource-governor-resource-pool.md), [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) y [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md).

Para obtener información específica sobre cómo administrar los grupos de recursos externos usados en el aprendizaje automático, vea [Resource governance for machine learning in SQL Server](../../machine-learning/administration/resource-governor.md) (Gobernanza de recursos para aprendizaje automático en SQL Server). 

## <a name="permissions"></a>Permisos

Requiere el permiso `CONTROL SERVER`.

## <a name="examples"></a>Ejemplos

El grupo externo ha restringido el uso de CPU al 75 por ciento. La cantidad máxima de memoria es el 30 por ciento de la memoria disponible en el equipo.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```sql
CREATE EXTERNAL RESOURCE POOL ep_1
WITH (  
    MAX_CPU_PERCENT = 75
    , MAX_MEMORY_PERCENT = 30
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```sql
CREATE EXTERNAL RESOURCE POOL ep_1
WITH (  
    MAX_CPU_PERCENT = 75
    , AFFINITY CPU = AUTO
    , MAX_MEMORY_PERCENT = 30
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

## <a name="see-also"></a>Consulte también

+ [Opción de configuración de servidor Scripts externos habilitados](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [sp_execute_external_script &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
+ [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
+ [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)
+ [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)
+ [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)
+ [Grupo de recursos de servidor del regulador de recursos](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
+ [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)
+ [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)
+ [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)
