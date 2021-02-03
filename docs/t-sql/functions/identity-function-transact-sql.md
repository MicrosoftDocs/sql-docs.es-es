---
description: IDENTITY (Función) (Transact-SQL)
title: IDENTITY (Function) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY function
- SELECT statement [SQL Server], IDENTITY function
- inserting identity columns
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY function
ms.assetid: ebec77eb-fc02-4feb-b6c5-f0098d43ccb6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: bad6595806a3b50cc0634cecbae2c4db098799ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189924"
---
# <a name="identity-function-transact-sql"></a>IDENTITY (Función) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Solo se usa en una instrucción SELECT con una cláusula INTO *table* para insertar una columna de identidad en una nueva tabla. Aunque es similar, la función IDENTITY no es la propiedad IDENTITY que se utiliza con CREATE TABLE y ALTER TABLE.  
  
> [!NOTE]  
>  Para crear un número que se incremente automáticamente y que se pueda usar en varias tablas, o que se pueda llamar desde las aplicaciones sin hacer referencia a ninguna tabla, vea [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
IDENTITY (data_type [ , seed , increment ] ) AS column_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *data_type*  
 Se trata del tipo de datos de la columna de identidad. Los tipos de datos válidos para una columna de identidad son cualquier tipo de datos de la categoría de los enteros, excepto el tipo de datos **bit** o el tipo de datos **decimal**.  
  
 *seed*  
 Se trata del valor entero que se asignará a la primera fila de la tabla. A cada fila siguiente se le asigna el siguiente valor de identidad, que es igual al último valor IDENTITY más el valor de *increment*. Si no se especifica *seed* ni *increment*, el valor predeterminado de ambos es 1.  
  
 *increment*  
 Se trata del incremento que se debe agregar al valor de *seed* en las sucesivas filas de la tabla.  
  
 *column_name*  
 Se trata del nombre de la columna que se va a insertar en la nueva tabla.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve lo mismo que *data_type*.  
  
## <a name="remarks"></a>Comentarios  
 Debido a que esta función crea una columna en una tabla, se debe especificar un nombre para la columna en la lista de selección de una de las formas siguientes:  
  
```sql  
--(1)  
SELECT IDENTITY(int, 1,1) AS ID_Num  
INTO NewTable  
FROM OldTable;  
  
--(2)  
SELECT ID_Num = IDENTITY(int, 1, 1)  
INTO NewTable  
FROM OldTable;  
```  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se insertan todas las filas de la tabla `Contact` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] en una nueva tabla denominada `NewContact`. La función IDENTITY se utiliza para iniciar los números de identificación a partir de 100 en lugar de 1 en la tabla `NewContact`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.NewContact', N'U') IS NOT NULL  
    DROP TABLE Person.NewContact;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
SELECT  IDENTITY(smallint, 100, 1) AS ContactNum,  
        FirstName AS First,  
        LastName AS Last  
INTO Person.NewContact  
FROM Person.Person;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
SELECT ContactNum, First, Last FROM Person.NewContact;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [IDENTITY &#40;propiedad&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [SELECT @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/select-local-variable-transact-sql.md)   
 [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [sys.identity_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-identity-columns-transact-sql.md)  
  
  
