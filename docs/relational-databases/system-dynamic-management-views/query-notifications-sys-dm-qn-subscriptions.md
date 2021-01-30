---
description: Notificaciones de consulta-sys.dm_qn_subscriptions
title: sys.dm_qn_subscriptions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_qn_subscriptions
- dm_qn_subscriptions_TSQL
- sys.dm_qn_subscriptions
- sys.dm_qn_subscriptions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_qn_subscriptions dynamic management view
ms.assetid: a3040ce6-f5af-48fc-8835-c418912f830c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bd2a8ddda0c28dcfeea53e860926c626b78d4af3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158451"
---
# <a name="query-notifications---sysdm_qn_subscriptions"></a>Notificaciones de consulta-sys.dm_qn_subscriptions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de las suscripciones de notificaciones de consultas activas en el servidor. Puede usar esta vista para comprobar si hay suscripciones activas en el servidor o en una base de datos especificada, o para comprobar una entidad de seguridad de servidor especificada.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. de una suscripción.|  
|**database_id**|**int**|Id. de la base de datos en la que se ha ejecutado la notificación. Esta base de datos almacena información relacionada con esta suscripción.|  
|**Junction**|**varbinary(85)**|Id. de seguridad de la entidad de seguridad del servidor que creó y es propietaria de esta suscripción.|  
|**object_id**|**int**|Id. de la tabla interna que almacena información acerca de los parámetros de suscripción.|  
|**created**|**datetime**|Fecha y hora en que se creó la suscripción.|  
|**timeout**|**int**|Tiempo de espera de la suscripción en segundos. La notificación se marcará para activarse después de transcurrido este tiempo.<br /><br /> Nota: el tiempo de activación real puede ser mayor que el tiempo de espera especificado. Sin embargo, si se produce un cambio que invalida la suscripción después del tiempo de espera especificado, pero antes de que se desencadene la suscripción, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] garantiza que se produzca la activación en el momento en que se realizó el cambio.|  
|**status**|**int**|Indica el estado de la suscripción. Vea la tabla bajo las notas para obtener la lista de códigos.|  
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Por|Tipo|  
|----------|--------|--------|----------|  
|**sys.dm_qn_subscriptions**|**sys.databases**|**database_id**|Varios a uno|  
|**sys.dm_qn_subscriptions**|**sys.internal_tables**|**object_id**|Varios a uno|  
  
## <a name="remarks"></a>Observaciones  
 El código de estado 0 indica un estado indefinido.  
  
 Los siguientes códigos de estado indican que una suscripción se desencadenó debido a un cambio:  
  
|Código|Estado secundario|Información|  
|----------|------------------|----------|  
|65798|La suscripción se desencadenó porque los datos cambiaron|La suscripción se activó al realizar la inserción|  
|65799|La suscripción se desencadenó porque los datos cambiaron|Eliminar|  
|65800|La suscripción se desencadenó porque los datos cambiaron|Actualizar|  
|65801|La suscripción se desencadenó porque los datos cambiaron|Merge|  
|65802|La suscripción se desencadenó porque los datos cambiaron|Truncar tabla|  
|66048|La suscripción se desencadenó porque el tiempo de espera expiró|Modo de información indefinido|  
|66315|La suscripción se desencadenó porque el objeto cambió|Se quitó el objeto o el usuario|  
|66316|La suscripción se desencadenó porque el objeto cambió|Se modificó el objeto|  
|66565|La suscripción se desencadenó porque la base de datos se desasoció o se quitó|Se reinició el servidor o la base de datos|  
|66571|La suscripción se desencadenó porque la base de datos se desasoció o se quitó|Se quitó el objeto o el usuario|  
|66572|La suscripción se desencadenó porque la base de datos se desasoció o se quitó|Se modificó el objeto|  
|67341|La suscripción se desencadenó debido a la falta de recursos en el servidor|La suscripción se desencadenó debido a la falta de recursos en el servidor|  
  
 Los siguientes códigos de estado indican que una suscripción no pudo crearse:  
  
|Código|Estado secundario|Información|  
|----------|------------------|----------|  
|132609|Se produjo un error en la creación de la suscripción porque no se admite la instrucción|La consulta es demasiado compleja|  
|132610|Se produjo un error en la creación de la suscripción porque no se admite la instrucción|Instrucción no válida para la suscripción|  
|132611|Se produjo un error en la creación de la suscripción porque no se admite la instrucción|Se establecieron opciones no válidas para la suscripción|  
|132612|Se produjo un error en la creación de la suscripción porque no se admite la instrucción|Nivel de aislamiento no válido|  
|132622|Se produjo un error en la creación de la suscripción porque no se admite la instrucción|Se utiliza internamente|  
|132623|Se produjo un error en la creación de la suscripción porque no se admite la instrucción|Por encima del límite de plantilla por tabla|  
  
 Los siguientes códigos de estado se utilizan internamente y se clasifican como modos de iniciación y de comprobación de eliminación:  
  
|Código|Estado secundario|Información|  
|----------|------------------|----------|  
|198656|Se utiliza internamente: modos de iniciación y de comprobación de eliminación|Modo de información indefinido|  
|198928|La subscription se destruyó|La suscripción se desencadenó porque la base de datos se adjuntó|  
|198929|La subscription se destruyó|La suscripción se desencadenó porque se quitó el usuario|  
|198930|La subscription se destruyó|La suscripción se quitó debido a que se volvió a realizar la suscripción|  
|198931|La subscription se destruyó|Se eliminó la suscripción|  
|199168|La suscripción está activa|Modo de información indefinido|  
|199424|La suscripción se inicializó pero no aún no está activa|Modo de información indefinido|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW SERVER STATE en el servidor.  
  
> [!NOTE]  
>  Si el usuario no tiene el permiso VIEW SERVER STATE, esta vista devuelve información acerca de suscripciones que son propiedad del usuario actual.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-return-active-query-notification-subscriptions-for-the-current-user"></a>A. Devolver las suscripciones de notificación de consultas activas del usuario actual  
 En el ejemplo siguiente se devuelven las suscripciones de notificación de consultas activas del usuario actual. Si el usuario tiene permisos VIEW SERVER STATE, se devuelven todas las suscripciones activas del servidor.  
  
```  
SELECT id, database_id, sid, object_id, created, timeout, status  
FROM sys.dm_qn_subscriptions;  
GO  
```  
  
### <a name="b-returning-active-query-notification-subscriptions-for-a-specified-user"></a>B. Devolver las suscripciones de notificación de consultas activas del usuario especificado  
 En el ejemplo siguiente se devuelven las suscripciones de notificación de consultas activas suscritas por el inicio de sesión `Ruth0`.  
  
```  
SELECT id, database_id, sid, object_id, created, timeout, status  
FROM sys.dm_qn_subscriptions  
WHERE sid = SUSER_SID('Ruth0');  
GO  
```  
  
### <a name="c-returning-internal-table-metadata-for-query-notification-subscriptions"></a>C. Devolver los metadatos de las tablas internas para las suscripciones de notificación de consultas  
 En el ejemplo siguiente se devuelven los metadatos de la tabla interna para las suscripciones de notificación de consultas.  
  
```  
SELECT qn.id AS query_subscription_id  
    ,it.name AS internal_table_name  
    ,it.object_id AS internal_table_id  
FROM sys.internal_tables AS it  
JOIN sys.dm_qn_subscriptions AS qn ON it.object_id = qn.object_id  
WHERE it.internal_type_desc = 'QUERY_NOTIFICATION';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con notificaciones de consulta &#40;Transact-SQL&#41;](./system-dynamic-management-views.md)   
 [KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)  
  
