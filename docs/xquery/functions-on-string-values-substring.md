---
title: substring (función de XQuery) | Microsoft Docs
description: Obtenga información sobre la subcadena de la función XQuery () que devuelve la parte especificada de una cadena de origen.
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
- substring function [XQuery]
- fn:substring function
ms.assetid: 2b3b8651-de51-46dc-af82-c86c45eac871
author: rothja
ms.author: jroth
ms.openlocfilehash: 7457341a5489485973869e46c2ba721b86352217
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345010"
---
# <a name="functions-on-string-values---substring"></a>Funciones usadas en valores de cadena: substring
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve parte del valor de *$sourceString*, comenzando en la posición indicada por el valor de *$startingLoc* y continúa para el número de caracteres indicado por el valor de *$length*.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?) as xs:string?  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?,  
                          $length as xs:decimal?) as xs:string?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$sourceString*  
 Cadena de origen.  
  
 *$startingLoc*  
 Punto inicial de la cadena de origen donde comienza la subcadena. Si este valor es negativo o es 0, solo se devuelven los caracteres en posiciones mayores que cero. Si es mayor que la longitud de la *$sourceString*, se devuelve la cadena de longitud cero.  
  
 *$length*  
 [opcional] Número de caracteres que se va a recuperar. Si no se especifica, devuelve todos los caracteres de la ubicación especificada en *$startingLoc* hasta el final de la cadena.  
  
## <a name="remarks"></a>Observaciones  
 La versión de la función con tres argumentos devuelve los caracteres de `$sourceString` cuya posición `$p` sea:  
  
 `fn:round($startingLoc) <= $p < fn:round($startingLoc) + fn:round($length)`  
  
 El valor de *$length* puede ser mayor que el número de caracteres del valor de *$sourceString* después de la posición inicial. En este caso, la subcadena devuelve los caracteres hasta el final de *$sourceString*.  
  
 El primer carácter de una cadena se encuentra en la posición 1.  
  
 Si el valor de *$sourceString* es la secuencia vacía, se trata como la cadena de longitud cero. De lo contrario, si *$startingLoc* o *$length* es la secuencia vacía, se devuelve la secuencia vacía.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caracteres adicionales (pares suplentes)  
 El comportamiento de pares suplentes en las funciones XQuery depende del nivel de compatibilidad de la base de datos y, en algunos casos, del URI del espacio de nombres predeterminado de las funciones. Para obtener más información, vea la sección "las funciones XQuery son compatibles con suplentes" en el tema [cambios importantes en las características de motor de base de datos en SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Vea también el [nivel de compatibilidad de Alter database &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) y la [Intercalación y compatibilidad con Unicode](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="implementation-limitations"></a>Limitaciones de la implementación  
 SQL Server requiere que los parámetros *$startingLoc* y *$length* sean de tipo XS: decimal en lugar de XS: Double.  
  
 SQL Server permite que *$startingLoc* y *$length* sean una secuencia vacía, porque la secuencia vacía es un valor posible como resultado de la asignación de errores dinámicos a ().  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de datos.  
  
### <a name="a-using-the-substring-xquery-function-to-retrieve-partial-summary-product-model-descriptions"></a>A. Utilizar la función substring() de XQuery para recuperar descripciones resumidas parciales de modelos de productos  
 La consulta recupera los primeros 50 caracteres del texto que describe el modelo de producto, el elemento <`Summary`> del documento.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Prod>{ substring(string((/pd:ProductDescription/pd:Summary)[1]), 1, 50) }</Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('/pd:ProductDescription')  = 1;  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La función **String ()** devuelve el valor de cadena del `Summary` elemento<>. Esta función se usa, porque el `Summary` elemento <> contiene el texto y los subelementos (elementos de formato HTML), y dado que se omitirán estos elementos y se recuperará todo el texto.  
  
-   La función **substring ()** recupera los primeros 50 caracteres del valor de cadena recuperado por la **cadena ()**.  
  
 Éste es un resultado parcial:  
  
```  
ProductModelID Result  
-------------- ----------------------------------------------------  
19      <Prod>Our top-of-the-line competition mountain bike.</Prod>   
23      <Prod>Suitable for any type of riding, on or off-roa</Prod>  
...  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
