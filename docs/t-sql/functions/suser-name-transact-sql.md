---
description: SUSER_NAME (Transact-SQL)
title: SUSER_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/12/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUSER_NAME
- SUSER_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- security identification names [SQL Server]
- logins [SQL Server], users
- identification names for logins [SQL Server]
- users [SQL Server], logins
- SUSER_NAME function
- logins [SQL Server], names
- names [SQL Server], logins
ms.assetid: ae598d9f-9baa-49b8-b1c1-042854206de4
author: VanMSFT
ms.author: vanto
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b163b8aae38c389b8b20a6853fce685cac0dfd8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200282"
---
# <a name="suser_name-transact-sql"></a>SUSER_NAME (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Devuelve el nombre de identificación de inicio de sesión del usuario.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SUSER_NAME ( [ server_user_id ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
_server\_user\_id_  
Es el número de identificación de inicio de sesión del usuario. _server\_user\_id_, que es opcional, es **int**. _server\_user\_id_ puede ser el número de identificación de inicio de sesión de cualquier inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o de cualquier usuario o grupo de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows que tenga permiso para conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si no se especifica _server\_user\_id_, se devuelve el nombre de identificación de inicio de sesión del usuario actual. Si el parámetro contiene la palabra NULL, se devolverá NULL.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
**nvarchar(128)**  
  
## <a name="remarks"></a>Comentarios  
En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versión 7.0, el número de identificación de seguridad (SID) reemplaza al número de identificación de usuario del servidor (SUID).  
  
SUSER_NAME solo devuelve un nombre de un inicio de sesión que tenga una entrada en la tabla del sistema **syslogins**.  
  
Se puede usar SUSER_NAME en una lista de selección, en una cláusula WHERE, y en cualquier parte en la que se permita una expresión. Use paréntesis después SUSER_NAME, incluso si no se especifica ningún parámetro.  

> [!NOTE]
> Aunque la función SUSER_NAME se admite en Azure SQL Database, no se admite el uso de *Ejecutar como* con SUSER_NAME. 
  
## <a name="examples"></a>Ejemplos  
En el ejemplo siguiente se devuelve el nombre de identificación de inicio de sesión del usuario con el número de identificación de inicio de sesión `1`.  
  
```sql
SELECT SUSER_NAME(1);  
```  
  
## <a name="see-also"></a>Vea también  
[SUSER_ID &#40;Transact-SQL&#41;](../../t-sql/functions/suser-id-transact-sql.md)   
[Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
