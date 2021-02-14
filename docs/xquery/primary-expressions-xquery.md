---
title: Expresiones principales (XQuery) | Microsoft Docs
description: Obtenga información sobre las expresiones primarias de XQuery que incluyen literales, referencias de variables, expresiones de elementos de contexto, constructores y llamadas de función.
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
- variable references [XQuery]
- primary expressions [XQuery]
- function calls [XQuery]
- expressions [XQuery], primary
- literals [XQuery]
- context item expressions [XQuery]
ms.assetid: d4183c3e-12b5-4ca0-8413-edb0230cb159
author: rothja
ms.author: jroth
ms.openlocfilehash: 2f474aae3e764224bec01830b42766216a27bc9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339953"
---
# <a name="primary-expressions-xquery"></a>Expresiones principales (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Las expresiones principales de XQuery incluyen literales, referencias de variables, expresiones de elementos de contexto, constructores y llamadas a funciones.  
  
## <a name="literals"></a>Literales  
 Los literales de XQuery pueden ser numéricos o de cadena. Un literal de cadena puede incluir referencias de entidades predefinidas. Una referencia de entidad es una secuencia de caracteres. La secuencia empieza con un "y" comercial (&amp;) que representa un solo carácter que, en otra situación, tendría un significado sintáctico. A continuación se exponen las referencias de entidades predefinidas de XQuery.  
  
|Referencia de entidad|Representa|  
|----------------------|----------------|  
|`&lt;`|\<|  
|`&gt;`|>|  
|`&amp;`|&|  
|`&quot;`|"|  
|`&apos;`|'|  
  
 Un literal de cadena también puede contener una referencia de carácter, una referencia de estilo XML a un carácter Unicode, que se identifica mediante su punto de código decimal o hexadecimal. Por ejemplo, el símbolo del euro se puede representar mediante la referencia de carácter "&\# 8364;".  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utiliza la versión 1.0 de XML como base para el análisis.  
  
### <a name="examples"></a>Ejemplos  
 Los ejemplos siguientes ilustran la utilización de literales y las referencias de entidad y carácter.  
  
 Este código devuelve un error porque los caracteres `<'` y `'>` tienen un significado especial.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <SalaryRange>Salary > 50000 and < 100000</SalaryRange>')  
GO  
```  
  
 Si utiliza una referencia de entidad en su lugar, la consulta funcionará:  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <SalaryRange>Salary &gt; 50000 and &lt; 100000</SalaryRange>')  
GO  
```  
  
 En el ejemplo siguiente se ilustra el uso de una referencia de carácter para representar el símbolo del euro.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <a>€12.50</a>')  
```  
  
 Éste es el resultado.  
  
 `<a>€12.50</a>`  
  
 En el ejemplo siguiente, la consulta está delimitada por apóstrofos. Por tanto, el apóstrofo del valor de cadena se representa mediante dos apóstrofos adyacentes.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query('<a>I don''t know</a>')  
Go  
```  
  
 Éste es el resultado.  
  
 `<a>I don't know</a>`  
  
 Las funciones booleanas integradas, **true ()** y **false ()**, se pueden usar para representar valores booleanos, tal y como se muestra en el ejemplo siguiente.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query('<a>{true()}</a>')  
GO  
```  
  
 El constructor de elemento directo especifica una expresión entre llaves. Ésta se reemplazará por su valor en el XML resultante.  
  
 Éste es el resultado.  
  
 `<a>true</a>`  
  
## <a name="variable-references"></a>Referencias de variable  
 Una referencia de variable en XQuery es un QName precedido de un signo $. Esta implementación solo admite las referencias de variable sin prefijo. Por ejemplo, en la consulta siguiente se define la variable `$i` en la expresión FLWOR:  
  
```  
DECLARE @var XML  
SET @var = '<root>1</root>'  
SELECT @var.query('  
 for $i in /root return data($i)')  
GO  
```  
  
 La consulta siguiente no funcionará porque se ha agregado un prefijo de espacio de nombres al nombre de la variable.  
  
```  
DECLARE @var XML  
SET @var = '<root>1</root>'  
SELECT @var.query('  
DECLARE namespace x="https://X";  
for $x:i in /root return data($x:i)')  
GO  
```  
  
 Puede utilizar la función de extensión SQL: variable () para hacer referencia a variables SQL, como se muestra en la consulta siguiente.  
  
```  
DECLARE @price money  
SET @price=2500  
DECLARE @x xml  
SET @x = ''  
SELECT @x.query('<value>{sql:variable("@price") }</value>')  
```  
  
 Éste es el resultado.  
  
 `<value>2500</value>`  
  
#### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones de la implementación:  
  
-   No se admiten las variables con prefijos de espacio de nombres.  
  
-   No se admite la importación de módulos.  
  
-   No se admiten las declaraciones de variables externas. Una solución a esto es usar la [función SQL: variable ()](../xquery/xquery-extension-functions-sql-variable.md).  
  
## <a name="context-item-expressions"></a>Expresiones de elementos de contexto  
 El elemento de contexto es el elemento que se está procesando en el contexto de una expresión de ruta de acceso. Se inicializa en una instancia de tipo de datos XML distinta de NULL con el nodo de documento. También puede cambiarse mediante el método Nodes (), en el contexto de las expresiones XPath o de los predicados [].  
  
 Una expresión que contiene un punto (.) devuelve el elemento de contexto. Por ejemplo, la consulta siguiente evalúa cada elemento <`a`> para la presencia del atributo `attr` . Si el atributo está presente, se devuelve el elemento. Tenga en cuenta que la condición del predicado especifica que el nodo de contexto se especifica mediante un solo punto.  
  
```  
DECLARE @var XML  
SET @var = '<ROOT>  
<a>1</a>  
<a attr="1">2</a>  
</ROOT>'  
SELECT @var.query('/ROOT[1]/a[./@attr]')  
```  
  
 Éste es el resultado.  
  
 `<a attr="1">2</a>`  
  
## <a name="function-calls"></a>Llamadas de función  
 Puede llamar a funciones de XQuery integradas y a las [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] funciones SQL: variable () y SQL: column (). Para obtener una lista de las funciones implementadas, vea [funciones de XQuery con el tipo de datos XML](../xquery/xquery-functions-against-the-xml-data-type.md).  
  
#### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones de la implementación:  
  
-   No se admite la declaración de funciones en el prólogo de XQuery.  
  
-   No se admite la importación de funciones.  
  
## <a name="see-also"></a>Consulte también  
 [Construcción XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)
 
