---
title: Control de errores (XQuery) | Microsoft Docs
description: Obtenga información sobre el control de errores en XQuery y vea ejemplos de control de errores dinámicos.
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
- static errors
- errors [XQuery]
- XQuery, error handling
- dynamic errors [XQuery]
ms.assetid: 7dee3c11-aea0-4d10-9126-d54db19448f2
author: rothja
ms.author: jroth
ms.openlocfilehash: 61e6b8b53e4519eb88f94f9521434b2e48aac40d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335874"
---
# <a name="error-handling-xquery"></a>Control de errores (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  La especificación W3C permite emitir errores de tipo de forma estática o dinámica y define errores estáticos, dinámicos y de tipo.  
  
## <a name="compilation-and-error-handling"></a>Compilación y control de errores  
 Las expresiones XQuery e instrucciones XML DML sintácticamente incorrectas generan errores de compilación. En la fase de compilación, se comprueba la corrección del tipo estático de las expresiones XQuery y las instrucciones DML, y se utilizan esquemas XML para detectar inferencias de tipos para XML con tipo. Si una expresión puede generar un error en tiempo de ejecución debido a una infracción de seguridad de tipos, se producen errores de tipo estático. Ejemplos de errores estáticos son la suma de una cadena a un entero y la consulta de datos con tipo en un nodo no existente  
  
 Como desviación del estándar W3C, los errores en tiempo de ejecución de XQuery se convierten en secuencias vacías. Estas secuencias se pueden propagar al resultado de la consulta como XML vacío o NULL, dependiendo del contexto de invocación.  
  
 Una conversión explícita al tipo correcto permite a los usuarios resolver errores estáticos, aunque los errores de conversión en tiempo de ejecución se transformarán en secuencias vacías.  
  
## <a name="static-errors"></a>Errores estáticos  
 Los errores estáticos se devuelven utilizando el mecanismo de error de [!INCLUDE[tsql](../includes/tsql-md.md)]. En [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], los errores de tipo de XQuery se devuelven de forma estática. Para obtener más información, vea [XQuery y tipos estáticos](../xquery/xquery-and-static-typing.md).  
  
## <a name="dynamic-errors"></a>Errores dinámicos  
 En XQuery, la mayoría de los errores dinámicos se asignan a una secuencia vacía ("()"). No obstante, existen dos excepciones: las condiciones de sobrecarga en funciones de agregado de XQuery y los errores de validación de XML-DML. Tenga en cuenta que la mayoría de los errores dinámicos se asignan a una secuencia vacía. De lo contrario, la ejecución de consultas que se aprovecha de los índices XML puede producir errores inesperados. Por tanto, para proporcionar una ejecución eficaz sin generar errores inesperados, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] asigna los errores dinámicos a ().  
  
 A menudo, en una situación en la que el error dinámico se produciría dentro de un predicado, no emitir el error es no cambiar la semántica, porque () se asigna a False. Sin embargo, en algunos casos, devolver () en lugar de un error dinámico puede dar lugar a resultados inesperados. Los ejemplos siguientes lo muestran.  
  
### <a name="example-using-the-avg-function-with-a-string"></a>Ejemplo: utilizar la función avg() con una cadena  
 En el ejemplo siguiente, se llama a la [función AVG](../xquery/aggregate-functions-avg.md) para calcular el promedio de los tres valores. Uno de estos valores es una cadena. Puesto que la instancia XML en este caso no tiene tipo, todos los datos que contiene son de tipo atómico sin tipo. La función **AVG ()** convierte primero estos valores a **xs: Double** antes de calcular el promedio. Sin embargo, el valor, `"Hello"` , no se puede convertir a **xs: Double** y crea un error dinámico. En este caso, en lugar de devolver un error dinámico, la conversión de `"Hello"` a **xs: Double** produce una secuencia vacía. La función **AVG ()** omite este valor, calcula la media de los otros dos valores y devuelve 150.  
  
```  
DECLARE @x xml  
SET @x=N'<root xmlns:myNS="test">  
 <a>100</a>  
 <b>200</b>  
 <c>Hello</c>  
</root>'  
SELECT @x.query('avg(//*)')  
```  
  
### <a name="example-using-the-not-function"></a>Ejemplo: utilizar la función not  
 Cuando se usa la [función not](../xquery/functions-on-boolean-values-not-function.md) en un predicado, por ejemplo, `/SomeNode[not(Expression)]` y la expresión produce un error dinámico, se devolverá una secuencia vacía en lugar de un error. Al aplicar **Not ()** a la secuencia vacía, se devuelve true, en lugar de un error.  
  
### <a name="example-casting-a-string"></a>Ejemplo: convertir una cadena  
 En el ejemplo siguiente, la cadena literal "NaN" se convierte a xs:string, y después a xs:double. El resultado es un conjunto de filas vacío. Aunque la cadena "NaN" no se puede convertir correctamente a xs:double, esto no se puede determinar hasta el tiempo de ejecución, porque la cadena se convierte primero a xs:string.  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double(xs:string("NaN")) ')  
GO  
```  
  
 En este ejemplo, sin embargo, se produce un error de tipo estático.  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double("NaN") ')  
GO  
```  
  
#### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 No se admite la función **FN: error ()** .  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del lenguaje XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Conceptos básicos de XQuery](../xquery/xquery-basics.md)  
  
  
