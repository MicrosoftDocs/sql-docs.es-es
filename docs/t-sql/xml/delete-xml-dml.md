---
description: delete (XML DML)
title: delete (XML DML)
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
- delete keyword
- delete statement [XML DML]
- deleting nodes
ms.assetid: b22c93a4-b84d-4356-af4c-6013322a4b71
author: MightyPen
ms.author: genemi
ms.openlocfilehash: e3f06338515bbd8ad426606a4331c5fc19a1b0ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161061"
---
# <a name="delete-xml-dml"></a>delete (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Elimina nodos de una instancia XML.  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
delete Expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *Expression*  
 Es una expresión XQuery que identifica los nodos que se deben eliminar. Se eliminarán todos los nodos seleccionados por la expresión y los nodos o valores incluidos en los mismos. Tal y como se describe en [insert (XML DML)](../../t-sql/xml/insert-xml-dml.md), debe ser una referencia a un nodo existente en el documento. No puede ser un nodo construido. La expresión no puede ser el nodo raíz (/). Si la expresión devuelve una secuencia vacía, no se produce ninguna eliminación y no se devuelven errores.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-deleting-nodes-from-a-document-stored-in-an-untyped-xml-variable"></a>A. Eliminar nodos de un documento almacenado en una variable xml sin tipo  
 En el ejemplo siguiente se muestra cómo eliminar varios nodos de un documento. En primer lugar, se asigna una instancia XML a una variable de tipo **xml**. A continuación, las instrucciones delete XML DML posteriores eliminan varios nodos del documento.  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<?Instructions for=TheWC.exe ?>   
<Root>  
 <!-- instructions for the 1st work center -->  
<Location LocationID="10"   
            LaborHours="1.1"  
            MachineHours=".2" >Some text 1  
<step>Manufacturing step 1 at this work center</step>  
<step>Manufacturing step 2 at this work center</step>  
</Location>  
</Root>'  
SELECT @myDoc  
  
-- delete an attribute  
SET @myDoc.modify('  
  delete /Root/Location/@MachineHours  
')  
SELECT @myDoc  
  
-- delete an element  
SET @myDoc.modify('  
  delete /Root/Location/step[2]  
')  
SELECT @myDoc  
  
-- delete text node (in <Location>  
SET @myDoc.modify('  
  delete /Root/Location/text()  
')  
SELECT @myDoc  
  
-- delete all processing instructions  
SET @myDoc.modify('  
  delete //processing-instruction()  
')  
SELECT @myDoc  
```  
  
### <a name="b-deleting-nodes-from-a-document-stored-in-an-untyped-xml-column"></a>B. Eliminar nodos de un documento almacenado en una columna xml sin tipo  
 En el siguiente ejemplo, una instrucción XML DML **delete** elimina el segundo elemento secundario de <`Features`> del documento almacenado en la columna.  
  
```sql
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
GO
-- verify the contents before delete  
SELECT x.query(' //ProductDescription/Features')  
FROM T  
-- delete the second feature  
UPDATE T  
SET x.modify('delete /Root/ProductDescription/Features/*[2]')  
-- verify the deletion  
SELECT x.query(' //ProductDescription/Features')  
FROM T  
```  
  
 Observe lo siguiente en la consulta anterior:  
  
-   Se usa el [método modify() del tipo de datos xml](../../t-sql/xml/modify-method-xml-data-type.md) para especificar la palabra clave XML DML **delete**.  
  
-   Se usa el [método query() del tipo de datos xml](../../t-sql/xml/query-method-xml-data-type.md) para consultar el documento.  
  
### <a name="c-deleting-nodes-from-a-typed-xml-column"></a>C. Eliminar nodos de una columna xml con tipo  
 En este ejemplo se eliminan nodos de un documento XML con instrucciones de fabricación almacenado en una columna **xml** con tipo.  
  
 En el ejemplo, se crea en primer lugar una tabla (T) con una columna **xml** con tipo en la base de datos AdventureWorks. A continuación, se copia una instancia XML con instrucciones de fabricación de la columna Instructions de la tabla ProductModel en la tabla T y se eliminan uno o varios nodos del documento.  
  
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
WHERE ProductModelID = 7  
GO  
SELECT Instructions  
FROM T  
--1) insert <Location 1000/>. Note: <Root> must be singleton in the query  
UPDATE T  
SET Instructions.modify('  
  DECLARE namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  INSERT <MI:Location LocationID="1000"  LaborHours="1000" >  
           These are manu steps at location 1000.   
           <MI:step>New step1 instructions</MI:step>  
           Instructions for step 2 are here  
           <MI:step>New step 2 instructions</MI:step>  
         </MI:Location>  
  AS first  
  INTO   (/MI:root)[1]  
')  
GO 
SELECT Instructions  
FROM T  
  
-- delete an attribute  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  delete(/MI:root/MI:Location[@LocationID=1000]/@LaborHours)   
')  
GO  
SELECT Instructions  
FROM T  
-- delete text in <location>  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  delete(/MI:root/MI:Location[@LocationID=1000]/text())   
')  
GO  
SET Instructions  
FROM T  
-- delete 2nd manu step at location 1000  
UPDATE T  
SET Instructions.modify('  
  declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  delete(/MI:root/MI:Location[@LocationID=1000]/MI:step[2])   
')  
GO  
SELECT Instructions  
FROM T  
-- cleanup  
DROP TABLE T  
GO 
```  
  
## <a name="see-also"></a>Consulte también  
 [Comparar XML con tipo y XML sin tipo](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Crear instancias de datos XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [métodos del tipo de datos xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Lenguaje de manipulación de datos XML &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
