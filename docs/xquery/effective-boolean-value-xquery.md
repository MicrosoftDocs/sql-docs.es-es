---
title: Valor booleano efectivo (XQuery) | Microsoft Docs
description: Obtenga información sobre los valores booleanos efectivos en XQuery.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- effective Boolean value [XQuery]
- Boolean values
- XQuery, effective Boolean values
- EBV
ms.assetid: 506682b1-b6c9-45e2-aa54-7abd5844c3f1
author: rothja
ms.author: jroth
ms.openlocfilehash: c9e390546feb848f965df2bfd60a2cc68c8fb3ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341557"
---
# <a name="effective-boolean-value-xquery"></a>Valor booleano efectivo (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Éstos son los valores booleanos efectivos:  
  
-   False si el operando es una secuencia vacía o un valor booleano false.  
  
-   De lo contrario, el valor es true.  
  
 El valor booleano efectivo puede calcularse para expresiones que devuelven un único valor booleano, una secuencia de nodo o una secuencia vacía. Tenga en cuenta que el valor booleano se calcula de forma implícita, cuando se procesan los siguientes tipos de expresiones:  
  
-   Expresiones lógicas  
  
-   La [función not](../xquery/functions-on-boolean-values-not-function.md)  
  
-   La cláusula WHERE de una expresión FLWOR  
  
-   [Expresiones condicionales](../xquery/conditional-expressions-xquery.md)  
  
-   [Expresiones cuantificadas](../xquery/quantified-expressions-xquery.md)  
  
 A continuación se muestra un ejemplo de un valor booleano efectivo. Cuando se procesa la expresión **If** , se determina el valor booleano efectivo de la condición. Como `/a[1]` devuelve una secuencia vacía, el valor booleano efectivo es false. El resultado se devuelve como XML con un nodo de texto (false).  
  
```  
value is false  
DECLARE @x XML  
SET @x = '<b/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 En el siguiente ejemplo, el valor booleano efectivo es true, porque la expresión devuelve una secuencia no vacía.  
  
```  
DECLARE @x XML  
SET @x = '<a/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 Al consultar las variables o columnas **XML** con tipo, puede tener nodos de tipo booleano. Los **datos ()** en este caso devuelven un valor booleano. Si la expresión de consulta devuelve un valor booleano true, el valor booleano efectivo será true, tal y como se muestra en el siguiente ejemplo. En el ejemplo también se ilustra lo siguiente:  
  
-   Se crea una colección de esquemas XML. El elemento \<b> de la colección es de tipo booleano.  
  
-   Se crea y se consulta una variable **XML** con tipo.  
  
-   La expresión `data(/b[1])` devuelve un valor booleano true. Por tanto, el valor booleano efectivo en este caso es true.  
  
-   La expresión `data(/b[2])` devuelve un valor booleano false. Por tanto, el valor booleano efectivo en este caso es false.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="s" type="string"/>  
      <element name="b" type="boolean"/>  
</schema>'  
go  
DECLARE @x XML(SC)  
SET @x = '<b>true</b><b>false</b>'  
SELECT @x.query('if (data(/b[1])) then "true" else "false"')  
SELECT @x.query('if (data(/b[2])) then "true" else "false"')  
go  
```  
  
## <a name="see-also"></a>Consulte también  
 [Conceptos básicos de XQuery](../xquery/xquery-basics.md)   
 [Instrucción e iteración de FLWOR &#40;XQuery&#41;](../xquery/flwor-statement-and-iteration-xquery.md)  
  
  
