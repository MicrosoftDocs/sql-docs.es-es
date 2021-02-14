---
title: Especificar AXIS en un paso de expresión de trazado | Microsoft Docs
description: Obtenga información sobre cómo especificar un paso de eje en una expresión de ruta de acceso XQuery.
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
- attribute axis [SQL Server]
- axis step [XQuery]
- descendant axis
- self axis
- path expressions [XQuery]
- child axis
- descendant-or-self axis
- parent axis
ms.assetid: c44fb843-0626-4496-bde0-52ca0bac0a9e
author: rothja
ms.author: jroth
ms.openlocfilehash: 2207dcce2787fb6f51668e777b01ce345d1f33ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344666"
---
# <a name="path-expressions---specifying-axis"></a>Expresiones de ruta de acceso: Especificación de ejes
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Un paso de eje de una expresión de ruta de acceso incluye los siguientes componentes:  
  
-   Un eje  
  
-   Una [prueba de nodo](../xquery/path-expressions-specifying-node-test.md)  
  
-   [Cero o más calificadores de paso (opcional)](../xquery/path-expressions-specifying-predicates.md)  
  
 Para obtener más información, vea [expresiones de ruta de acceso &#40;XQuery&#41;](../xquery/path-expressions-xquery.md).  
  
 La implementación de XQuery en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] admite los pasos de eje siguientes:  
  
|Eje|Descripción|  
|----------|-----------------|  
|**Child1**|Devuelve elementos secundarios del nodo de contexto.|  
|**descendant**|Devuelve todos los descendientes del nodo de contexto.|  
|**parent**|Devuelve el elemento primario del nodo de contexto.|  
|**attribute**|Devuelve atributos del nodo de contexto.|  
|**mismo**|Devuelve el propio nodo de contexto.|  
|**descendiente o propio**|Devuelve el nodo de contexto y todos los descendientes del mismo.|  
  
 Todos estos ejes, excepto el eje **primario** , son ejes hacia delante. El eje **primario** es un eje inverso, porque busca hacia atrás en la jerarquía del documento. Por ejemplo, la expresión de ruta de acceso relativa `child::ProductDescription/child::Summary` tiene dos pasos, y cada uno especifica un eje `child`. En el primer paso se recuperan los \<ProductDescription> elementos secundarios del nodo de contexto. Para cada \<ProductDescription> nodo de elemento, el segundo paso recupera los \<Summary> elementos secundarios del nodo de elemento.  
  
 La expresión de ruta de acceso relativa, `child::root/child::Location/attribute::LocationID`, tiene tres pasos. Cada uno de los dos primeros especifica un eje `child` y el tercero especifica el eje `attribute`. Cuando se ejecuta en los documentos XML de instrucciones de fabricación de la tabla **Production. ProductModel** , la expresión devuelve el `LocationID` atributo del \<Location> nodo de elemento secundario del \<root> elemento.  
  
## <a name="examples"></a>Ejemplos  
 Los ejemplos de consulta de este tema se especifican con las columnas de tipo **XML** de la base de datos **AdventureWorks** .  
  
### <a name="a-specifying-a-child-axis"></a>A. Especificar un eje child  
 Para un modelo de producto específico, la consulta siguiente recupera los nodos \<Features> de elemento secundarios del \<ProductDescription> nodo de elemento de la descripción del catálogo de productos almacenada en la `Production.ProductModel` tabla.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  /child::PD:ProductDescription/child::PD:Features')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   El `query()` método del tipo de datos **XML** especifica la expresión de ruta de acceso.  
  
-   Ambos pasos de la expresión de ruta de acceso especifican un eje `child` y los nombres de nodo, `ProductDescription` y `Features`, como pruebas de nodo. Para obtener información acerca de las pruebas de nodo, consulte [especificar una prueba de nodo en un paso de expresión de ruta de acceso](../xquery/path-expressions-specifying-node-test.md).  
  
### <a name="b-specifying-descendant-and-descendant-or-self-axes"></a>B. Especificar ejes descendant y descendant-or-self  
 En el ejemplo siguiente se utilizan ejes descendant y descendant-or-self. La consulta de este ejemplo se especifica en una variable de tipo **XML** . Se ha simplificado la instancia XML para ilustrar claramente la diferencia en los resultados generados.  
  
```  
declare @x xml  
set @x='  
<a>  
 <b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
 </b>  
</a>'  
declare @y xml  
set @y = @x.query('  
  /child::a/child::b  
')  
select @y  
```  
  
 En el resultado siguiente, la expresión devuelve el nodo de elemento secundario `<b>` del nodo de elemento `<a>`:  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
