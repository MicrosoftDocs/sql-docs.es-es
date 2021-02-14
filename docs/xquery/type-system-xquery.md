---
title: Sistema de tipos (XQuery) | Microsoft Docs
description: Obtenga información sobre el sistema de tipos XQuery que incluye tipos integrados de esquemas XML y tipos definidos en el espacio de nombres XPath-Datatypes.
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- type system [XQuery]
- typed values [XQuery]
- XQuery, sequence
- string values [XQuery]
- XQuery, XPath data type namespace
- xdt prefix [XML in SQL Server]
- XQuery, type system
- built-in XML schema types [SQL Server]
- xs prefix [XML in SQL Server]
ms.assetid: 22d6f861-d058-47ee-b550-cbe9092dcb12
author: rothja
ms.author: jroth
ms.openlocfilehash: a92b26642f5759044233d5d63b5a432ee5fe426c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352844"
---
# <a name="type-system-xquery"></a>Sistema de tipos (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery es un lenguaje con establecimiento inflexible de tipos para tipos de esquema y un lenguaje con establecimiento flexible de tipos para datos sin tipo. Los tipos predefinidos de XQuery son los siguientes:  
  
-   Tipos integrados de esquema XML en el espacio de **http://www.w3.org/2001/XMLSchema** nombres.  
  
-   Tipos definidos en el **http://www.w3.org/2004/07/xpath-datatypes** espacio de nombres.  
  
 En este tema también se trata lo siguiente:  
  
-   El valor con tipo y el valor de cadena de un nodo.  
  
