---
description: SYSTEM_USER (Transact-SQL)
title: SYSTEM_USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYSTEM_USER_TSQL
- SYSTEM_USER
dev_langs:
- TSQL
helpviewer_keywords:
- current user names
- system-supplied user names [SQL Server]
- users [SQL Server], logins
- logins [SQL Server], identification name
- current system user names
- SYSTEM_USER function
- inserting system user name into table
- system usernames [SQL Server]
- users [SQL Server], names
ms.assetid: 565984cd-60c6-4df7-83ea-2349b838ccb2
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3432cf3a1ba598f86ee5ac52b28922128171121e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197826"
---
# <a name="system_user-transact-sql"></a>SYSTEM_USER (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Permite insertar en una tabla un valor proporcionado por el sistema para el inicio de sesión actual cuando no se especifica ningún valor predeterminado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SYSTEM_USER  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto  
 **nvarchar(128)**  
  
## <a name="remarks"></a>Comentarios  
 En las instrucciones CREATE TABLE y ALTER TABLE puede utilizar la función SYSTEM_USER con restricciones DEFAULT. También puede utilizarla como cualquier función estándar.  
  
 Si el nombre de usuario y el nombre de inicio de sesión con diferentes, SYSTEM_USER devuelve el nombre de inicio de sesión.  
  
 Si el usuario actual ha iniciado la sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la autenticación de Windows, SYSTEM_USER devuelve el nombre de identificación del inicio de sesión de Windows con el formato: *DOMAIN*\\*user_login_name*. Sin embargo, si el usuario actual ha iniciado la sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la autenticación de SQL Server, SYSTEM_USER devuelve el nombre de identificación de inicio de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], por ejemplo `WillisJo` para un usuario que ha iniciado la sesión como `WillisJo`.  
  
 SYSTEM_USER devuelve el nombre del contexto de ejecución actual. Si se ha usado la instrucción EXECUTE AS para cambiar el contexto, SYSTEM_USER devuelve el nombre del contexto suplantado.  

 No se puede ejecutar como SYSTEM_USER.

## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-system_user-to-return-the-current-system-user-name"></a>A. Usar SYSTEM_USER para devolver el nombre de usuario actual del sistema  
 En el siguiente ejemplo se declara una variable `char`, se almacena en ella el valor actual de `SYSTEM_USER` y, a continuación, se imprime el valor almacenado en la variable.  
  
```sql
DECLARE @sys_usr CHAR(30);  
SET @sys_usr = SYSTEM_USER;  
SELECT 'The current system user is: '+ @sys_usr;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
----------------------------------------------------------
The current system user is: WillisJo

(1 row(s) affected)
 ```  
  
### <a name="b-using-system_user-with-default-constraints"></a>B. Usar SYSTEM_USER con restricciones DEFAULT  
 En el siguiente ejemplo se crea una tabla con `SYSTEM_USER` como una restricción `DEFAULT` para la columna `SRep_tracking_user`.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE Sales.Sales_Tracking  
(  
    Territory_id INT IDENTITY(2000, 1) NOT NULL,  
    Rep_id INT NOT NULL,  
    Last_sale DATETIME NOT NULL DEFAULT GETDATE(),  
    SRep_tracking_user VARCHAR(30) NOT NULL DEFAULT SYSTEM_USER  
);  
GO  
INSERT Sales.Sales_Tracking (Rep_id)  
VALUES (151);  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (293, '19980515');  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (27882, '19980620');  
INSERT Sales.Sales_Tracking (Rep_id)  
VALUES (21392);  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (24283, '19981130');  
GO  
```  
  
 En la siguiente consulta se selecciona toda la información de la tabla `Sales_Tracking`:  
  
```sql
SELECT * FROM Sales_Tracking ORDER BY Rep_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Territory_id Rep_id Last_sale            SRep_tracking_user
-----------  ------ -------------------- ------------------
2000         151    Mar 4 1998 10:36AM   ArvinDak
2001         293    May 15 1998 12:00AM  ArvinDak
2003         21392  Mar 4 1998 10:36AM   ArvinDak
2004         24283  Nov 3 1998 12:00AM   ArvinDak
2002         27882  Jun 20 1998 12:00AM  ArvinDak
  
(5 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-system_user-to-return-the-current-system-user-name"></a>C. Usar SYSTEM_USER para devolver el nombre de usuario actual del sistema  
 El siguiente ejemplo devuelve el valor actual de `SYSTEM_USER`.  
  
```sql
SELECT SYSTEM_USER;  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP &#40;Transact-SQL&#41;](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER &#40;Transact-SQL&#41;](../../t-sql/functions/current-user-transact-sql.md)   
 [SESSION_USER &#40;Transact-SQL&#41;](../../t-sql/functions/session-user-transact-sql.md)   
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [USER &#40;Transact-SQL&#41;](../../t-sql/functions/user-transact-sql.md)  
  
  

