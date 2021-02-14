---
title: Especificar la prueba de nodo en un paso de expresión de ruta de acceso | Microsoft Docs
description: Obtenga información sobre cómo especificar una prueba de nodo en el paso de eje de una expresión de ruta de acceso XQuery.
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
- axis step [XQuery]
- node test [XQuery]
ms.assetid: ffe27a4c-fdf3-4c66-94f1-7e955a36cadd
author: rothja
ms.author: jroth
ms.openlocfilehash: 578d10bc078edf886eb7980c3b7a099c99ef59a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338546"
---
# <a name="path-expressions---specifying-node-test"></a>Expresiones de ruta de acceso: Especificar prueba de nodo
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Un paso de eje de una expresión de ruta de acceso incluye los siguientes componentes:  
  
-   [Un eje](../xquery/path-expressions-specifying-axis.md)  
  
-   Una prueba de nodo  
  
-   [Cero o más calificadores de paso (opcional)](../xquery/path-expressions-specifying-predicates.md)  
  
 Para obtener más información, vea [expresiones de ruta de acceso &#40;XQuery&#41;](../xquery/path-expressions-xquery.md).  
  
 Una prueba de nodo es una condición y es el segundo componente del paso de eje en una expresión de ruta de acceso. Todos los nodos seleccionados por un paso deben satisfacer esta condición. Para la expresión de ruta de acceso `/child::ProductDescription`, la prueba de nodo es `ProductDescription`. Este paso solo recupera los nodos de elemento secundarios cuyo nombre sea ProductDescription.  
  
 Una condición de prueba de nodo puede incluir lo siguiente:  
  
-   Un nombre de nodo. Solo se devuelven nodos de la clase de nodo principal con el nombre especificado.  
  
-   Un tipo de nodo. Solo se devuelven nodos del tipo especificado.  
  
> [!NOTE]  
>  Los nombres de nodo especificados en expresiones de ruta de acceso XQuery no siguen las mismas reglas de distinción de intercalación que las consultas Transact-SQL y siempre distinguen entre mayúsculas y minúsculas.  
  
## <a name="node-name-as-node-test"></a>Nombre de nodo como prueba de nodo  
 Al especificar un nombre de nodo como prueba de nodo en un paso de expresión de ruta de acceso, es preciso entender el concepto de clase de nodo principal. Cada eje, elemento secundario, elemento primario o atributo tiene un tipo de nodo principal. Por ejemplo:  
  
-   Un eje attribute solo puede contener atributos. Por tanto, el nodo de atributo es la clase de nodo principal del eje attribute.  
  
-   Para otros ejes, si los nodos seleccionados por el eje pueden contener nodos de elemento, el elemento es la clase de nodo principal para ese eje.  
  
 Cuando especifique un nombre de nodo como prueba de nodo, el paso devolverá los siguientes tipos de nodo:  
  
-   Nodos que pertenecen a la clase de nodo principal del eje.  
  
-   Nodos que tienen el mismo nombre que se especifica en la prueba de nodo.  
  
 Por ejemplo, observe la siguiente expresión de ruta de acceso:  
  
```  
child::ProductDescription   
```  
  
 Esta expresión de un paso especifica un eje `child` y el nombre de nodo `ProductDescription` como prueba de nodo. La expresión devuelve solo aquellos nodos que pertenecen a la clase de nodo principal del eje child, nodos de elemento y nodos cuyo nombre es ProductDescription.  
  
 La expresión de ruta de acceso `/child::PD:ProductDescription/child::PD:Features/descendant::*,` incluye tres pasos. Estos pasos especifican ejes child y descendant. En cada paso, el nombre de nodo se especifica como la prueba de nodo. El carácter comodín (`*`) del tercer paso señala todos los nodos que pertenecen a la clase de nodo principal para el eje descendant. La clase de nodo principal del eje determina el tipo de nodo seleccionado. El nombre de nodo filtra los nodos seleccionados.  
  
 Como resultado, cuando esta expresión se ejecuta en documentos XML de catálogo de productos en la tabla **ProductModel** , recupera todos los nodos de elemento secundarios del \<Features> nodo de elemento secundario del \<ProductDescription> elemento.  
  
 La expresión de ruta `/child::PD:ProductDescription/attribute::ProductModelID` de acceso,, se compone de dos pasos. Ambos pasos especifican un nombre de nodo como la prueba de nodo. Además, el segundo paso utiliza el eje attribute. Por tanto, cada paso selecciona nodos de la clase de nodo principal de su eje que tenga especificado el nombre como la prueba de nodo. Por lo tanto, la expresión devuelve el nodo de atributo **ProductModelID** del \<ProductDescription> nodo de elemento.  
  
 Al especificar nombres de nodo para pruebas de nodo, puede utilizar el carácter comodín (*) para especificar el nombre local de un nodo o para su prefijo de espacio de nombres, tal y como muestra el ejemplo siguiente:  
  
```  
declare @x xml  
set @x = '  
<greeting xmlns="ns1">  
   <salutation>hello</salutation>  
</greeting>  
<greeting xmlns="ns2">  
   <salutation>welcome</salutation>  
</greeting>  
<farewell xmlns="ns1" />'  
select @x.query('//*:greeting')  
select @x.query('declare namespace ns="ns1"; /ns:*')  
```  
  
## <a name="node-type-as-node-test"></a>Tipo de nodo como prueba de nodo  
 Para consultar tipos de nodo que no sean nodos de elemento, utilice una prueba de tipo de nodo. Existen cuatro pruebas de tipo de nodo disponibles, tal y como muestra la tabla siguiente.  
  
|Tipo de nodo|Devoluciones|Ejemplo|  
|---------------|-------------|-------------|  
|`comment()`|True para un nodo de comentario.|`following::comment()` selecciona todos los nodos de comentario que aparecen después del nodo de contexto.|  
|`node()`|True para un nodo de cualquier clase.|`preceding::node()` selecciona todos los nodos que aparecen antes del nodo de contexto.|  
|`processing-instruction()`|True para un nodo de instrucciones de procesamiento.|`self::processing instruction()` selecciona todos los nodos de instrucciones de procesamiento incluidos en el nodo de contexto.|  
|`text()`|True para un nodo de texto.|`child::text()` selecciona los nodos de texto que son secundarios del nodo de contexto.|  
  
 Si el tipo de nodo (por ejemplo, text(), comment(), etc.) se especifica como la prueba de nodo, el paso solo devuelve nodos de la clase especificada, con independencia de la clase de nodo principal del eje. Por ejemplo, la siguiente expresión de ruta de acceso solo devuelve los nodos de comentario secundarios del nodo de contexto:  
  
```  
child::comment()  
```  
  
 Del mismo modo, `/child::ProductDescription/child::Features/child::comment()` recupera nodos de comentario secundarios del nodo de \<Features> elemento secundario del \<ProductDescription> nodo de elemento.  
  
## <a name="examples"></a>Ejemplos  
 Los ejemplos siguientes comparan el nombre de nodo y la clase de nodo.  
  
### <a name="a-results-of-specifying-the-node-name-and-the-node-type-as-node-tests-in-a-path-expression"></a>A. Resultados de especificar el nombre de nodo y el tipo de nodo como pruebas de nodo en una expresión de ruta de acceso  
 En el ejemplo siguiente, se asigna un documento XML simple a una variable de tipo **XML** . El documento se consulta por medio de diferentes expresiones de ruta de acceso. A continuación, se comparan los resultados.  
  
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
select @x.query('  
/child::a/child::b/descendant::*  
')  
```  
  
 Esta expresión pregunta por los nodos de elemento descendiente del nodo de elemento `<b>`.  
  
 El asterisco (`*`) de la prueba de nodo señala un carácter comodín para el nombre de nodo. El eje descendant tiene el nodo de elemento como la clase de nodo principal. Por tanto, la expresión devuelve todos los nodos de elemento descendiente del nodo de elemento `<b>`. Es decir, se devuelven los nodos de elemento `<c>` y `<d>`, tal y como muestra el resultado siguiente:  
  
```  
<c>text2  
     <d>text3</d>  
</c>  
<d>text3</d>  
```  
  
 Si especifica un eje descendant-or-self en lugar de especificar uno descendant, se devuelven el nodo de contexto y sus descendientes:  
  
```  
/child::a/child::b/descendant-or-self::*  
```  
  
 Esta expresión devuelve el nodo de elemento `<b>` y sus nodos de elemento descendiente. Al devolver los nodos descendientes, la clase de nodo principal del eje descendant-or-self (el tipo de nodo de elemento) determina las clases de nodos que se devuelven.  
  
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
  
 La expresión anterior utilizaba un carácter comodín como nombre de nodo. En su lugar, puede utilizar la función `node()`, como muestra esta expresión:  
  
```  
/child::a/child::b/descendant::node()  
```  
  
 Dado `node()` que es un tipo de nodo, recibirá todos los nodos del eje descendiente. El resultado es el siguiente:  
  
```  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
 De nuevo, si especifica un eje descendant-or-self y `node()` como la prueba de nodo, recibirá todos los nodos descendientes, nodos de elemento y nodos de texto, además del nodo de contexto (el elemento `<b>`).  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
### <a name="b-specifying-a-node-name-in-the-node-test"></a>B. Especificar un nombre de nodo en la prueba de nodo  
 El ejemplo siguiente especifica un nombre de nodo como la prueba de nodo en todas las expresiones de ruta de acceso. Como resultado, todas las expresiones devuelven nodos de la clase de nodo principal del eje que especifica el nombre de nodo en la prueba de nodo.  
  
 La siguiente expresión de consulta devuelve el `Warranty` elemento <> del documento XML del catálogo de productos almacenado en la `Production.ProductModel` tabla:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:Warranty  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   La palabra clave `namespace` del prólogo de XQuery define un prefijo que se utiliza en el cuerpo de la consulta. Para obtener más información acerca del prólogo de XQuery, vea el [prólogo de XQuery](../xquery/modules-and-prologs-xquery-prolog.md) .  
  
-   Los tres pasos de la expresión de ruta de acceso especifican el eje child y el nombre de nodo como la prueba de nodo.  
  
-   La parte del calificador de paso opcional del paso de eje no se especifica en ningún paso de la expresión.  
  
 La consulta devuelve los elementos secundarios del elemento <`Warranty`> del elemento `Features` secundario <> del elemento <> `ProductDescription` .  
  
 El resultado es el siguiente:  
  
```  
<wm:Warranty xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
  <wm:Description>parts and labor</wm:Description>  
</wm:Warranty>     
```  
  
 En la consulta siguiente, la expresión de ruta de acceso especifica un carácter comodín (`*`) en una prueba de nodo.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 El carácter comodín se especifica para el nombre de nodo. Por lo tanto, la consulta devuelve todos los nodos de elemento secundarios del nodo de `Features` elemento secundario <> del `ProductDescription` nodo del elemento de> <.  
  
 La siguiente consulta es similar a la consulta anterior, salvo que además del carácter comodín se especifica un espacio de nombres. Como resultado, se devuelven todos los nodos de elemento secundarios en ese espacio de nombres. Tenga en cuenta que el `Features` elemento> de <puede contener elementos de distintos espacios de nombres.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Puede utilizar el carácter comodín como prefijo de espacio de nombres, tal y como muestra esta consulta:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*:Maintenance  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Esta consulta devuelve los `Maintenance` elementos secundarios del nodo de elemento <> en todos los espacios de nombres del documento XML del catálogo de productos.  
  
### <a name="c-specifying-node-kind-in-the-node-test"></a>C. Especificar la clase de nodo en la prueba de nodo  
 El ejemplo siguiente especifica la clase de nodo como la prueba de nodo en todas las expresiones de ruta de acceso. Como resultado, todas las expresiones devuelven nodos de la clase especificada en la prueba de nodo.  
  
 En la consulta siguiente, la expresión de ruta de acceso especifica una clase de nodo en su tercer paso.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::text()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 En la consulta siguiente, se especifica:  
  
-   La expresión de ruta de acceso incluye tres pasos separados por una barra diagonal (`/`).  
  
-   Cada uno de estos pasos especifica un eje child.  
  
-   Los dos primeros pasos especifican un nombre de nodo como la prueba de nodo; el tercero especifica una clase de nodo como la prueba de nodo.  
  
-   La expresión devuelve nodos de texto secundarios del `Features` elemento secundario <> del nodo del `ProductDescription` elemento de> <.  
  
 Solo se devuelve un nodo de texto. El resultado es el siguiente:  
  
```  
These are the product highlights.   
```  
  
 La consulta siguiente devuelve los elementos secundarios del nodo de comentario del `ProductDescription` elemento <>:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::comment()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   El segundo paso especifica una clase de nodo como la prueba de nodo.  
  
-   Como resultado, la expresión devuelve los elementos secundarios del nodo de comentario de los nodos de elemento de <`ProductDescription`>.  
  
 El resultado es el siguiente:  
  
```  
<!-- add one or more of these elements... one for each specific product in this product model -->  
<!-- add any tags in <specifications> -->      
```  
  
 La consulta siguiente recupera los nodos de instrucciones de procesamiento de nivel superior:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 El resultado es el siguiente:  
  
```  
<?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>   
```  
  
 Puede pasar un parámetro literal de cadena a la prueba de nodo `processing-instruction()`. En este caso, la consulta devuelve las instrucciones de procesamiento cuyo valor de atributo de nombre es el literal de cadena especificado en el argumento.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction("xml-stylesheet")  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
## <a name="implementation-limitations"></a>Limitaciones de la implementación  
 A continuación, se indican las limitaciones específicas:  
  
-   No se admiten las pruebas de nodo SequenceType extendidas.  
  
-   No se admite processing-instruction(name). En su lugar, incluya el nombre entre comillas.  
  
  
