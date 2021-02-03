---
title: SET IDENTITY_INSERT (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la instrucción SET IDENTITY_INSERT. Cuando se establece en ON, permite insertar valores explícitos en la columna de identidad de una tabla.
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET IDENTITY_INSERT
- SET_IDENTITY_INSERT_TSQL
- IDENTITY_INSERT_TSQL
- IDENTITY_INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY_INSERT option
- SET IDENTITY_INSERT statement
- identity values [SQL Server], explicit values
- identity columns [SQL Server], explicit values
ms.assetid: a5dd49f2-45c7-44a8-b182-e0a5e5c373ee
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: da09ff4d042cac7ededad5090e45b61f99ba42e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190406"
---
# <a name="set-identity_insert-transact-sql"></a>SET IDENTITY_INSERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Permite insertar valores explícitos en la columna identidad de una tabla.  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SET IDENTITY_INSERT [ [ database_name . ] schema_name . ] table_name { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *database_name*  
 Es el nombre de la base de datos en la que reside la tabla especificada.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece la tabla.  
  
 *table_name*  
 Es el nombre de la tabla con una columna de identidad.  
  
## <a name="remarks"></a>Observaciones  
 En todo momento, solo una tabla de una sesión puede tener la propiedad IDENTITY_INSERT establecida en ON. Si ya existe una tabla con esta propiedad establecida en ON y se ejecuta una instrucción SET IDENTITY_INSERT ON para otra tabla, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un mensaje de error que indica que SET IDENTITY_INSERT ya está establecido en ON y la tabla para la que se ha establecido.  
  
 Si el valor insertado es mayor que el valor de identidad actual de la tabla, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza automáticamente el nuevo valor insertado como valor de identidad actual.  
  
 La opción SET IDENTITY_INSERT se establece en tiempo de ejecución, no en tiempo de análisis.  
  
## <a name="permissions"></a>Permisos  
 El usuario debe ser el propietario de la tabla o disponer del permiso ALTER en esta.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente crea una tabla con una columna de identidad y muestra cómo se puede utilizar la opción `SET IDENTITY_INSERT` para rellenar un vacío en los valores de identidad causado por una instrucción `DELETE`.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Create tool table.  
CREATE TABLE dbo.Tool(  
   ID INT IDENTITY NOT NULL PRIMARY KEY,   
   Name VARCHAR(40) NOT NULL  
);  
GO  
-- Inserting values into products table.  
INSERT INTO dbo.Tool(Name)   
VALUES ('Screwdriver')  
        , ('Hammer')  
        , ('Saw')  
        , ('Shovel');  
GO  
  
-- Create a gap in the identity values.  
DELETE dbo.Tool  
WHERE Name = 'Saw';  
GO  
  
SELECT *   
FROM dbo.Tool;  
GO  
  
-- Try to insert an explicit ID value of 3;  
-- should return an error:
-- An explicit value for the identity column in table 'AdventureWorks2012.dbo.Tool' can only be specified when a column list is used and IDENTITY_INSERT is ON.
INSERT INTO dbo.Tool (ID, Name) VALUES (3, 'Garden shovel');  
GO  
-- SET IDENTITY_INSERT to ON.  
SET IDENTITY_INSERT dbo.Tool ON;  
GO  
  
-- Try to insert an explicit ID value of 3.  
INSERT INTO dbo.Tool (ID, Name) VALUES (3, 'Garden shovel');  
GO  
  
SELECT *   
FROM dbo.Tool;  
GO  
-- Drop products table.  
DROP TABLE dbo.Tool;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [IDENTITY &#40;propiedad&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
