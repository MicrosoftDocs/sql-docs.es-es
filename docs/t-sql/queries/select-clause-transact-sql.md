---
description: SELECT (cláusula de Transact-SQL)
title: SELECT (cláusula de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SELECT Clause
- SELECT_Clause_TSQL
- DISTINCT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- parentheses [SQL Server]
- identity columns [SQL Server], SELECT clause
- SELECT clause
- $IDENTITY keyword
- user-defined types [SQL Server], invoking methods and properties
- SELECT statement [SQL Server], processing orders
- clauses [SQL Server], SELECT
- $ROWGUID keyword
- queries [SQL Server], results
ms.assetid: 2616d800-4853-4cf1-af77-d32d68d8c2ef
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2a633f4cfb8a0c9e5158b24c89035c5e97588a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207597"
---
# <a name="select-clause-transact-sql"></a>SELECT (cláusula de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Especifica las columnas que va a devolver la consulta.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SELECT [ ALL | DISTINCT ]  
[ TOP ( expression ) [ PERCENT ] [ WITH TIES ] ]   
<select_list>   
<select_list> ::=   
    {   
      *   
      | { table_name | view_name | table_alias }.*   
      | {  
          [ { table_name | view_name | table_alias }. ]  
               { column_name | $IDENTITY | $ROWGUID }   
          | udt_column_name [ { . | :: } { { property_name | field_name }   
            | method_name ( argument [ ,...n] ) } ]  
          | expression  
          [ [ AS ] column_alias ]   
         }  
      | column_alias = expression   
    } [ ,...n ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **ALL**  
 Especifica que el conjunto de resultados puede incluir filas duplicadas. ALL es el valor predeterminado.  
  
 DISTINCT  
 Especifica que el conjunto de resultados solo puede incluir filas únicas. Los valores NULL se consideran iguales desde el punto de vista de la palabra clave DISTINCT.  
  
 TOP (*expression* ) [ PERCENT ] [ WITH TIES ]  
 Indica que el conjunto de resultados de la consulta solamente devolverá un primer conjunto o porcentaje de filas especificado. *expression* puede ser un número o un porcentaje de las filas.  
  
 Por compatibilidad con versiones anteriores, se permite el uso de TOP *expression* sin paréntesis en las instrucciones SELECT, aunque no se recomienda. Para más información, vea [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
\< select_list > Columnas que se van a seleccionar para el conjunto de resultados. La lista de selección es una serie de expresiones separadas por comas. El número máximo de expresiones que se puede especificar en la lista de selección es 4.096.  
  
 \*  
 Especifica que se deben devolver todas las columnas de todas las tablas y vistas de la cláusula FROM. Las columnas se devuelven por tabla o vista, tal como se especifique en la cláusula FROM, en el orden en que se encuentran en la tabla o vista.  
  
 *table_name* | *view_name* | *table* _ *alias*.*  
 Limita el ámbito de \* a la tabla o vista especificada.  
  
 *column_name*  
 Es el nombre de una columna que se va a devolver. Para evitar referencias ambiguas, como sucedería si dos tablas de la cláusula FROM tuvieran columnas con nombres duplicados, se debe calificar *column_name*. Por ejemplo, las tablas SalesOrderHeader y SalesOrderDetail de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] tienen una columna denominada ModifiedDate. Si se combinan ambas tablas en una consulta, se puede especificar la fecha de modificación de las entradas SalesOrderDetail en la lista de selección como SalesOrderDetail.ModifiedDate.  
  
 *expression*  
 Es una constante, una función o una combinación de nombres de columna, constantes y funciones conectados mediante un operador, varios operadores o una subconsulta.  
  
 $IDENTITY  
 Devuelve la columna de identidad. Para obtener más información, vea [IDENTITY &#40;Propiedad&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md), [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) y [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 Si más de una tabla de la cláusula FROM contiene una columna con la propiedad IDENTITY, se debe calificar $IDENTITY con el nombre de tabla específico; por ejemplo, T1.$IDENTITY.  
  
 $ROWGUID  
 Devuelve la columna GUID de fila.  
  
 Si más de una tabla de la cláusula FROM tiene la propiedad ROWGUIDCOL, se debe calificar $ROWGUID con el nombre de tabla específico; por ejemplo, T1.$ROWGUID.  
  
 *udt_column_name*  
 Es el nombre de la columna que tiene un tipo CLR (Common Language Runtime) definido por el usuario que se va a devolver.  
  
> [!NOTE]  
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] devuelve los valores de los tipos definidos por el usuario en representación binaria. Para devolver los valores de los tipos definidos por el usuario en formato XML o de cadena, use [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) o [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 { . | :: }  
 Especifica un método, una propiedad o un campo de un tipo definido por el usuario CLR. Use . para métodos, propiedades o campos de instancia (no estáticos). Use :: para métodos, propiedades o campos estáticos. Para invocar un método, propiedad o campo de un tipo definido por el usuario CLR, debe tener el permiso EXECUTE para el tipo.  
  
 *property_name*  
 Es una propiedad pública de *udt_column_name*.  
  
 *field_name*  
 Es un miembro de datos público de *udt_column_name*.  
  
 *method_name*  
 Es un método público de *udt_column_name* que toma uno o más argumentos. *method_name* no puede ser un método mutador.  
  
 En el ejemplo siguiente se seleccionan los valores de la columna `Location`, definida como de tipo `point`, de la tabla `Cities`, mediante la invocación de un método del tipo denominado `Distance`:  
  
```sql
CREATE TABLE dbo.Cities (  
     Name VARCHAR(20),  
     State VARCHAR(20),  
     Location POINT);  
GO  
DECLARE @p POINT (32, 23), @distance FLOAT;  
GO  
SELECT Location.Distance (@p)  
FROM Cities;  
```  
  
 *column_ alias*  
 Es un nombre alternativo que se utiliza para reemplazar el nombre de la columna en el conjunto de resultados de la consulta. Por ejemplo, se puede especificar un alias como Quantity, Quantity to Date o Qty para una columna denominada quantity.  
  
 Los alias se emplean también para especificar nombres para los resultados de expresiones; por ejemplo:  
  
 ```sql
 USE AdventureWorks2012;  
 GO  
 SELECT AVG(UnitPrice) AS [Average Price]  
 FROM Sales.SalesOrderDetail;
 ```  
  
 *column_alias* se puede usar en una cláusula ORDER BY. Sin embargo, no puede utilizarse en una cláusula WHERE, GROUP BY o HAVING. Si la expresión de consulta forma parte de una instrucción DECLARE CURSOR, *column_alias* no se puede usar en la cláusula FOR UPDATE.  
  
## <a name="remarks"></a>Observaciones  
 La longitud de los datos devueltos de las columnas **text** o **ntext** incluidas en la lista de selección se establece en el valor menor de los siguientes: el tamaño real de la columna **text**, la configuración predeterminada de TEXTSIZE para la sesión o el límite de la aplicación codificado de forma rígida. Para cambiar la longitud del texto devuelto de la sesión, utilice la instrucción SET. De forma predeterminada, la longitud máxima de los datos de texto que se devuelven con una instrucción SELECT es de 4.000 bytes.  
  
 El [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] provoca la excepción 511 y revierte la instrucción que se está ejecutando en ese momento si se produce alguno de estos comportamientos:  
  
-   La instrucción SELECT produce una fila de resultados o una fila de la tabla de trabajo intermedia que supera los 8.060 bytes.  
  
-   La instrucción DELETE, INSERT o UPDATE intenta realizar una acción en una fila que supera los 8.060 bytes.  
  
 Se produce un error si no se proporciona un nombre a una columna creada con una instrucción SELECT INTO o CREATE VIEW.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplos de SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  
