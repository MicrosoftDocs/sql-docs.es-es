---
description: sp_releaseapplock (Transact-SQL)
title: sp_releaseapplock (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_releaseapplock_TSQL
- sp_releaseapplock
dev_langs:
- TSQL
helpviewer_keywords:
- sp_releaseapplock
ms.assetid: 51b03c2f-0d54-40f5-9172-e747942d4a46
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 020a2964a4f73872669a4f9623a8c24aa2a88fd5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185658"
---
# <a name="sp_releaseapplock-transact-sql"></a>sp_releaseapplock (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Libera un bloqueo en un recurso de aplicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_releaseapplock [ @Resource = ] 'resource_name'   
     [ , [ @LockOwner = ] 'lock_owner' ]  
     [ , [ @DbPrincipal = ] 'database_principal' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @Resource =] '*resource_name*'  
 Es un nombre de recurso de bloqueo especificado por la aplicación cliente. La aplicación debe garantizar que el recurso es único. El nombre especificado se convierte internamente mediante un algoritmo hash en un valor que puede almacenarse en el administrador de bloqueos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *resource_name* es de tipo **nvarchar (255)** y no tiene ningún valor predeterminado. *resource_name* se comparan con binarios, por lo que distingue mayúsculas de minúsculas, independientemente de la configuración de intercalación de la base de datos actual.  
  
 [ @LockOwner =] '*lock_owner*'  
 Es el propietario del bloqueo, que es el valor de *lock_owner* cuando se solicitó el bloqueo. *lock_owner* es **nvarchar(32)**. El valor puede ser **Transaction** (predeterminado) o **Session**. Cuando el valor de *lock_owner* es **Transaction**, de forma predeterminada o se especifica explícitamente, sp_getapplock debe ejecutarse desde una transacción.  
  
 [ @DbPrincipal =] '*database_principal*'  
 Es el usuario, el rol o el rol de aplicación que tiene permisos para un objeto de una base de datos. El autor de la llamada de la función debe ser miembro de *database_principal*, DBO o el rol fijo de base de datos db_owner para poder llamar a la función correctamente. El valor predeterminado es public.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 \>= 0 (correcto) o < 0 (error)  
  
|Value|Resultado|  
|-----------|------------|  
|0|El bloqueo se ha liberado correctamente.|  
|-999|Indica un error de validación de parámetros u otro error de llamada.|  
  
## <a name="remarks"></a>Observaciones  
 Si una aplicación llama a sp_getapplock varias veces para el mismo recurso de bloqueo, es necesario llamar a sp_releaseapplock el mismo número de veces para liberar el bloqueo.  
  
 Los bloqueos se liberan si el servidor se cierra por cualquier razón.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se libera el bloqueo asociado a la transacción actual en el recurso `Form1` de la base de datos `AdventureWorks2012`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_getapplock @DbPrincipal = 'dbo', @Resource = 'Form1',   
     @LockMode = 'Shared';  
EXEC sp_releaseapplock @DbPrincipal = 'dbo', @Resource = 'Form1';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [APPLOCK_MODE &#40;&#41;de Transact-SQL ](../../t-sql/functions/applock-mode-transact-sql.md)   
 [APPLOCK_TEST &#40;&#41;de Transact-SQL ](../../t-sql/functions/applock-test-transact-sql.md)   
 [sp_getapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
  
  
