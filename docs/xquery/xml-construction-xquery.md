---
title: Construcción de XML (XQuery) | Microsoft Docs
description: Obtenga información sobre cómo construir estructuras XML en una XQuery mediante constructores directos y calculados.
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
- white space [XQuery]
- computed constructor
- construct XML structures [XQuery]
- constructors [XQuery]
- prolog
- direct constructor [SQL Server]
- XML [SQL Server], construction
- XQuery, XML construction
ms.assetid: a6330b74-4e52-42a4-91ca-3f440b3223cf
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c171dab70a909edb197f5a498d2207bcb2e555b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352323"
---
# <a name="xml-construction-xquery"></a>Construcción de XML (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  En XQuery, puede usar los constructores **directos** y **calculados** para construir estructuras XML dentro de una consulta.  
  
> [!NOTE]  
>  No hay ninguna diferencia entre los constructores **directos** y los **calculados** .  
  
## <a name="using-direct-constructors"></a>Utilizar constructores directos  
 Cuando se utilizan constructores directos, se especifica una sintaxis parecida a XML al construir el XML. En los ejemplos siguientes se muestra la construcción de XML mediante los constructores directos.  
  
### <a name="constructing-elements"></a>Construir elementos  
 Cuando se utilizan notaciones XML, se pueden construir elementos. En el ejemplo siguiente se usa la expresión de constructor de elemento directo y se crea un \<ProductModel> elemento. El elemento construido tiene tres elementos secundarios  
  
-   Un nodo de texto.  
  
-   Dos nodos de elemento, \<Summary> y \<Features> .  
  
    -   El \<Summary> elemento tiene un nodo de texto secundario cuyo valor es "Some Description".  
  
    -   El \<Features> elemento tiene tres nodos de elemento secundarios,, \<Color> \<Weight> y \<Warranty> . Cada uno de estos nodos tiene un nodo de texto secundario y los valores Red, 25 y 2 years parts and labor respectivamente.  
  
```sql
declare @x xml;  
set @x='';  
select @x.query('<ProductModel ProductModelID="111">;  
This is product model catalog description.  
<Summary>Some description</Summary>  
<Features>  
  <Color>Red</Color>  
  <Weight>25</Weight>  
  <Warranty>2 years parts and labor</Warranty>  
</Features></ProductModel>')  
  
```  
  
 El XML resultante es el siguiente:  
  
```xml
<ProductModel ProductModelID="111">  
  This is product model catalog description.  
  <Summary>Some description</Summary>  
  <Features>  
    <Color>Red</Color>  
    <Weight>25</Weight>  
    <Warranty>2 years parts and labor</Warranty>  
  </Features>  
</ProductModel>  
```  
  
 Aunque la construcción de elementos a partir de expresiones constantes, tal como se muestra en este ejemplo, resulta útil, la auténtica ventaja de esta característica del lenguaje XQuery es la capacidad de construir XML que extrae datos de una base de datos de forma dinámica. Puede utilizar llaves para especificar expresiones de consulta. En el XML resultante, se sustituye la expresión por su valor. Por ejemplo, la siguiente consulta crea un `NewRoot` elemento> <con un elemento secundario (<`e`>). El valor del elemento <`e`> se calcula especificando una expresión de ruta de acceso dentro de llaves ("{...}").  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
SELECT @x.query('<NewRoot><e> { /root } </e></NewRoot>');  
```  
  
 Las llaves actúan como tokens de cambio de contexto y cambian la consulta de construcción de XML a evaluación de consulta. En este caso, se evalúa la expresión de ruta de acceso de XQuery entre llaves, `/root`, y se sustituyen los resultados por la misma.  
  
 El resultado es el siguiente:  
  
```xml
<NewRoot>  
  <e>  
    <root>5</root>  
  </e>  
</NewRoot>  
```  
  
 La consulta siguiente es similar a la anterior. Sin embargo, la expresión entre llaves especifica la función **Data ()** para recuperar el valor atómico del `root` elemento <> y lo asigna al elemento construido, <`e`>.  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('  
                           <NewRoot>  
                             <e> { data(/root) } </e>  
                           </NewRoot>' ));  
SELECT @y;  
```  
  
 El resultado es el siguiente:  
  
