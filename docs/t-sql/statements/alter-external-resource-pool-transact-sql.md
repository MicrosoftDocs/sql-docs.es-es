---
description: ALTER EXTERNAL RESOURCE POOL (Transact-SQL)
title: ALTER EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- ALTER_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER EXTERNAL RESOURCE POOL statement
ms.assetid: 634c327d-971b-49ba-b8a2-e243a04040db
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 13cceefecf9d5c67cfde6cd10ec1b86fc564ebf6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204215"
---
# <a name="alter-external-resource-pool-transact-sql"></a>ALTER EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Modifica un grupo externo de Resource Governor que especifica los recursos que pueden usarse en procesos externos. 

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
ALTER EXTERNAL RESOURCE POOL { pool_name | "default" }
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

ALTER EXTERNAL RESOURCE POOL { pool_name | "default" }
[ WITH (
    [ MAX_CPU_PERCENT = value ]
    [ [ , ] AFFINITY CPU =
            {
                AUTO
              | ( <cpu_range_spec> )
              | NUMANODE = (( <NUMA_node_id> )
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

{ *pool_name* | "default" }  
Es el nombre de un grupo de recursos externos definidos por el usuario ya existente o el grupo de recursos externo predeterminado creado al instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
"default" debe ir entre comillas ("") o corchetes ([]) si se usa con `ALTER EXTERNAL RESOURCE POOL` para evitar el conflicto con `DEFAULT`, que es una palabra reservada del sistema.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
MAX_CPU_PERCENT =*value*  
Especifica el promedio máximo de ancho de banda de CPU que pueden recibir todas las solicitudes en el grupo de recursos externo cuando haya contención de CPU. *value* es un valor entero. El intervalo permitido para *value* es de 1 a 100.

MAX_MEMORY_PERCENT =*value*  
Especifica la memoria total del servidor que puede ser usada por las solicitudes en este grupo de recursos externos. *value* es un valor entero. El intervalo permitido para *value* es de 1 a 100.

MAX_PROCESSES =*value*  
Especifica el número máximo de procesos permitidos para el grupo de recursos externos. Especifique 0 para establecer un umbral ilimitado para el grupo, que estará enlazado solamente por recursos del equipo.
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
MAX_CPU_PERCENT =*value*  
Especifica el promedio máximo de ancho de banda de CPU que pueden recibir todas las solicitudes en el grupo de recursos externo cuando haya contención de CPU. *value* es un valor entero. El intervalo permitido para *value* es de 1 a 100.

AFFINITY {CPU = AUTO | ( \<CPU_range_spec> ) | NUMANODE = (\<NUMA_node_range_spec>)}  
Adjunte el grupo de recursos externos a las CPU específicas.

AFFINITY CPU = **(** \<CPU_range_spec> **)** asigna el grupo de recursos externos a las CPU de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identificadas por los valores CPU_ID dados. Cuando se usa AFFINITY NUMANODE = **(** \<NUMA_node_range_spec> **)** , se establece una afinidad entre el grupo de recursos externos y las CPU físicas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondientes al nodo o al intervalo de nodos NUMA especificado.

MAX_MEMORY_PERCENT =*value*  
Especifica la memoria total del servidor que puede ser usada por las solicitudes en este grupo de recursos externos. *value* es un valor entero. El intervalo permitido para *value* es de 1 a 100.

MAX_PROCESSES =*value*  
Especifica el número máximo de procesos permitidos para el grupo de recursos externos. Especifique 0 para establecer un umbral ilimitado para el grupo, que estará enlazado solamente por recursos del equipo.
::: moniker-end
## <a name="remarks"></a>Observaciones

El [!INCLUDE[ssDE](../../includes/ssde-md.md)] implementa el grupo de recursos al ejecutar la instrucción [ALTER RESOURCE GOVERNOR RECONFIGURE](../../t-sql/statements/alter-resource-governor-transact-sql.md).

Para obtener información general sobre los grupos de recursos, vea [Grupo de recursos de servidor del regulador de recursos](../../relational-databases/resource-governor/resource-governor-resource-pool.md), [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) y [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md).  

Para obtener información específica sobre el uso de grupos de recursos externos para controlar trabajos de aprendizaje automático, vea [Resource governance for machine learning in SQL Server](../../machine-learning/administration/resource-governor.md) (Gobernanza de recursos para aprendizaje automático en SQL Server).
## <a name="permissions"></a>Permisos

Requiere el permiso `CONTROL SERVER`.

## <a name="examples"></a>Ejemplos

La instrucción siguiente cambia un grupo externo, mediante la restricción del uso de CPU al 50 % y la memoria máxima al 25 % de la memoria disponible en el equipo.
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```sql
ALTER EXTERNAL RESOURCE POOL ep_1
WITH (
    MAX_CPU_PERCENT = 50
    , MAX_MEMORY_PERCENT = 25
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```sql
ALTER EXTERNAL RESOURCE POOL ep_1
WITH (
    MAX_CPU_PERCENT = 50
    , AFFINITY CPU = AUTO
    , MAX_MEMORY_PERCENT = 25
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

## <a name="see-also"></a>Consulte también

+ [Resource governance for machine learning in SQL Server](../../machine-learning/administration/resource-governor.md) (Gobernanza de recursos para aprendizaje automático en SQL Server)
+ [Opción de configuración de servidor Scripts externos habilitados](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)
+ [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)
+ [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)
+ [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)
+ [Grupo de recursos de servidor del regulador de recursos](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
+ [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md) 
