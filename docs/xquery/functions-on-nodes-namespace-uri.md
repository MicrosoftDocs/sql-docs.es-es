---
title: namespace-uri (función de XQuery) | Microsoft Docs
description: Obtenga información sobre cómo usar la función namespace-uri en una expresión XQuery para devolver el URI de espacio de nombres de un QName especificado.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:namespace-uri function
- namespace-uri function
ms.assetid: 9b48d216-26c8-431d-9ab4-20ab187917f4
author: rothja
ms.author: jroth
ms.openlocfilehash: 4a9734dd4efd2f4dcedd539218a0a7d989211749
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353278"
---
# <a name="functions-on-nodes---namespace-uri"></a>Funciones usadas en nodos: namespace-uri
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Devuelve el identificador URI de espacio de nombres del QName especificado en *$arg* como XS: String.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
fn:namespace-uri() as xs:string  
fn:namespace-uri($arg as node()?) as xs:string  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Nombre del nodo cuya parte del URI del espacio de nombres se va a recuperar.  
  
## <a name="remarks"></a>Observaciones  
  
-   Si se omite este argumento, el valor predeterminado es el nodo del contexto.  
  
-   En SQL Server, **FN: namespace-uri ()** sin un argumento solo se puede usar en el contexto de un predicado dependiente del contexto. En concreto, solo se puede utilizar entre corchetes ([ ]).  
  
-   Si *$arg* es la secuencia vacía, se devuelve la cadena de longitud cero.  
  
-   Si *$arg* es un nodo de elemento o atributo cuyo QName expandido no se encuentra en un espacio de nombres, la función devuelve la cadena de longitud cero.  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-retrieve-namespace-uri-of-a-specific-node"></a>A. Recuperar el URI de espacio de nombres de un nodo específico  
 La consulta siguiente se especifica en una instancia XML sin tipo. La expresión de la consulta, `namespace-uri(/ROOT[1])`, recupera la parte del URI del espacio de nombres del nodo especificado.  
  
```  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('namespace-uri(/ROOT[1])')  
```  
  
 Dado que el QName especificado no tiene la parte del URI del espacio de nombres sino únicamente la parte del nombre local, el resultado es una cadena de longitud cero.  
  
 La siguiente consulta se especifica en la columna **XML** con tipo instructions. La expresión `namespace-uri(/AWMI:root[1]/AWMI:Location[1])` devuelve el identificador URI de espacio de nombres del primer <`Location`> elemento secundario del `root` elemento <>.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     namespace-uri(/AWMI:root[1]/AWMI:Location[1])') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 El resultado es el siguiente:  
  
```  
https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions  
```  
  
### <a name="b-using-namespace-uri-without-argument-in-a-predicate"></a>B. Usar namespace-uri() sin argumento en un predicado  
 La siguiente consulta se especifica para la columna xml con tipo CatalogDescription. La expresión devuelve todos los nodos de elementos cuyo URI del espacio de nombres es `https://www.adventure-works.com/schemas/OtherFeatures`. La función namespace-**URI ()** se especifica sin un argumento y usa el nodo de contexto.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
   /p1:ProductDescription//*[namespace-uri() = "https://www.adventure-works.com/schemas/OtherFeatures"]  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Este es el resultado parcial:  
  
```  
<p1:wheel xmlns:p1="https://www.adventure-works.com/schemas/OtherFeatures">High performance wheels.</p1:wheel>  
<p2:saddle xmlns:p2="https://www.adventure-works.com/schemas/OtherFeatures">  
  <p3:i xmlns:p3="http://www.w3.org/1999/xhtml">Anatomic design</p3:i> and made from durable leather for a full-day of riding in comfort.</p2:saddle>  
...  
```  
  
 Puede cambiar el URI del espacio de nombres de la consulta anterior por `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain`. A continuación, recibirá todos los nodos de elemento secundarios del `ProductDescription` elemento <> cuya parte del identificador URI de espacio de nombres del QName expandido es `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain` .  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   La función **namespace-uri ()** devuelve instancias de tipo XS: String en lugar de XS: anyURI.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones en nodos](./xquery-functions-against-the-xml-data-type.md)   
 [Función de nombre local &#40;XQuery&#41;](../xquery/functions-on-nodes-local-name.md)  
  
