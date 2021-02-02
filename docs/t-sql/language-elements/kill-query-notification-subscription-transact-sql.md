---
title: KILL QUERY NOTIFICATION SUBSCRIPTION
description: Quite las suscripciones de notificación de consulta de una instancia. Esta instrucción puede quitar una suscripción específica o todas las suscripciones.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 07/27/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- KILL QUERY NOTIFICATION SUBSCRIPTION
- KILL_QUERY_NOTIFICATION_SUBSCRIPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- KILL QUERY NOTIFICATION SUBSCRIPTION statement
- removing subscriptions
- subscriptions [SQL Server query notifications], stopping
- query notifications [SQL Server], subscriptions
ms.assetid: 8aeadf51-286c-4748-bef2-d25858b250bf
author: cawrites
ms.author: chadam
ms.openlocfilehash: fb6fee6ecfbd2e71dd975ecc06fcc55785d20514
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99112335"
---
# <a name="kill-query-notification-subscription-transact-sql"></a>KILL QUERY NOTIFICATION SUBSCRIPTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita las suscripciones de notificación de consulta de la instancia. Esta instrucción puede quitar una suscripción específica o todas las suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
KILL QUERY NOTIFICATION SUBSCRIPTION   
   { ALL | subscription_id }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 ALL  
 Quita todas las suscripciones de la instancia.  
  
 *subscription_id*  
 Quita la suscripción con el identificador de suscripción *subscription_id*.  
  
## <a name="remarks"></a>Observaciones  
 La instrucción KILL QUERY NOTIFICATION SUBSCRIPTION quita las suscripciones de notificación de consulta sin generar ningún mensaje de notificación.  
  
 *subscription_id* es el identificador de la suscripción, como se muestra en la vista de administración dinámica [sys.dm_qn_subscriptions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/query-notifications-sys-dm-qn-subscriptions.md).  
  
 Si el Id. de suscripción especificado no existe, la instrucción genera un error.  
  
## <a name="permissions"></a>Permisos  
 El permiso de ejecución de esta instrucción está restringido a los miembros del rol fijo de servidor **sysadmin**.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-removing-all-query-notification-subscriptions-in-the-instance"></a>A. Quitar todas las suscripciones de notificación de consulta de la instancia  
 En el siguiente ejemplo se quitan todas las suscripciones de notificación de consulta de la instancia  
  
```sql  
KILL QUERY NOTIFICATION SUBSCRIPTION ALL ;  
```  
  
### <a name="b-removing-a-single-query-notification-subscription"></a>B. Quitar una sola suscripción de notificación de consulta  
 En el siguiente ejemplo se quita la suscripción de notificación de consulta con el Id. `73`.  
  
```sql  
KILL QUERY NOTIFICATION SUBSCRIPTION 73 ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_qn_subscriptions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/query-notifications-sys-dm-qn-subscriptions.md)  
  
  
