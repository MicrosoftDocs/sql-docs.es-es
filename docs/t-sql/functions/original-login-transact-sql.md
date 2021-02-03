---
description: ORIGINAL_LOGIN (Transact-SQL)
title: ORIGINAL_LOGIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ORIGINAL_LOGIN_TSQL
- ORIGINAL_LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- logins [SQL Server], context switches
- context switching [SQL Server], login names
- original login names [SQL Server]
- ORIGINAL_LOGIN function
- names [SQL Server], logins
ms.assetid: ddfb0991-cde3-4b97-a5b7-ee450133f160
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b7409baa8bcdac97746f8da2751a16e323c77850
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183938"
---
# <a name="original_login-transact-sql"></a>ORIGINAL_LOGIN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el nombre del inicio de sesión que se conectó a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Puede utilizar esta función para devolver la identidad del inicio de sesión original en sesiones en las que hay varios cambios de contexto explícitos o implícitos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ORIGINAL_LOGIN( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **sysname**  
  
## <a name="remarks"></a>Observaciones  
 Esta función puede resultar útil para la auditoría de la identidad del contexto de conexión original. Mientras que funciones como [SESSION_USER](../../t-sql/functions/session-user-transact-sql.md) y [CURRENT_USER](../../t-sql/functions/current-user-transact-sql.md) devuelven el contexto de ejecución actual, ORIGINAL_LOGIN devuelve la identidad del inicio de sesión que se conectó en primer lugar a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en esa sesión.  
 
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente cambia el contexto de ejecución de la sesión actual desde el solicitante de las instrucciones para `login1`. Las funciones `SUSER_SNAME` y `ORIGINAL_LOGIN` se utilizan para devolver el usuario de la sesión actual (el usuario al que se cambió el contexto), y la cuenta de inicio de sesión original. 
 
  >[!NOTE]
  > Aunque en Azure SQL Database se admite la función ORIGINAL_LOGIN, el script siguiente producirá un error porque *Execute as LOGIN* no se admite en Azure SQL Database. 
  
```sql  
USE AdventureWorks2012;  
GO  
--Create a temporary login and user.  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE USER user1 FOR LOGIN login1;  
GO  
--Execute a context switch to the temporary login account.  
DECLARE @original_login sysname;  
DECLARE @current_context sysname;  
EXECUTE AS LOGIN = 'login1';  
SET @original_login = ORIGINAL_LOGIN();  
SET @current_context = SUSER_SNAME();  
SELECT 'The current executing context is: '+ @current_context;  
SELECT 'The original login in this session was: '+ @original_login  
GO  
-- Return to the original execution context  
-- and remove the temporary principal.  
REVERT;  
GO  
DROP LOGIN login1;  
DROP USER user1;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)   
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)  
  
  
