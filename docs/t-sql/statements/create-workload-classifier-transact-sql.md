---
description: CREATE WORKLOAD CLASSIFIER (Transact-SQL)
title: CREATE WORKLOAD CLASSIFIER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/11/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- CREATE WORKLOAD CLASSIFIER
- CREATE_WORKLOAD_CLASSIFIER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD CLASSIFIER statement
ms.assetid: ''
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 5f592228f6570dfef5df91875ad60318806a1ad3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177862"
---
# <a name="create-workload-classifier-transact-sql"></a>CREATE WORKLOAD CLASSIFIER (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Crea un objeto clasificador para su uso en la administración de cargas de trabajo.  El clasificador asigna las solicitudes entrantes a un grupo de cargas de trabajo según los parámetros especificados en la definición de la instrucción del clasificador.  Los clasificadores se evalúan con cada solicitud enviada.  Si una solicitud no coincide con un clasificador, se asigna al grupo de cargas de trabajo predeterminado.  El grupo de cargas de trabajo predeterminado es la clase de recurso smallrc.

> [!NOTE]
> El clasificador de cargas de trabajo ocupa el lugar de la asignación de clase de recursos sp_addrolemember.  Después de crear los clasificadores de carga de trabajo, ejecute sp_droprolemember para quitar las asignaciones de clases de recursos redundantes.

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE WORKLOAD CLASSIFIER classifier_name  
WITH  
    (   WORKLOAD_GROUP = 'name'  
    ,   MEMBERNAME = 'security_account' 
[ [ , ] WLM_LABEL = 'label' ]  
[ [ , ] WLM_CONTEXT = 'context' ]  
[ [ , ] START_TIME = 'HH:MM' ]  
[ [ , ] END_TIME = 'HH:MM' ]  
  
[ [ , ] IMPORTANCE = { LOW | BELOW_NORMAL | NORMAL | ABOVE_NORMAL | HIGH }]) 
[;]
```
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumentos

 *classifier_name*  
 Especifica el nombre por el que se identifica el clasificador de carga de trabajo.  "classifier_name" es un elemento "sysname".  Puede tener hasta 128 caracteres y debe ser único en la instancia.

 *WORKLOAD_GROUP* =  *'name'*    
 Si se cumplen las condiciones en las reglas de clasificador, "name" asigna la solicitud a un grupo de cargas de trabajo.  "name" es un elemento "sysname".  Puede tener hasta 128 caracteres y debe ser un nombre de grupo de cargas de trabajo válido en el momento de crear el clasificador.

 Los grupos de cargas de trabajo disponibles se pueden encontrar en la vista de catálogo [sys.workload_management_workload_groups](../../relational-databases/system-catalog-views/sys-workload-management-workload-groups-transact-sql.md).

 *MEMBERNAME* =  *"cuenta_de_seguridad"*     
 Cuenta de seguridad que se usa para la clasificación.  "Security_account" es un elemento "sysname" y no tiene ningún valor predeterminado. "Security_account" puede ser un usuario o un rol de base de datos, el inicio de sesión de Azure Active Directory o un grupo de Azure Active Directory.
 
 *WLM_LABEL*   
 Especifica el valor de etiqueta con el que se puede clasificar una solicitud.  La etiqueta es un parámetro opcional de tipo nvarchar(255).  Use [OPTION (LABEL)](/azure/sql-data-warehouse/sql-data-warehouse-develop-label) en la solicitud para que coincida con la configuración del clasificador.

Ejemplo:

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'  
 ,WLM_LABEL      = 'dimension_loads' )

SELECT COUNT(*) 
  FROM DimCustomer
  OPTION (LABEL = 'dimension_loads')
```

*WLM_CONTEXT*  
Especifica el valor de contexto de sesión con el que se puede clasificar una solicitud.  El contexto es un parámetro opcional de tipo nvarchar(255).  Use [sys.sp_set_session_context](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) con el nombre de la variable igual a `wlm_context` antes de enviar una solicitud para establecer el contexto de la sesión.

Ejemplo:

```sql
CREATE WORKLOAD CLASSIFIER wcDataLoad WITH  
( WORKLOAD_GROUP = 'wgDataLoad'
 ,MEMBERNAME     = 'ELTRole'
 ,WLM_CONTEXT    = 'dim_load' )
 
--set session context
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = 'dim_load'

--run multiple statements using the wlm_context setting
SELECT COUNT(*) FROM stg.daily_customer_load
SELECT COUNT(*) FROM stg.daily_sales_load

--turn off the wlm_context session setting
EXEC sys.sp_set_session_context @key = 'wlm_context', @value = null
```

