---
title: 'Función SQL: column () (XQuery) | Microsoft Docs'
description: 'Obtenga información sobre cómo se puede usar la función SQL: column () de XQuery para enlazar datos relacionales no XML dentro de XML y reunir datos relacionales y XML.'
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
- sql:column function
- sql:column() function
ms.assetid: e8f67bdf-b489-49a9-9d0f-2069c1750467
author: rothja
ms.author: jroth
ms.openlocfilehash: 3d2f550e3516594a269257786e109df9c84a1082
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341877"
---
# <a name="xquery-extension-functions---sqlcolumn"></a>Funciones de extensión de XQuery: sql:column()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Como se describe en el tema [enlazar datos relacionales dentro de XML](../t-sql/xml/binding-relational-data-inside-xml-data.md), puede usar la función **SQL: column (()** al utilizar [métodos de tipo de datos XML](../t-sql/xml/xml-data-type-methods.md) para exponer un valor relacional dentro de XQuery.  
  
 Por ejemplo, el [método Query () (tipo de datos XML)](../t-sql/xml/query-method-xml-data-type.md) se utiliza para especificar una consulta en una instancia XML almacenada en una variable o columna de tipo **XML** . En algunos casos, es posible que desee que la consulta utilice valores de una columna no XML para obtener datos relacionales y XML al mismo tiempo. Para ello, utilice la función **SQL: column ()** .  
  
 El valor SQL se asignará al valor XQuery correspondiente y su tipo será un tipo base XQuery equivalente al tipo SQL correspondiente.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sql:column("columnName")  
```  
  
## <a name="remarks"></a>Comentarios  
 Tenga en cuenta que la referencia a una columna especificada en la función **SQL: column ()** dentro de una expresión XQuery hace referencia a una columna de la fila que se está procesando.  
  
 En [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , solo se puede hacer referencia a una instancia **XML** en el contexto de la expresión de origen de una instrucción INSERT de XML-DML; de lo contrario, no se puede hacer referencia a columnas que son de tipo **XML** o un tipo definido por el usuario CLR.  
  
 La función **SQL: column ()** no se admite en las operaciones de combinación. En su lugar, se puede utilizar la operación APPLY.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-sqlcolumn-to-retrieve-the-relational-value-inside-xml"></a>A. Usar sql:column() para recuperar el valor relacional en XML  
 Al crear XML, el ejemplo siguiente muestra cómo recuperar valores de una columna relacional no XML para enlazar datos XML y relacionales.  
  
 La consulta crea XML con el formato siguiente:  
  
```xml
<Product ProductID="771" ProductName="Mountain-100 Silver, 38" ProductPrice="3399.99" ProductModelID="19"   
  ProductModelName="Mountain 100" />  
```  
  
 Tenga en cuenta las siguientes observaciones acerca del XML creado:  
  
-   Los valores del atributo **ProductID**, **ProductName** y **ProductPrice** se obtienen de la tabla **Product** .  
  
-   El valor del atributo **ProductModelID** se recupera de la tabla **ProductModel** .  
  
-   Para que la consulta sea más interesante, el valor del atributo **ProductModelName** se obtiene de la columna **CatalogDescription** de **tipo XML**. Dado que no toda la información del catálogo de modelos de productos XML se almacena, la instrucción `if` se utiliza para recuperar el valor solo si existe.  
  
    ```sql
    SELECT P.ProductID, CatalogDescription.query('  
    declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           <Product   
               ProductID=       "{ sql:column("P.ProductID") }"  
               ProductName=     "{ sql:column("P.Name") }"  
               ProductPrice=    "{ sql:column("P.ListPrice") }"  
               ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
               { if (not(empty(/pd:ProductDescription))) then  
                 attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
                else   
                   ()  
    }  
            </Product>  
    ') as Result  
    FROM Production.ProductModel PM, Production.Product P  
    WHERE PM.ProductModelID = P.ProductModelID  
    AND   CatalogDescription is not NULL  
    ORDER By PM.ProductModelID  
    ```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   Dado que los valores se recuperan de dos tablas diferentes, la cláusula FROM especifica dos tablas. La condición en la cláusula WHERE filtra el resultado y recupera solo productos cuyos modelos disponen de descripciones de catálogo.  
  
-   La palabra clave **namespace** del [prólogo de XQuery](../xquery/modules-and-prologs-xquery-prolog.md) define el prefijo del espacio de nombres XML, "PD", que se utiliza en el cuerpo de la consulta. Observe que los alias de tabla, "P" y "PM", se definen en la cláusula FROM de la propia consulta.  
  
-   La función **SQL: column ()** se usa para traer valores que no sean XML dentro de XML.  
  
 Éste es el resultado parcial:  
  
```  
ProductID               Result  
-----------------------------------------------------------------  
771         <Product ProductID="771"                   ProductName="Mountain-100 Silver, 38"   
                  ProductPrice="3399.99" ProductModelID="19"   
                  ProductModelName="Mountain 100" />  
...  
```  
  
 La consulta siguiente crea XML que contiene información específica del producto. Esta información incluye ProductID, ProductName, ProductPrice y, si está disponible, ProductModelName para todos los productos que pertenecen a un modelo específico, ProductModelID=19. A continuación, el XML se asigna a la @x variable de tipo **XML** .  
  
```sql
declare @x xml  
SELECT @x = CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <Product   
           ProductID=       "{ sql:column("P.ProductID") }"  
           ProductName=     "{ sql:column("P.Name") }"  
           ProductPrice=    "{ sql:column("P.ListPrice") }"  
           ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
           { if (not(empty(/pd:ProductDescription))) then  
             attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
            else   
               ()  
}  
        </Product>  
')   
FROM Production.ProductModel PM, Production.Product P  
WHERE PM.ProductModelID = P.ProductModelID  
And P.ProductModelID = 19  
select @x  
```  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server funciones de extensión de XQuery]()   
 [Comparar XML con tipo y XML sin tipo](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Datos XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Crear instancias de datos XML](../relational-databases/xml/create-instances-of-xml-data.md)   
 [métodos del tipo de datos xml](../t-sql/xml/xml-data-type-methods.md)   
 [Lenguaje de manipulación de datos XML &#40;XML DML&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
