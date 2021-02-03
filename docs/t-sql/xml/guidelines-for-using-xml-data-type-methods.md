---
description: Directrices para utilizar los métodos del tipo de datos xml
title: Directrices para utilizar los métodos del tipo de datos xml
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- xml data type [SQL Server], methods
- methods [XML in SQL Server]
ms.assetid: 1a483aa1-42de-4c88-a4b8-c518def3d496
author: MightyPen
ms.author: genemi
ms.openlocfilehash: adf76774e56841978d64661704f91cd6fd04d3fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193134"
---
# <a name="guidelines-for-using-xml-data-type-methods"></a>Directrices para utilizar los métodos del tipo de datos xml

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se describen instrucciones para usar los métodos de tipo de datos **xml**.

## <a name="the-print-statement"></a>La instrucción PRINT

Los métodos del tipo de datos **xml** no se pueden usar en la instrucción PRINT, como se muestra en el ejemplo siguiente. Los métodos del tipo de datos **xml** se tratan como subconsultas y éstas no están permitidas en la instrucción PRINT. Como resultado, el ejemplo siguiente devuelve un error:

```sql
DECLARE @x XML
SET @x = '<root>Hello</root>'
PRINT @x.value('/root[1]', 'varchar(20)') -- will not work because this is treated as a subquery (select top 1 col from table)
```

Una solución es asignar primero el resultado del método **value()** a una variable de tipo **xml** y, después, especificar la variable en la consulta.

```sql
DECLARE @x XML
DECLARE @c VARCHAR(max)
SET @x = '<root>Hello</root>'
SET @c = @x.value('/root[1]', 'VARCHAR(11)')
PRINT @c
```

## <a name="the-group-by-clause"></a>La cláusula GROUP BY

Los métodos del tipo de datos **xml** se tratan internamente como subconsultas. Como GROUP BY requiere un valor escalar y no permite agregados ni subconsultas, no se pueden especificar los métodos del tipo de datos **xml** en la cláusula GROUP BY. Una solución es llamar a una función definida por el usuario que utilice métodos XML en su interior.

## <a name="reporting-errors"></a>La notificación de errores

Al informar de errores, los métodos del tipo de datos **xml** generan un único error con el formato siguiente:

```
Msg errorNumber, Level levelNumber, State stateNumber:
XQuery [database.table.method]: description_of_error
```

Por ejemplo:

```
Msg 2396, Level 16, State 1:
XQuery [xmldb_test.xmlcol.query()]: Attribute may not appear outside of an element
```

## <a name="singleton-checks"></a>Comprobaciones de singleton

Los pasos de ubicación, los parámetros de funciones y los operadores que requieren singleton devolverán un error si el compilador no puede determinar si se garantiza un singleton en tiempo de ejecución. Este problema es frecuente con datos sin tipo. Por ejemplo, la búsqueda de un atributo requiere un elemento primario singleton. Es suficiente con un ordinal que seleccione un solo nodo primario. Es posible que la evaluación de una combinación **node()** -**value()** para extraer valores de atributos no requiera la especificación del ordinal. Esto se muestra en el ejemplo siguiente.

### <a name="example-known-singleton"></a>Ejemplo: singleton conocido

En este ejemplo, el método **nodes()** genera una fila distinta por cada elemento `<book>`. El método **value()** que se evalúa en un nodo `<book>` extrae el valor de `@genre` y, siendo un atributo, es un singleton.

```sql
SELECT nref.value('@genre', 'VARCHAR(max)') LastName
FROM T CROSS APPLY xCol.nodes('//book') AS R(nref)
```

El esquema XML se utiliza para comprobar el tipo del XML con tipo. Si se especifica un nodo como singleton en el esquema XML, el compilador usa esa información y no se produce ningún error. En caso contrario, se necesita un ordinal que seleccione un solo nodo. En particular, el uso de ejes descendant-or-self (//), como en `/book//title`, pierde inferencia de cardinalidad de singleton para el elemento `<title>`, incluso si el esquema XML especifica que sea así. Por tanto, se debe volver a escribir como `(/book//title)[1]`.

Es importante ser consciente de la diferencia entre `//first-name[1]` y `(//first-name)[1]` en la comprobación de tipos. La primera expresión devuelve una secuencia de nodos `<first-name>` en la que cada nodo es el nodo `<first-name>` que está más a la izquierda entre los de su mismo nivel. La última expresión devuelve el primer nodo singleton `<first-name>` por orden de los documentos en la instancia XML.

### <a name="example-using-value"></a>Ejemplo: uso de value()

La siguiente consulta en una columna XML sin tipo da como resultado un error de compilación estático. Esto se debe a que **value()** espera un nodo singleton como primer argumento y el compilador no puede determinar si solo va a aparecer un nodo `<last-name>` en tiempo de ejecución:

```sql
SELECT xCol.value('//author/last-name', 'NVARCHAR(50)') LastName
FROM T
```

A continuación, se ofrece una solución que debe contemplar:

```sql
SELECT xCol.value('//author/last-name[1]', 'NVARCHAR(50)') LastName
FROM T
```

No obstante, esta solución no resuelve el error, ya que pueden aparecer varios nodos `<author>` en cada instancia XML. Resulta útil volver a escribir lo siguiente:

```sql
SELECT xCol.value('(//author/last-name/text())[1]', 'NVARCHAR(50)') LastName
FROM T
```

Esta consulta devuelve el valor del primer elemento `<last-name>` de cada instancia XML.

## <a name="see-also"></a>Consulte también

- [Métodos del tipo de datos xml](../../t-sql/xml/xml-data-type-methods.md)
