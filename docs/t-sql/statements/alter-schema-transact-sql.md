---
description: ALTER SCHEMA (Transact-SQL)
title: ALTER SCHEMA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SCHEMA
- ALTER_SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], transferring
- transferring objects between schemas
- ALTER SCHEMA statement
- schemas [SQL Server], modifying
- modifying schemas
ms.assetid: 0a760138-460e-410a-a3c1-d60af03bf2ed
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c87d341f26970e368374702dfd20b99b6f56c441
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339298"
---
# <a name="alter-schema-transact-sql"></a>ALTER SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Transfiere un elemento protegible entre esquemas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER SCHEMA schema_name   
   TRANSFER [ <entity_type> :: ] securable_name   
[;]  
  
<entity_type> ::=  
    {  
    Object | Type | XML Schema Collection  
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER SCHEMA schema_name   
   TRANSFER [ OBJECT :: ] securable_name   
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *schema_name*  
 Es el nombre de un esquema de la base de datos actual, al que se moverá el elemento protegible. No puede ser SYS ni INFORMATION_SCHEMA.  
  
 \<entity_type>  
 Es la clase de entidad para la que se va a cambiar el propietario. El valor predeterminado es objeto.  
  
 *securable_name*  
 Es el nombre de una o dos partes de un elemento protegible en el ámbito de esquema que se va a mover al esquema.  
  
## <a name="remarks"></a>Observaciones  
 Usuarios y esquemas están completamente separados.  
  
 ALTER SCHEMA solo se puede utilizar para mover elementos protegibles entre esquemas de la misma base de datos. Para cambiar o quitar un elemento protegible de un esquema, use la instrucción ALTER o DROP específica para ese elemento protegible.  
  
 Si se usa un nombre de una parte para *securable_name*, se usarán las reglas de resolución de nombres actualmente vigentes para localizar el elemento protegible.  
  
 Todos los permisos asociados al elemento protegible se quitarán cuando se mueva el elemento protegible al nuevo esquema. Si el propietario del elemento protegible se ha establecido de forma explícita, el propietario no cambiará. Si el propietario del elemento protegible se ha establecido en SCHEMA OWNER, el propietario seguirá siendo SCHEMA OWNER; no obstante, después del traslado, SCHEMA OWNER se resolverá en el propietario del nuevo esquema. El principal_id del nuevo propietario será NULL.  
  
 Mover un procedimiento almacenado, una función, una vista o un desencadenador no hará que cambie el nombre de esquema (si lo hay) de la columna de definición de la vista de catálogo [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) o bien obtenido usando la función integrada [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md). Por lo tanto, se desaconseja usar ALTER SCHEMA para mover estos tipos de objetos. En su lugar, quite el objeto y vuelva a crearlo en el nuevo esquema.  
  
 Mover un objeto como una tabla o una columna no hace que las referencias a ese objeto se actualicen automáticamente. Será necesario pues modificar de forma manual los objetos que hagan referencia al objeto que se ha movido. Por ejemplo, si se mueve una tabla y en un desencadenador existe una referencia a esa tabla, será necesario modificar el desencadenador para reflejar el nuevo nombre de esquema. Use [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) para ver las dependencias del objeto antes de moverlo.  

 Para cambiar el esquema de una tabla con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], haga clic con el botón derecho en la tabla en el Explorador de objetos y, después, haga clic en **Diseño**. Presione **F4** para abrir la ventana Propiedades. En el cuadro **Esquema**, seleccione un nuevo esquema.  
 
 ALTER SCHEMA usa un bloqueo de nivel de esquema.
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Permisos  
 Para transferir un elemento protegible de un esquema a otro, el usuario actual debe tener el permiso CONTROL para el elemento protegible (no el esquema) y el permiso ALTER para el esquema de destino.  
  
 Si el elemento protegible tiene una especificación EXECUTE AS OWNER y el propietario se establece en SCHEMA OWNER, el usuario también debe tener el permiso IMPERSONATE para el propietario del esquema de destino.  
  
 Cuando se mueve el elemento protegible, se quitan todos los permisos asociados a él.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-transferring-ownership-of-a-table"></a>A. Transferir la propiedad de una tabla  
 En el ejemplo siguiente se modifica el esquema `HumanResources` transfiriendo la tabla `Address` del esquema `Person` al esquema "HumanResources".  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER SCHEMA HumanResources TRANSFER Person.Address;  
GO  
```  
  
### <a name="b-transferring-ownership-of-a-type"></a>B. Transferir la propiedad de un tipo  
 El ejemplo siguiente crea un tipo en el esquema `Production` y, a continuación, transfiere el tipo al esquema `Person`.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
CREATE TYPE Production.TestType FROM [VARCHAR](10) NOT NULL ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
  
-- Change the type to the Person schema.  
ALTER SCHEMA Person TRANSFER type::Production.TestType ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-transferring-ownership-of-a-table"></a>C. Transferir la propiedad de una tabla  
 En el siguiente ejemplo se crea una tabla `Region` en el esquema `dbo`, se crea un esquema `Sales` y, por último, se mueve la tabla `Region` desde el esquema `dbo` al esquema `Sales`.  
  
```sql  
CREATE TABLE dbo.Region   
    (Region_id INT NOT NULL,  
    Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
  
CREATE SCHEMA Sales;  
GO  
  
ALTER SCHEMA Sales TRANSFER OBJECT::dbo.Region;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/create-schema-transact-sql.md)   
 [DROP SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/drop-schema-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

