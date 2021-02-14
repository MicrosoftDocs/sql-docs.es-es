---
title: Coincidencia de tipos de secuencia | Microsoft Docs
description: Obtenga información acerca de cómo hacer coincidir el tipo de secuencia devuelto por una expresión XQuery con un tipo específico.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence type matching [XQuery]
- XQuery, sequence type matching
ms.assetid: 8c56fb69-ca04-4aba-b55a-64ae216c492d
author: rothja
ms.author: jroth
ms.openlocfilehash: 11c0eb693468b980db88995713dc6af99180c373
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352820"
---
# <a name="type-system---sequence-type-matching"></a>Sistema de tipo: Equiparación de tipos de secuencia
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Un valor de expresión XQuery siempre es una secuencia de cero o más elementos. Un elemento puede ser un valor atómico o un nodo. El tipo de secuencia hace referencia a la capacidad de equiparar el tipo de secuencia devuelto por una expresión de consulta a un tipo específico. Por ejemplo:  
  
-   Si el valor de la expresión es atómico, es posible que desee saber si es de tipo entero, decimal o cadena.  
  
-   Si el valor de la expresión es un nodo XML, es posible que desee saber si es un nodo de comentarios, un nodo de instrucciones de procesamiento o un nodo de texto.  
  
-   También puede saber si la expresión devuelve un elemento XML o un nodo de atributo con un nombre y tipo específicos.  
  
 Para conocer el tipo de secuencia correspondiente se puede utilizar el operador booleano `instance of`. Para obtener más información sobre la `instance of` expresión, vea [expresiones SequenceType &#40;XQuery&#41;](../xquery/sequencetype-expressions-xquery.md).  
  
## <a name="comparing-the-atomic-value-type-returned-by-an-expression"></a>Comparar el tipo de valor atómico devuelto por una expresión  
 Si una expresión devuelve una secuencia de valores atómicos, es posible que se deba buscar el tipo del valor en la secuencia. Los ejemplos siguientes muestran cómo se puede utilizar la sintaxis de tipo de secuencia para evaluar el tipo de valor atómico devuelto por una expresión.  
  
### <a name="example-determining-whether-a-sequence-is-empty"></a>Ejemplo: determinar si una secuencia está vacía  
 El tipo de secuencia **Empty ()** se puede usar en una expresión de tipo de secuencia para determinar si la secuencia devuelta por la expresión especificada es una secuencia vacía.  
  
 En el ejemplo siguiente, el esquema XML permite que el `root` elemento <> sea nillable:  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" nillable="true" type="byte"/>  
</schema>'  
GO  
```  
  
 Ahora, si una instancia XML con tipo especifica un valor para el `root` elemento <>, `instance of empty()` devuelve false.  
  
```  
DECLARE @var XML(SC1)  
SET @var = '<root>1</root>'  
-- The following returns False  
SELECT @var.query('data(/root[1]) instance of  empty() ')  
GO  
```  
  
 Si el `root` elemento <> es nillable en la instancia de, su valor es una secuencia vacía y `instance of empty()` devuelve true.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" />'  
SELECT @var.query('data(/root[1]) instance of  empty() ')  
GO  
```  
  
### <a name="example-determining-the-type-of-an-attribute-value"></a>Ejemplo: determinar el tipo de un valor de atributo  
 En ocasiones se desea evaluar el tipo de secuencia devuelto por una expresión antes de su procesamiento. Por ejemplo, puede haber un esquema XML en el que se defina un nodo como tipo de unión. En el ejemplo siguiente, el esquema XML de la colección define el atributo `a` como un tipo de unión cuyo valor puede ser de tipo decimal o de cadena.  
  
```  
-- Drop schema collection if it exists.  
-- DROP XML SCHEMA COLLECTION SC.  
-- GO  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
  <element name="root">  
    <complexType>  
       <sequence/>  
         <attribute name="a">  
            <simpleType>  
               <union memberTypes="decimal string"/>  
            </simpleType>  
         </attribute>  
     </complexType>  
  </element>  
</schema>'  
GO  
```  
  
 Antes de procesar una instancia XML con tipo, se puede conocer el tipo del valor `a` del atributo. En el ejemplo siguiente, el valor `a` del atributo es un tipo decimal. Por tanto `, instance of xs:decimal` devuelve True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root a="2.5"/>'  
SELECT @var.query('data((/root/@a)[1]) instance of xs:decimal')  
GO  
```  
  
 Ahora, cambie el valor `a` del atributo a un tipo de cadena. `instance of xs:string` devolverá True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root a="Hello"/>'  
SELECT @var.query('data((/root/@a)[1]) instance of xs:string')  
GO  
```  
  
### <a name="example-cardinality-in-sequence-expressions"></a>Ejemplo: cardinalidad en expresiones de secuencia  
 Este ejemplo ilustra el efecto de la cardinalidad en una expresión de secuencia. El siguiente esquema XML define un `root` elemento de> de <que es de tipo byte y es nillable.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" nillable="true" type="byte"/>  
</schema>'  
GO  
```  
  
 En la consulta siguiente, dado que la expresión devuelve un singleton de tipo byte, `instance of` devuelve True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root>111</root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte ')   
GO  
```  
  
 Si hace que la <`root`> elemento Nil, su valor es una secuencia vacía. En otras palabras, la expresión, `/root[1]`, devuelve una secuencia vacía. En consecuencia, `instance of xs:byte` devuelve False. Observe que, en este caso, la cardinalidad predeterminada es 1.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"></root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte ')   
GO  
-- result = false  
```  
  
 Si se especifica la cardinalidad agregando el indicador de repetición (`?`), la expresión de secuencia devuelve True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"></root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte? ')   
GO  
-- result = true  
```  
  
 Tenga en cuenta que la comprobación en una expresión de tipo de secuencia se realiza en dos etapas:  
  
1.  Primero, se determina si el tipo de la expresión coincide con el tipo especificado.  
  
2.  En caso afirmativo, la comprobación determina si el número de elementos devueltos por la expresión coincide con el indicador de repetición especificado.  
  
 Si se dan ambas condiciones, la expresión `instance of` devuelve True.  
  
### <a name="example-querying-against-an-xml-type-column"></a>Ejemplo: realizar una consulta contra una columna de tipo XML  
 En el ejemplo siguiente, se especifica una consulta en una columna instructions de tipo **XML** de la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de datos. Es una columna XML con tipo, porque tiene un esquema asociado. El esquema XML define el atributo `LocationID` del tipo entero. Por lo tanto, en la expresión de secuencia, `instance of xs:integer?` devuelve true.  
  
```  
SELECT Instructions.query('   
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
data(/AWMI:root[1]/AWMI:Location[1]/@LocationID) instance of xs:integer?') as Result   
FROM Production.ProductModel   
WHERE ProductModelID = 7  
```  
  
## <a name="comparing-the-node-type-returned-by-an-expression"></a>Comparar el tipo de nodo devuelto por una expresión  
 Si una expresión devuelve una secuencia de nodos, es posible que se deba determinar el tipo del nodo en la secuencia. Los ejemplos siguientes muestran cómo se puede utilizar la sintaxis de tipo de secuencia para evaluar el tipo de nodo devuelto por una expresión. Puede utilizar los siguientes tipos de secuencia:  
  
-   **Item ()** : coincide con cualquier elemento de la secuencia.  
  
-   **node ()** : determina si la secuencia es un nodo.  
  
-   **processing-instruction ()** : determina si la expresión devuelve una instrucción de procesamiento.  
  
-   **comment ()** : determina si la expresión devuelve un comentario.  
  
-   **Document-node ()** : determina si la expresión devuelve un nodo de documento.  
  
 El siguiente ejemplo ilustra estos tipos de secuencia.  
  
### <a name="example-using-sequence-types"></a>Ejemplo: utilizar los tipos de secuencia  
 En este ejemplo, se ejecutan varias consultas en una variable XML sin tipo. Estas consultas ilustran el uso de tipos de secuencia.  
  
```  
DECLARE @var XML  
SET @var = '<?xml-stylesheet href="someValue" type="text/xsl" ?>  
<root>text node  
  <!-- comment 1 -->   
  <a>Data a</a>  
  <!-- comment  2 -->  
</root>'  
```  
  
 En la primera consulta, la expresión devuelve el valor con tipo del elemento <`a`>. En la segunda consulta, la expresión devuelve el elemento <`a`>. Ambos son elementos. Por tanto, ambas consultas devuelven True.  
  
```  
SELECT @var.query('data(/root[1]/a[1]) instance of item()')  
SELECT @var.query('/root[1]/a[1] instance of item()')  
```  
  
 Todas las expresiones XQuery en las tres consultas siguientes devuelven el nodo de elemento secundario del `root` elemento <>. Por tanto, la expresión de tipo de secuencia (`instance of node()`) devuelve True y las otras dos expresiones (`instance of text()` y `instance of document-node()`) devuelven False.  
  
```  
SELECT @var.query('(/root/*)[1] instance of node()')  
SELECT @var.query('(/root/*)[1] instance of text()')  
SELECT @var.query('(/root/*)[1] instance of document-node()')   
```  
  
 En la consulta siguiente, la `instance of document-node()` expresión devuelve true, porque el elemento primario del `root` elemento <> es un nodo de documento.  
  
```  
SELECT @var.query('(/root/..)[1] instance of document-node()') -- true  
```  
  
 En la consulta siguiente, la expresión recupera el primer nodo de la instancia XML. Debido a que es un nodo de instrucciones de procesamiento, la expresión `instance of processing-instruction()` devuelve True.  
  
```  
SELECT @var.query('(/node())[1] instance of processing-instruction()')  
```  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones específicas:  
  
-   no se admite **el nodo de documento () con la** sintaxis de tipo de contenido.  
  
-   no se admite **la sintaxis de processing-instruction (Name)** .  
  
## <a name="element-tests"></a>Pruebas de elementos  
 Una prueba de elementos tiene como finalidad equiparar el nodo de elemento devuelto por una expresión a un nodo de elemento con un nombre y tipo específicos. Se pueden utilizar estas pruebas de elementos:  
  
```  
element ()  
element(ElementName)  
element(ElementName, ElementType?)   
element(*, ElementType?)  
```  
  
## <a name="attribute-tests"></a>Pruebas de atributos  
 La prueba de atributos determina si el atributo devuelto por una expresión es un nodo de atributo. Se pueden utilizar estas pruebas de atributos.  
  
 `attribute()`  
  
 `attribute(AttributeName)`  
  
 `attribute(AttributeName, AttributeType)`  
  
## <a name="test-examples"></a>Ejemplos de pruebas  
 Los ejemplos siguientes ilustran situaciones en las que son útiles las pruebas de elementos y atributos.  
  
### <a name="example-a"></a>Ejemplo A  
 El siguiente esquema XML define el `CustomerType` tipo complejo donde <`firstName` elementos> y <`lastName`> son opcionales. Puede que sea necesario determinar, para una instancia XML específica, si existe un nombre para un cliente concreto.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="myNS" xmlns:ns="myNS">  
  <complexType name="CustomerType">  
     <sequence>  
        <element name="firstName" type="string" minOccurs="0"   
                  nillable="true" />  
        <element name="lastName" type="string" minOccurs="0"/>  
     </sequence>  
  </complexType>  
  <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
DECLARE @var XML(SC)  
SET @var = '<x:customer xmlns:x="myNS">  
<firstName>SomeFirstName</firstName>  
<lastName>SomeLastName</lastName>  
</x:customer>'  
```  
  
 La consulta siguiente utiliza una `instance of element (firstName)` expresión para determinar si el primer elemento secundario de <`customer`> es un elemento cuyo nombre es <`firstName`>. En este caso, devuelve True.  
  
```  
SELECT @var.query('declare namespace x="myNS";   
     (/x:customer/*)[1] instance of element (firstName)')  
GO  
```  
  
 Si quita el elemento <`firstName`> de la instancia, la consulta devolverá FALSE.  
  
 También se puede utilizar lo siguiente:  
  
-   La sintaxis de tipo de secuencia `element(ElementName, ElementType?)`, como se muestra en la consulta siguiente. Busca un nodo de elemento nillable o no nillable coincidente cuyo nombre sea `firstName` y cuyo tipo sea `xs:string`.  
  
    ```  
    SELECT @var.query('declare namespace x="myNS";   
    (/x:customer/*)[1] instance of element (firstName, xs:string?)')  
    ```  
  
-   La sintaxis de tipo de secuencia `element(*, type?)`, como se muestra en la consulta siguiente. Busca un nodo de elemento coincidente cuyo tipo sea `xs:string`, sin importar su nombre.  
  
    ```  
    SELECT @var.query('declare namespace x="myNS"; (/x:customer/*)[1] instance of element (*, xs:string?)')  
    GO  
    ```  
  
### <a name="example-b"></a>Ejemplo B  
 En el ejemplo siguiente se muestra cómo determinar si el nodo devuelto por una expresión es un nodo de elemento con un nombre específico. Usa la prueba **Element ()** .  
  
 En el ejemplo siguiente, los dos <`Customer`> elementos de la instancia XML que se consultan son de dos tipos diferentes, `CustomerType` y `SpecialCustomerType` . Suponga que desea conocer el tipo de <`Customer` elemento> devuelto por la expresión. En la siguiente colección de esquemas XML se definen los tipos `CustomerType` y `SpecialCustomerType`.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
          targetNamespace="myNS"  xmlns:ns="myNS">  
  <complexType name="CustomerType">  
    <sequence>  
      <element name="firstName" type="string"/>  
      <element name="lastName" type="string"/>  
    </sequence>  
  </complexType>  
  <complexType name="SpecialCustomerType">  
     <complexContent>  
       <extension base="ns:CustomerType">  
        <sequence>  
            <element name="Age" type="int"/>  
        </sequence>  
       </extension>  
     </complexContent>  
    </complexType>  
   <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
```  
  
 Esta colección de esquemas XML se usa para crear una variable **XML** con tipo. La instancia XML asignada a esta variable tiene dos <`customer`> elementos de dos tipos diferentes. El primer elemento es de tipo `CustomerType` y el segundo elemento es de tipo `SpecialCustomerType`.  
  
```  
DECLARE @var XML(SC)  
SET @var = '  
<x:customer xmlns:x="myNS">  
   <firstName>FirstName1</firstName>  
   <lastName>LastName1</lastName>  
</x:customer>  
<x:customer xsi:type="x:SpecialCustomerType" xmlns:x="myNS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
   <firstName> FirstName2</firstName>  
   <lastName> LastName2</lastName>  
   <Age>21</Age>  
</x:customer>'  
```  
  
 En la consulta siguiente, la expresión `instance of element (*, x:SpecialCustomerType ?)` devuelve False, porque la expresión devuelve el primer elemento de cliente que no es de tipo `SpecialCustomerType`.  
  
```  
SELECT @var.query('declare namespace x="myNS";   
    (/x:customer)[1] instance of element (*, x:SpecialCustomerType ?)')  
```  
  
 Si cambia la expresión de la consulta anterior y recupera el segundo <`customer` elemento> ( `/x:customer)[2]` ), `instance of` devolverá True.  
  
### <a name="example-c"></a>Ejemplo C  
 En este ejemplo se utiliza la prueba de atributos. El siguiente esquema XML define el tipo complejo CustomerType, con los atributos CustomerID y Age. El atributo Age es opcional. Para una instancia XML específica, puede que desee determinar si el atributo Age está presente en el elemento <`customer`>.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
       targetNamespace="myNS" xmlns:ns="myNS">  
<complexType name="CustomerType">  
  <sequence>  
     <element name="firstName" type="string" minOccurs="0"   
               nillable="true" />  
     <element name="lastName" type="string" minOccurs="0"/>  
  </sequence>  
  <attribute name="CustomerID" type="integer" use="required" />  
  <attribute name="Age" type="integer" use="optional" />  
 </complexType>  
 <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
```  
  
 La consulta siguiente devuelve True porque hay un nodo de atributo cuyo nombre es `Age` en la instancia XML que se consulta. En esta expresión se utiliza la prueba de atributos `attribute(Age)`. Como los atributos no están ordenados, la consulta utiliza la expresión FLWOR para recuperar todos los atributos y, después, comprobar cada atributo mediante la expresión `instance of`. En primer lugar, el ejemplo crea una colección de esquemas XML para crear una variable **XML** con tipo.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<x:customer xmlns:x="myNS" CustomerID="1" Age="22" >  
<firstName>SomeFName</firstName>  
<lastName>SomeLName</lastName>  
</x:customer>'  
SELECT @var.query('declare namespace x="myNS";   
FOR $i in /x:customer/@*  
RETURN  
    IF ($i instance of attribute (Age)) THEN  
        "true"  
        ELSE  
        ()')     
GO  
  
```  
  
 Si se quita el atributo opcional `Age` de la instancia, la consulta anterior devuelve False.  
  
 En la prueba de atributos se puede especificar el nombre y tipo del atributo (`attribute(name,type)`).  
  
```  
SELECT @var.query('declare namespace x="myNS";   
FOR $i in /x:customer/@*  
RETURN  
    IF ($i instance of attribute (Age, xs:integer)) THEN  
        "true"  
        ELSE  
        ()')  
```  
  
 También puede especificar la `attribute(*, type)` Sintaxis del tipo de secuencia. De esta manera se buscará una coincidencia para el nodo de atributo, si el tipo de atributo coincide con el tipo especificado, sea cual sea su nombre.  
  
### <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones específicas:  
  
-   En la prueba del elemento, el nombre del tipo debe ir seguido del indicador de repetición (**?**).  
  
-   no se admite el **elemento (ElementName, TypeName)** .  
  
-   no se admite el **elemento ( \* , TypeName)** .  
  
-   **Schema-Element ()** no se admite.  
  
-   no se admite **el atributo de esquema (attributeName)** .  
  
-   No se admite la consulta explícita de **xsi: Type** o **xsi: nil** .  
  
## <a name="see-also"></a>Consulte también  
 [Sistema de tipos &#40;XQuery&#41;](../xquery/type-system-xquery.md)  
  
  
