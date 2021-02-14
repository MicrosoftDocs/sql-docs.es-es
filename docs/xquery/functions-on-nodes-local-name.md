---
title: Función local-name (XQuery) | Microsoft Docs
description: Obtenga información sobre cómo usar la función de XQuery local-name () para devolver la parte del nombre local de un nodo.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:local-name function
- local-name function
ms.assetid: c901ef5d-89c5-482a-bf64-3eefbcf3098d
author: rothja
ms.author: jroth
ms.openlocfilehash: 9d336db0ac3a6f9c490e3b25a9bee98e1228594f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353291"
---
# <a name="functions-on-nodes---local-name"></a>Funciones usadas en nodos: local-name
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve la parte local del nombre de *$arg* como XS: String, que será la cadena de longitud cero o tendrá la forma léxica de XS: NCName. Si no se proporciona el argumento, el valor predeterminado será el nodo de contexto.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
fn:local-name() as xs:string  
fn:local-name($arg as node()?) as xs:string  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Nombre del nodo cuya parte de nombre local se recuperará.  
  
## <a name="remarks"></a>Comentarios  
  
-   En SQL Server, **FN: local-name ()** sin un argumento solo se puede usar en el contexto de un predicado dependiente del contexto. En concreto, solo se puede utilizar entre corchetes (`[ ]`).  
  
-   Si se proporciona el argumento y es la secuencia vacía, la función devolverá la cadena de longitud cero.  
  
-   Si el nodo de destino no tiene nombre porque es un nodo de documento, un comentario o un nodo de texto, la función devolverá la cadena de longitud cero.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-retrieve-local-name-of-a-specific-node"></a>A. Recuperar el nombre local de un nodo específico  
 La consulta siguiente se especifica en una instancia XML sin tipo. La expresión de consulta, `local-name(/ROOT[1])`, recupera la parte de nombre local del nodo especificado.  
  
```  
declare @x xml  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('local-name(/ROOT[1])')  
-- result = ROOT  
```  
  
 La consulta siguiente se especifica en la columna Instructions, una columna xml con tipo, de la tabla ProductModel. La expresión, `local-name(/AWMI:root[1]/AWMI:Location[1])`, devuelve el nombre local, `Location`, del nodo especificado.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     local-name(/AWMI:root[1]/AWMI:Location[1])') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
-- result = Location  
```  
  
### <a name="b-using-local-name-without-argument-in-a-predicate"></a>B. Usar local-name sin argumento en un predicado  
 La siguiente consulta se especifica en la columna instructions, la columna **XML** con tipo, de la tabla ProductModel. La expresión devuelve todos los elementos secundarios del elemento <`root`> cuya parte del nombre local de QName sea "Location". La función **local-name ()** se especifica en el predicado y no tiene ningún argumento que la función usa el nodo de contexto.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
  /AWMI:root//*[local-name() = "Location"]') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 La consulta devuelve todos los `Location` elementos secundarios del elemento <> del `root` elemento <>.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones en nodos](./xquery-functions-against-the-xml-data-type.md)   
 [Función namespace-uri &#40;XQuery&#41;](../xquery/functions-on-nodes-namespace-uri.md)  
  
