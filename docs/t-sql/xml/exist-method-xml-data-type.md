---
description: exist() (método del tipo de datos xml)
title: exist() (método del tipo de datos xml)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- exist() method
- exist method
ms.assetid: a55b75e0-0a17-4787-a525-9b095410f7af
author: MightyPen
ms.author: genemi
ms.openlocfilehash: b29a0143b2fca6bbcafaa94acd60e096a121480d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206884"
---
# <a name="exist-method-xml-data-type"></a>exist() (método del tipo de datos xml)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve un **bit** que representa una de las siguientes condiciones:  
  
-   1, que representa True, si la expresión XQuery de una consulta devuelve un resultado no vacío, es decir, si devuelve al menos un nodo XML.  
  
-   0, que representa False, si devuelve un resultado vacío.  
  
-   NULL si la instancia con datos del tipo **xml** con la que se ejecuta la consulta incluye valores NULL.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
exist (XQuery)   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 XQuery  
 Es una expresión XQuery, un literal de cadena.  
  
## <a name="remarks"></a>Observaciones  
  
> [!NOTE]  
>  El método **exist()** devuelve 1 para la expresión XQuery que devuelve un resultado no vacío. Si se especifican las funciones **true()** o **false()** dentro del método **exist()** , el método **exist()** devolverá 1, porque las funciones **true()** y **false()** devuelven los valores booleanos True y False respectivamente. En otras palabras, devuelven un resultado no vacío. Por tanto, **exist()** devolverá 1 (True), como se muestra en el siguiente ejemplo:  
  
```sql
DECLARE @x XML;  
SET @x='';  
SELECT @x.exist('true()');   
```  
  
## <a name="examples"></a>Ejemplos  
 En los ejemplos siguientes se muestra cómo especificar el método **exist()** .  
  
### <a name="example-specifying-the-exist-method-against-an-xml-type-variable"></a>Ejemplo: especificar el método exist() con una variable de tipo xml  
 En el siguiente ejemplo, @x es una variable de tipo **xml** (xml sin tipo) y @f es una variable de tipo entero que almacena el valor devuelto por el método **exist()** . El método **exist()** devuelve True (1) si el valor de fecha almacenado en la instancia XML es `2002-01-01`.  
  
```sql  
DECLARE @x XML;  
DECLARE @f BIT;  
SET @x = '<root Somedate = "2002-01-01Z"/>';  
SET @f = @x.exist('/root[(@Somedate cast as xs:date?) eq xs:date("2002-01-01Z")]');  
SELECT @f;  
```  
  
 Al comparar las fechas del método **exist()** , tenga en cuenta lo siguiente:  
  
-   El código `cast as xs:date?` se usa para convertir el valor al tipo **xs:date** con fines comparativos.  
  
-   El valor del atributo **\@Somedate** no tiene tipo. Al comparar este valor, se convierte implícitamente al tipo de la derecha de la comparación, el tipo **xs:date**.  
  
-   En lugar de **cast as xs:date()** , puede usar la función constructora **xs:date()** . Para más información, vea [Constructor Functions &#40;XQuery&#41;](../../xquery/constructor-functions-xquery.md) (Funciones de constructor [XQuery]).  
  
 El ejemplo siguiente es similar al anterior, con la diferencia de que tiene un elemento <`Somedate`>.  
  
```sql
DECLARE @x XML;  
DECLARE @f BIT;  
SET @x = '<Somedate>2002-01-01Z</Somedate>';  
SET @f = @x.exist('/Somedate[(text()[1] cast as xs:date ?) = xs:date("2002-01-01Z") ]')  
SELECT @f;  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   El método **text()** devuelve un nodo de texto que incluye el valor sin tipo `2002-01-01`. (El tipo XQuery es **xdt:untypedAtomic**). Debe convertir explícitamente este valor con tipo de **x** a **xsd:date**, puesto que en este caso no se admite la conversión implícita.  
  
### <a name="example-specifying-the-exist-method-against-a-typed-xml-variable"></a>Ejemplo: especificar el método exist() con una variable xml con tipo  
 En el ejemplo siguiente se muestra el uso del método **exist()** con una variable del tipo **xml**. Se trata de una variable XML con tipo, pues especifica el nombre de la colección del espacio de nombres del esquema, `ManuInstructionsSchemaCollection`.  
  
 En este ejemplo, en primer lugar se asigna a esta variable un documento con instrucciones de fabricación y, después, se usa el método **exist()** para comprobar si el documento incluye un elemento <`Location`> cuyo valor del atributo **LocationID** es 50.  
  
 El método **exist()** especificado con la variable @x devuelve 1 (True) si el documento con instrucciones de fabricación incluye un elemento <`Location`> con `LocationID=50`. De lo contrario, el método devolverá 0 (False).  
  
```sql
DECLARE @x XML (Production.ManuInstructionsSchemaCollection);  
SELECT @x=Instructions  
FROM Production.ProductModel  
WHERE ProductModelID=67;  
--SELECT @x  
DECLARE @f INT;  
SET @f = @x.exist(' declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    /AWMI:root/AWMI:Location[@LocationID=50]  
');  
SELECT @f;  
```  
  
### <a name="example-specifying-the-exist-method-against-an-xml-type-column"></a>Ejemplo: especificar el método exist() con una columna de tipo xml  
 La consulta siguiente recupera los identificadores de modelo de producto cuyas descripciones de catálogo no incluyen las especificaciones, elemento <`Specifications`>:  
  
```sql
SELECT ProductModelID, CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
    declare namespace  pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /pd:ProductDescription[not(pd:Specifications)]'  
    ) = 1;  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La cláusula WHERE selecciona únicamente las filas de la tabla **ProductDescription** que satisfacen la condición especificada en la columna de tipo **CatalogDescription xml**.  
  
-   El método **exist()** de la cláusula WHERE devuelve 1 (True) si el XML no incluye ningún elemento <`Specifications`>. Observe el uso de la [función not() (XQuery)](../../xquery/functions-on-boolean-values-not-function.md).  
  
-   La [función sql:column() (XQuery)](../../xquery/xquery-extension-functions-sql-column.md) se usa para recuperar el valor de una columna distinta de XML.  
  
-   Esta consulta devuelve un conjunto de filas vacío.  
  
 La consulta especifica los métodos **query()** y **exist()** del tipo de datos xml, y ambos métodos declaran los mismos espacios de nombres en el prólogo de la consulta. En este caso, puede utilizar WITH XMLNAMESPACES para declarar el prefijo y utilizarlo en la consulta.  
  
```sql
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[not(pd:Specifications)]'  
    ) = 1;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Agregar espacios de nombres a consultas con WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Comparar XML con tipo y XML sin tipo](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Crear instancias de datos XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [métodos del tipo de datos xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Lenguaje de manipulación de datos XML &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
