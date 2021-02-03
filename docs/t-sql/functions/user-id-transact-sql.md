---
description: USER_ID (Transact-SQL)
title: USER_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USER_ID
- USER_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- USER_ID function
- identification numbers [SQL Server]
- IDs [SQL Server], databases
- users [SQL Server], database ID numbers
- database IDs [SQL Server]
- identification numbers [SQL Server], databases
ms.assetid: 67fd29bc-eda9-4d4d-b148-5d3659181a43
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 860016421276c6c02c8d0d523476a856f70f227b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210623"
---
# <a name="user_id-transact-sql"></a>USER_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve el número de identificación para un usuario de la base de datos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use [DATABASE_PRINCIPAL_ID](../../t-sql/functions/database-principal-id-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
USER_ID ( [ 'user' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *user*  
 Es el nombre de usuario que se va a emplear. *user* es **nchar**. Si se especifica un valor **char**, se convierte implícitamente en **nchar**. Es obligatorio utilizar paréntesis.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
## <a name="remarks"></a>Observaciones  
 Cuando *user* se omite, se da por supuesto que es el usuario actual. Si el parámetro contiene la palabra NULL, devolverá NULL. Cuando se llama a USER_ID después de EXECUTE AS, USER_ID devuelve el identificador del contexto suplantado.  
  
 Cuando una entidad de seguridad de Windows que no se ha asignado a un usuario específico de base de datos tiene acceso a una base de datos en forma de pertenencia a un grupo, USER_ID devuelve 0 (el identificador de público). Si este tipo de entidad de seguridad crea un objeto sin especificar un esquema, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creará un usuario implícito y un esquema asignados a dicha entidad. El usuario creado en casos como éste no se puede utilizar para conectarse a la base de datos. Las llamadas a USER_ID efectuadas por la entidad de seguridad de Windows asignada a un usuario implícito devolverán el identificador de éste.  
  
 Se puede utilizar USER_ID en una lista de selección, en una cláusula WHERE y en cualquier lugar en el que se permita una expresión. Para obtener más información, vea [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve el número de identificación para el usuario `AdventureWorks2012` de `Harold`.  
  
```sql
USE AdventureWorks2012;  
SELECT USER_ID('Harold');  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [DATABASE_PRINCIPAL_ID &#40;Transact-SQL&#41;](../../t-sql/functions/database-principal-id-transact-sql.md)   
 [Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
