---
title: Función Number (XQuery) | Microsoft Docs
description: Obtenga información sobre el número de función de XQuery () que devuelve el valor numérico de un argumento especificado.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- number function
- fn:number function
ms.assetid: dff6d19b-765c-4df9-afff-9a0e7be9b91b
author: rothja
ms.author: jroth
ms.openlocfilehash: 6aa035be000fbbda12a7205c33925daf656c3da0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345022"
---
# <a name="functions-on-nodes---number"></a>Funciones usadas en nodos: number
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve el valor numérico del nodo que se indica mediante *$arg*.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:number() as xs:double?   
fn:number($arg as node()?) as xs:double?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Nodo cuyo valor se devolverá como un número.  
  
## <a name="remarks"></a>Comentarios  
 Si no se especifica *$arg* , se devuelve el valor numérico del nodo de contexto, convertido en Double. En SQL Server, **FN: Number ()** sin un argumento solo se puede usar en el contexto de un predicado dependiente del contexto. En concreto, solo se puede utilizar entre corchetes ([ ]). Por ejemplo, la siguiente expresión devuelve el `ROOT` elemento <>.  
  
```  
declare @x xml  
set @x='<ROOT>111</ROOT>'  
select @x.query('/ROOT[number()=111]')  
```  
  
 Si el valor del nodo no es una representación léxica válida de un tipo simple numérico, tal como se define en el **esquema XML parte 2: tipos de información, recomendación de W3C**, la función devuelve una secuencia vacía. No se admiten los valores NaN.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-number-xquery-function-to-retrieve-the-numeric-value-of-an-attribute"></a>A. Usar la función number() de XQuery para recuperar el valor numérico de un atributo  
 La consulta siguiente recupera el valor numérico del atributo de tamaño de lote de la primera ubicación de centro de trabajo del proceso de fabricación del modelo de producto 7.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     for $i in (//AWMI:root//AWMI:Location)[1]  
     return   
       <Location LocationID="{ ($i/@LocationID) }"   
                   LotSizeA="{  $i/@LotSize }"  
                   LotSizeB="{  number($i/@LotSize) }"  
                   LotSizeC="{ number($i/@LotSize) + 1 }" >  
  
       </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La función **Number ()** no es necesaria, tal como se muestra en la consulta para el atributo **LotSizeA** . Se trata de una función de XPath 1.0 incluida sobre todo para mantener la compatibilidad con versiones anteriores.  
  
-   El XQuery para **LotSizeB** especifica la función Number y es redundante.  
  
-   La consulta de **montones** ilustra el uso de un valor numérico en una operación aritmética.  
  
 El resultado es el siguiente:  
  
```  
ProductModelID   Result  
----------------------------------------------  
7              <Location LocationID="10"   
                         LotSizeA="100"   
                         LotSizeB="100"   
                         LotSizeC="101" />  
```  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **Number ()** solo acepta nodos. No acepta valores atómicos.  
  
-   Cuando los valores no se pueden devolver como un número, la función **Number ()** devuelve la secuencia vacía en lugar de Nan.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
