---
description: CURRENT_USER (Transact-SQL)
title: CURRENT_USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURRENT_USER
- CURRENT_USER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- current user names
- niladic functions
- CURRENT_USER
- users [SQL Server], names
ms.assetid: 29248949-325b-4063-9f55-5a445fb35c6e
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c17dd2856ead158c19c1adbeb737fe7570829774
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184106"
---
# <a name="current_user-transact-sql"></a>CURRENT_USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Esta función devuelve el nombre de usuario actual. Esta función es equivalente a `USER_NAME()`.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CURRENT_USER  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valores devueltos
**sysname**
  
## <a name="remarks"></a>Observaciones  
`CURRENT_USER` devuelve el nombre del contexto de seguridad actual. Si `CURRENT_USER` se ejecuta después de una llamada a `EXECUTE AS`, cambia el contexto; `CURRENT_USER` devolverá el nombre del contexto suplantado. Si una entidad de seguridad de Windows ha tenido acceso a la base de datos en forma de miembro de un grupo, `CURRENT_USER` devolverá el nombre de la entidad de seguridad de Windows en vez del nombre del grupo.
  
Vea [SUSER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-name-transact-sql.md) y [SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md) para obtener información sobre cómo devolver el inicio de sesión del usuario actual.
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-current_user-to-return-the-current-user-name"></a>A. Usar CURRENT_USER para devolver el nombre del usuario actual  
En este siguiente se devuelve el nombre del usuario actual.
  
```sql
SELECT CURRENT_USER;  
GO  
```  
  
### <a name="b-using-current_user-as-a-default-constraint"></a>B. Usar CURRENT_USER como restricción DEFAULT  
En este ejemplo se crea una tabla que usa `CURRENT_USER` como restricción `DEFAULT` para la columna `order_person` en una fila de ventas.
  
```sql
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
      WHERE TABLE_NAME = 'orders22')  
   DROP TABLE orders22;  
GO  
SET NOCOUNT ON;  
CREATE TABLE orders22  
(  
order_id int IDENTITY(1000, 1) NOT NULL,
cust_id  int NOT NULL,
order_date smalldatetime NOT NULL DEFAULT GETDATE(),
order_amt money NOT NULL,
order_person char(30) NOT NULL DEFAULT CURRENT_USER
);  
GO  
```  
  
Este ejemplo inserta un registro en la tabla. El usuario denominado `Wanida` ejecuta estas instrucciones.
  
```sql
INSERT orders22 (cust_id, order_amt)  
VALUES (5105, 577.95);  
GO  
SET NOCOUNT OFF;  
GO  
```  
  
Esta consulta selecciona toda la información de la tabla `orders22`.
  
```sql
SELECT * FROM orders22;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
order_id    cust_id     order_date           order_amt    order_person
----------- ----------- -------------------- ------------ ------------
1000        5105        2005-04-03 23:34:00  577.95       Wanida
  
(1 row(s) affected)
```
  
### <a name="c-using-current_user-from-an-impersonated-context"></a>C. Usar CURRENT_USER desde un contexto suplantado  
En este ejemplo, el usuario `Wanida` ejecuta el siguiente código [!INCLUDE[tsql](../../includes/tsql-md.md)] para suplantar al usuario "Arnalfo".
  
```sql
SELECT CURRENT_USER;  
GO  
EXECUTE AS USER = 'Arnalfo';  
GO  
SELECT CURRENT_USER;  
GO  
REVERT;  
GO  
SELECT CURRENT_USER;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Wanida
Arnalfo
Wanida
```
  
## <a name="see-also"></a>Consulte también
[USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)  
[SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md)  
[sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
  
  

