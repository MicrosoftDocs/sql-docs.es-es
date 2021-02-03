---
description: insert (XML DML)
title: insert (XML DML)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- XML DML [SQL Server]
- inserting nodes
- insert keyword [XML DML]
- insert XML DML statement
ms.assetid: 0c95c2b3-5cc2-4c38-9e25-86493096c442
author: MightyPen
ms.author: genemi
ms.openlocfilehash: d5eae7b06bb10c356bbb1c3975b05dd7cd267708
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199342"
---
# <a name="insert-xml-dml"></a>insert (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Inserta uno o más nodos identificados por *Expression1* como nodos secundarios o del mismo nivel que el nodo identificado por *Expression2*.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql 
insert Expression1 (  
{AS first | AS last} INTO | AFTER | BEFORE  
Expression2  
)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *Expression1*  
 Identifica uno o varios nodos que se van a insertar. Puede ser una instancia XML constante; una referencia a una instancia del tipo de datos XML con tipo de la misma colección de esquemas XML en la que se aplica el método modify; una instancia del tipo de datos XML sin tipo que usa una función **sql:column()** /**sql:variable()** independiente; o una expresión XQuery. La expresión puede proporcionar un nodo, y también un nodo de texto, o una secuencia ordenada de nodos. No se puede resolver en el nodo raíz (/). Si la expresión da como resultado un valor o una secuencia de valores, los valores se insertan como un solo nodo de texto y cada valor de la secuencia se separa con un espacio. Si se especifican varios nodos como constante, los nodos se incluyen entre paréntesis y se separan mediante comas. No es posible insertar secuencias heterogéneas, como una secuencia de elementos, atributos o valores. Si *Expression1* se resuelve en una secuencia vacía, no se produce ninguna inserción ni se devuelven errores.  
  
 into  
 Los nodos identificados por *Expression1* se insertan como descendientes directos (nodos secundarios) del nodo identificado por *Expression2*. Si el nodo de *Expression2* ya tiene uno o más nodos secundarios, se debe usar **as first** o **as last** para especificar dónde se quiere agregar el nuevo nodo. Se agregaría al principio o al final de la lista de nodos secundarios respectivamente. Las palabras clave **as first** y **as last** se omiten cuando se insertan atributos.  
  
 after  
 Los nodos identificados por *Expression1* se insertan como nodos del mismo nivel justo después del nodo identificado por *Expression2*. La palabra clave **after** no se puede usar para insertar atributos. Por ejemplo, no se puede utilizar para insertar un constructor de atributos o devolver un atributo desde una consulta XQuery.  
  
 before  
 Los nodos identificados por *Expression1* se insertan como nodos del mismo nivel justo antes del nodo identificado por *Expression2*. La palabra clave **before** no se puede usar cuando se insertan atributos. Por ejemplo, no se puede utilizar para insertar un constructor de atributos o devolver un atributo desde una consulta XQuery.  
  
 *Expression2*  
 Identifica un nodo. Los nodos identificados en *Expression1* se insertan con respecto al nodo identificado por *Expression2*. Puede ser una expresión XQuery que devuelva una referencia a un nodo que exista en el documento al que se hace referencia actualmente. Si se devuelve más de un nodo, se produce un error en la operación de inserción. Si *Expression2* devuelve una secuencia vacía, no se produce ninguna inserción ni se devuelven errores. Si *Expression2* no es un singleton de manera estática, se devuelve un error estático. *Expression2* no puede ser una instrucción de procesamiento, un comentario ni un atributo. Tenga en cuenta que *Expression2* debe ser una referencia a un nodo existente en el documento y no a un nodo construido.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-inserting-element-nodes-into-the-document"></a>A. Insertar nodos de elemento en el documento  
 En el siguiente ejemplo se muestra cómo insertar elementos en un documento. Primero, se asigna un documento XML a una variable de tipo **xml**. Después, por medio de varias instrucciones **insert** XML DML, se muestra cómo se insertan nodos de elemento en el documento. Después de cada inserción, la instrucción SELECT muestra el resultado.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;         
SET @myDoc = '<Root>         
    <ProductDescription ProductID="1" ProductName="Road Bike">         
        <Features>         
        </Features>         
    </ProductDescription>         
</Root>'  ;       
SELECT @myDoc;     
-- insert first feature child (no need to specify as first or as last)         
SET @myDoc.modify('         
INSERT <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>   
INTO (/Root/ProductDescription/Features)[1]') ;  
SELECT @myDoc ;        
-- insert second feature. We want this to be the first in sequence so use 'as first'         
SET @myDoc.modify('         
INSERT <Warranty>1 year parts and labor</Warranty>          
AS first         
INTO (/Root/ProductDescription/Features)[1]         
')  ;       
SELECT @myDoc  ;       
-- insert third feature child. This one is the last child of <Features> so use 'as last'         
SELECT @myDoc         
SET @myDoc.modify('         
INSERT <Material>Aluminium</Material>          
AS last         
INTO (/Root/ProductDescription/Features)[1]         
')         
SELECT @myDoc ;        
-- Add fourth feature - this time as a sibling (and not a child)         
-- 'after' keyword is used (instead of as first or as last child)         
SELECT @myDoc  ;       
SET @myDoc.modify('         
INSERT <BikeFrame>Strong long lasting</BikeFrame>   
AFTER (/Root/ProductDescription/Features/Material)[1]         
')  ;       
SELECT @myDoc;  
GO  
```  
  
 Tenga en cuenta que diversas expresiones de ruta de acceso de este ejemplo especifican "[1]" como requisito para los tipos estáticos. De esta manera se garantiza que hay un único nodo de destino.  
  
### <a name="b-inserting-multiple-elements-into-the-document"></a>B. Insertar varios elementos en el documento  
 En el ejemplo siguiente, primero se asigna un documento a una variable de tipo **xml**. Luego se asigna una secuencia de dos elementos que representan las características del producto a una segunda variable de tipo **xml**. Después, esta secuencia se inserta en la primera variable.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc = N'<Root>             
<ProductDescription ProductID="1" ProductName="Road Bike">             
    <Features> </Features>             
</ProductDescription>             
</Root>';  
DECLARE @newFeatures xml;  
SET @newFeatures = N'<Warranty>1 year parts and labor</Warranty>            
<Maintenance>3 year parts and labor extended maintenance is available</Maintenance>';           
-- insert new features from specified variable            
SET @myDoc.modify('             
insert sql:variable("@newFeatures")             
into (/Root/ProductDescription/Features)[1] ')             
SELECT @myDoc;  
GO  
```  
  
### <a name="c-inserting-attributes-into-a-document"></a>C. Insertar atributos en un documento  
 En el ejemplo siguiente se muestra cómo se insertan los atributos en un documento. Primero, se asigna un documento a una variable de tipo **xml**. Después, se usa una serie de instrucciones **insert** XML DML para insertar atributos en el documento. Después de cada inserción de atributo, la instrucción SELECT muestra el resultado.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;            
SET @myDoc =   
'<Root>             
    <Location LocationID="10" >             
        <step>Manufacturing step 1 at this work center</step>             
        <step>Manufacturing step 2 at this work center</step>             
    </Location>             
</Root>' ;  
SELECT @myDoc;          
-- insert LaborHours attribute             
SET @myDoc.modify('             
insert attribute LaborHours {".5" }             
into (/Root/Location[@LocationID=10])[1] ');           
SELECT @myDoc;          
-- insert MachineHours attribute but its value is retrived from a sql variable @Hrs             
DECLARE @Hrs FLOAT;            
SET @Hrs =.2;          
SET @myDoc.modify('             
insert attribute MachineHours {sql:variable("@Hrs") }             
into   (/Root/Location[@LocationID=10])[1] ');            
SELECT @myDoc;             
-- insert sequence of attribute nodes (note the use of ',' and ()              
-- around the attributes.             
SET @myDoc.modify('             
insert (              
           attribute SetupHours {".5" },             
           attribute SomeOtherAtt {".2"}             
        )             
INTO (/Root/Location[@LocationID=10])[1] ');             
SELECT @myDoc;  
GO  
```  
  
### <a name="d-inserting-a-comment-node"></a>D. Insertar un nodo de comentario  
 En esta consulta primero se asigna un documento XML a una variable de tipo **xml**. Después, se utiliza XML DML para insertar un nodo de comentario detrás del primer elemento <`step`>.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;             
SET @myDoc =   
'<Root>             
    <Location LocationID="10" >             
        <step>Manufacturing step 1 at this work center</step>             
        <step>Manufacturing step 2 at this work center</step>             
    </Location>             
</Root>' ;           
SELECT @myDoc;             
SET @myDoc.modify('             
insert <!-- some comment -->             
after (/Root/Location[@LocationID=10]/step[1])[1] ');            
SELECT @myDoc;  
GO  
```  
  
### <a name="e-inserting-a-processing-instruction"></a>E. Insertar una instrucción de procesamiento  
 En la consulta siguiente primero se asigna un documento XML a una variable de tipo **xml**. Después, se utiliza la palabra clave XML DML para insertar una instrucción de procesamiento al principio del documento.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>   
    <Location LocationID="10" >   
        <step>Manufacturing step 1 at this work center</step>   
        <step>Manufacturing step 2 at this work center</step>   
    </Location>   
</Root>' ;  
SELECT @myDoc ;  
SET @myDoc.modify('   
insert <?Program = "Instructions.exe" ?>   
before (/Root)[1] ') ;  
SELECT @myDoc ;  
GO  
```  
  
### <a name="f-inserting-data-using-a-cdata-section"></a>F. Insertar datos mediante una sección CDATA  
 Cuando se inserta texto que incluye caracteres que no son válidos en XML, como < o >, se pueden utilizar secciones CDATA para insertar los datos, como se muestra en la consulta siguiente. La consulta especifica una sección CDATA, pero se agrega como un nodo de texto con los caracteres no válidos convertidos en entidades. Por ejemplo, `<` se guarda como `&lt;`.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;             
SET @myDoc =   
'<Root>             
    <ProductDescription ProductID="1" ProductName="Road Bike">             
        <Features> </Features>             
    </ProductDescription>             
</Root>' ;            
SELECT @myDoc ;            
SET @myDoc.modify('             
insert <![CDATA[ <notxml> as text </notxml> or cdata ]]>   
into  (/Root/ProductDescription/Features)[1] ') ;   
SELECT @myDoc ;  
GO  
```  
  
 La consulta inserta un nodo de texto en el elemento <`Features`>:  
  
```  
<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features> &lt;notxml@gt; as text &lt;/notxml&gt; or cdata </Features>  
</ProductDescription>  
</Root>       
```  
  
### <a name="g-inserting-text-node"></a>G. Insertar un nodo de texto  
 En esta consulta primero se asigna un documento XML a una variable de tipo **xml**. Después, se utiliza XML DML para insertar un nodo de texto como el primer elemento secundario del elemento <`Root`>. Para especificar el texto se utiliza el constructor de texto.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  
</Features>  
</ProductDescription>  
</Root>'  
SELECT @myDoc;  
SET @myDoc.modify('  
 insert text{"Product Catalog Description"}   
 as first into (/Root)[1]  
');  
SELECT @myDoc;  
```  
  
### <a name="h-inserting-a-new-element-into-an-untyped-xml-column"></a>H. Insertar un nuevo elemento en una columna xml sin tipo  
 En el ejemplo siguiente se usa XML DML para actualizar una instancia XML almacenada en una columna de tipo **xml**:  
  
```sql
USE AdventureWorks;  
GO  
CREATE TABLE T (i INT, x XML);  
GO  
INSERT INTO T VALUES(1,'<Root>  
    <ProductDescription ProductID="1" ProductName="Road Bike">  
        <Features>  
            <Warranty>1 year parts and labor</Warranty>  
            <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
        </Features>  
    </ProductDescription>  
</Root>');  
GO  
-- insert a new element  
UPDATE T  
SET x.modify('insert <Material>Aluminium</Material> as first  
  into   (/Root/ProductDescription/Features)[1]  
');  
GO  
```  
  
 De nuevo, cuando se inserta el nodo de elemento <`Material`>, la expresión de ruta de acceso debe devolver un único destino. Esto se especifica explícitamente al agregar [1] al final de la expresión.  
  
```sql
-- check the update  
SELECT x.query(' //ProductDescription/Features')  
FROM T;  
GO  
```  
  
### <a name="i-inserting-based-on-an-if-condition-statement"></a>I. Inserción basada en una instrucción de condición IF  
 En el ejemplo siguiente se especifica una condición IF como parte de Expression1 en la instrucción **insert** XML DML. Si la condición es True, se agrega un atributo al elemento <`WorkCenter`>.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>  
    <Location LocationID="10" LaborHours="1.2" >  
        <step>Manufacturing step 1 at this work center</step>  
    <step>Manufacturing step 2 at this work center</step>  
    </Location>  
</Root>';  
SELECT @myDoc  
SET @myDoc.modify('  
insert  
if (/Root/Location[@LocationID=10])  
then attribute MachineHours {".5"}  
else ()  
    as first into   (/Root/Location[@LocationID=10])[1] ');  
SELECT @myDoc;  
GO  
```  
  
 El ejemplo siguiente es similar, con la diferencia de que la instrucción **insert** XML DML inserta un elemento en el documento si la condición es True. En otras palabras, si el elemento <`WorkCenter`> tiene dos elementos secundarios <`step`> o menos.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>  
    <Location LocationID="10" LaborHours="1.2" >  
        <step>Manufacturing step 1 at this work center</step>  
        <step>Manufacturing step 2 at this work center</step>  
    </Location>  
</Root>';  
SELECT @myDoc;  
SET @myDoc.modify('  
insert  
if (count(/Root/Location/step) <= 2)  
then element step { "This is a new step" }  
else ()  
    as last into   (/Root/Location[@LocationID=10])[1] ');  
SELECT @myDoc;  
GO  
```  
  
 El resultado es el siguiente:  
  
```  
<Root>  
 <WorkCenter WorkCenterID="10" LaborHours="1.2">  
  <step>Manufacturing step 1 at this work center</step>  
  <step>Manufacturing step 2 at this work center</step>  
  <step>This is a new step</step>  
 </WorkCenter>  
```  
  
### <a name="j-inserting-nodes-in-a-typed-xml-column"></a>J. Insertar nodos en una columna xml con tipo  
 En este ejemplo se insertan un elemento y un atributo en un XML de instrucciones de fabricación almacenado en una columna **xml** con tipo.  
  
 En el ejemplo, primero se crea una tabla (T) con una columna **xml** con tipo en la base de datos AdventureWorks. A continuación, se copia en la tabla T una instancia XML con instrucciones de fabricación procedentes de la columna Instructions de la tabla ProductModel. Después se aplican las inserciones a XML en la tabla T.  
  
```sql
USE AdventureWorks;  
GO            
DROP TABLE T;  
GO             
CREATE TABLE T(
  ProductModelID INT PRIMARY KEY,    
  Instructions XML (Production.ManuInstructionsSchemaCollection));  
GO  
INSERT T              
    SELECT ProductModelID, Instructions             
    FROM Production.ProductModel             
    WHERE ProductModelID=7;  
GO             
SELECT Instructions             
FROM T;  
-- now insertion begins             
--1) insert a new manu. Location. The <Root> specified as              
-- expression 2 in the insert() must be singleton.      
UPDATE T   
SET Instructions.modify('   
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
insert <MI:Location LocationID="1000" >   
           <MI:step>New instructions go here</MI:step>   
         </MI:Location>   
as first   
into   (/MI:root)[1]   
') ;  
  
SELECT Instructions             
FROM T ;  
-- 2) insert attributes in the new <Location>             
UPDATE T             
SET Instructions.modify('             
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";             
insert attribute LaborHours { "1000" }             
into (/MI:root/MI:Location[@LocationID=1000])[1] ');   
GO             
SELECT Instructions             
FROM T ;  
GO             
--cleanup             
DROP TABLE T ;  
GO             
```  
  
## <a name="see-also"></a>Consulte también  
 [Comparar XML con tipo y XML sin tipo](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Crear instancias de datos XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [métodos del tipo de datos xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Lenguaje de manipulación de datos XML &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