```  
  
 En esta expresión, si especifica un eje descendant para la expresión de ruta de acceso,  
  
 `/child::a/child::b/descendant::*`, se solicitan todos los descendientes del `b` nodo elemento de> de <.  
  
 El asterisco (*) en la prueba de nodo representa el nombre del nodo como prueba de nodo. Por tanto, el tipo de nodo principal del eje descendant, el nodo de elemento, determina los tipos de nodos devueltos. Es decir, la expresión devuelve todos los nodos de elemento. No se devuelven los nodos de texto. Para obtener más información sobre el tipo de nodo principal y su relación con la prueba de nodo, vea el tema [especificar una prueba de nodo en un paso de expresión de ruta de acceso](../xquery/path-expressions-specifying-node-test.md) .  
  
 Se devuelven los nodos de elemento <`c`> y <`d`>, como se muestra en el resultado siguiente:  
  
```  
<c>text2  
     <d>text3</d>  
</c>  
<d>text3</d>  
```  
  
 Si especifica un eje descendant-or-self en lugar del eje descendiente, `/child::a/child::b/descendant-or-self::*` devuelve el nodo de contexto, el elemento <`b`> y su descendiente.  
  
 El resultado es el siguiente:  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
  
<c>text2  
     <d>text3</d>  
</c>  
  
<d>text3</d>   
```  
  
 La consulta de ejemplo siguiente en la base de datos **AdventureWorks** recupera todos los nodos de elementos descendientes del elemento `Features` secundario <> del elemento <`ProductDescription`>:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  /child::PD:ProductDescription/child::PD:Features/descendant::*  
')  
FROM  Production.ProductModel  
WHERE ProductModelID=19  
```  
  
### <a name="c-specifying-a-parent-axis"></a>C. Especificar un eje parent  
 La consulta siguiente devuelve el `Summary` elemento secundario <> del elemento <`ProductDescription`> del documento XML del catálogo de productos almacenado en la `Production.ProductModel` tabla.  
  
 En este ejemplo se usa el eje primario para volver al elemento primario del `Feature` elemento <> y recuperar el `Summary` elemento secundario <> del elemento <> `ProductDescription` .  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
/child::PD:ProductDescription/child::PD:Features/parent::PD:ProductDescription/child::PD:Summary  
')  
FROM   Production.ProductModel  
WHERE  ProductModelID=19  
  
```  
  
 En este ejemplo de consulta, la expresión de ruta de acceso utiliza el eje `parent`. Puede volver a escribir la expresión sin el eje parent, tal como se muestra a continuación:  
  
```  
/child::PD:ProductDescription[child::PD:Features]/child::PD:Summary  
```  
  
 A continuación se ofrece un ejemplo más útil del eje parent.  
  
 Cada descripción del catálogo de modelos de producto almacenada en la columna **CatalogDescription** de la tabla **ProductModel** tiene un `<ProductDescription>` elemento que tiene el `ProductModelID` atributo y el `<Features>` elemento secundario, tal y como se muestra en el fragmento siguiente:  
  
```  
<ProductDescription ProductModelID="..." >  
  ...  
  <Features>  
    <Feature1>...</Feature1>  
    <Feature2>...</Feature2>  
   ...  
</ProductDescription>  
```  
  
 La consulta establece una variable de iteración, `$f`, en la instrucción FLWOR para devolver el elemento secundario del elemento `<Features>`. Para obtener más información, vea [instrucción FLWOR e iteración &#40;&#41;XQuery ](../xquery/flwor-statement-and-iteration-xquery.md). Por cada característica, la cláusula `return` construye un XML de la forma siguiente:  
  
```  
<Feature ProductModelID="...">...</Feature>  
<Feature ProductModelID="...">...</Feature>  
```  
  
 Para agregar la `ProductModelID` para cada `<Feature` elemento>, `parent` se especifica el eje:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
  for $f in /child::PD:ProductDescription/child::PD:Features/child::*  
  return  
   <Feature  
     ProductModelID="{ ($f/parent::PD:Features/parent::PD:ProductDescription/attribute::ProductModelID)[1]}" >  
          { $f }  
   </Feature>  
')  
FROM  Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Éste es el resultado parcial:  
  
```  
<Feature ProductModelID="19">  
  <wm:Warranty   
   xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
    <wm:Description>parts and labor</wm:Description>  
  </wm:Warranty>  
</Feature>  
<Feature ProductModelID="19">  
  <wm:Maintenance   
   xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <wm:NoOfYears>10 years</wm:NoOfYears>  
    <wm:Description>maintenance contract available through your dealer   
                  or any AdventureWorks retail store.</wm:Description>  
  </wm:Maintenance>  
</Feature>  
<Feature ProductModelID="19">  
  <p1:wheel   
   xmlns:p1="https://www.adventure-works.com/schemas/OtherFeatures">  
      High performance wheels.  
  </p1:wheel>  
</Feature>  
```  
  
 Tenga en cuenta que se agrega el predicado `[1]` en la expresión de ruta de acceso para garantizar que se devuelve un valor singleton.  
  
  
