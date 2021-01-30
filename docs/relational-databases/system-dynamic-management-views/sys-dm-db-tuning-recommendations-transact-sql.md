---
title: sys.dm_db_tuning_recommendations (Transact-SQL) | Microsoft Docs
description: Obtenga información sobre cómo buscar posibles problemas de rendimiento y las correcciones recomendadas en SQL Server y Azure SQL Database
ms.custom: ''
ms.date: 07/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_tuning_recommendations
- dm_db_tuning_recommendations
- sys.dm_db_tuning_recommendations_TSQL
- dm_db_tuning_recommendations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database tuning recommendations feature [SQL Server], sys.dm_db_tuning_recommendations dynamic management view
- sys.dm_db_tuning_recommendations dynamic management view
ms.assetid: ced484ae-7c17-4613-a3f9-6d8aba65a110
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9037aaefe27cd50deb9b61af423a8074ab86f65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204807"
---
# <a name="sysdm_db_tuning_recommendations-transact-sql"></a>recomendaciones para la optimización de sys.dm \_ dB \_ \_ (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Devuelve información detallada sobre las recomendaciones de optimización.  
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], las vistas de administración dinámica no pueden exponer información que impactaría a la contención de la base de datos ni acerca de otras bases de datos a las que el usuario tenga acceso. Para evitar exponer esta información, se filtran todas las filas que contienen datos que no pertenecen al inquilino conectado.

| **Nombre de la columna** | **Tipo de datos** | **Descripción** |
| --- | --- | --- |
| **name** | **nvarchar(4000)** | Nombre único de la recomendación. |
| **type** | **nvarchar(4000)** | El nombre de la opción de ajuste automático que generó la recomendación, por ejemplo, `FORCE_LAST_GOOD_PLAN` |
| **reason** | **nvarchar(4000)** | Motivo por el que se proporcionó esta recomendación. |
| **válido \_ desde** | **datetime2** | La primera vez que se generó esta recomendación. |
| **última \_ actualización** | **datetime2** | Última vez que se generó esta recomendación. |
| **state** | **nvarchar(4000)** | Documento JSON que describe el estado de la recomendación. Los campos siguientes están disponibles:<br />-   `currentValue` : estado actual de la recomendación.<br />-   `reason` : constante que describe por qué la recomendación está en el estado actual.|
| **\_acción ejecutable de \_ is** | **bit** | 1 = la recomendación se puede ejecutar en la base de datos mediante un [!INCLUDE[tsql_md](../../includes/tsql-md.md)] script.<br />0 = la recomendación no se puede ejecutar en la base de datos (por ejemplo: solo información o recomendación revertida) |
| **\_acción revertida \_** | **bit** | 1 = la recomendación puede ser supervisada y revertida automáticamente por el motor de base de datos.<br />0 = la recomendación no se puede supervisar y revertir automáticamente. La mayoría de &quot; las acciones ejecutables serán &quot; &quot; revertidas &quot; . |
| **hora de inicio de la acción de ejecución \_ \_ \_** | **datetime2** | Fecha en que se aplica la recomendación. |
| **duración de la acción de ejecución \_ \_** | **time** | Duración de la acción de ejecución. |
| **ejecutar \_ acción \_ iniciada \_ por** | **nvarchar(4000)** | `User` = El usuario forzó manualmente el plan en la recomendación. <br /> `System` = El sistema aplica automáticamente la recomendación. |
| **ejecutar \_ la \_ hora de inicio de la acción \_** | **datetime2** | Fecha en que se aplicó la recomendación. |
| **revertir \_ la \_ hora de inicio de la acción \_** | **datetime2** | Fecha en que se ha revertido la recomendación. |
| **duración de la acción de reversión \_ \_** | **time** | Duración de la acción de reversión. |
| **revertir \_ acción \_ iniciada \_ por** | **nvarchar(4000)** | `User` = El usuario no ha aplicado manualmente el plan recomendado. <br /> `System` = El sistema revirtió automáticamente la recomendación. |
| **revertir \_ \_ hora de inicio \_ de la acción** | **datetime2** | Fecha en que se ha revertido la recomendación. |
| **score** | **int** | Valor estimado/impacto para esta recomendación en la escala 0-100 (cuanto mayor sea el mejor) |
| **indicaciones** | **nvarchar(max)** | Documento JSON que contiene más detalles sobre la recomendación. Los campos siguientes están disponibles:<br /><br />`planForceDetails`<br />-    `queryId` - \_ ID. de consulta de la consulta devuelta.<br />-    `regressedPlanId` -plan_id del plan con regresión.<br />-   `regressedPlanExecutionCount` -Número de ejecuciones de la consulta con el plan con regresión antes de que se detecte la regresión.<br />-    `regressedPlanAbortedCount` -Número de errores detectados durante la ejecución del plan de regresión.<br />-    `regressedPlanCpuTimeAverage` -Tiempo medio de CPU (en micro segundos) consumido por la consulta devuelta antes de que se detecte la regresión.<br />-    `regressedPlanCpuTimeStddev` : Desviación estándar del tiempo de CPU consumido por la consulta devuelta antes de que se detecte la regresión.<br />-    `recommendedPlanId` -plan_id del plan que se debe forzar.<br />-   `recommendedPlanExecutionCount`-Número de ejecuciones de la consulta con el plan que se deben forzar antes de que se detecte la regresión.<br />-    `recommendedPlanAbortedCount` -Número de errores detectados durante la ejecución del plan que deben forzarse.<br />-    `recommendedPlanCpuTimeAverage` -Tiempo medio de CPU (en micro segundos) consumido por la consulta ejecutada con el plan que se debe forzar (se calcula antes de que se detecte la regresión).<br />-    `recommendedPlanCpuTimeStddev` Desviación estándar del tiempo de CPU consumido por la consulta devuelta antes de que se detecte la regresión.<br /><br />`implementationDetails`<br />-  `method` : Método que se debe usar para corregir la regresión. El valor es siempre `TSql` .<br />-    `script` - [!INCLUDE[tsql_md](../../includes/tsql-md.md)] script que debe ejecutarse para forzar el plan recomendado. |
  
