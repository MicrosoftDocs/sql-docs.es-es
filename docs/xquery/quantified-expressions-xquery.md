---
title: Expresiones cuantificadas (XQuery) | Microsoft Docs
description: Aprenda a usar expresiones cuantificadas en XQuery para aplicar existencial o la cuantificación universal a una expresión sobre una o varias secuencias.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- universal quantifiers
- effective Boolean value [XQuery]
- quantified expressions [XQuery]
- XQuery, quantified expressions
- every expression [XQuery]
- existential quantifiers [XQuery]
- some expression [XQuery]
- EBV
- expressions [XQuery], quantifiers
ms.assetid: a3a75a6c-8f67-4923-8406-1ada546c817f
author: rothja
ms.author: jroth
ms.openlocfilehash: e673895553b6a3207d0ef86337c47e2f6def2bfe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339914"
---
# <a name="quantified-expressions-xquery"></a>Expresiones cuantificadas (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Los cuantificadores existenciales y universales especifican semánticas distintas para los operadores booleanos que se aplican a dos secuencias. Esto se muestra en la tabla siguiente.  
  
 *Cuantificador existencial*  
 Dadas dos secuencias, si algún elemento de la primera tiene una coincidencia en la segunda, según el operador de comparación utilizado, el valor devuelto será True.  
  
 *Cuantificador universal*  
 Dadas dos secuencias, si cada elemento de la primera tiene una coincidencia en la segunda, el valor devuelto será True.  
  
 XQuery admite las expresiones cuantificadas de la forma siguiente:  
  
```  
( some | every ) <variable> in <Expression> (,...) satisfies <Expression>  
```  
  
 Puede utilizar estas expresiones en una consulta para aplicar explícitamente una cuantificación existencial o universal a una expresión en una o varias secuencias. En [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], la expresión de la cláusula `satisfies` debe dar uno de los resultados siguientes: una secuencia de nodos, una secuencia vacía o un valor booleano. El valor booleano efectivo del resultado de la expresión se utilizará en la cuantificación. La cuantificación existencial que usa **algunos** devolverá True si al menos uno de los valores enlazados por el cuantificador tiene un resultado TRUE en la expresión de satisfacción. La cuantificación universal que usa **cada** debe tener true para todos los valores enlazados por el cuantificador.  
  
 Por ejemplo, la consulta siguiente comprueba cada \<Location> elemento para ver si tiene un atributo LocationID.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        if (every $WC in //AWMI:root/AWMI:Location   
            satisfies $WC/@LocationID)  
        then  
             <Result>All work centers have workcenterLocation ID</Result>  
         else  
             <Result>Not all work centers have workcenterLocation ID</Result>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Dado que LocationID es un atributo necesario del \<Location> elemento, recibirá el resultado esperado:  
  
```  
<Result>All work centers have Location ID</Result>   
```  
  
 En lugar de utilizar el [método Query ()](../t-sql/xml/query-method-xml-data-type.md), puede utilizar el [método Value ()](../t-sql/xml/value-method-xml-data-type.md) para devolver el resultado al mundo relacional, tal como se muestra en la consulta siguiente. La consulta devolverá True si todas las ubicaciones de centro de trabajo tienen atributos LocationID. De lo contrario, la consulta devolverá False.  
  
```  
SELECT Instructions.value('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        every $WC in  //AWMI:root/AWMI:Location   
            satisfies $WC/@LocationID',   
  'nvarchar(10)') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 La consulta siguiente comprueba si una de las imágenes de producto es pequeña. En el XML de catálogo de productos, se almacenan varios ángulos para cada imagen de producto de distinto tamaño. Asegúrese también de que cada XML de catálogo de productos incluye al menos una imagen pequeña. La consulta siguiente permite comprobarlo:  
  
```  
SELECT ProductModelID, CatalogDescription.value('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     some $F in /PD:ProductDescription/PD:Picture  
        satisfies $F/PD:Size="small"', 'nvarchar(20)') as SmallPicturesStored  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Éste es un resultado parcial:  
  
```  
ProductModelID SmallPicturesStored   
-------------- --------------------  
19             true        
```  
  
## <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La aserción de tipos no se admite como parte del enlazamiento de la variable en las expresiones cuantificadas.  
  
## <a name="see-also"></a>Consulte también  
 [Expresiones XQuery](../xquery/xquery-expressions.md)  
  
  
