---
description: USE (Transact-SQL)
title: USE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/28/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USE_TSQL
- USE
dev_langs:
- TSQL
helpviewer_keywords:
- USE statement
- database context [SQL Server]
- context changes [SQL Server]
- modifying database context
ms.assetid: c05acac8-c063-4770-8e36-d7f71d500b10
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d83799db882df09ae57e7225d8c3f52cdb04cb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208755"
---
# <a name="use-transact-sql"></a>USE (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Cambia el contexto de la base de datos al de la base de datos especificada o a la instantánea en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
USE { database_name }   
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *database_name*  
 Es el nombre de la base de datos o instantánea de base de datos a la que se cambia el contexto de usuario. Los nombres de base de datos y de instantánea de base de datos deben cumplir las mismas reglas que los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
 En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], el parámetro de base de datos solo puede hacer referencia a la base de datos actual. Si se proporciona una base de datos distinta de la base de datos actual, la instrucción `USE` no cambia entre bases de datos y se devuelve el código de error 40508. Para cambiar de base de datos, debe conectarse directamente a la base de datos. La instrucción USE está marcada como no aplicable a SQL Database al inicio de esta página porque, aunque se puede tener la instrucción `USE` en un lote, esta no hace nada.
  
## <a name="remarks"></a>Observaciones  
 Cuando un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el inicio de sesión se conecta automáticamente a su base de datos predeterminada y adquiere el contexto de seguridad de un usuario de la base de datos. Si no se ha creado ningún usuario de la base de datos para el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el inicio de sesión se conecta como guest. Si el usuario de la base de datos no tiene permiso CONNECT en la base de datos, la instrucción USE no funcionará correctamente. Si no se ha asignado ninguna base de datos predeterminada al inicio de sesión, su base de datos predeterminada se establecerá en master.  
  
 USE se ejecuta en tiempo de compilación y de ejecución, y surte efecto inmediatamente. Por lo tanto, las instrucciones que aparecen en un lote después de la instrucción USE se ejecutan en la base de datos especificada.  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso CONNECT en la base de datos de destino.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se cambia el contexto de la base de datos al de la base de datos `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../statements/create-database-transact-sql.md)   
 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)  
  
