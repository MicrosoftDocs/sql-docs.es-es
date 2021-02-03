---
description: USER_NAME (Transact-SQL)
title: USER_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USER_NAME
- USER_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- IDs [SQL Server], databases
- USER_NAME function
- users [SQL Server], database username
- names [SQL Server], database users
- identification numbers [SQL Server], databases
- database usernames [SQL Server]
ms.assetid: ab32d644-4228-449a-9ef0-5a975c305775
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0ba6cb306d78a8c03e95c2d8e235ede1cf8126d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210665"
---
# <a name="user_name-transact-sql"></a>USER_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve un nombre de usuario de base de datos a partir de un número de identificación especificado.  
  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
USER_NAME ( [ id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *id*  
 Es el número de identificación asociado a un usuario de la base de datos. *id* es **int**. Es obligatorio utilizar paréntesis.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **nvarchar(128)**  
  
## <a name="remarks"></a>Comentarios  
 Cuando se omite *id*, se supone que se trata del usuario actual en el contexto actual. Si el parámetro contiene la palabra NULL, se devolverá NULL. Cuando se llama a USER_NAME sin especificar un *id* después de una instrucción EXECUTE AS, USER_NAME devuelve el nombre del usuario representado. Si una entidad de seguridad de Windows ha tenido acceso a la base de datos en forma de miembro de un grupo, USER_NAME devuelve el nombre de la entidad de seguridad de Windows en vez del nombre del grupo.  
 
> [!NOTE]
> Aunque la función USER_NAME se admite en Azure SQL Database, no se admite el uso de *Ejecutar como* con USER_NAME. 
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-user_name"></a>A. Usar USER_NAME  
 En el siguiente ejemplo se devuelve el nombre de usuario del Id. de usuario `13`.  
  
```sql  
SELECT USER_NAME(13);  
GO  
```  
  
### <a name="b-using-user_name-without-an-id"></a>B. Usar USER_NAME sin un identificador  
 En el siguiente ejemplo se busca el nombre del usuario actual sin especificar un identificador.  
  
```sql  
SELECT USER_NAME();  
GO  
```  
  
 Este es el conjunto de resultados para un usuario que pertenezca al rol fijo de servidor sysadmin.  
  
 ```
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="c-using-user_name-in-the-where-clause"></a>C. Usar USER_NAME en la cláusula WHERE  
 En el siguiente ejemplo se busca la fila en `sysusers` en la que el nombre sea igual al resultado de aplicar la función del sistema `USER_NAME` al número de identificador de usuario `1`.  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
name  
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="d-calling-user_name-during-impersonation-with-execute-as"></a>D. Llamar a USER_NAME durante la suplantación con EXECUTE AS  
 En el siguiente ejemplo se muestra cómo se comporta `USER_NAME` durante la suplantación.  
  
```sql  
SELECT USER_NAME();  
GO  
EXECUTE AS USER = 'Zelig';  
GO  
SELECT USER_NAME();  
GO  
REVERT;  
GO  
SELECT USER_NAME();  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
DBO  
Zelig  
DBO
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-user_name-without-an-id"></a>E. Usar USER_NAME sin un identificador  
 En el siguiente ejemplo se busca el nombre del usuario actual sin especificar un identificador.  
  
```sql  
SELECT USER_NAME();  
```  
  
 Este es el conjunto de resultados para un usuario que tiene la sesión iniciada.  
  
```  
------------------------------   
User7                              
```  
  
### <a name="f-using-user_name-in-the-where-clause"></a>F. Usar USER_NAME en la cláusula WHERE  
 En el siguiente ejemplo se busca la fila en `sysusers` en la que el nombre sea igual al resultado de aplicar la función del sistema `USER_NAME` al número de identificador de usuario `1`.  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
name                             
------------------------------   
User7                              
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP &#40;Transact-SQL&#41;](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER &#40;Transact-SQL&#41;](../../t-sql/functions/current-user-transact-sql.md)   
 [SESSION_USER &#40;Transact-SQL&#41;](../../t-sql/functions/session-user-transact-sql.md)   
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md)  
  
