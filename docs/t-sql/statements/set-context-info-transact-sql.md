---
description: SET CONTEXT_INFO (Transact-SQL)
title: SET CONTEXT_INFO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_CONTEXT_INFO_TSQL
- SET CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- context information [SQL Server]
- CONTEXT_INFO option
- SET CONTEXT_INFO statement
ms.assetid: a0b7b9f3-dbda-4350-a274-bd9ecd5c0a74
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 271cf281b2e3dc5f015dd02bc1778fab24be0794
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464230"
---
# <a name="set-context_info-transact-sql"></a>SET CONTEXT_INFO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Asocia hasta 128 bytes de información binaria con la sesión o la conexión actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SET CONTEXT_INFO { binary_str | @binary_var }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *binary_str*  
 Es una constante de tipo **binary** o una constante que se puede convertir de forma implícita a **binary**, para asociarla a la sesión o conexión actual.  
  
 **@** *binary_var*  
 Es una variable **varbinary** o **binary** que alberga un valor de contexto para asociarlo con la sesión o conexión actual.  
  
## <a name="remarks"></a>Comentarios  
 El modo preferido para recuperar la información de contexto para la sesión actual es utilizar la función CONTEXT_INFO. La información de contexto de la sesión se almacena también en las columnas **context_info** de las siguientes vistas del sistema:  
  
-   **sys.dm_exec_requests**  
  
-   **sys.dm_exec_sessions**  
  
-   **sys.sysprocesses**  
  
 SET CONTEXT_INFO no se puede especificar en una función definida por el usuario. No puede proporcionar un valor NULL para SET CONTEXT_INFO porque las vistas que albergan los valores no permiten valores NULL.  
  
 SET CONTEXT_INFO no acepta expresiones distintas de nombres de constantes o variables. Para establecer la información de contexto en el resultado de una llamada de función, debe incluir primero el resultado de la llamada de función en una variable **binary** o **varbinary**.  
  
 Cuando emite SET CONTEXT_INFO en un procedimiento almacenado o un desencadenador, a diferencia de otras instrucciones SET, los nuevos valores de la información de contexto persisten después de completarse el desencadenador o el procedimiento almacenado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-setting-context-information-by-using-a-constant"></a>A. Establecer información de contexto utilizando una constante  
 El ejemplo siguiente muestra `SET CONTEXT_INFO` estableciendo el valor y mostrando los resultados. Tenga en cuenta que para consultar `sys.dm_exec_sessions` se requieren permisos SELECT y VIEW SERVER STATE, mientras que el uso de la función CONTEXT_INFO no los requiere.  
  
```sql
SET CONTEXT_INFO 0x01010101;  
GO  
SELECT context_info   
FROM sys.dm_exec_sessions  
WHERE session_id = @@SPID;  
GO  
```  
  
### <a name="b-setting-context-information-by-using-a-function"></a>B. Establecer información de contexto utilizando una función  
 El ejemplo siguiente muestra el uso del resultado de una función para establecer el valor de contexto, donde el valor de la función se debe colocar primero en una variable **binary**.  
  
```sql
DECLARE @BinVar varbinary(128);  
SET @BinVar = CAST(REPLICATE( 0x20, 128 ) AS varbinary(128) );  
SET CONTEXT_INFO @BinVar;  
  
SELECT CONTEXT_INFO() AS MyContextInfo;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md) [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)  
 [SESSION_CONTEXT  &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)  
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sp_set_session_context  &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)  
  
