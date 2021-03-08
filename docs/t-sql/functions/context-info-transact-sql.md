---
description: CONTEXT_INFO  (Transact-SQL)
title: CONTEXT_INFO  (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONTEXT_INFO_TSQL
- CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- CONTEXT_INFO function
- Multiple Active Result Sets
- context information [SQL Server]
- MARS [SQL Server]
- session context information [SQL Server]
ms.assetid: 571320f5-7228-4b0e-9d01-ab732d2d1eab
author: cawrites
ms.author: chadam
ms.openlocfilehash: f0ca8fcf210e58537afabbbb225a593e5b537332
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835354"
---
# <a name="context_info--transact-sql"></a>CONTEXT_INFO  (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Esta función devuelve el valor **context_info** establecido para la sesión o lote actual, o derivada del uso de la instrucción [SET CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md).
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CONTEXT_INFO()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-value"></a>Valor devuelto
El valor **context_info**.
  
Si **context_info** no se ha establecido:
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve NULL.  
-   [!INCLUDE[ssSDS](../../includes/sssds-md.md)] devuelve un GUID único específico de sesión.  
  
## <a name="remarks"></a>Observaciones  
La característica de conjuntos de resultados activos múltiples (MARS) permite a las aplicaciones ejecutar varios lotes o solicitudes al mismo tiempo en la misma conexión. Cuando uno de los lotes de una conexión MARS ejecuta SET CONTEXT_INFO, la función `CONTEXT_INFO` devuelve el nuevo valor de contexto cuando la función `CONTEXT_INFO` se ejecuta en el mismo lote que la instrucción SET. Si la función `CONTEXT_INFO` se ejecuta en uno o más de los otros lotes de conexión, `CONTEXT_FUNCTION` no devuelve el nuevo valor a menos que esos lotes hayan comenzado después de completar el lote que ejecutó la instrucción SET.
  
## <a name="permissions"></a>Permisos  
No requiere permisos especiales. Las siguientes vistas del sistema almacenan la información de contexto, pero la consulta directa de estas vistas requiere los permisos SELECT y VIEW SERVER STATE:
- **sys.dm_exec_requests**
- **sys.dm_exec_sessions**
- **sys.sysprocesses**
  
## <a name="examples"></a>Ejemplos  
En este ejemplo sencillo se establece el valor de **context_info** en `0x1256698456` y, después, se usa la función `CONTEXT_INFO` para recuperarlo.
  
```sql
SET CONTEXT_INFO 0x1256698456;  
GO  
SELECT CONTEXT_INFO();  
GO  
```  
  
## <a name="see-also"></a>Consulte también
[SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)
[SESSION_CONTEXT  &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)  
[sp_set_session_context  &#40;Transact-SQL&#41;](../../sql/relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)  
  
