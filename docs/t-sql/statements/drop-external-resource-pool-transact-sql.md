---
description: DROP EXTERNAL RESOURCE POOL (Transact-SQL)
title: DROP EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- DROP EXTERNAL RESOURCE POOL
- DROP_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL RESOURCE POOL statement
ms.assetid: e2fa01bd-96ff-4ea9-bb08-6cb6b6adf68c
author: dphansen
ms.author: davidph
manager: cgronlund
ms.openlocfilehash: 70c4cb5ab7f6808e846bbd8a55ac1d8e75af4ab4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197563"
---
# <a name="drop-external-resource-pool-transact-sql"></a>DROP EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Elimina un grupo de recursos externos de Resource Governor que sirve para definir los recursos de los procesos externos. 

::: moniker range="=sql-server-2016||>=sql-server-linux-ver15"
En [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] en [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], el grupo externo rige `rterm.exe`, `BxlServer.exe` y otros procesos generados por ellos.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
En [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)], el grupo externo rige `rterm.exe`, `python.exe`, `BxlServer.exe` y otros procesos generados por ellos.
::: moniker-end

Los grupos de recursos externos se crean por medio de [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md) y se modifican a través de [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md).  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP EXTERNAL RESOURCE POOL pool_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*pool_name*  
Nombre del grupo de recursos externos que se va a eliminar.  
  
## <a name="remarks"></a>Observaciones

No se puede quitar un grupo de recursos externo si contiene grupos de cargas de trabajo.  

No se pueden quitar los grupos de recursos de servidor predeterminados o internos del regulador de recursos.  

Si va a ejecutar instrucciones de DDL, se recomienda familiarizarse primero con los estados del regulador de recursos. Para obtener más información, vea [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  

## <a name="permissions"></a>Permisos

Requiere el permiso `CONTROL SERVER`.  

## <a name="examples"></a>Ejemplos

En el siguiente ejemplo se quita el grupo de recursos externos denominado `ex_pool`.  

```sql
DROP EXTERNAL RESOURCE POOL ex_pool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>Consulte también

+ [Opción de configuración de servidor Scripts externos habilitados](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)
+ [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
+ [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)
+ [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)
