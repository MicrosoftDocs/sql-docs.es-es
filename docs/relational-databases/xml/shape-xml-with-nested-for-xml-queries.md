---
title: Dar forma a XML con consultas FOR XML anidadas | Microsoft Docs
description: Vea un ejemplo del uso de consultas FOR XML anidadas para dar forma al XML resultante.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML query
- queries [XML in SQL Server], nested FOR XML
- XML [SQL Server], FOR XML queries
ms.assetid: 8dc42c05-16e8-4b7b-a5d3-550b55acae26
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6fe40251156c3a668a0f5c78c2a8aaddb80322e6
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2020
ms.locfileid: "85758531"
---
# <a name="shape-xml-with-nested-for-xml-queries"></a>Dar forma a XML con consultas FOR XML anidadas
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En el ejemplo siguiente se consulta la tabla `Production.Product` para recuperar los valores `ListPrice` y `StandardCost` de un producto específico. Para hacer interesante la consulta, se devuelven ambos precios en un elemento <`Price`>, y cada elemento <`Price`> tiene un atributo `PriceType`.  
  
## <a name="example"></a>Ejemplo  
 Ésta es la forma esperada del XML:  
  
```  
<xsd:schema xmlns:schema="urn:schemas-microsoft-com:sql:SqlRowSet2" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet2" elementFormDefault="qualified">  
  <xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="https://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />  
  <xsd:element name="Production.Product" type="xsd:anyType" />  
</xsd:schema>  
<Production.Product xmlns="urn:schemas-microsoft-com:sql:SqlRowSet2" ProductID="520">  
  <Price xmlns="" PriceType="ListPrice">133.34</Price>  
  <Price xmlns="" PriceType="StandardCost">98.77</Price>  
</Production.Product>  
```  
  
 Esta es la consulta FOR XML anidada:  
  
```  
USE AdventureWorks2012;  
GO  
SELECT Product.ProductID,   
          (SELECT 'ListPrice' as PriceType,   
                   CAST(CAST(ListPrice as NVARCHAR(40)) as XML)   
           FROM    Production.Product Price   
           WHERE   Price.ProductID=Product.ProductID   
           FOR XML AUTO, TYPE),  
          (SELECT  'StandardCost' as PriceType,   
                   CAST(CAST(StandardCost as NVARCHAR(40)) as XML)   
           FROM    Production.Product Price   
           WHERE   Price.ProductID=Product.ProductID   
           FOR XML AUTO, TYPE)  
FROM Production.Product  
WHERE ProductID=520  
for XML AUTO, TYPE, XMLSCHEMA  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La instrucción SELECT externa construye el elemento <`Product`> que tiene un atributo **ProductID** y dos secundarios <`Price`>.  
  
-   Las dos instrucciones SELECT internas construyen dos elementos <`Price`>, cada uno con un atributo **PriceType** y XML que devuelve el precio del producto.  
  
-   La directiva XMLSCHEMA de la instrucción SELECT externa genera el esquema XSD insertado que describe la forma del XML resultante.  
  
 Para hacer interesante la consulta, puede escribir la consulta FOR XML y después escribir una consulta XQuery para el resultado con el fin de cambiar la forma del XML, como se muestra en la siguiente consulta:  
  
```  
SELECT ProductID,   
 ( SELECT p2.ListPrice, p2.StandardCost  
   FROM Production.Product p2   
   WHERE Product.ProductID = p2.ProductID  
   FOR XML AUTO, ELEMENTS XSINIL, type ).query('  
                                   for $p in /p2/*  
                                   return   
                                    <Price PriceType = "{local-name($p)}">  
                                     { data($p) }  
                                    </Price>  
                                  ')  
FROM Production.Product  
WHERE ProductID = 520  
FOR XML AUTO, TYPE  
```  
  
 En el ejemplo anterior se usa el método **query()** del tipo de datos **xml** para consultar el XML devuelto por la consulta FOR XML interna y construir el resultado esperado.  
  
 El resultado es el siguiente:  
  
```  
<Production.Product ProductID="520">  
  <Price PriceType="ListPrice">133.3400</Price>  
  <Price PriceType="StandardCost">98.7700</Price>  
</Production.Product>  
```  
  
## <a name="see-also"></a>Consulte también  
 [Usar consultas FOR XML anidadas](../../relational-databases/xml/use-nested-for-xml-queries.md)  
  
  
