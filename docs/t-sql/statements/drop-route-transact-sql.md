---
description: DROP ROUTE (Transact-SQL)
title: DROP ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP ROUTE
- DROP_ROUTE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dropping routes
- DROP ROUTE statement
- deleting routes
- routes [Service Broker], removing
- removing routes
ms.assetid: d8fab0bc-d54a-46ca-9437-552db7477d40
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fbc1e4543c23f64adf99642103a78dae7379c962
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188433"
---
# <a name="drop-route-transact-sql"></a>DROP ROUTE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita una ruta y elimina la información de la ruta de tabla de enrutamiento de la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP ROUTE route_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *route_name*  
 Es el nombre de la ruta que se va a quitar. No se pueden especificar nombres de servidor, base de datos o esquema.  
  
## <a name="remarks"></a>Comentarios  
 La tabla de enrutamiento que almacena las rutas es una tabla de metadatos que se puede leer a través de la vista de catálogo **sys.routes**. La tabla de enrutamiento solo se puede actualizar mediante las instrucciones CREATE ROUTE, ALTER ROUTE y DROP ROUTE.  
  
 Es posible quitar una ruta, aunque alguna conversación la utilice. No obstante, si no hay otra ruta para el servicio remoto, los mensajes de esas conversaciones permanecerán en la cola de transmisión hasta que se cree la ruta al servicio remoto o se exceda el tiempo de espera de la conversación.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, el permiso para quitar una ruta corresponde al propietario de la ruta, a los miembros de los roles fijos de base de datos db_ddladmin o db_owner, y a los miembros del rol fijo de servidor sysadmin.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se elimina la ruta `ExpenseRoute`.  
  
```sql  
DROP ROUTE ExpenseRoute ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-route-transact-sql.md)   
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.routes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-routes-transact-sql.md)  
  
  