-   La [función de datos &#40;&#41;XQuery ](../xquery/data-accessor-functions-data-xquery.md) y la [función de cadena &#40;&#41;XQuery ](../xquery/data-accessor-functions-string-xquery.md).  
  
-   La equiparación del tipo de secuencia devuelto por una expresión.  
  
## <a name="built-in-types-of-xml-schema"></a>Tipos integrados de esquema XML  
 Los tipos integrados de esquema XML tienen el prefijo de espacio de nombres predefinido xs. Algunos de estos tipos son **xs: Integer** y **xs: String**. Todos estos tipos integrados son compatibles y se pueden utilizar al crear una colección de esquemas XML.  
  
 Al consultar XML con tipo, el tipo estático y dinámico de los nodos se determina a partir de la colección de esquemas XML asociada a la columna o variable que se consulta. Para obtener más información sobre los tipos estáticos y dinámicos, vea [contexto de expresión y evaluación de consultas &#40;XQuery&#41;](../xquery/expression-context-and-query-evaluation-xquery.md). Por ejemplo, la siguiente consulta se especifica en una columna **XML** con tipo ( `Instructions` ). La expresión utiliza `instance of` para comprobar que el valor con tipo del atributo `LotSize` devuelto es de tipo `xs:decimal`.  
  
```  
SELECT Instructions.query('  
   DECLARE namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   data(/AWMI:root[1]/AWMI:Location[@LocationID=10][1]/@LotSize)[1] instance of xs:decimal  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Esta información de tipos la proporciona la colección de esquemas XML asociada a la columna.  
  
## <a name="types-defined-in-xpath-data-types-namespace"></a>Tipos definidos en el espacio de nombres de tipos de datos XPath  
 Los tipos definidos en el **http://www.w3.org/2004/07/xpath-datatypes** espacio de nombres tienen un prefijo predefinido de **XDT**. La siguiente información se aplica a estos tipos:  
  
-   Estos tipos no se pueden utilizar al crear una colección de esquemas XML. Estos tipos se utilizan en el sistema de tipos XQuery y se usan para [XQuery y para tipos estáticos](../xquery/xquery-and-static-typing.md). Puede convertir a los tipos atómicos, por ejemplo, **XDT: untypedAtomic**, en el espacio de nombres **XDT** .  
  
-   Al consultar XML sin tipo, el tipo estático y dinámico de nodos de elemento es **XDT: sin tipo** y el tipo de los valores de atributo es **XDT: untypedAtomic**. El resultado de un método **query ()** genera XML sin tipo. Esto significa que los nodos XML se devuelven como **XDT: untyped** y **XDT: untypedAtomic**, respectivamente.  
  
-   No se admiten los tipos **XDT: dayTimeDuration** y **XDT: yearMonthDuration** .  
  
 En el ejemplo siguiente, la consulta se especifica con una variable XML sin tipo. La expresión, `data(/a[1]`), devuelve una secuencia de un valor atómico. La función `data()` devuelve el valor con tipo del elemento `<a>`. Dado que se consulta XML sin tipo, el tipo de valor devuelto es `xdt:untypedAtomic`. Por tanto, `instance of` devuelve True.  
  
```  
DECLARE @x xml  
SET @x='<a>20</a>'  
SELECT @x.query( 'data(/a[1]) instance of xdt:untypedAtomic' )  
```  
  
 En lugar de recuperar el valor con tipo, la expresión (`/a[1]`) del ejemplo siguiente devuelve una secuencia de un elemento, el elemento `<a>`. La expresión `instance of` realiza la prueba de elementos para comprobar si el valor devuelto por la expresión es un nodo de elemento `xdt:untyped type`.  
  
```  
DECLARE @x xml  
SET @x='<a>20</a>'  
-- Is this an element node whose name is "a" and type is xdt:untyped.  
SELECT @x.query( '/a[1] instance of element(a, xdt:untyped?)')  
-- Is this an element node of type xdt:untyped.  
SELECT @x.query( '/a[1] instance of element(*, xdt:untyped?)')  
-- Is this an element node?  
SELECT @x.query( '/a[1] instance of element()')  
```  
  
> [!NOTE]  
>  Cuando se consulta una instancia XML con tipo y la expresión de la consulta incluye el eje principal, deja de estar disponible la información del tipo estático de los nodos resultantes. Sin embargo, el tipo dinámico sigue asociado a los nodos.  
  
## <a name="typed-value-vs-string-value"></a>Valor con tipo y valor de cadena  
 Cada nodo tiene un valor con tipo y un valor de cadena. Para los datos XML con tipo, la colección de esquemas XML asociada a la columna o variable que se consulta proporciona el tipo del valor con tipo. Para los datos XML sin tipo, el tipo del valor con tipo es **XDT: untypedAtomic**.  
  
 Puede usar la función **Data ()** o **String ()** para recuperar el valor de un nodo:  
  
-   La [función de datos &#40;XQuery&#41;](../xquery/data-accessor-functions-data-xquery.md) devuelve el valor con tipo de un nodo.  
  
-   La [función de cadena &#40;XQuery&#41;](../xquery/data-accessor-functions-string-xquery.md) devuelve el valor de cadena del nodo.  
  
 En la siguiente colección de esquemas XML, `root` se define el elemento <> del tipo entero:  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="integer"/>  
</schema>'  
GO  
```  
  
 En el ejemplo siguiente, la expresión primero recupera el valor con tipo de `/root[1]` y, después, le suma `3`.  
  
```  
DECLARE @x xml(SC)  
SET @x='<root>5</root>'  
SELECT @x.query('data(/root[1]) + 3')  
```  
  
 En el ejemplo siguiente, la expresión genera un error, porque `string(/root[1])` en la expresión devuelve un valor de tipo cadena. A continuación, este valor se pasa a un operador aritmético que solo utiliza como operandos valores de tipo numérico.  
  
```  
-- Fails because the argument is string type (must be numeric primitive type).  
DECLARE @x xml(SC)  
SET @x='<root>5</root>'  
SELECT @x.query('string(/root[1]) + 3')  
```  
  
 En el ejemplo siguiente se calcula el total de los atributos `LaborHours`. La `data()` función recupera los valores con tipo de los `LaborHours` atributos de todos los <`Location`> elementos de un modelo de producto. Según el esquema XML asociado a la `Instruction` columna, `LaborHours` es de tipo **xs: decimal** .  
  
```  
SELECT Instructions.query('   
DECLARE namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
             sum(data(//AWMI:Location/@LaborHours))   
') AS Result   
FROM Production.ProductModel   
WHERE ProductModelID=7  
```  
  
 Esta consulta devuelve 12.75 como resultado.  
  
> [!NOTE]  
>  El uso explícito de la función **Data ()** en este ejemplo solo es para fines ilustrativos. Si no se especifica, **SUM ()** aplica implícitamente la función **Data ()** para extraer los valores con tipo de los nodos.  
  
## <a name="see-also"></a>Consulte también  
 [Plantillas y permisos de SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)   
 [Conceptos básicos de XQuery](../xquery/xquery-basics.md)  
  
  
