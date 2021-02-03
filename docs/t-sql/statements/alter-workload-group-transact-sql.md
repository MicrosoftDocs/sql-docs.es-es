---
description: ALTER WORKLOAD GROUP (Transact-SQL)
title: ALTER WORKLOAD GROUP (Transact-SQL)
ms.custom: ''
ms.date: 05/05/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_WORKLOAD_GROUP_TSQL
- ALTER WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER WORKLOAD GROUP statement
ms.assetid: 957addce-feb0-4e54-893e-5faca3cd184c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: 8f387c9ab6d67f11211107b4e7c44d38d23702cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199373"
---
# <a name="alter-workload-group-transact-sql"></a>ALTER WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server y SQL Managed Instance

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server y SQL Managed Instance

[!INCLUDE [ALTER WORKLOAD GROUP](../../includes/alter-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

Modifica un grupo de cargas de trabajo existente.

Vea la sección sobre el comportamiento de `ALTER WORKLOAD GROUP` a continuación para obtener más información sobre cómo se comporta `ALTER WORKLOAD GROUP` en un sistema con solicitudes en ejecución y en cola. 

Las restricciones aplicadas a [CREATE WORKLOAD GROUP](create-workload-group-transact-sql.md) también se aplican a `ALTER WORKLOAD GROUP`.  Antes de modificar los parámetros, consulte [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md) para asegurarse de que los valores se encuentren dentro de los rangos aceptables.

## <a name="syntax"></a>Sintaxis

```syntaxsql
ALTER WORKLOAD GROUP group_name
 WITH
 ([       MIN_PERCENTAGE_RESOURCE = value ]
  [ [ , ] CAP_PERCENTAGE_RESOURCE = value ]
  [ [ , ] REQUEST_MIN_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ] 
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]
  ```

## <a name="arguments"></a>Argumentos

group_name  
Es el nombre del grupo de cargas de trabajo definido por el usuario existente que se va a modificar.  group_name no es modificable. 

MIN_PERCENTAGE_RESOURCE = valor  
El valor es un entero comprendido entre 0 y 100.  Al modificar min_percentage_resource, la suma de min_percentage_resource en todos los grupos de cargas de trabajo no puede superar 100.  La modificación de min_percentage_resource requiere que todas las consultas en ejecución se completen en el grupo de cargas de trabajo antes de que se complete el comando.  Consulte la sección sobre el comportamiento de ALTER WORKLOAD GROUP de este documento para obtener más información.

CAP_PERCENTAGE_RESOURCE = valor  
El valor es un entero comprendido entre 1 y 100.  El valor de cap_percentage_resource debe ser mayor que min_percentage_resource.  La modificación de cap_percentage_resource requiere que todas las consultas en ejecución se completen en el grupo de cargas de trabajo antes de que se complete el comando.  Consulte la sección sobre el comportamiento de ALTER WORKLOAD GROUP de este documento para obtener más información. 

REQUEST_MIN_RESOURCE_GRANT_PERCENT = valor  
El valor es un decimal comprendido entre 0,75 y 100.  El valor de request_min_resource_grant_percent debe ser un divisor de min_percentage_resource y ser menor que cap_percentage_resource. 
  
REQUEST_MAX_RESOURCE_GRANT_PERCENT = valor  
El valor es un decimal y debe ser mayor que request_min_resource_grant_percent.

IMPORTANCE = { LOW \|  BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }  
Modifica la importancia predeterminada de una solicitud para el grupo de cargas de trabajo.

QUERY_EXECUTION_TIMEOUT_SEC = valor  
Modifica el tiempo máximo, en segundos, que se puede ejecutar una consulta antes de que se cancele. El valor debe ser 0 o un entero positivo. El valor predeterminado de value es 0, que indica una cantidad ilimitada.   

## <a name="permissions"></a>Permisos

Requiere el permiso CONTROL DATABASE

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se comprueban los valores de la vista de catálogo para wgDataLoads y se cambian los valores.

```sql
SELECT *
FROM sys.workload_management_workload_groups  
WHERE [name] = 'wgDataLoads'

ALTER WORKLOAD GROUP wgDataLoads WITH
( MIN_PERCENTAGE_RESOURCE            = 40
 ,CAP_PERCENTAGE_RESOURCE            = 80
 ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 10 )
 ```

## <a name="alter-workload-group-behavior"></a>Comportamiento de ALTER WORKLOAD GROUP

En todo momento hay tres tipos de solicitudes en el sistema:
- Solicitudes que todavía no se han clasificado.
- Solicitudes que están clasificadas y en espera para los bloqueos de objetos o los recursos del sistema.
- Solicitudes que están clasificadas y en ejecución.

En función de las propiedades del grupo de cargas de trabajo que se esté modificando, el momento en el que se aplique la configuración será diferente.

**Importance o query_execution_timeout** En el caso de las propiedades Importance y query_execution_timeout, las solicitudes non-classified obtienen los nuevos valores de configuración.  Las solicitudes en espera y en ejecución se ejecutan con la configuración anterior.  La solicitud `ALTER WORKLOAD GROUP` se ejecuta inmediatamente, sin tener en cuenta si hay consultas en ejecución en el grupo de cargas de trabajo.

**Request_min_resource_grant_percent o request_max_resource_grant_percent** En el caso de request_min_resource_grant_percent y request_max_resource_grant_percent, las solicitudes en ejecución se ejecutan con la configuración anterior.  Las solicitudes en espera y las no clasificadas obtienen los nuevos valores de configuración.  La solicitud `ALTER WORKLOAD GROUP` se ejecuta inmediatamente, sin tener en cuenta si hay consultas en ejecución en el grupo de cargas de trabajo.

**Min_percentage_resource o cap_percentage_resource** En el caso de min_percentage_resource y cap_percentage_resource, las solicitudes en ejecución se ejecutan con la configuración anterior.  Las solicitudes en espera y las no clasificadas obtienen los nuevos valores de configuración. 

Para cambiar min_percentage_resource y cap_percentage_resource, es necesario purgar las solicitudes en ejecución en el grupo de cargas de trabajo que se esté modificando.  Al reducir min_percentage_resource, los recursos liberados se devuelven al grupo de recursos compartidos, lo que permite que las solicitudes de otros grupos de cargas de trabajo puedan usarlos.  Por el contrario, al aumentar min_percentage_resource, se esperará hasta que se completen las solicitudes que usen solo los recursos necesarios del grupo compartido.  La operación ALTER WORKLOAD GROUP tendrá acceso prioritario a los recursos compartidos en comparación con otras solicitudes que estén en espera para su ejecución en el grupo compartido.  Si la suma de min_percentage_resource supera el 100 %, se producirá inmediatamente un error en la solicitud ALTER WORKLOAD GROUP. 

**Comportamiento de bloqueo** La modificación de un grupo de cargas de trabajo requiere un bloqueo global en todos los grupos de cargas de trabajo.  Se colocará una solicitud para modificar un grupo de cargas de trabajo detrás de las solicitudes de creación o anulación de grupos de carga de trabajo en la cola.  Si las instrucciones de modificación se envían en un lote, estas se procesarán siguiendo el orden de envío.  

## <a name="see-also"></a>Consulte también

- [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](create-workload-group-transact-sql.md)
- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](drop-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [Inicio rápido: Configuración del aislamiento de cargas de trabajo mediante T-SQL](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