*START_TIME* y *END_TIME*  
Especifica el valor de hora de inicio y fin con el que se puede clasificar una solicitud.  Tanto start_time como end_time tienen el formato HH:MM en la zona horaria UTC.  Start_time y end_time deben especificarse juntos.

Ejemplo:

```sql
CREATE WORKLOAD CLASSIFIER wcELTLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoads'
 ,MEMBERNAME     = 'ELTRole'  
 ,START_TIME     = '22:00'
 ,END_TIME       = '02:00' )
```

*IMPORTANCE* = { LOW \| BELOW_NORMAL \| NORMAL \| ABOVE_NORMAL \| HIGH }  
Especifica la importancia relativa de una solicitud.  IMPORTANCE puede ser es uno de los siguientes valores:

- LOW
- BELOW_NORMAL
- NORMAL (valor predeterminado)
- ABOVE_NORMAL
- HIGH  

Si no se especifica la importancia, se usa la configuración de importancia del grupo de cargas de trabajo.  La importancia predeterminada del grupo de cargas de trabajo es Normal.  La importancia influye en el orden en el que se programan las solicitudes, lo que da el primer acceso a los recursos y bloqueos.

## <a name="classification-parameter-weighting"></a>Ponderación de los parámetros de clasificación

Una solicitud puede coincidir con varios clasificadores.  Existe una ponderación en los parámetros de clasificadores.  El clasificador de mayor peso coincidente se usa para asignar un grupo de cargas de trabajo y una importancia.  La ponderación es como se indica a continuación:

|Parámetro clasificador |Peso   |
|---------------------|---------|
|USER                 |64       |
|ROLE                 |32       |
|WLM_LABEL            |16       |
|WLM_CONTEXT          |8        |
|START_TIME/END_TIME  |4        |

Tenga en cuenta las siguientes configuraciones de clasificador.

```sql
CREATE WORKLOAD CLASSIFIER classifierA WITH  
( WORKLOAD_GROUP = 'wgDashboards'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = HIGH
 ,WLM_LABEL      = 'salereport' )

CREATE WORKLOAD CLASSIFIER classifierB WITH  
( WORKLOAD_GROUP = 'wgUserQueries'  
 ,MEMBERNAME     = 'userloginA'
 ,IMPORTANCE     = LOW
 ,START_TIME     = '18:00'
 ,END_TIME       = '07:00' )
```

El `userloginA` del usuario está configurado para ambos clasificadores.  Si userloginA ejecuta una consulta con una etiqueta igual a `salesreport` entre las 18:00 y las 07:00 UTC, la solicitud se clasificará en el grupo de cargas de trabajo wgDashboards con importancia ALTA.  Puede que la expectativa fuese clasificar la solicitud en wgUserQueries con importancia BAJA para los informes fuera de las horas de trabajo, pero el peso de WLM_LABEL es superior a START_TIME/END_TIME.  La ponderación de classifierA es 80 (64 para el usuario, más 16 para WLM_LABEL).  La ponderación de classifierB es 68 (64 para el usuario, 4 para START_TIME/END_TIME).  En este caso, puede agregar WLM_LABEL a classifierB.

 Para obtener más información, vea la [ponderación de las cargas de trabajo](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification#classification-weighting).

## <a name="permissions"></a>Permisos

 Requiere el permiso CONTROL DATABASE.  
  
## <a name="examples"></a>Ejemplos

 En el ejemplo siguiente se muestra cómo crear un clasificador de cargas de trabajo denominado `wgcELTRole`. Usa el grupo de cargas de trabajo staticrc20, el usuario `ELTRole` y establece la importancia en `above_normal`.

```sql
CREATE WORKLOAD CLASSIFIER wgcELTRole
  WITH (WORKLOAD_GROUP = 'staticrc20'
       ,MEMBERNAME = 'ELTRole'
      ,IMPORTANCE = above_normal);
```

## <a name="see-also"></a>Consulte también

[Clasificación de [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)</br>
[DROP WORKLOAD CLASSIFIER (Transact-SQL)](../../t-sql/statements/drop-workload-classifier-transact-sql.md)</br>
[sys.workload_management_workload_classifiers](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifiers-transact-sql.md)</br>
[sys.workload_management_workload_classifier_details](../../relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql.md)