```xml
<NewRoot>  
  <e>5</e>  
</NewRoot>  
```  
  
 Si desea utilizar llaves como parte del texto en lugar de como token de cambio de contexto, debe utilizarlas con valores de escape como "}}" o "{{", tal como se muestra en el ejemplo siguiente:  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('  
<NewRoot> Hello, I can use {{ and  }} as part of my text</NewRoot>'));  
SELECT @y;  
```  
  
 El resultado es el siguiente:  
  
```xml
<NewRoot> Hello, I can use { and  } as part of my text</NewRoot>  
```  
  
 La consulta siguiente es otro ejemplo de construcción de elementos mediante el constructor de elemento directo. Además, el valor de la <`FirstLocation`> elemento se obtiene ejecutando la expresión entre llaves. La expresión de consulta devuelve los pasos de fabricación de la primera ubicación de centro de trabajo de la columna Instructions de la tabla Production.ProductModel.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        <FirstLocation>  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 El resultado es el siguiente:  
  
```xml
<FirstLocation>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">  
      Insert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>.   
  </AWMI:step>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">  
      Attach <AWMI:tool>Trim Jig TJ-26</AWMI:tool> to the upper and lower right corners of the aluminum sheet.   
  </AWMI:step>  
   ...  
</FirstLocation>  
```  
  
#### <a name="element-content-in-xml-construction"></a>Contenido de elemento en la construcción de XML  
 En el ejemplo siguiente se ilustra el comportamiento de las expresiones en la construcción de contenido de elemento mediante el constructor de elemento directo. En el ejemplo siguiente, el constructor de elemento directo especifica una expresión. Para esta expresión, se crea un nodo de texto en el XML resultante.  
  
```sql
declare @x xml;  
set @x='  
<root>  
  <step>This is step 1</step>  
  <step>This is step 2</step>  
  <step>This is step 3</step>  
</root>';  
select @x.query('  
<result>  
 { for $i in /root[1]/step  
    return string($i)  
 }  
</result>');  
  
```  
  
 La secuencia de valores atómicos resultante de la evaluación de la expresión se agrega al nodo de texto con un espacio adicional entre los valores atómicos adyacentes, tal como se muestra en el resultado. El elemento construido tiene un elemento secundario. Se trata de un nodo de texto que incluye el valor que se muestra en el resultado.  
  
```xml
<result>This is step 1 This is step 2 This is step 3</result>  
```  
  
 Si en lugar de una expresión se especifican tres expresiones independientes que generan tres nodos de texto, los nodos de texto adyacentes se combinarán en uno solo, mediante concatenación, en el XML resultante.  
  
```sql
declare @x xml;  
set @x='  
<root>  
  <step>This is step 1</step>  
  <step>This is step 2</step>  
  <step>This is step 3</step>  
</root>';  
select @x.query('  
<result>  
 { string(/root[1]/step[1]) }  
 { string(/root[1]/step[2]) }  
 { string(/root[1]/step[3]) }  
</result>');  
```  
  
 El nodo del elemento construido tiene un elemento secundario. Se trata de un nodo de texto que incluye el valor que se muestra en el resultado.  
  
```xml
<result>This is step 1This is step 2This is step 3</result>  
```  
  
### <a name="constructing-attributes"></a>Construir atributos  
 Cuando se construyen elementos mediante el constructor de elemento directo, también se pueden especificar atributos del elemento mediante sintaxis parecida a XML, tal como se muestra en este ejemplo:  
  
```sql
declare @x xml;  
set @x='';  
select @x.query('<ProductModel ProductModelID="111">;  
This is product model catalog description.  
<Summary>Some description</Summary>  
</ProductModel>')  
```  
  
 El XML resultante es el siguiente:  
  
```xml
<ProductModel ProductModelID="111">  
  This is product model catalog description.  
  <Summary>Some description</Summary>  
</ProductModel>  
```  
  
 El elemento construido <`ProductModel`> tiene un atributo ProductModelID y estos nodos secundarios:  
  
-   Un nodo de texto, `This is product model catalog description.`  
  
-   Un nodo de elemento, <`Summary`>. Este nodo tiene un nodo de texto secundario, `Some description`.  
  
 Cuando se construye un atributo, se puede especificar su valor con una expresión entre llaves. En este caso, el resultado de la expresión se devuelve como el valor del atributo.  
  
 En el ejemplo siguiente, la función **Data ()** no es estrictamente necesaria. Dado que va a asignar el valor de la expresión a un atributo, **Data ()** se aplica implícitamente para recuperar el valor con tipo de la expresión especificada.  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('<NewRoot attr="{ data(/root) }" ></NewRoot>'));  
SELECT @y;  
```  
  
 El resultado es el siguiente:  
  
```xml
<NewRoot attr="5" />  
```  
  
 A continuación se muestra otro ejemplo en el cual se especifican expresiones para la construcción de los atributos LocationID y SetupHrs. Estas expresiones se evalúan con el XML en la columna Instruction. El valor con tipo de la expresión se asigna a los atributos.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        <FirstLocation   
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7;  
```  
  
 Éste es el resultado parcial:  
  
```xml
<FirstLocation LocationID="10" SetupHours="0.5" >  
  <AWMI:step ...   
  </AWMI:step>  
  ...  
</FirstLocation>  
```  
  
#### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   No se admiten varias expresiones de atributos o las mixtas (cadena y expresión XQuery). Por ejemplo, tal como se muestra en la consulta siguiente, se construye XML en el que `Item` es una constante y el valor `5` se obtiene evaluando una expresión de consulta:  
  
    ```xml
    <a attr="Item 5" />  
    ```  
  
     La consulta siguiente devuelve un error porque se mezcla una cadena de constantes con una expresión ({/x}), algo que no se admite:  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    SELECT @x.query( '<a attr="Item {/x}"/>' )   
    ```  
  
     En este caso, tiene las opciones siguientes:  
  
    -   Formar el valor de atributo concatenando dos valores atómicos. Estos valores atómicos se serializan en el valor del atributo con un espacio entre los mismos:  
  
        ```sql
        SELECT @x.query( '<a attr="{''Item'', data(/x)}"/>' )   
        ```  
  
         El resultado es el siguiente:  
  
        ```xml
        <a attr="Item 5" />  
        ```  
  
    -   Utilice la [función concat](../xquery/functions-on-string-values-concat.md) para concatenar los dos argumentos de cadena en el valor de atributo resultante:  
  
        ```sql
        SELECT @x.query( '<a attr="{concat(''Item'', /x[1])}"/>' )   
        ```  
  
         En este caso, no se agrega un espacio entre los dos valores de cadena. Si desea agregarlo, debe indicarlo explícitamente.  
  
         El resultado es el siguiente:  
  
        ```xml
        <a attr="Item5" />  
        ```  
  
-   No se admiten varias expresiones como valor de un atributo. Por ejemplo, la consulta siguiente genera un error:  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    SELECT @x.query( '<a attr="{/x}{/x}"/>' )  
    ```  
  
-   No se admiten las secuencias heterogéneas. Si se intenta asignar una secuencia heterogénea como valor de atributo, se devolverá un error, como se muestra en el ejemplo siguiente. En este ejemplo, se especifica una secuencia heterogénea, una cadena "Item" y un elemento <`x`>, como el valor del atributo:  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    select @x.query( '<a attr="{''Item'', /x }" />')  
    ```  
  
     Si aplica la función **Data ()** , la consulta funciona porque recupera el valor atómico de la expresión, `/x` , que se concatena con la cadena. A continuación se muestra una secuencia de valores atómicos:  
  
    ```sql
    SELECT @x.query( '<a attr="{''Item'', data(/x)}"/>' )   
    ```  
  
     El resultado es el siguiente:  
  
    ```xml
    <a attr="Item 5" />  
    ```  
  
-   El orden del nodo de atributo se aplica durante la serialización, y no durante la comprobación de tipos estáticos. Por ejemplo, en la consulta siguiente se produce un error porque ésta intenta agregar un atributo después de un nodo que no es un atributo.  
  
    ```sql
    select convert(xml, '').query('  
    element x { attribute att { "pass" }, element y { "Element text" }, attribute att2 { "fail" } }  
    ')  
    go  
    ```  
  
     La consulta del ejemplo anterior devuelve el siguiente error:  
  
    ```  
    XML well-formedness check: Attribute cannot appear outside of element declaration. Rewrite your XQuery so it returns well-formed XML.  
    ```  
  
### <a name="adding-namespaces"></a>Agregar espacios de nombres  
 Cuando se construye XML mediante los constructores directos, es posible crear los nombres del elemento y el atributo construidos como completos mediante un prefijo de espacio de nombres. Puede enlazar el prefijo al espacio de nombres de los modos siguientes:  
  
-   Mediante un atributo de declaración de espacio de nombres.  
  
-   Mediante la cláusula WITH XMLNAMESPACES.  
  
-   En el prólogo de las consultas XQuery.  
  
#### <a name="using-a-namespace-declaration-attribute-to-add-namespaces"></a>Utilizar un atributo de declaración de espacio de nombres para agregar espacios de nombres  
 En el ejemplo siguiente se utiliza un atributo de declaración de espacio de nombres en la construcción de <de elementos `a`> para declarar un espacio de nombres predeterminado. La construcción del elemento secundario <`b`> deshace la declaración del espacio de nombres predeterminado declarado en el elemento primario.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <a xmlns="a">  
    <b xmlns=""/>  
  </a>' )   
```  
  
 El resultado es el siguiente:  
  
```xml
<a xmlns="a">  
  <b xmlns="" />  
</a>  
```  
  
 Puede asignar un prefijo al espacio de nombres. El prefijo se especifica en la construcción del elemento <`a`>.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <x:a xmlns:x="a">  
    <b/>  
  </x:a>' )  
```  
  
 El resultado es el siguiente:  
  
```xml
<x:a xmlns:x="a">  
  <b />  
</x:a>  
```  
  
 Se puede anular la declaración de un espacio de nombres predeterminado en la construcción de XML, pero no la de un prefijo de espacio de nombres. La consulta siguiente devuelve un error, porque no se puede anular la declaración de un prefijo tal y como se especifica en la construcción del elemento <`b`>.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <x:a xmlns:x="a">  
    <b xmlns:x=""/>  
  </x:a>' )  
```  
  
 El espacio de nombres recientemente construido está disponible para su uso en la consulta. Por ejemplo, la consulta siguiente declara un espacio de nombres al construir el elemento, <`FirstLocation`> y especifica el prefijo en las expresiones para los valores de atributo LocationID y SetupHrs.  
  
```sql
SELECT Instructions.query('  
        <FirstLocation xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
 Tenga en cuenta que la creación de un nuevo prefijo de espacio de nombres de este modo reemplazará las declaraciones de espacios de nombres ya existentes de este prefijo. Por ejemplo, la declaración de espacio de nombres, `AWMI="https://someURI"` , en el prólogo de la consulta se reemplaza por la declaración de espacio de nombres en el `FirstLocation` elemento <>.  
  
```sql
SELECT Instructions.query('  
declare namespace AWMI="https://someURI";  
        <FirstLocation xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
#### <a name="using-a-prolog-to-add-namespaces"></a>Utilizar un prólogo para agregar espacios de nombres  
 En este ejemplo se ilustra el modo en el que se pueden agregar espacios de nombres al XML construido. Se declara un espacio de nombres predeterminado en el prólogo de la consulta.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
           declare default element namespace "a";  
            <a><b xmlns=""/></a>' )  
```  
  
 Tenga en cuenta que en la construcción de <de elementos `b`>, el atributo de declaración de espacio de nombres se especifica con una cadena vacía como su valor. Esta acción anula la declaración del espacio de nombres predeterminado que se declara en el principal.  
  

El resultado es el siguiente:  

```xml
<a xmlns="a">  
  <b xmlns="" />  
</a>  
```  
  
### <a name="xml-construction-and-white-space-handling"></a>Construcción de XML y control de los espacios en blanco  
 El contenido de los elementos en la construcción de XML puede incluir caracteres de espacio en blanco. Estos caracteres se controlan de las formas siguientes:  
  
-   Los caracteres de espacio en blanco de los identificadores URI de espacio de nombres se tratan como el tipo XSD **anyURI**. A continuación se especifica cómo se controlan:  
  
    -   Los caracteres de espacio en blanco situados al principio y al final se recortan.  
  
    -   Los caracteres de espacio en blanco internos se contraen en un solo espacio.  
  
-   Los caracteres de avance de línea situados dentro del contenido de los atributos se sustituyen por espacios. Todos los demás caracteres de espacio en blanco permanecen tal cual están.  
  
-   Los espacios en blanco dentro de los elementos se mantienen.  
  
 En el ejemplo siguiente se ilustra el control de los espacios en blanco en la construcción de XML:  
  
```sql
-- line feed is repaced by space.  
declare @x xml  
set @x=''  
select @x.query('  
  
declare namespace myNS="   https://       
 abc/  
xyz  
  
";  
<test attr="    my   
test   attr   
value    " >  
  
<a>  
  
This     is  a  
  
test  
  
</a>  
</test>  
') as XML_Result  
  
```  
  
 El resultado es el siguiente:  
  
```xml
-- result  
<test attr="<test attr="    my test   attr  value    "><a>  
  
This     is  a  
  
test  
  
</a></test>  
"><a>  
  
This     is  a  
  
test  
  
</a></test>  
```  
  
### <a name="other-direct-xml-constructors"></a>Otros constructores directos de XML  
 Los constructores para el procesamiento de instrucciones y los comentarios XML utilizan la misma sintaxis que la de la construcción XML correspondiente. También se admiten los constructores calculados para nodos de texto, pero se utilizan básicamente en XML DML para construir nodos de texto.  
  
 **Nota:** Para obtener un ejemplo del uso de un constructor de nodo de texto explícito, vea el ejemplo específico en el [&#41;de XML DML de inserción &#40;](../t-sql/xml/insert-xml-dml.md).  
  
 En la consulta siguiente, el XML construido incluye un elemento, dos atributos, un comentario y una instrucción de procesamiento. Tenga en cuenta que se usa una coma antes del> de <`FirstLocation` , ya que se está construyendo una secuencia.  
  
```sql
SELECT Instructions.query('  
  declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   <?myProcessingInstr abc="value" ?>,   
   <FirstLocation   
        WorkCtrID = "{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
        SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
       <!-- some comment -->  
       <?myPI some processing instructions ?>  
       { (/AWMI:root/AWMI:Location[1]/AWMI:step) }  
    </FirstLocation>   
') as Result   
FROM Production.ProductModel  
where ProductModelID=7;  
  
```  
  
 Éste es el resultado parcial:  
  
```xml
<?myProcessingInstr abc="value" ?>  
<FirstLocation WorkCtrID="10" SetupHrs="0.5">  
  <!-- some comment -->  
  <?myPI some processing instructions ?>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">I  
  nsert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>.   
  </AWMI:step>  
    ...  
</FirstLocation>  
  
```  
  
## <a name="using-computed-constructors"></a>Utilizar constructores calculados  
 . En este caso, se especifican las palabras clave que identifican el tipo de nodo que se desea construir. Solo se admiten las palabras clave siguientes:  
  
-   elemento  
  
-   atributo  
  
-   text  
  
 En el caso de los nodos de elemento y atributo, estas palabras clave van seguidas del nombre del nodo y de la expresión, entre llaves, que genera el contenido del nodo. En el ejemplo siguiente, se construye este XML:  
  
```xml
<root>  
  <ProductModel PID="5">Some text <summary>Some Summary</summary></ProductModel>  
</root>  
```  
  
 Ésta es la consulta que utiliza constructores calculados para generar el XML:  
  
```sql
declare @x xml  
set @x=''  
select @x.query('element root   
               {   
                  element ProductModel  
     {  
attribute PID { 5 },  
text{"Some text "},  
    element summary { "Some Summary" }  
 }  
               } ')  
  
```  
  
 La expresión que genera el contenido del nodo puede especificar una expresión de consulta.  
  
```sql
declare @x xml  
set @x='<a attr="5"><b>some summary</b></a>'  
select @x.query('element root   
               {   
                  element ProductModel  
     {  
attribute PID { /a/@attr },  
text{"Some text "},  
    element summary { /a/b }  
 }  
               } ')  
```  
  
 Tenga en cuenta que los constructores de elementos y atributos calculados, tal como se define en la especificación de XQuery, permiten calcular los nombres de los nodos. Si utiliza constructores directos en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], se deben especificar los nombres de nodos, tales como elemento y atributo, como literales constantes. Por tanto, no existe ninguna diferencia entre los constructores directos y los calculados para los elementos y atributos.  
  
 En el ejemplo siguiente, el contenido de los nodos construidos se obtiene a partir de las instrucciones de fabricación XML almacenadas en la columna instructions del tipo de datos **XML** de la tabla ProductModel.  
  
```sql
SELECT Instructions.query('  
  declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   element FirstLocation   
     {  
        attribute LocationID { (/AWMI:root/AWMI:Location[1]/@LocationID)[1] },  
        element   AllTheSteps { /AWMI:root/AWMI:Location[1]/AWMI:step }  
     }  
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
 Éste es el resultado parcial:  
  
```xml
<FirstLocation LocationID="10">  
  <AllTheSteps>  
    <AWMI:step> ... </AWMI:step>  
    <AWMI:step> ... </AWMI:step>  
    ...  
  </AllTheSteps>  
</FirstLocation>    
```  
  
## <a name="additional-implementation-limitations"></a>Limitaciones adicionales de la implementación  
 No se pueden utilizar constructores de atributos calculados para declarar un nuevo espacio de nombres. Además, en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no se admiten los constructores calculados siguientes:  
  
-   Constructores de nodos de documentos calculados  
  
-   Constructores de instrucciones de procesamiento calculados  
  
-   Constructores de comentarios calculados  
  
## <a name="see-also"></a>Consulte también  
 [Expresiones XQuery](../xquery/xquery-expressions.md)  
  
  
