---
description: APPLOCK_TEST (Transact-SQL)
title: APPLOCK_TEST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPLOCK_TEST_TSQL
- APPLOCK_TEST
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], applications
- APPLOCK_TEST function
- applications [SQL Server], locks
- sessions [SQL Server], application locks
- testing application locks
ms.assetid: 4ea33d04-f8e9-46ff-ae61-985bd3eaca2c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 742673470ba1298a77ed8f67a7097bf835db15d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202288"
---
# <a name="applock_test-transact-sql"></a>APPLOCK_TEST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Esta función devuelve información acerca de si se puede conceder o no un bloqueo para un recurso de aplicación determinado para el propietario de un bloqueo específico, sin adquirir el bloqueo. Como función de bloqueo de la aplicación, APPLOCK_TEST funciona en la base de datos actual. La base de datos es el ámbito de los bloqueos de la aplicación.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
APPLOCK_TEST ( 'database_principal' , 'resource_name' , 'lock_mode' , 'lock_owner' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
**'** *database_principal* **'**  
Es el usuario, el rol o el rol de aplicación al que se pueden conceder permisos para los objetos de la base de datos. Para llamar a la función correctamente, el autor de la llamada a la función debe ser miembro de *database_principal*, de dbo o del rol fijo de base de datos db_owner.
  
**'** *resource_name* **'**  
Es un nombre de recurso de bloqueo especificado por la aplicación cliente. La aplicación debe asegurar que el nombre del recurso sea exclusivo. El nombre especificado se convierte internamente mediante un algoritmo hash en un valor que puede almacenarse en el administrador de bloqueos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  *resource_name* es **nvarchar(255)** y no tiene ningún valor predeterminado. *resource_name* se compara con un binario y distingue mayúsculas de minúsculas, independientemente de la configuración de intercalación de la base de datos actual.
  
**'** *lock_mode* **'**  
El modo de bloqueo que se debe obtener para un recurso concreto. *lock_mode* es **nvarchar(32)** y carece de valor predeterminado. *lock_mode* puede tener cualquiera de estos valores: **Shared**, **Update**, **IntentShared**, **IntentExclusive**, **Exclusive**.
  
**'** *lock_owner* **'**  
El propietario del bloqueo, que es el valor de *lock_owner* cuando se solicitó el bloqueo. *lock_owner* es **nvarchar(32)** y el valor puede ser **Transaction** (predeterminado) o **Session**. Si se especifica el valor predeterminado o **Transaction** explícitamente, debe ejecutarse APPLOCK_TEST desde una transacción.
  
## <a name="return-types"></a>Tipos de valores devueltos
**smallint**
  
## <a name="return-value"></a>Valor devuelto
0 si no se puede conceder el bloqueo al propietario especificado o 1 si se puede conceder el bloqueo.
  
## <a name="function-properties"></a>Propiedades de las funciones
**No determinista**
  
**Nonindexable**
  
**Nonparallelizable**
  
## <a name="examples"></a>Ejemplos  
Dos usuarios (**usuario A** y **usuario B**) con sesiones independientes ejecutan la siguiente secuencia de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
El **Usuario A** ejecuta:
  
```sql
USE AdventureWorks2012;  
GO  
BEGIN TRAN;  
DECLARE @result INT;  
EXEC @result=sp_getapplock  
    @DbPrincipal='public',  
    @Resource='Form1',  
    @LockMode='Shared',  
    @LockOwner='Transaction';  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
GO  
```  
  
Después, el **Usuario B** ejecuta:
  
```sql
Use AdventureWorks2012;  
GO  
BEGIN TRAN;  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
--Result set: NoLock  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Shared', 'Transaction');  
--Result set: 1 (Lock is grantable.)  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: 0 (Lock is not grantable.)  
GO  
```  
  
Después, el **Usuario A** ejecuta:
  
```sql
EXEC sp_releaseapplock @Resource='Form1', @DbPrincipal='public';  
GO  
```  
  
Después, el **Usuario B** ejecuta:
  
```sql
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: '1' (The lock is grantable.)  
GO  
```  
  
Después, el **Usuario A** y el **Usuario B** ejecutan:
  
```sql
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>Vea también
[APPLOCK_MODE &#40;Transact-SQL&#41;](../../t-sql/functions/applock-mode-transact-sql.md)  
[sp_getapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
[sp_releaseapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)
