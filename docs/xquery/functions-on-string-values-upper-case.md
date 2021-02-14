---
title: Función en mayúsculas (XQuery) | Microsoft Docs
description: Obtenga información sobre cómo usar la función de XQuery en mayúsculas (), que convierte los caracteres en mayúsculas equivalentes.
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
- upper-case
- upper-case Function (XQuery)
ms.assetid: 5bd01ad2-7adf-48fb-bf42-41e200419d37
author: rothja
ms.author: jroth
ms.openlocfilehash: 62a59ec49dfccfba7c710baf9a80eae2c301a94b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344964"
---
# <a name="functions-on-string-values---upper-case"></a>Funciones usadas en valores de cadena: upper-case
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Esta función convierte cada carácter de *$arg* en su equivalente en mayúsculas. La conversión de grafía binaria de Microsoft Windows para los puntos de código Unicode especifica cómo se convierten los caracteres a letra mayúscula. Esta norma es diferente de la asignación para la norma de punto de código Unicode.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:upper-case($arg as xs:string?) as xs:string  
```  
  
## <a name="arguments"></a>Argumentos  
  
|Término|Definición|  
|-|-|
|*$arg*|Valor de cadena que se va a convertir a letra mayúscula.|  
  
## <a name="remarks"></a>Comentarios  
 Si el valor de *$arg* está vacío, se devuelve una cadena de longitud cero.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-changing-a-string-to-upper-case"></a>A. Cambiar una cadena a letra mayúscula  
 En el ejemplo siguiente se cambia la cadena de entrada ' abcDEF! @4 ' en mayúsculas.  
  
```  
DECLARE @x xml = N'abcDEF!@4';  
SELECT @x.value('fn:upper-case(/text()[1])', 'nvarchar(10)');  
```  
  
### <a name="b-search-for-a-specific-character-string"></a>B. Buscar una cadena de caracteres concreta  
 En este ejemplo se muestra cómo utilizar la función de letra mayúscula para realizar una búsqueda sin distinción entre mayúsculas y minúsculas.  
  
```  
USE AdventureWorks  
GO  
--WITH XMLNAMESPACES clause specifies the namespace prefix  
--to use.   
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
--The XQuery contains() function is used to determine whether  
--any of the text nodes below the <Summary> element contain  
--the word 'frame'. The upper-case() function is used to make  
--the search case-insensitive.  
  
SELECT ProductModelID, CatalogDescription.query('  
      <Prod>  
         { /pd:ProductDescription/@ProductModelID }  
         { /pd:ProductDescription/pd:Summary }  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
/pd:ProductDescription/pd:Summary//text()[  
          contains(upper-case(.), "FRAME")]')  = 1  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
 `ProductModelID Result`  
  
 `-------------- ---------`  
  
 `19     <Prod ProductModelID="19">`  
  
 `<pd:Summary xmlns:pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">Our top-of-the-line competition mountain bike.`  
  
 `Performance-enhancing options include the innovative HL Frame,`  
  
 `super-smooth front suspension, and traction for all terrain.`  
  
 `</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
 `25     <Prod ProductModelID="25">`  
  
 `<pd:Summary xmlns:pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">This bike is ridden by race winners. Developed with the`  
  
 `Adventure Works Cycles professional race team, it has a extremely light`  
  
 `heat-treated aluminum frame, and steering that allows precision control.`  
  
 `</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
