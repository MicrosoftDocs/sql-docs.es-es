---
description: IDENT_CURRENT (Transact-SQL)
title: IDENT_CURRENT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENT_CURRENT
- IDENT_CURRENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- last identity value generated for table
- identity values [SQL Server], last generated
- identity columns, current value
- IDENT_CURRENT function
ms.assetid: 21517ced-39f5-4cd8-8d9c-0a0b8aff554a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 608f7c4745d2d5590fc4d2956f7fca580c9b4111
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184013"
---
# <a name="ident_current-transact-sql"></a>IDENT_CURRENT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve el último valor de identidad generado para una tabla o vista especificadas. El último valor de identidad generado puede ser para cualquier sesión y cualquier ámbito.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
IDENT_CURRENT( 'table_or_view' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*table_or_view*  
Es el nombre de la tabla o vista cuyo valor de identidad se devuelve. *table_or_view* es **varchar**, sin valor predeterminado.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
**numeric**([@@MAXPRECISION](../../t-sql/functions/max-precision-transact-sql.md),0))  
  
## <a name="exceptions"></a>Excepciones  
Devuelve NULL si se produce un error o si el autor de la llamada no tiene permiso para ver el objeto.  
  
En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un usuario solo puede ver los metadatos de elementos protegibles que posea o para los que se le haya concedido permiso. Esto significa que las funciones integradas de emisión de metadatos, como IDENT_CURRENT, pueden devolver NULL si el usuario no tiene ningún permiso para el objeto. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Comentarios  
IDENT_CURRENT es similar a las funciones de identidad SCOPE_IDENTITY y @@IDENTITY de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Las tres funciones devuelven los últimos valores de identidad generados. Pero la definición de *últimos valores de identidad* para cada una de estas funciones es diferente en cuanto al ámbito y la sesión:  

-   IDENT_CURRENT devuelve el último valor de identidad generado para una tabla específica en cualquier sesión y cualquier ámbito.  
-   @@IDENTITY devuelve el último valor de identidad generado para cualquier tabla en la sesión actual, en todos los ámbitos.  
-   SCOPE_IDENTITY devuelve el último valor de identidad generado para cualquier tabla en la sesión y el ámbito actuales.  
  
Cuando el valor de IDENT_CURRENT es NULL (porque la tabla nunca ha contenido filas ni ha sido truncada), la función IDENT_CURRENT devuelve el valor de inicialización.  
  
Las instrucciones y transacciones con errores pueden cambiar la identidad actual de una tabla y crear huecos en los valores de columna de identidad. El valor de identidad jamás se revierte, aun cuando no se haya confirmado la transacción que intentó insertar el valor en la tabla. Por ejemplo, si se produce un error en una instrucción INSERT debido a una infracción de tipo IGNORE_DUP_KEY, el valor de identidad actual de la tabla se sigue incrementando.  

Cuando se usa IDENT_CURRENT en una vista que contiene combinaciones, se devuelve NULL. Esto es independiente de si solo una o varias tablas combinadas tienen una columna de identidad. 
  
> [!IMPORTANT]
> Tenga cuidado al usar IDENT_CURRENT para predecir el siguiente valor de identidad generado. El valor generado real puede ser diferente de IDENT_CURRENT más IDENT_INCR a causa de las inserciones realizadas por otras sesiones.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-the-last-identity-value-generated-for-a-specified-table"></a>A. Devolver el último valor de identidad generado para una tabla especificada  
 En el ejemplo siguiente se devuelve el último valor de identidad generado para la tabla `Person.Address` en la base de datos `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT IDENT_CURRENT ('Person.Address') AS Current_Identity;  
GO  
```  
  
### <a name="b-comparing-identity-values-returned-by-ident_current-identity-and-scope_identity"></a>B. Comparar valores de identidad devueltos por IDENT_CURRENT, @@IDENTITY y SCOPE_IDENTITY  
 En el ejemplo siguiente se muestran los distintos valores de identidad devueltos por `IDENT_CURRENT`, `@@IDENTITY` y `SCOPE_IDENTITY`.  
  
```sql 
USE AdventureWorks2012;  
GO  
IF OBJECT_ID(N't6', N'U') IS NOT NULL   
    DROP TABLE t6;  
GO  
IF OBJECT_ID(N't7', N'U') IS NOT NULL   
    DROP TABLE t7;  
GO  
CREATE TABLE t6(id INT IDENTITY);  
CREATE TABLE t7(id INT IDENTITY(100,1));  
GO  
CREATE TRIGGER t6ins ON t6 FOR INSERT   
AS  
BEGIN  
   INSERT t7 DEFAULT VALUES  
END;  
GO  
--End of trigger definition  
  
SELECT id FROM t6;  
--IDs empty.  
  
SELECT id FROM t7;  
--ID is empty.  
  
--Do the following in Session 1  
INSERT t6 DEFAULT VALUES;  
SELECT @@IDENTITY;  
/*Returns the value 100. This was inserted by the trigger.*/  
  
SELECT SCOPE_IDENTITY();  
/* Returns the value 1. This was inserted by the   
INSERT statement two statements before this query.*/  
  
SELECT IDENT_CURRENT('t7');  
/* Returns value inserted into t7, that is in the trigger.*/  
  
SELECT IDENT_CURRENT('t6');  
/* Returns value inserted into t6. This was the INSERT statement four statements before this query.*/  
  
-- Do the following in Session 2.  
SELECT @@IDENTITY;  
/* Returns NULL because there has been no INSERT action   
up to this point in this session.*/  
  
SELECT SCOPE_IDENTITY();  
/* Returns NULL because there has been no INSERT action   
up to this point in this scope in this session.*/  
  
SELECT IDENT_CURRENT('t7');  
/* Returns the last value inserted into t7.*/  
```  
  
## <a name="see-also"></a>Consulte también  
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)   
 [IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)   
 [IDENT_SEED &#40;Transact-SQL&#41;](../../t-sql/functions/ident-seed-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