## <a name="remarks"></a>Observaciones  
 La información devuelta por `sys.dm_db_tuning_recommendations` se actualiza cuando el motor de base de datos identifica una posible regresión de rendimiento de consultas y no se conserva. Las recomendaciones solo se mantienen hasta que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se reinicia. Los administradores de bases de datos deben realizar periódicamente copias de seguridad de la recomendación de optimización si quieren mantenerlas después del reciclaje del servidor. 

 `currentValue` el campo de la `state` columna puede tener los valores siguientes:
 
 | Estado | Descripción |
 |--------|-------------|
 | `Active` | La recomendación está activa y aún no se ha aplicado. El usuario puede tomar el script de recomendación y ejecutarlo manualmente. |
 | `Verifying` | La recomendación se aplica mediante [!INCLUDE[ssde_md](../../includes/ssde_md.md)] y el proceso de comprobación interno compara el rendimiento del plan forzado con el plan con regresión. |
 | `Success` | La recomendación se aplicó correctamente. |
 | `Reverted` | La recomendación se revierte porque no hay ninguna mejora significativa del rendimiento. |
 | `Expired` | La recomendación ha expirado y ya no se puede aplicar. |

El documento JSON de la `state` columna contiene el motivo que describe por qué es la recomendación en el estado actual. Los valores del campo motivo pueden ser: 

