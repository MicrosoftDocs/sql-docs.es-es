---
description: DROP RESOURCE POOL (Transact-SQL)
title: DROP RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP RESOURCE POOL
- DROP_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP RESOURCE POOL
ms.assetid: 18cd6dd9-7a6d-4a08-b9d5-649af23583d5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 88d29f1e9e252c09c6abc0e41ced89ef6f5f6b30
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188445"
---
# <a name="drop-resource-pool-transact-sql"></a>DROP RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita un grupo de recursos de servidor del regulador de recursos definido por el usuario.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP RESOURCE POOL pool_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *pool_name*  
 Es el nombre de un grupo de recursos de servidor definido por el usuario existente.  
  
## <a name="remarks"></a>Observaciones  
 No se puede quitar un grupo de recursos de servidor si contiene grupos de cargas de trabajo.  
  
 No se pueden quitar los grupos de recursos de servidor predeterminados o internos del regulador de recursos.  
  
 Si va a ejecutar instrucciones de DDL, se recomienda familiarizarse primero con los estados del regulador de recursos. Para obtener más información, vea [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL SERVER.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se quita el grupo de recursos de servidor denominado `big_pool`.  
  
```sql  
DROP RESOURCE POOL big_pool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Regulador de recursos](../../relational-databases/resource-governor/resource-governor.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
