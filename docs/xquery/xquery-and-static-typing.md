---
title: XQuery y tipos estáticos | Microsoft Docs
description: Obtenga información sobre la inferencia de tipos estáticos y la comprobación de tipos estáticos en XQuery.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, static typing
- static typing
- checking static types
- inference [XQuery]
ms.assetid: d599c791-200d-46f8-b758-97e761a1a5c0
author: rothja
ms.author: jroth
ms.openlocfilehash: 6dcaa7bbbd5674c6db3b6d8ec5185cc31e890707
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352312"
---
# <a name="xquery-and-static-typing"></a>XQuery y el establecimiento de tipos estáticos
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  XQuery en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es un lenguaje con establecimiento de tipos en modo estático. Es decir, provoca errores de tipo durante la compilación de consultas cuando una expresión devuelve un valor con un tipo o una cardinalidad no aceptado por una función o un operador determinados. Además, la comprobación de tipos estáticos también puede detectar si hay un error de asignación de tipo en una expresión de ruta de acceso de un documento XML con tipo. El compilador XQuery aplica en primer lugar la fase de normalización que agrega las operaciones implícitas, como la atomización y, a continuación, realiza la inferencia de tipos estáticos y la comprobación de tipos estáticos.  
  
## <a name="static-type-inference"></a>Inferencia de tipos estáticos  
 La inferencia de tipos estáticos determina el tipo de devolución de una expresión. Para determinarlo, obtiene los tipos estáticos de los parámetros de entrada y la semántica estática de la operación y deduce el tipo estático del resultado. Por ejemplo, el tipo estático de la expresión 1 + 2,3 se determina del modo siguiente:  
  
-   El tipo estático de 1 es **xs: Integer** y el tipo estático de 2,3 es **xs: decimal**. En función de la semántica dinámica, la semántica estática de la **+** operación convierte el entero en un decimal y, a continuación, devuelve un decimal. El tipo estático deducido sería **xs: decimal**.  
  
 En el caso de las instancias XML sin tipo, existen tipos especiales que indican que no se ha asignado un tipo a los datos. Esta información se utiliza durante la comprobación de tipos estáticos y para realizar determinadas conversiones implícitas.  
  
 En el caso de los datos con tipo, se deduce el tipo de entrada a partir de la colección de esquemas XML que restringe la instancia de tipo de datos XML. Por ejemplo, si el esquema solo permite elementos de tipo **xs: Integer**, los resultados de una expresión de ruta de acceso que usa ese elemento serán cero o más elementos de tipo **xs: Integer**. Esto se expresa actualmente mediante una expresión como, por ejemplo, `element(age,xs:integer)*` donde el asterisco ( \* ) indica la cardinalidad del tipo resultante. En este ejemplo, la expresión puede dar como resultado cero o más elementos de nombre "Age" y tipo **xs: Integer**. Otras cardinalidades son exactamente una y se expresan con el nombre de tipo solo, cero o uno y se expresan mediante un signo de interrogación (**?**), y 1 o más, y se expresan mediante el signo más ( **+** ).  
  
 En ocasiones, la inferencia de tipos estáticos puede deducir que una expresión siempre devolverá la secuencia vacía. Por ejemplo, si una expresión de ruta de acceso en un tipo de datos XML con tipo busca un \<name> elemento dentro de un \<customer> elemento (/Customer/Name), pero el esquema no permite un \<name> dentro de \<customer> , la inferencia de tipos estáticos infiere que el resultado estará vacío. Se usará para detectar consultas incorrectas y se informará como un error estático, a menos que la expresión sea () o **Data (())**.  
  
 Las reglas de inferencia detalladas se proporcionan en la semántica formal de la especificación XQuery. Microsoft la ha modificado ligeramente para trabajar con instancias de tipos de datos XML con tipo. El cambio más importante del estándar es que el nodo de documento implícito conoce el tipo de la instancia de tipo de datos XML. Como resultado, el tipo de una expresión de ruta de acceso con la forma /age se asignará de forma precisa basándose en esa información.  
  
 Mediante el uso de [SQL Server Profiler plantillas y permisos](../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md), puede ver los tipos estáticos devueltos como parte de las compilaciones de consultas. Si desea verlos, su seguimiento debería incluir el evento XQuery Static Type en la categoría de eventos TSQL.  
  
## <a name="static-type-checking"></a>Comprobación de tipos estáticos  
 La comprobación de tipos estáticos garantiza que la ejecución en tiempo de ejecución solo recibirá valores de tipo apropiado para la operación. Puesto que no es necesario comprobar los tipos en tiempo de ejecución, se pueden detectar posibles errores en una fase temprana de la compilación. Esto ayuda a mejorar el rendimiento. No obstante, el establecimiento de tipos estáticos requiere que el creador de la consulta sea más cuidadoso en la formulación.  
  
 A continuación se indican los tipos apropiados que se pueden utilizar:  
  
-   Tipos permitidos explícitamente por una función u operación.  
  
