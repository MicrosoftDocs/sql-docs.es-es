---
description: SESSION_CONTEXT (Transact-SQL)
title: SESSION_CONTEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SESSION_CONTEXT
- sys.SESSION_CONTEXT
- SESSION_CONTEXT_TSQL
- sys.SESSION_CONTEXT_TSQL
helpviewer_keywords:
- SESSION_CONTEXT function
ms.assetid: b6bdbc54-331a-43cc-ab3d-3872d6a12100
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e2d7985bcfacc880a5c1561faa160fb4ec7aee42
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212217"
---
# <a name="session_context-transact-sql"></a>SESSION_CONTEXT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Devuelve el valor de la clave especificada en el contexto de la sesión actual. El valor se establece con el procedimiento [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
SESSION_CONTEXT(N'key')  
```  
  
## <a name="arguments"></a>Argumentos
 'key'  
 Clave (de tipo sysname) del valor que se va a recuperar.  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **sql_variant**  
  
## <a name="return-value"></a>Valor devuelto  
 Valor asociado con la clave especificada en el contexto de la sesión, o NULL si no se ha establecido ningún valor para esa clave.  
  
## <a name="permissions"></a>Permisos  
 Cualquier usuario puede leer el contexto de la sesión.  
  
## <a name="remarks"></a>Observaciones  
 El comportamiento de MARS en SESSION_CONTEXT es similar al de CONTEXT_INFO. Si un lote MARS establece un par clave-valor, el nuevo valor no se devolverá en otros lotes MARS en la misma conexión, a menos que dichos lotes se inicien después de que el lote que estableció el nuevo valor se haya completado. Si hay varios lotes MARS activos en una conexión, los valores no se pueden establecer en “read_only”. Esto evita que se produzcan condiciones de carrera y no determinismo sobre qué valor “gana”.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo, el valor de contexto de la sesión de la clave `user_id` se establece en 4 y, después, se usa la función **SESSION_CONTEXT** para recuperarlo.  
  
```sql  
EXEC sp_set_session_context 'user_id', 4;  
SELECT SESSION_CONTEXT(N'user_id');  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)   
 [CURRENT_TRANSACTION_ID &#40;Transact-SQL&#41;](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  
