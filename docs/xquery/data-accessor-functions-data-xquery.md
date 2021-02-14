---
title: Data (función de XQuery) | Microsoft Docs
description: Obtenga información sobre cómo usar los datos de la función XQuery () para devolver el valor con tipo de cada elemento de una secuencia especificada de elementos.
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
- fn:data function
- data function [XQuery]
ms.assetid: 511b5d7d-c679-4cb2-a3dd-170cc126f49d
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e20c00d93082994cd5dc230bfe1b4356b3ed031
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338503"
---
# <a name="data-accessor-functions---data-xquery"></a>Funciones del descriptor de acceso a datos: data (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Devuelve el valor con tipo de cada elemento especificado por *$arg*.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn:data ($arg as item()*) as xdt:untypedAtomic*  
```  
  
## <a name="arguments"></a>Argumentos  
 *$arg*  
 Secuencia de elementos cuyos valores con tipo van a devolverse.  
  
## <a name="remarks"></a>Comentarios  
 La siguiente información se aplica a los valores con tipo:  
  
-   El valor con tipo de un valor atómico es el propio valor atómico.  
  
-   El valor con tipo de un nodo de texto es el valor de cadena del nodo de texto.  
  
-   El valor con tipo de un comentario es el valor de cadena del comentario.  
  
-   El valor con tipo de una instrucción de procesamiento es el contenido de la instrucción de procesamiento, sin el nombre de destino de la instrucción de procesamiento.  
  
-   El valor con tipo de un nodo de documento es su valor de cadena.  
  
 La siguiente información se aplica a los nodos de atributo y de elemento:  
  
-   Si un nodo de atributo se escribe con un tipo de esquema XML, su valor con tipo será el valor con tipo en consecuencia.  
  
-   Si el nodo de atributo no tiene tipo, su valor con tipo es igual a su valor de cadena que se devuelve como una instancia de **XDT: untypedAtomic**.  
  
-   Si el nodo de elemento no tiene tipo, su valor con tipo será igual a su valor de cadena que se devuelve como una instancia de **XDT: untypedAtomic**.  
  
 La siguiente información se aplica a los nodos de elemento con tipo:    
  
-   Si el elemento tiene un tipo de contenido simple, **Data ()** devuelve el valor con tipo del elemento.  
  
-   Si el nodo es de tipo complejo, incluido XS: anyType, **Data ()** devuelve un error estático.  
  
 Aunque el uso de la función **Data ()** suele ser opcional, como se muestra en los ejemplos siguientes, si se especifica la función **Data ()** , se aumenta explícitamente la legibilidad de la consulta. Para obtener más información, vea [conceptos básicos de XQuery](../xquery/xquery-basics.md).  
  
 No se pueden especificar **datos ()** en XML construido, como se muestra a continuación:  
  
```  
declare @x xml  
set @x = ''  
select @x.query('data(<SomeNode>value</SomeNode>)')  
```  
  
## <a name="examples"></a>Ejemplos  
 En este tema se proporcionan ejemplos de XQuery con instancias XML almacenadas en varias columnas de tipo **XML** de la base de datos AdventureWorks.  
  
### <a name="a-using-the-data-xquery-function-to-extract-typed-value-of-a-node"></a>A. Utilizar la función data() de XQuery para extraer el valor con tipo de un nodo  
 La consulta siguiente muestra cómo se usa la función **Data ()** para recuperar valores de un atributo, un elemento y un nodo de texto:  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query(N'  
 for $pd in //p1:ProductDescription  
 return   
    <Root   
      ProductID = "{ data( ($pd//@ProductModelID)[1] ) }"   
      Feature =   "{ data( ($pd/p1:Features/wm:Warranty/wm:Description)[1] ) }" >  
    </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 El resultado es el siguiente:  
  
```  
<Root ProductID="19" Feature="parts and labor"/>  
```  
  
 Como se mencionó, la función **Data ()** es opcional cuando se construyen atributos. Si no se especifica la función **Data ()** , se asume implícitamente. La siguiente consulta genera los mismos resultados que la consulta anterior:  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
      for $pd in //p1:ProductDescription  
         return   
          <Root    
                ProductID = "{ ($pd/@ProductModelID)[1] }"    
                Feature =   "{ ($pd/p1:Features/wm:Warranty/wm:Description)[1] }" >  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 En los siguientes ejemplos se muestran instancias en las que se requiere la función **Data ()** .  
  
 En la consulta siguiente, **$PD/P1: Specifications/material** devuelve el `Material` elemento> <. Además, los **datos ($PD/P1: Specifications/material)** devuelven datos de caracteres con el tipo XDT: untypedAtomic, porque <`Material`> no tiene tipo. Cuando la entrada no tiene tipo, el resultado de **Data ()** se escribe como **XDT: untypedAtomic**.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $pd in //p1:ProductDescription  
         return   
          <Root>  
             { $pd/p1:Specifications/Material }  
             { data($pd/p1:Specifications/Material) }  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 El resultado es el siguiente:  
  
```  
<Root>  
  <Material>Almuminum Alloy</Material>Almuminum Alloy  
</Root>  
```  
  
 En la consulta siguiente, **Data ($PD/P1: Features/WM: garantía)** devuelve un error estático, porque <`Warranty`> es un elemento de tipo complejo.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
 <Root>  
   {     /p1:ProductDescription/p1:Features/wm:Warranty }  
   { data(/p1:ProductDescription/p1:Features/wm:Warranty) }  
 </Root>  
 ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID = 23  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de XQuery con el tipo de datos xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
