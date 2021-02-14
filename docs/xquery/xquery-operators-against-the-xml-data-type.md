---
title: Operadores XQuery con el tipo de datos XML | Microsoft Docs
description: Obtenga información sobre los operadores de XQuery que se pueden utilizar con el tipo de datos XML.
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
- XQuery, operators
- operators [XQuery]
- xml data type [SQL Server], XQuery
ms.assetid: 39ca3d2e-e928-4333-872b-75c4ccde8e79
author: rothja
ms.author: jroth
ms.openlocfilehash: c340049fd9655febfe3974a895b9f93e8cf1e2fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352367"
---
# <a name="xquery-operators-against-the-xml-data-type"></a>Operadores XQuery con el tipo de datos XML
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  XQuery admite los operadores siguientes:  
  
-   Operadores numéricos (+, -, *, div, mod)  
  
-   Operadores de comparación de valores (eq, ne, lt, gt, le, ge)  
  
-   Operadores de comparación general (=,! =, \<, > , \<=, > =)  
  
 Para obtener más información sobre estos operadores, vea [expresiones de comparación &#40;XQuery&#41;](../xquery/comparison-expressions-xquery.md)  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-general-operators"></a>A. Utilizar operadores generales  
 Esta consulta ilustra el uso de operadores generales que se aplican a secuencias y también para comparar secuencias. La consulta recupera una secuencia de números de teléfono para cada cliente de la columna **AdditionalContactInfo** de la tabla **Contact** . Después, esta secuencia se compara con la secuencia de dos números de teléfono ("111-111-1111", "222-2222").  
  
 La consulta utiliza el **=** operador de comparación. Cada nodo de la secuencia del lado derecho del **=** operador se compara con cada nodo de la secuencia del lado izquierdo. Si los nodos coinciden, la comparación del nodo es **true**. Después, el valor se convierte a int y se compara con 1, y la consulta devuelve el Id. de cliente.  
  
```sql
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS ACI,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS ACT)  
SELECT ContactID   
FROM   Person.Contact  
WHERE  AdditionalContactInfo.value('  
      //ACI:AdditionalContactInfo//ACT:telephoneNumber/ACT:number =   
          ("111-111-1111", "222-2222")',  
      'bit')= cast(1 as bit)  
```  
  
 Hay otra manera de observar cómo funciona la consulta anterior: cada valor de número de teléfono de teléfono recuperado de la columna **AdditionalContactInfo** se compara con el conjunto de dos números de teléfono. Si el valor está en el conjunto, se devuelve ese cliente en el resultado.  
  
### <a name="b-using-a-numeric-operator"></a>B. Utilizar un operador numérico  
 El operador + de esta consulta es un operador de valor, porque se aplica a un solo elemento. Por ejemplo, el valor 1 se suma a un tamaño de lote devuelto por la consulta:  
  
```sql
SELECT ProductModelID, Instructions.query('  
     declare namespace   
 AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
     for $i in (/AWMI:root/AWMI:Location)[1]  
     return   
       <Location LocationID="{ ($i/@LocationID) }"  
                   LotSize  = "{  number($i/@LotSize) }"  
                   LotSize2 = "{ number($i/@LotSize) + 1 }"  
                   LotSize3 = "{ number($i/@LotSize) + 2 }" >  
  
       </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
### <a name="c-using-a-value-operator"></a>C. Utilizar un operador de valor  
 La consulta siguiente recupera el <`Picture`> elementos para un modelo de producto en el que el tamaño de la imagen es "pequeño":  
  
```sql
SELECT CatalogDescription.query('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     for $P in /PD:ProductDescription/PD:Picture[PD:Size eq "small"]  
     return  
           $P  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Dado que ambos operandos del operador **EQ** son valores atómicos, se utiliza el operador Value en la consulta. Puede escribir la misma consulta mediante el operador de comparación general ( **=** ).  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos XML](../xquery/xquery-functions-against-the-xml-data-type.md)   
 [Datos XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Referencia del lenguaje XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
