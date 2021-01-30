---
description: sp_set_session_context (Transact-SQL)
title: sp_set_session_context (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_set_session_context
- sp_set_session_context_TSQL
- sys.sp_set_session_context
- sys.sp_set_session_context_TSQL
helpviewer_keywords:
- sp_set_session_context
ms.assetid: 7a3a3b2a-1408-4767-a376-c690e3c1fc5b
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4ad470228567b26a618800ec004a2a45c5f5440e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209292"
---
# <a name="sp_set_session_context-transact-sql"></a>sp_set_session_context (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Establece un par clave-valor en el contexto de la sesión.  
  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_set_session_context [ @key= ] N'key', [ @value= ] 'value'  
    [ , [ @read_only = ] { 0 | 1 } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @key =] N'key '  
 La clave que se establece, de tipo **sysname**. El tamaño máximo de la clave es de 128 bytes.  
  
 [ @value =] ' valor '  
 Valor de la clave especificada, de tipo **sql_variant**. Si se establece un valor NULL, se libera la memoria. El tamaño máximo es 8.000 bytes.  
  
 [ @read_only =] {0 | 1}  
 Marca de tipo **bit**. Si es 1, el valor de la clave especificada no se puede cambiar de nuevo en esta conexión lógica. Si es 0 (valor predeterminado), el valor se puede cambiar.  
  
## <a name="permissions"></a>Permisos  
 Cualquier usuario puede establecer un contexto de sesión para su sesión.  
  
## <a name="remarks"></a>Observaciones  
 Al igual que otros procedimientos almacenados, solo se pueden pasar como parámetros los literales y las variables (no las llamadas a expresiones o funciones).  
  
 El tamaño total del contexto de la sesión está limitado a 1 MB. Si establece un valor que hace que se supere este límite, se producirá un error en la instrucción. Puede supervisar el uso de memoria global en [sys.dm_os_memory_objects &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).  
  
 Puede supervisar el uso general de memoria consultando [sys.dm_os_memory_cache_counters &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql.md) como se indica a continuación: `SELECT * FROM sys.dm_os_memory_cache_counters WHERE type = 'CACHESTORE_SESSION_CONTEXT';`  
  
## <a name="examples"></a>Ejemplos  
A. En el ejemplo siguiente se muestra cómo establecer y devolver una clave de contexto de sesión denominada Language con un valor de English.  
  
```  
EXEC sys.sp_set_session_context @key = N'language', @value = 'English';  
SELECT SESSION_CONTEXT(N'language');  
```  
  
 En el siguiente ejemplo se muestra el uso de la marca de solo lectura opcional.  
  
```  
EXEC sys.sp_set_session_context @key = N'user_id', @value = 4, @read_only = 1;  
```  

B. En el ejemplo siguiente se muestra cómo establecer y recuperar una clave de contexto de sesión denominada client_correlation_id con un valor de 12323ad.
```
-- set value
EXEC sp_set_session_context 'client_correlation_id', '12323ad'; 

--check value
SELECT SESSION_CONTEXT(N'client_correlation_id');

```

## <a name="see-also"></a>Consulte también  
 [CURRENT_TRANSACTION_ID &#40;Transact-SQL&#41;](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [SESSION_CONTEXT &#40;&#41;de Transact-SQL ](../../t-sql/functions/session-context-transact-sql.md)   
 [Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  
