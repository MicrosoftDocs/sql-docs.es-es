---
title: 'Función SQL: variable () (XQuery) | Microsoft Docs'
description: 'Obtenga información sobre cómo usar la función de extensión XQuery SQL: variable () para exponer una variable que contenga un valor relacional de SQL dentro de una expresión XQuery.'
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sql:variable() function
- sql:variable function
ms.assetid: 6e2e5063-c1cf-4b5a-b642-234921e3f4f7
author: rothja
ms.author: jroth
ms.openlocfilehash: d879088a98bfa7e826c53e4b90822bc65caff57a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350387"
---
# <a name="xquery-extension-functions---sqlvariable"></a>Funciones de extensión de XQuery: sql:variable()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Expone una variable que contiene un valor relacional SQL dentro de una expresión XQuery.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sql:variable("variableName") as xdt:anyAtomicType?  
```  
  
## <a name="remarks"></a>Comentarios  
 Tal y como se describe en el tema [enlazar datos relacionales dentro de XML](../t-sql/xml/binding-relational-data-inside-xml-data.md), puede utilizar esta función al utilizar [métodos de tipo de datos XML](../t-sql/xml/xml-data-type-methods.md) para exponer un valor relacional dentro de XQuery.  
  
 Por ejemplo, el [método Query ()](../t-sql/xml/query-method-xml-data-type.md) se utiliza para especificar una consulta en una instancia XML almacenada en una variable o columna de tipo de datos **XML** . En ocasiones, es posible que también se desee que la consulta utilice valores de una variable [!INCLUDE[tsql](../includes/tsql-md.md)], o un parámetro, para combinar los datos relacionales y XML. Para ello, utilice la función **SQL: variable** .  
  
 El valor SQL se asignará a un valor XQuery correspondiente y su tipo será un tipo base XQuery que sea equivalente al tipo SQL correspondiente.  
  
 Solo puede hacer referencia a una instancia **XML** en el contexto de la expresión de origen de una instrucción INSERT de XML-DML; de lo contrario, no puede hacer referencia a valores que son de tipo **XML** o de un tipo definido por el usuario Common Language Runtime (CLR).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-the-sqlvariable-function-to-bring-a-transact-sql-variable-value-into-xml"></a>A. Usar la función sql:variable() para incorporar una variable Transact-SQL a XML  
 En el siguiente ejemplo se crea una instancia XML formada por los siguientes elementos:  
  
-   Un valor (`ProductID`) de una columna no XML. La [función SQL: column ()](../xquery/xquery-extension-functions-sql-column.md) se utiliza para enlazar este valor en el XML.  
  
-   Un valor (`ListPrice`) de una columna no XML de otra tabla. De nuevo, se utiliza `sql:column()` para enlazar este valor en el XML.  
  
-   Un valor (`DiscountPrice`) de una variable [!INCLUDE[tsql](../includes/tsql-md.md)]. El método `sql:variable()` se utiliza para enlazar este valor en el XML.  
  
-   Un valor ( `ProductModelName` ) de una columna de tipo **XML** para que la consulta sea más interesante.  
  
 Esta es la consulta:  
  
```sql
DECLARE @price money  
  
SET @price=2500.00  
SELECT ProductID, Production.ProductModel.ProductModelID,CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
       <Product   
           ProductID="{ sql:column("Production.Product.ProductID") }"  
           ProductModelID= "{ sql:column("Production.Product.ProductModelID") }"  
           ProductModelName="{/pd:ProductDescription[1]/@ProductModelName }"  
           ListPrice="{ sql:column("Production.Product.ListPrice") }"  
           DiscountPrice="{ sql:variable("@price") }"  
        />')   
FROM Production.Product   
JOIN Production.ProductModel  
ON Production.Product.ProductModelID = Production.ProductModel.ProductModelID  
WHERE ProductID=771  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La función XQuery incluida en el método `query()` construye el XML.  
  
-   La `namespace` palabra clave se usa para definir un prefijo de espacio de nombres en el [prólogo de XQuery](../xquery/modules-and-prologs-xquery-prolog.md). Esto se hace porque el valor del atributo `ProductModelName` se recupera de la columna de tipo `CatalogDescription xml`, que tiene un esquema asociado a ella.  
  
 El resultado es el siguiente:  
  
```xml
<Product ProductID="771" ProductModelID="19"   
         ProductModelName="Mountain 100"   
         ListPrice="3399.99" DiscountPrice="2500" />  
```  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server funciones de extensión de XQuery](./xquery-extension-functions-sql-column.md)   
 [Comparar XML con tipo y XML sin tipo](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Datos XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Crear instancias de datos XML](../relational-databases/xml/create-instances-of-xml-data.md)   
 [métodos del tipo de datos xml](../t-sql/xml/xml-data-type-methods.md)   
 [Lenguaje de manipulación de datos XML &#40;XML DML&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
