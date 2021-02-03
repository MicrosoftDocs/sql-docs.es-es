---
description: CREATE WORKLOAD GROUP (Transact-SQL)
title: CREATE WORKLOAD GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/27/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WORKLOAD GROUP
- WORKLOAD_GROUP_TSQL
- CREATE WORKLOAD GROUP
- CREATE_WORKLOAD_GROUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD GROUP statement
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current'
ms.openlocfilehash: f4af023d2b59662117f8db05763ec1f7e237eb38
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177852"
---
# <a name="create-workload-group-transact-sql"></a>CREATE WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server y SQL Managed Instance

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]
  
::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-workload-group-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-and-sql-managed-instance"></a>SQL Server y SQL Managed Instance

[!INCLUDE [CREATE WORKLOAD GROUP](../../includes/create-workload-group.md)]

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-workload-group-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](create-workload-group-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

Crea un grupo de cargas de trabajo. Los grupos de cargas de trabajo son contenedores de un conjunto de solicitudes y son la base de la configuración de la administración de cargas de trabajo en un sistema. Los grupos de cargas de trabajo proporcionan la capacidad de reservar recursos para el aislamiento de la carga de trabajo, contienen recursos, definen recursos por solicitud y cumplen las reglas de ejecución. Una vez completada la instrucción, la configuración entra en vigor.

:::image type="icon" source="../../database-engine/configure-windows/media/topic-link.gif"::: [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

```syntaxsql
CREATE WORKLOAD GROUP group_name
 WITH
 (   MIN_PERCENTAGE_RESOURCE = value 
   , CAP_PERCENTAGE_RESOURCE = value 
   , REQUEST_MIN_RESOURCE_GRANT_PERCENT = value
  [ [ , ] REQUEST_MAX_RESOURCE_GRANT_PERCENT = value ]
  [ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH } ]
  [ [ , ] QUERY_EXECUTION_TIMEOUT_SEC = value ] )
  [ ; ]

```

*group_name*</br>
Especifica el nombre por el que se identifica el grupo de cargas de trabajo. *group_name* es sysname. Puede tener hasta 128 caracteres y debe ser único en la instancia.

*MIN_PERCENTAGE_RESOURCE* = value</br>
Especifica una asignación de recursos mínima garantizada para este grupo de cargas de trabajo que no se comparte con otros grupos de cargas de trabajo. *value* es un entero comprendido entre 0 y 100. La suma de min_percentage_resource en todos los grupos de cargas de trabajo no puede ser superior a 100. El valor de min_percentage_resource no puede ser mayor que cap_percentage_resource. Hay valores efectivos mínimos permitidos por nivel de servicio. Vea [Valores efectivos](#effective-values) para obtener más detalles.

*CAP_PERCENTAGE_RESOURCE* = value</br>
Especifica el uso máximo de recursos para todas las solicitudes de un grupo de cargas de trabajo. El intervalo de enteros permitido para value es de 1 a 100. El valor de cap_percentage_resource debe ser mayor que min_percentage_resource. Se puede reducir el valor efectivo de cap_percentage_resource si min_percentage_resource se configura mayor que cero en otros grupos de cargas de trabajo.

*REQUEST_MIN_RESOURCE_GRANT_PERCENT* = value</br>
Establece la cantidad mínima de recursos asignados por solicitud. *value* es un parámetro necesario con un número decimal comprendido entre 0,75 y 100,00. El valor de request_min_resource_grant_percent debe ser un múltiplo de 0,25, debe ser un factor de min_percentage_resource y ser menor que cap_percentage_resource. Hay valores efectivos mínimos permitidos por nivel de servicio. Vea [Valores efectivos](#effective-values) para obtener más detalles.

Por ejemplo:

```sql
CREATE WORKLOAD GROUP wgSample 
WITH
  ( MIN_PERCENTAGE_RESOURCE = 26                -- integer value
    , REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3.25 -- factor of 26 (guaranteed a minimum of 8 concurrency)
    , CAP_PERCENTAGE_RESOURCE = 100 )
```

Tenga en cuenta los valores que se usan para las clases de recursos como directriz para request_min_resource_grant_percent.  La tabla siguiente contiene las asignaciones de recursos para Gen2.

|Clase de recurso|Porcentaje de recursos|
|---|---|
|Smallrc|3 %|
|Mediumrc|10 %|
|Largerc|22 %|
|Xlargerc|70%|
|||

*REQUEST_MAX_RESOURCE_GRANT_PERCENT* = value</br>         
Establece la cantidad máxima de recursos asignados por solicitud. *value* es un parámetro decimal opcional con un valor predeterminado igual a request_min_resource_grant_percent. *value* debe ser mayor o igual que request_min_resource_grant_percent. Cuando el valor de request_max_resource_grant_percent es mayor que request_min_resource_grant_percent y los recursos del sistema están disponibles, se asignan recursos adicionales a una solicitud.

*IMPORTANCE* = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }</br>        
Especifica la importancia predeterminada de una solicitud para el grupo de cargas de trabajo. Importance puede ser una de las siguientes, siendo NORMAL el valor predeterminado:

- LOW
- BELOW_NORMAL
- NORMAL (valor predeterminado)
- ABOVE_NORMAL
- HIGH

La importancia establecida en el grupo de cargas de trabajo está predeterminada para todas las solicitudes en el grupo de cargas de trabajo. Un usuario también puede establecer la importancia en el nivel de clasificador, lo que puede invalidar la configuración de importancia del grupo de cargas de trabajo. Esto permite diferenciar la importancia de las solicitudes de un grupo de cargas de trabajo para acceder a los recursos no reservados más rápido. Cuando la suma de min_percentage_resource entre grupos de cargas de trabajo es inferior a 100, hay recursos no reservados que se asignan en función de la importancia.

*QUERY_EXECUTION_TIMEOUT_SEC* = value</br>     
Especifica el tiempo máximo, en segundos, que se puede ejecutar una consulta antes de que se cancele. *valor* debe ser 0 o un entero positivo. El valor predeterminado de value es 0, que indica que no se agota nunca el tiempo de espera de la consulta. QUERY_EXECUTION_TIMEOUT_SEC empieza el recuento una vez que la consulta se encuentra en estado de ejecución, y no cuando la consulta se pone en cola.

## <a name="remarks"></a>Observaciones

Los grupos de cargas de trabajo correspondientes a las clases de recursos se crean automáticamente para favorecer la compatibilidad con versiones anteriores. Estos grupos de cargas de trabajo definidos por el sistema no se pueden quitar. Se pueden crear otros 8 grupos de cargas de trabajo definidos por el usuario.

Si se crea un grupo de cargas de trabajo con `min_percentage_resource` mayor que cero, la instrucción `CREATE WORKLOAD GROUP` se pondrá en cola hasta que haya suficientes recursos para crear el grupo de cargas de trabajo.

## <a name="effective-values"></a>Valores efectivos

Los parámetros `min_percentage_resource`, `cap_percentage_resource`, `request_min_resource_grant_percent` y `request_max_resource_grant_percent` tienen valores efectivos que se ajustan en el contexto del nivel de servicio actual y la configuración de otros grupos de cargas de trabajo.

El parámetro `request_min_resource_grant_percent` tiene un valor efectivo porque se requieren los mínimos recursos por consulta según el nivel de servicio.  Por ejemplo, en el nivel de servicio más bajo, DW100c, se necesita un mínimo del 25 % de recursos por solicitud.  Si el grupo de cargas de trabajo está configurado con el 3 % en `request_min_resource_grant_percent` y `request_max_resource_grant_percent`, los valores efectivos de ambos parámetros se ajustan al 25 % cuando se inicia la instancia.  Si la instancia se escala verticalmente a Dw1000c y los valores configurados y efectivos para ambos parámetros son un 3 %, porque el 3 % es el valor mínimo admitido en ese nivel de servicio.  Si la instancia se escala a más de Dw1000c, los valores configurados y efectivos para ambos parámetros permanecerán en el 3 %.  Vea la siguiente tabla para obtener más información sobre los valores efectivos en los diferentes niveles de servicio.

|Nivel de servicio|Valor efectivo más bajo para REQUEST_MIN_RESOURCE_GRANT_PERCENT|Número máximo de consultas concurrentes|
|---|---|---|
|DW100c|25 %|4|
|DW200c|12,5 %|8|
|DW300c|8 %|12|
|DW400c|6,25 %|16|
|DW500c|5 %|20|
|DW1000c|3 %|32|
|DW1500c|3 %|32|
|DW2000c|2 %|48|
|DW2500c|2 %|48|
|DW3000c|1,5 %|64|
|DW5000c|1,5 %|64|
|DW6000c|0,75 %|128|
|DW7500c|0,75 %|128|
|DW10000c|0,75 %|128|
|DW15000c|0,75 %|128|
|DW30000c|0,75 %|128|
||||

El parámetro `min_percentage_resource` debe ser mayor que el valor efectivo de `request_min_resource_grant_percent` o igual a dicho valor. Un grupo de cargas de trabajo con un valor de `min_percentage_resource` configurado menor que el valor efectivo de `min_percentage_resource` tiene el valor ajustado en cero en tiempo de ejecución. Cuando esto sucede, los recursos configurados para `min_percentage_resource` se pueden compartir entre todos los grupos de cargas de trabajo. Por ejemplo, el grupo de cargas de trabajo `wgAdHoc` con un valor de `min_percentage_resource` del 10 % que se ejecuta en DW1000c tendría un valor efectivo de `min_percentage_resource` del 10 % (3 % es el valor mínimo admitido en DW1000c). `wgAdhoc` en DW100c tendría un valor de min_percentage_resource efectivo del 0 %. El 10 % configurado para `wgAdhoc` se compartiría entre todos los grupos de cargas de trabajo.

El parámetro `cap_percentage_resource` también tiene un valor efectivo. Si un grupo de cargas de trabajo `wgAdhoc` se configura con un valor de `cap_percentage_resource` del 100 % y se crea otro grupo de cargas de trabajo `wgDashboards` con un valor de `min_percentage_resource` del 25 %, el valor de `cap_percentage_resource` efectivo para `wgAdhoc` pasa a ser del 75 %.

La forma más fácil de comprender los valores en tiempo de ejecución de los grupos de cargas de trabajo consiste en consultar la vista del sistema [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md).

## <a name="permissions"></a>Permisos

Requiere el permiso `CONTROL DATABASE`.

## <a name="see-also"></a>Consulte también

- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](drop-workload-group-transact-sql.md)
- [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](alter-workload-group-transact-sql.md)
- [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md)
- [sys.dm_workload_management_workload_groups_stats](../../relational-databases/system-dynamic-management-views/sys-dm-workload-management-workload-group-stats-transact-sql.md)
- [Inicio rápido: Configuración del aislamiento de cargas de trabajo mediante T-SQL](/azure/sql-data-warehouse/quickstart-configure-workload-isolation-tsql)

::: moniker-end
