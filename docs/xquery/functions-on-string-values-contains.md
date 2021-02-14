---
title: Función Contains (XQuery) | Microsoft Docs
description: Obtenga información sobre cómo usar la función Contains en una expresión XQuery para determinar si un valor de cadena especificado contiene el valor de subcadena especificado.
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
- contains function (XQuery)
- fn:contains function
ms.assetid: 2c88c015-04fc-429b-84b2-835596a28b65
author: rothja
ms.author: jroth
ms.openlocfilehash: 18f57d57e1a944a1be66a880231533e6cf34493e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344976"
---
# <a name="functions-on-string-values---contains"></a>Funciones usadas en valores de cadena: contains
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve un valor de tipo XS: Boolean que indica si el valor de *$ARG 1* contiene un valor de cadena especificado por *$arg 2*.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:contains ($arg1 as xs:string?, $arg2 as xs:string?) as xs:boolean?  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg 1*  
 Valor de cadena que se va a comprobar.  
  
 *$arg 2*  
 Subcadena que se va a buscar.  
  
## <a name="remarks"></a>Comentarios  
 Si el valor de *$arg 2* es una cadena de longitud cero, la función devuelve **true**. Si el valor de *$ARG 1* es una cadena de longitud cero y el valor de *$arg 2* no es una cadena de longitud cero, la función devuelve **false**.  
  
 Si el valor de *$ARG 1* o *$arg 2* es la secuencia vacía, el argumento se trata como la cadena de longitud cero.  
  
 La función contains() usa la intercalación de punto de código Unicode predeterminada de XQuery para la comparación de cadenas.  
  
 El valor de subcadena especificado para *$arg 2* debe ser menor o igual que 4000 caracteres. Si el valor especificado es superior a 4000 caracteres, se produce una condición de error dinámico y la función Contains () devuelve una secuencia vacía en lugar de un valor booleano de **true** o **false**. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no genera errores dinámicos en expresiones XQuery.  
  
 Para obtener comparaciones sin distinción entre mayúsculas y minúsculas, se pueden usar las funciones [en mayúsculas o](../xquery/functions-on-string-values-upper-case.md) minúsculas.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caracteres adicionales (pares suplentes)  
 El comportamiento de pares suplentes en las funciones XQuery depende del nivel de compatibilidad de la base de datos y, en algunos casos, del URI del espacio de nombres predeterminado de las funciones. Para obtener más información, vea la sección "las funciones XQuery son compatibles con suplentes" en el tema [cambios importantes en las características de motor de base de datos en SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Vea también el [nivel de compatibilidad de Alter database &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) y la [Intercalación y compatibilidad con Unicode](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo XML en la base de datos AdventureWorks.  
  
### <a name="a-using-the-contains-xquery-function-to-search-for-a-specific-character-string"></a>A. Utilizar la función contains() de XQuery para buscar una cadena de caracteres específica  
 La consulta siguiente busca los productos que contengan la palabra Aerodynamic en las descripciones resumidas. La consulta devuelve el ProductID y el <`Summary` elemento> para estos productos.  
  
```  
--The product model description document uses  
--namespaces. The WHERE clause uses the exit()  
--method of the xml data type. Inside the exit method,  
--the XQuery contains()function is used to  
--determine whether the <Summary> text contains the word  
--Aerodynamic.   
  
USE AdventureWorks  
GO  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
      <Prod>  
         { /pd:ProductDescription/@ProductModelID }  
         { /pd:ProductDescription/pd:Summary }  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
   /pd:ProductDescription/pd:Summary//text()  
    [contains(., "Aerodynamic")]') = 1  
```  
  
 Results  
  
 `ProductModelID Result`  
  
 `-------------- ---------`  
  
 `28     <Prod ProductModelID="28">`  
  
 `<pd:Summary xmlns:pd=`  
  
 `"https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">`  
  
 `A TRUE multi-sport bike that offers streamlined riding and`  
  
 `a revolutionary design. Aerodynamic design lets you ride with`  
  
 `the pros, and the gearing will conquer hilly roads.</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
