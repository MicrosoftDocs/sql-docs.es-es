---
title: Funciones de XQuery con el tipo de datos XML | Microsoft Docs
description: Obtenga información sobre las funciones de XQuery que se admiten para el tipo de datos XML.
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
- XQuery, functions
- xml data type [SQL Server], XQuery
- functions [SQL Server], XQuery
ms.assetid: 8df0877d-a03f-4ca9-b84e-908c4bb42b5e
author: rothja
ms.author: jroth
ms.openlocfilehash: 82f3eae7098b3ac5e86355aed1e13bdd5c137015
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352817"
---
# <a name="xquery-functions-against-the-xml-data-type"></a>Funciones de XQuery con el tipo de datos xml
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  En este tema y sus temas secundarios se describen las funciones que se pueden utilizar al especificar XQuery con el tipo de datos **XML** . Para conocer las especificaciones del W3C, vea [http://www.w3.org/TR/2004/WD-xpath-functions-20040723](https://go.microsoft.com/fwlink/?LinkId=4873) .  
  
 Las funciones XQuery pertenecen al http://www.w3.org/2004/07/xpath-functions espacio de nombres. Las especificaciones de W3C utilizan el prefijo de espacio de nombres "fn": para describir estas funciones. No es necesario especificar explícitamente el prefijo de espacio de nombres "fn": cuando se utilicen estas funciones. Debido a esto y para facilitar la lectura, los prefijos de espacio de nombres no se suelen usar en esta documentación.  
  
 En la tabla siguiente se enumeran las funciones de XQuery que se admiten con el tipo de datos **XML**.  
  
|Category|Nombre de la función|  
|--------------|-------------------|  
|[Funciones en valores numéricos]()|[umbral](../xquery/numeric-values-functions-ceiling.md)|  
||[floor](../xquery/numeric-values-functions-floor.md)|  
||[retorno](../xquery/numeric-values-functions-round.md)|  
|[Funciones XQuery en valores de cadena]()|[concat](../xquery/functions-on-string-values-concat.md)|  
||[contains](../xquery/functions-on-string-values-contains.md)|  
||[substring](../xquery/functions-on-string-values-substring.md)|  
||[Función en minúsculas &#40;XQuery&#41;](../xquery/functions-on-string-values-lower-case.md)|  
||[longitud de cadena](../xquery/functions-on-string-values-string-length.md)|  
||[Función en mayúsculas &#40;XQuery&#41;](../xquery/functions-on-string-values-upper-case.md)|  
|Funciones en valores booleanos|[not](../xquery/functions-on-boolean-values-not-function.md)|  
|[Funciones en nodos]()|[número](../xquery/functions-on-nodes-number.md)|  
||[local-name (función de XQuery)](../xquery/functions-on-nodes-local-name.md)|  
||[namespace-uri (función de XQuery)](../xquery/functions-on-nodes-namespace-uri.md)|  
|[Funciones de contexto]()|[last](../xquery/context-functions-last-xquery.md)|  
||[position](../xquery/context-functions-position-xquery.md)|  
|[Funciones utilizadas en secuencias]()|[empty](../xquery/functions-on-sequences-empty.md)|  
||[distinct-values](../xquery/functions-on-sequences-distinct-values.md)|  
||[id. (función de XQuery)](../xquery/functions-on-sequences-id.md)|  
|[Funciones de agregado &#40;XQuery&#41;]()|[count](../xquery/aggregate-functions-count.md)|  
||[latencia](../xquery/aggregate-functions-avg.md)|  
||[min](../xquery/aggregate-functions-min.md)|  
||[max](../xquery/aggregate-functions-max.md)|  
||[sum](../xquery/aggregate-functions-sum.md)|  
|[Funciones de constructor &#40;XQuery&#41;](../xquery/constructor-functions-xquery.md)|[Funciones de constructor](../xquery/constructor-functions-xquery.md)|  
|[Funciones del descriptor de acceso a datos](../xquery/data-accessor-functions.md)|[string](../xquery/data-accessor-functions-string-xquery.md)|  
||[data](../xquery/data-accessor-functions-data-xquery.md)|  
|[Funciones de constructor booleano &#40;XQuery&#41;]()|[true (función de XQuery)](../xquery/boolean-constructor-functions-true-xquery.md)|  
||[false (función de XQuery)](../xquery/boolean-constructor-functions-false-xquery.md)|  
|[Funciones relacionadas con QNames &#40;XQuery&#41;](./functions-related-to-qnames-expanded-qname.md)|[expanded-QName (XQuery)](../xquery/functions-related-to-qnames-expanded-qname.md)|  
||[local-name-from-QName (XQuery)](../xquery/functions-related-to-qnames-local-name-from-qname.md)|  
||[namespace-uri-from-QName (XQuery)](../xquery/functions-related-to-qnames-namespace-uri-from-qname.md)|  
|[Funciones de extensión de XQuery en SQL Server](./xquery-extension-functions-sql-column.md)|[SQL: column () (función de XQuery)](../xquery/xquery-extension-functions-sql-column.md)|  
||[sql:variable() (función de XQuery)](../xquery/xquery-extension-functions-sql-variable.md)|  
  
## <a name="see-also"></a>Consulte también  
 [métodos del tipo de datos xml](../t-sql/xml/xml-data-type-methods.md)   
 [Referencia del lenguaje XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Datos XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)  
  