| Motivo | Descripción |
|--------|-------------|
| `SchemaChanged` | Recomendación expirada porque se cambia el esquema de una tabla a la que se hace referencia. Se creará una nueva recomendación si se detecta una nueva regresión del plan de consulta en el nuevo esquema. |
| `StatisticsChanged`| Recomendación expirada debido al cambio de la estadística en una tabla a la que se hace referencia. Se creará una nueva recomendación si se detecta una nueva regresión del plan de consulta en función de las estadísticas nuevas. |
| `ForcingFailed` | No se puede forzar el plan recomendado en una consulta. Busque `last_force_failure_reason` en la vista [Sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md) para encontrar el motivo del error. |
| `AutomaticTuningOptionDisabled` | `FORCE_LAST_GOOD_PLAN` la opción está deshabilitada por el usuario durante el proceso de comprobación. Habilite `FORCE_LAST_GOOD_PLAN` la opción con [ALTER database SET AUTOMATIC_TUNING &#40;instrucción&#41;de TRANSACT-SQL ](../../t-sql/statements/alter-database-transact-sql-set-options.md) u fuerce el plan manualmente mediante el script de la `[details]` columna. |
| `UnsupportedStatementType` | No se puede forzar el plan en la consulta. Algunos ejemplos de consultas no admitidas son los cursores y la `INSERT BULK` instrucción. |
| `LastGoodPlanForced` | La recomendación se aplicó correctamente. |
| `AutomaticTuningOptionNotEnabled`| [!INCLUDE[ssde_md](../../includes/ssde_md.md)] se identificó una posible regresión de rendimiento, pero la `FORCE_LAST_GOOD_PLAN` opción no está habilitada; consulte [ALTER database SET AUTOMATIC_TUNING &#40;TRANSACT-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). Aplique la recomendación manualmente o habilite la `FORCE_LAST_GOOD_PLAN` opción. |
| `VerificationAborted`| El proceso de comprobación se ha anulado debido al reinicio o a la limpieza de Almacén de consultas. |
| `VerificationForcedQueryRecompile`| La consulta se vuelve a compilar porque no hay una mejora significativa del rendimiento. |
| `PlanForcedByUser`| El usuario forzó manualmente el plan mediante [sp_query_store_force_plan &#40;procedimiento de&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md) . El motor de base de datos no aplicará la recomendación si el usuario decidió forzar explícitamente algún plan. |
| `PlanUnforcedByUser` | El usuario no aplicó manualmente el plan mediante [sp_query_store_unforce_plan &#40;procedimiento de&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md) . Puesto que el usuario ha revertido explícitamente el plan recomendado, el motor de base de datos seguirá usando el plan actual y generará una nueva recomendación si se produce alguna regresión del plan en el futuro. |

 La estadística de la columna detalles no muestra las estadísticas del plan en tiempo de ejecución (por ejemplo, el tiempo de CPU actual). Los detalles de la recomendación se toman en el momento de la detección de regresión y describen por qué se ha [!INCLUDE[ssde_md](../../includes/ssde_md.md)] identificado la regresión de rendimiento. Use `regressedPlanId` y `recommendedPlanId` para consultar [almacén de consultas vistas de catálogo](../../relational-databases/performance/how-query-store-collects-data.md) para encontrar estadísticas exactas del plan en tiempo de ejecución.

## <a name="examples-of-using-tuning-recommendations-information"></a>Ejemplos de uso de información de recomendaciones de optimización  

### <a name="example-1"></a>Ejemplo 1
A continuación se obtiene el [!INCLUDE[tsql](../../includes/tsql-md.md)] script generado que fuerza un buen plan para una consulta determinada:  
 
```sql
SELECT name, reason, score,
    JSON_VALUE(details, '$.implementationDetails.script') AS script,
    details.* 
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON(details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressed_plan_id int '$.regressedPlanId',
            last_good_plan_id int '$.recommendedPlanId') AS details
WHERE JSON_VALUE(state, '$.currentValue') = 'Active';
```
### <a name="example-2"></a>Ejemplo 2
A continuación se obtiene el [!INCLUDE[tsql](../../includes/tsql-md.md)] script generado que fuerza un buen plan para una consulta determinada e información adicional sobre la ganancia estimada:

```sql
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                  *(regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
      error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON (Details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressedPlanId int '$.regressedPlanId',
            recommendedPlanId int '$.recommendedPlanId',
            regressedPlanErrorCount int,
            recommendedPlanErrorCount int,
            regressedPlanExecutionCount int,
            regressedPlanCpuTimeAverage float,
            recommendedPlanExecutionCount int,
            recommendedPlanCpuTimeAverage float
          ) AS planForceDetails;
```

### <a name="example-3"></a>Ejemplo 3
A continuación se obtiene el [!INCLUDE[tsql](../../includes/tsql-md.md)] script generado que fuerza un buen plan para una consulta determinada e información adicional que incluye el texto de la consulta y los planes de consulta almacenados en almacén de consultas:

```sql
WITH cte_db_tuning_recommendations
AS (SELECT reason,
        score,
        query_id,
        regressedPlanId,
        recommendedPlanId,
        current_state = JSON_VALUE(state, '$.currentValue'),
        current_state_reason = JSON_VALUE(state, '$.reason'),
        script = JSON_VALUE(details, '$.implementationDetails.script'),
        estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                * (regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
        error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
    FROM sys.dm_db_tuning_recommendations
    CROSS APPLY OPENJSON(Details, '$.planForceDetails')
    WITH ([query_id] int '$.queryId',
        regressedPlanId int '$.regressedPlanId',
        recommendedPlanId int '$.recommendedPlanId',
        regressedPlanErrorCount int,    
        recommendedPlanErrorCount int,
        regressedPlanExecutionCount int,
        regressedPlanCpuTimeAverage float,
        recommendedPlanExecutionCount int,
        recommendedPlanCpuTimeAverage float
        )
    )
SELECT qsq.query_id,
    qsqt.query_sql_text,
    dtr.*,
    CAST(rp.query_plan AS XML) AS RegressedPlan,
    CAST(sp.query_plan AS XML) AS SuggestedPlan
FROM cte_db_tuning_recommendations AS dtr
INNER JOIN sys.query_store_plan AS rp ON rp.query_id = dtr.query_id
    AND rp.plan_id = dtr.regressedPlanId
INNER JOIN sys.query_store_plan AS sp ON sp.query_id = dtr.query_id
    AND sp.plan_id = dtr.recommendedPlanId
INNER JOIN sys.query_store_query AS qsq ON qsq.query_id = rp.query_id
INNER JOIN sys.query_store_query_text AS qsqt ON qsqt.query_text_id = qsq.query_text_id;
```

Para obtener más información sobre las funciones JSON que se pueden usar para consultar valores en la vista recomendación, consulte [compatibilidad con JSON](../json/json-data-sql-server.md) en [!INCLUDE[ssde_md](../../includes/ssde_md.md)] .
  
## <a name="permissions"></a>Permisos  

Requiere el `VIEW SERVER STATE` permiso en [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] .   
Requiere el `VIEW DATABASE STATE` permiso para la base de datos en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .   

## <a name="see-also"></a>Consulte también  
 [Ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [sys.database_automatic_tuning_options &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)   
 [sys.database_query_store_options &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [Compatibilidad con JSON](../json/json-data-sql-server.md)
