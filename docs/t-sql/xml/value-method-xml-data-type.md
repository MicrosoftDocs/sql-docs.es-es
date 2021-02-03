---
description: value() (método del tipo de datos xml)
title: value() (método del tipo de datos xml)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- value method
- value() method
ms.assetid: 298a7361-dc9a-4902-9b1e-49a093cd831d
author: MightyPen
ms.author: genemi
ms.openlocfilehash: dce7148340136615b79d4bf4e4ec9dd15627c88e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181729"
---
# <a name="value-method-xml-data-type"></a>value() (método del tipo de datos xml)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Realiza una consulta XQuery en datos XML y devuelve un valor de tipo SQL. Este método devuelve un valor escalar.  
  
 Normalmente, este método se usa para extraer un valor de una instancia XML almacenada en una columna, un parámetro o una variable de tipo **xml**. De esta manera, se pueden especificar consultas SELECT que combinen o comparen datos XML con datos de columnas que no son XML.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
value (XQuery, SQLType)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *XQuery*  
 Es la expresión *XQuery*, un literal de cadena, que recupera datos de la instancia XML. La expresión XQuery debe devolver un valor como máximo. En caso contrario, se devolverá un error.  
  
 *SQLType*  
 Es el tipo SQL preferido, un literal de cadena, que se devuelve. El tipo de valor devuelto de este método coincide con el parámetro *SQLType*. *SQLType* no puede ser un tipo de datos **xml**, un tipo Common Language Runtime (CLR) definido por el usuario, ni un tipo de datos **image**, **text**, **ntext** ni **sql_variant**. *SQLType* puede ser un tipo de datos SQL definido por el usuario.  
  
 El método **value()** usa el operador CONVERT de [!INCLUDE[tsql](../../includes/tsql-md.md)] de manera implícita e intenta convertir el resultado de la expresión XQuery, la representación de cadena serializada, del tipo XSD al tipo SQL correspondiente especificado por la conversión [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para obtener más información sobre las reglas de conversión de tipos de CONVERT, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
> [!NOTE]  
>  Por motivos de rendimiento, en lugar de usar el método **value()** en un predicado para la comparación con un valor relacional, use **exist()** con **sql:column()** . Esto se muestra en el ejemplo D a continuación.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-the-value-method-against-an-xml-type-variable"></a>A. Utilizar el método value() con una variable de tipo xml  
 En el ejemplo siguiente, una instancia XML está almacenada en una variable de tipo `xml`. El método `value()` recupera el valor del atributo `ProductID` en el XML. Después, el valor se asigna a una variable `int`.  
  
```sql
DECLARE @myDoc XML  
DECLARE @ProdID INT  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>'  
  
SET @ProdID =  @myDoc.value('(/Root/ProductDescription/@ProductID)[1]', 'int' )  
SELECT @ProdID  
```  
  
 Como resultado se devuelve el valor 1.  
  
 Aunque hay un solo atributo `ProductID` en la instancia XML, las reglas de los tipos estáticos requieren que se especifique explícitamente que la expresión de ruta de acceso devuelva un singleton. Por tanto, se especifica `[1]` al final de la expresión de ruta de acceso. Para obtener más información sobre los tipos estáticos, vea [XQuery y el establecimiento de tipos estáticos](../../xquery/xquery-and-static-typing.md).  
  
### <a name="b-using-the-value-method-to-retrieve-a-value-from-an-xml-type-column"></a>B. Utilizar el método value() para recuperar un valor de una columna de tipo xml  
 La siguiente consulta se especifica en una columna de tipo **xml** (`CatalogDescription`) de la base de datos `AdventureWorks`. La consulta recupera los valores del atributo `ProductModelID` de cada instancia XML almacenada en la columna.  
  
```sql
SELECT CatalogDescription.value('             
    declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";             
       (/PD:ProductDescription/@ProductModelID)[1]', 'int') AS Result             
FROM Production.ProductModel             
WHERE CatalogDescription IS NOT NULL             
ORDER BY Result DESC             
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   Se utiliza la palabra clave `namespace` para definir un prefijo de espacio de nombres.  
  
-   Según los requisitos de los tipos estáticos, se agrega `[1]` al final de la expresión de ruta de acceso del método `value()` para indicar explícitamente que la expresión de ruta de acceso devuelva un singleton.  
  
 Éste es el resultado parcial:  
  
```  
-----------  
35           
34           
...  
```  
  
### <a name="c-using-the-value-and-exist-methods-to-retrieve-values-from-an-xml-type-column"></a>C. Utilizar los métodos value() y exist() para recuperar valores de una columna de tipo xml  
 En el siguiente ejemplo se muestra el uso del método `value()` y el [método exist()](../../t-sql/xml/exist-method-xml-data-type.md) del tipo de datos **xml**. El método `value()` se utiliza para recuperar los valores del atributo `ProductModelID` del XML. El método `exist()` de la cláusula `WHERE` se utiliza para filtrar las filas de la tabla.  
  
 La consulta recupera los identificadores de modelo de producto de las instancias XML que incluyen información de garantía (el elemento <`Warranty`>) entre sus características. La condición de la cláusula `WHERE` utiliza el método `exist()` para recuperar únicamente las filas que cumplan esta condición.  
  
```sql
SELECT CatalogDescription.value('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           (/PD:ProductDescription/@ProductModelID)[1] ', 'int') AS Result  
FROM  Production.ProductModel  
WHERE CatalogDescription.exist('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
  
     /PD:ProductDescription/PD:Features/wm:Warranty ') = 1  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La columna `CatalogDescription` es una columna XML con tipo. Esto significa que tiene asociada una colección de esquemas. En el [Prólogo de XQuery](../../xquery/modules-and-prologs-xquery-prolog.md), se usa la declaración del espacio de nombres para definir el prefijo que se va a usar posteriormente en el cuerpo de la consulta.  
  
-   Si el método `exist()` devuelve `1` (True), significa que la instancia XML incluye el elemento secundario <`Warranty`> entre sus características.  
  
-   Después, el método `value()` de la cláusula `SELECT` recupera los valores del atributo `ProductModelID` como enteros.  
  
 Éste es el resultado parcial:  
  
```  
Result       
-----------  
19           
23           
...  
```  
  
### <a name="d-using-the-exist-method-instead-of-the-value-method"></a>D. Utilizar el método exist() en lugar del método value()  
 Por motivos de rendimiento, en lugar de utilizar el método `value()` de un predicado para compararlo con un valor relacional, utilice `exist()` con `sql:column()`. Por ejemplo:  
  
```sql
CREATE TABLE T (c1 INT, c2 VARCHAR(10), c3 XML)  
GO  
  
SELECT c1, c2, c3   
FROM T  
WHERE c3.value( '/root[1]/@a', 'integer') = c1  
GO  
```  
  
 Esto se puede escribir de la forma siguiente:  
  
```sql
SELECT c1, c2, c3   
FROM T  
WHERE c3.exist( '/root[@a=sql:column("c1")]') = 1  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Agregar espacios de nombres a consultas con WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Comparar XML con tipo y XML sin tipo](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Crear instancias de datos XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [métodos del tipo de datos xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Lenguaje de manipulación de datos XML &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