-   Un subtipo de un tipo permitido explícitamente.  
  
 Los subtipos se definen en función de las reglas de subtipo para utilizar la derivación mediante restricción o ampliación del esquema XML. Por ejemplo, un tipo S es un subtipo del tipo T si todos los valores del tipo S también son instancias del tipo T.  
  
 Además, todos los valores enteros también son valores decimales, en función de la jerarquía de tipos de esquema XML. No obstante, no todos los valores decimales son enteros. Por tanto, un entero es un subtipo de un decimal, pero no viceversa. Por ejemplo, la **+** operación solo permite valores de ciertos tipos, como los tipos numéricos **xs: Integer**, **xs: decimal**, **xs: Float** y **xs: Double**. Si se pasan valores de otros tipos, como **xs: String**, la operación genera un error de tipo. Esto se denomina establecimiento estricto de tipos. Los valores de otros tipos, como el tipo atómico utilizado para indicar XML sin tipo, se pueden convertir implícitamente en valores de un tipo aceptado por la operación. Esto se denomina establecimiento flexible de tipos.  
  
 Si es necesaria tras una conversión implícita, la comprobación de tipos estáticos garantiza que solo se pasarán a una operación los valores de los tipos permitidos con la cardinalidad correcta. En el caso de "String" + 1, reconoce que el tipo estático de "String" es **xs: String**. Dado que este no es un tipo permitido para la **+** operación, se produce un error de tipo.  
  
 Si se agrega el resultado de una expresión arbitraria E1 a una expresión arbitraria E2 (E1 + E2), la inferencia de tipos estáticos determinará en primer lugar los tipos estáticos de E1 y E2 y, a continuación, comprobará sus tipos estáticos con los tipos permitidos para la operación. Por ejemplo, si el tipo estático de E1 puede ser **xs: String** o **xs: Integer**, la comprobación de tipos estáticos genera un error de tipo, aunque algunos valores en tiempo de ejecución pueden ser enteros. Lo mismo sería el caso si el tipo estático de E1 fuera **xs: Integer&#42;**. Dado que la **+** operación solo acepta exactamente un valor entero y E1 podría devolver cero o más de 1, la comprobación de tipos estáticos produce un error.  
  
 Como se ha mencionado anteriormente, la inferencia de tipos suele inferir un tipo más general que el tipo que el usuario conoce de los datos que se pasan. En estos casos, el usuario debe volver a escribir la consulta. A continuación se exponen algunos casos habituales:  
  
-   El tipo deduce un tipo más general, como un supertipo o una unión de tipos. Si el tipo es atómico, deberá utilizar la expresión de conversión o la función constructora para indicar el tipo estático real. Por ejemplo, si el tipo deducido de la expresión E1 es una opción entre **xs: String** o **xs: Integer** y la suma requiere **xs: Integer**, debe escribir `xs:integer(E1) + E2` en lugar de `E1+E2` . Esta expresión puede producir un error en tiempo de ejecución si se encuentra un valor de cadena que no se puede convertir a **xs: Integer**. Sin embargo, ahora la expresión pasará la comprobación de tipos estáticos. Esta expresión se asigna a la secuencia vacía.  
  
-   El tipo deduce una cardinalidad superior a la que contienen los datos realmente. Esto ocurre con frecuencia, porque el tipo de datos **XML** puede contener más de un elemento de nivel superior y una colección de esquemas XML no puede restringir esto. Para reducir el tipo estático y garantizar que efectivamente se pasa como máximo un valor, se debe utilizar el predicado de posición `[1]`. Por ejemplo, para sumar 1 al valor del atributo `c` del elemento `b` bajo el elemento a de nivel superior, debe usar `write (/a/b/@c)[1]+1`. Por otro lado, la palabra clave DOCUMENT se puede utilizar junto con una colección de esquemas XML.  
  
-   Algunas operaciones pierden información de tipo durante la inferencia. Por ejemplo, si no se puede determinar el tipo de un nodo, se convierte en **anyType**. Este tipo no se convierte implícitamente en ningún otro tipo. Estas conversiones se producen sobre todo durante la navegación, utilizando el eje primario. Si la expresión provoca un error de tipo estático, debe evitar utilizar dichas operaciones y volver a escribir la consulta.  
  
## <a name="type-checking-of-union-types"></a>Comprobación de tipos para tipos de unión  
 Los tipos de unión requieren un tratamiento especial debido a la comprobación de tipos. Dos de los problemas se muestran en los ejemplos siguientes.  
  
### <a name="example-function-over-union-type"></a>Ejemplo: función sobre tipo de unión  
 Considere una definición de elemento para <`r`> de un tipo de Unión:  
  
```  
<xs:element name="r">  
<xs:simpleType>  
   <xs:union memberTypes="xs:int xs:float xs:double"/>  
</xs:simpleType>  
</xs:element>  
```  
  
 En el contexto de XQuery, la función "Average" `fn:avg (//r)` devuelve un error estático, ya que el compilador de XQuery no puede agregar valores de tipos diferentes (**xs: int**, **xs: Float** o **xs: Double**) para el <`r`> elementos en el argumento de **FN: AVG ()**. Para resolver esto, rescriba la invocación de función como `fn:avg(for $r in //r return $r cast as xs:double ?)`.  
  
### <a name="example-operator-over-union-type"></a>Ejemplo: operador sobre tipo de unión  
 La operación de suma ("+") requiere tipos precisos para los operandos. Como resultado, la expresión `(//r)[1] + 1` devuelve un error estático que tiene la definición de tipo descrita previamente para el elemento <`r`>. Una posible solución es rescribirlo como `(//r)[1] cast as xs:int? +1`, donde "?" indica 0 o 1 repeticiones. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], requiere "cast as" con "?", porque cualquier conversión puede resultar en una secuencia vacía como consecuencia de los errores en tiempo de ejecución.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del lenguaje XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
