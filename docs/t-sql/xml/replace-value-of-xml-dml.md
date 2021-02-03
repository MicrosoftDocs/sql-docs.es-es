---
description: replace value of (XML DML)
title: replace value of (XML DML)
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
- update keyword
- replacement values [XML DML]
- updating node values
- replace value of XML DML statement
ms.assetid: c310f6df-7adf-493b-b56b-8e3143b13ae7
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6882fa93bb1d4837f909bc00f74fdf9e83e7c5c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174390"
---
# <a name="replace-value-of-xml-dml"></a>replace value of (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Actualiza el valor de un nodo en el documento.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
replace value of Expression1   
with Expression2  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*Expression1*  
Identifica un nodo cuyo valor se va a actualizar. Debe identificar un solo nodo. Es decir, *Expresión1* debe ser un singleton estático. Si el XML tiene un tipo, el tipo del nodo debe ser simple. Si se seleccionan varios nodos, se producirá un error. Si *Expression1* devuelve una secuencia vacía, no se reemplazará ningún valor ni se devolverá ningún error. *Expression1* debe devolver un solo elemento que incluya contenido de tipo simple (tipos de lista o atómicos), un nodo de texto o un nodo de atributo. *Expression1* no puede ser un tipo de unión, un tipo complejo, una instrucción de procesamiento, un nodo de documento ni un nodo de comentario, pues se devolvería un error.  
  
*Expression2*  
Identifica el nuevo valor del nodo. Puede ser una expresión que devuelve un nodo de tipo simple, puesto que se usará **data()** implícitamente. Si el valor es una lista de valores, la instrucción **update** reemplazará el valor antiguo por la lista. Si se modifica una instancia XML con tipo, *Expression2* debe ser del mismo tipo o un subtipo de *Expression1*. En caso contrario, se devolverá un error. Si se modifica una instancia XML sin tipo, *Expression2* debe ser una expresión que se pueda atomizar. En caso contrario, se devolverá un error.  
  
## <a name="examples"></a>Ejemplos  
Los ejemplos siguientes de la instrucción XML DML **replace value of** ilustran cómo se actualizan los nodos en un documento XML.  
  
### <a name="a-replacing-values-in-an-xml-instance"></a>A. Reemplazar valores en una instancia XML  
En el ejemplo siguiente, se asigna en primer lugar una instancia de documento a una variable de tipo **xml**. Después, las instrucciones XML DML **replace value of** actualizan los valores del documento.  
  
```sql
DECLARE @myDoc XML;  
SET @myDoc = '<Root>  
<Location LocationID="10"   
            LaborHours="1.1"  
            MachineHours=".2" >Manufacturing steps are described here.  
<step>Manufacturing step 1 at this work center</step>  
<step>Manufacturing step 2 at this work center</step>  
</Location>  
</Root>';  
SELECT @myDoc;  
  
-- update text in the first manufacturing step  
SET @myDoc.modify('  
  replace value of (/Root/Location/step[1]/text())[1]  
  with "new text describing the manu step"  
');  
SELECT @myDoc;  
-- update attribute value  
SET @myDoc.modify('  
  replace value of (/Root/Location/@LaborHours)[1]  
  with "100.0"  
');  
SELECT @myDoc;  
```  
  
El destino que se actualiza debe ser, como máximo, un nodo que se especifica de forma explícita en la expresión de ruta de acceso agregando un "[1]" al final de la expresión.  
  
### <a name="b-using-the-if-expression-to-determine-replacement-value"></a>B. Utilizar la expresión if para determinar el valor de reemplazo  
Puede especificar la expresión **if** en Expression2 de la instrucción **XML DML replace value of**, tal como se muestra en el ejemplo siguiente. Expression1 identifica que se debe actualizar el atributo LaborHours del primer centro de trabajo. Expression2 usa una expresión **if** para determinar el nuevo valor del atributo LaborHours.  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<Root>  
<Location LocationID="10"   
            LaborHours=".1"  
            MachineHours=".2" >Manu steps are described here.  
<step>Manufacturing step 1 at this work center</step>  
<step>Manufacturing step 2 at this work center</step>  
</Location>  
</Root>'  
--SELECT @myDoc  
  
SET @myDoc.modify('  
  replace value of (/Root/Location[1]/@LaborHours)[1]  
  with (  
       if (count(/Root/Location[1]/step) > 3) then  
         "3.0"  
       else  
          "1.0"  
      )  
')  
SELECT @myDoc  
```  
  
### <a name="c-updating-xml-stored-in-an-untyped-xml-column"></a>C. Actualizar XML almacenado en una columna XML sin tipo  
En el ejemplo siguiente se actualiza XML almacenado en una columna:  
  
```sql
DROP TABLE T  
GO  
CREATE TABLE T (i INT, x XML)  
GO  
INSERT INTO T VALUES(1,'<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>')  
go  
-- verify the current <ProductDescription> element  
SELECT x.query(' /Root/ProductDescription')  
FROM T  
-- update the ProductName attribute value  
UPDATE T  
SET x.modify('  
  replace value of (/Root/ProductDescription/@ProductName)[1]  
  with "New Road Bike" ')  
-- verify the update  
SELECT x.query(' /Root/ProductDescription')  
FROM T  
```  
  
### <a name="d-updating-xml-stored-in-a-typed-xml-column"></a>D. Actualizar XML almacenado en una columna XML con tipo  
En este ejemplo se reemplazan valores de un documento de instrucciones de fabricación almacenado en una columna XML con tipo.  
  
En el ejemplo, se crea en primer lugar una tabla (T) con una columna XML con tipo en la base de datos AdventureWorks. A continuación, se copia en la tabla T una instancia XML con instrucciones de fabricación procedentes de la columna Instructions de la tabla ProductModel. Después se aplican las inserciones a XML en la tabla T.  
  
```sql
USE AdventureWorks  
GO  
DROP TABLE T  
GO  
CREATE TABLE T(
  ProductModelID INT PRIMARY KEY,   
  Instructions XML (Production.ManuInstructionsSchemaCollection))  
GO  
INSERT T   
SELECT ProductModelID, Instructions  
FROM Production.ProductModel  
WHERE ProductModelID=7  
GO
--insert a new location - <Location 1000/>.   
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
insert <MI:Location LocationID="1000"  LaborHours="1000"  LotSize="1000" >  
           <MI:step>Do something using <MI:tool>hammer</MI:tool></MI:step>  
         </MI:Location>  
  as first  
  into (/MI:root)[1]  
')  
GO  
SELECT Instructions  
FROM T  
GO  
-- Now replace manu. tool in location 1000  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  replace value of (/MI:root/MI:Location/MI:step/MI:tool)[1]   
  with "screwdriver"  
')  
GO  
SELECT Instructions  
FROM T  
-- Now replace value of lot size  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  replace value of (/MI:root/MI:Location/@LotSize)[1]   
  with 500 cast as xs:decimal ?  
')  
GO  
SELECT Instructions  
FROM T  
```  
  
Tenga en cuenta el uso de **cast** al reemplazar el valor LotSize. Es necesario cuando el valor debe ser de un tipo específico. En este ejemplo, si el valor fuera 500, la conversión explícita no sería necesaria.  
  
## <a name="see-also"></a>Consulte también  
[Comparar XML con tipo y XML sin tipo](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
[Crear instancias de datos XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
[métodos del tipo de datos xml](../../t-sql/xml/xml-data-type-methods.md)   
[Lenguaje de manipulación de datos XML &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
