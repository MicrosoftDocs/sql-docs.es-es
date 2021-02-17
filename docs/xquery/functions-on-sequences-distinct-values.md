---
title: Función DISTINCT-Values (XQuery) | Microsoft Docs
description: Aprenda a usar la función DISTINCT-Values en una expresión XQuery para quitar valores duplicados de una secuencia.
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
- distinct-values function
- fn:distinct-values function
ms.assetid: f4c2bb53-2bec-4f1a-9c00-cf997fb7ae5b
author: rothja
ms.author: jroth
ms.openlocfilehash: b9a3561c25ca0c64e01c2603b64656320d499518
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341902"
---
# <a name="functions-on-sequences---distinct-values"></a>Funciones usadas en secuencias: distinct-values
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Quita los valores duplicados de la secuencia especificada por *$arg*. Si *$arg* es una secuencia vacía, la función devuelve la secuencia vacía.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:distinct-values($arg as xdt:anyAtomicType*) as xdt:anyAtomicType*  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Secuencia de valores atómicos.  
  
## <a name="remarks"></a>Observaciones  
 Todos los tipos de valores atomizados que se pasan a **DISTINCT-Values ()** deben ser subtipos del mismo tipo base. Los tipos base aceptados son los tipos que admiten la operación **EQ** . Entre estos tipos se incluyen los tres tipos base numéricos integrados, los tipos base de fecha y hora, xs:string, xs:boolean y xdt:untypedAtomic. Los valores de tipo xdt:untypedAtomic se convierten en xs:string. Si hay un mezcla de estos tipos, o si se pasan otros valores de otros tipos, se produce un error estático.  
  
 El resultado de **DISTINCT-Values ()** recibe el tipo base de los tipos pasados, como XS: String en el caso de XDT: untypedAtomic, con la cardinalidad original. Si la entrada está vacía estáticamente, el vacío es implícito y se genera un error estático.  
  
 Los valores de tipo xs:string se comparan con la intercalación de punto de código Unicode predeterminada de XQuery.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-distinct-values-function-to-remove-duplicate-values-from-the-sequence"></a>A. Usar la función distinct-values() para quitar valores duplicados de la secuencia  
 En este ejemplo, se asigna una instancia XML que contiene números de teléfono a una variable de tipo **XML** . La expresión XQuery especificada en esta variable utiliza la función **DISTINCT-Values ()** para compilar una lista de números de teléfono que no contienen duplicados.  
  
```  
declare @x xml  
set @x = '<PhoneNumbers>  
 <Number>111-111-1111</Number>  
 <Number>111-111-1111</Number>  
 <Number>222-222-2222</Number>  
</PhoneNumbers>'  
-- 1st select  
select @x.query('  
  distinct-values( data(/PhoneNumbers/Number) )  
') as result  
```  
  
 El resultado es el siguiente:  
  
```  
111-111-1111 222-222-2222    
```  
  
 En la consulta siguiente, se pasa una secuencia de números (1, 1, 2) a la función **DISTINCT-Values ()** . A continuación, la función quita el duplicado de la secuencia y devuelve los otros dos valores.  
  
```  
declare @x xml  
set @x = ''  
select @x.query('  
  distinct-values((1, 1, 2))  
') as result  
```  
  
 La consulta devuelve 1 y 2.  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **DISTINCT-Values ()** asigna valores enteros a XS: decimal.  
  
-   La función **DISTINCT-Values ()** solo admite los tipos mencionados anteriormente y no admite la combinación de tipos base.  
  
-   No se admite la función **DISTINCT-Values ()** en valores XS: Duration.  
  
-   No se admite la opción sintáctica que proporciona una intercalación.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
