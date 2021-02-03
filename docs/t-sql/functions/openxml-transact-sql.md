---
description: OPENXML (Transact-SQL)
title: OPENXML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENXML_TSQL
- OPENXML
dev_langs:
- TSQL
helpviewer_keywords:
- OPENXML statement
- rowsets [SQL Server], XML documents
- XML [SQL Server], rowset views
ms.assetid: 8088b114-7d01-435a-8e0d-b81abacc86d6
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 6a37cdab9cc3adda4043cd7e849a4d743eb9779f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183983"
---
# <a name="openxml-transact-sql"></a>OPENXML (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  OPENXML proporciona una vista de un conjunto de filas en un documento XML. Puesto que OPENXML es un proveedor de conjuntos de filas, puede utilizarse en instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] en las que pueden aparecer proveedores de conjuntos de filas como una tabla, vista o la función OPENROWSET.  
  
 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
OPENXML( idoc int [ in] , rowpattern nvarchar [ in ] , [ flags byte [ in ] ] )   
[ WITH ( SchemaDeclaration | TableName ) ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *idoc*  
 Es el identificador del documento de la representación interna de un documento XML. La representación interna de un documento XML se crea mediante una llamada a **sp_xml_preparedocument**.  
  
 *rowpattern*  
 El patrón XPath se usa para identificar los nodos con el fin de que se procesen como filas. Los nodos proceden del documento XML cuyo manipulador se pasa en el parámetro *idoc*.
  
 *flags*  
 Indica la asignación que debe utilizarse entre los datos XML y el conjunto de filas relacional, y cómo debe llenarse la columna de desbordamiento; *flags* es un parámetro de entrada opcional y puede tomar uno de los valores siguientes.  
  
|Valor del byte|Descripción|  
|----------------|-----------------|  
|**0**|Establece como valor predeterminado la asignación **centrada en atributos**.|  
|**1**|Usa la asignación **centrada en atributos**. Se puede combinar con XML_ELEMENTS. En este caso, la asignación **centrada en atributos** se aplica primero. A continuación, la asignación **centrada en elementos** se aplica en las columnas restantes.|  
|**2**|Usa la asignación **centrada en elementos**. Se puede combinar con XML_ATTRIBUTES. En este caso, la asignación **centrada en atributos** se aplica primero. A continuación, la asignación **centrada en elementos** se aplica en las columnas restantes.|  
|**8**|Puede combinarse (OR lógico) con XML_ATTRIBUTES o XML_ELEMENTS. Si se trata de una recuperación, esta marca informa de que los datos consumidos no se deberían copiar a la propiedad de desbordamiento **\@mp:xmltext**.|  
  
 _SchemaDeclaration_  
 Es la definición de esquema de la forma: _ColName_*ColType* [_ColPattern_ | _MetaProperty_] [**,**_ColNameColType_ [_ColPattern_ | _MetaProperty_]...]  
  
 _ColName_  
 Es el nombre de columna en el conjunto de filas.  
  
 *ColType*  
 Es el tipo de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la columna en el conjunto de filas. Si los tipos de columna son distintos del tipo de datos **xml** subyacente del atributo, se producirá una conversión de tipos.  
  
 *ColPattern*  
 Es un parámetro opcional, un patrón XPath general que describe la forma de asignar los nodos XML a las columnas. Si no se especifica *ColPattern*, se realiza la asignación predeterminada (asignación **centrada en atributos** o **centrada en elementos** especificada en los valores *flags*).  
  
 El patrón XPath especificado como *ColPattern* se usa para especificar la naturaleza especial de la asignación (en caso de una asignación **centrada en atributos** y **centrada en elementos**) que sobrescribe o mejora la asignación predeterminada especificada por *flags*.  
  
 El patrón XPath general especificado como *ColPattern* también admite las metapropiedades.  
  
 *MetaProperty*  
 Es una de las metapropiedades que proporciona OPENXML. Si se especifica *MetaProperty*, la columna contiene información proporcionada por la metapropiedad. Las metapropiedades permiten extraer información (como información sobre el espacio de nombres y la posición relativa) acerca de nodos XML. Estas metapropiedades proporcionan más información que la que se puede ver en la representación de texto.  
  
 *TableName*  
 Es el nombre de tabla que puede proporcionarse (en lugar de *SchemaDeclaration*) si ya existe una tabla con el esquema deseado y no se requieren patrones de columna.  
  
## <a name="remarks"></a>Comentarios  
 La cláusula WITH proporciona un formato de conjunto de filas (e información de asignación adicional si es necesario) mediante *SchemaDeclaration* o la especificación de un *TableName* existente. Si no se especifica la cláusula opcional WITH, los resultados se devuelven en un formato de tabla **irregular**. Las tablas irregulares representan la estructura refinada del documento XML (por ejemplo, los nombres de elementos o atributos, la jerarquía del documento, los espacios de nombre, las instrucciones de proceso, etc.) en una única tabla.  
  
 En la tabla siguiente se describe la estructura de la tabla **irregular**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**bigint**|Es el id. único del nodo del documento.<br /><br /> El elemento raíz tiene un valor de identificador 0. Los valores de identificador negativos están reservados.|  
|**parentid**|**bigint**|Identifica el elemento primario del nodo. El elemento primario especificado por este identificador no es necesariamente el elemento primario, pero depende del NodeType del nodo cuyo elemento primario indica este identificador. Por ejemplo, si se trata de un nodo de texto, su elemento primario puede ser un nodo de atributo.<br /><br /> Si el nodo está en el nivel superior del documento XML, su **ParentID** es NULL.|  
|**nodetype**|**int**|Identifica el tipo de nodo. Es un entero que se corresponde con la numeración del tipo de nodo XML DOM.<br /><br /> Los tipos de nodo son:<br /><br /> 1 = Nodo de elemento<br /><br /> 2 = Nodo de atributo<br /><br /> 3 = Nodo de texto|  
|**localname**|**nvarchar**|Proporciona el nombre local del elemento o atributo. Es NULL si el objeto DOM no tiene nombre.|  
|**prefijo**|**nvarchar**|Es el prefijo del espacio del nombre del nodo.|  
|**namespaceuri**|**nvarchar**|Es el URI del espacio de nombres del nodo. Si el valor es NULL, no hay ningún espacio de nombres.|  
|**datatype**|**nvarchar**|Es el tipo de datos reales de la fila del elemento o atributo y, en caso contrario, es NULL. El tipo de datos se infiere a partir de las DTD insertadas o del esquema insertado.|  
|**prev**|**bigint**|Es el id. XML del anterior elemento del mismo nivel. Es NULL si no existe ningún elemento previo directo del mismo nivel.|  
|**text**|**ntext**|Contiene el valor de atributo o el contenido de elemento en forma de texto (o es NULL si la entrada de la tabla **irregular** no requiere un valor).|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-a-simple-select-statement-with-openxml"></a>A. Usar una instrucción SELECT simple con OPENXML  
 En el siguiente ejemplo se crea una representación interna de la imagen XML utilizando `sp_xml_preparedocument`. A continuación se ejecuta una instrucción `SELECT` que usa un proveedor del conjunto de filas `OPENXML` contra la representación interna del documento XML.  
  
 El valor de *flag* se establece en `1`. Este valor indica una asignación **centrada en atributos**. Por tanto, los atributos XML se asignan a las columnas del conjunto de filas. El valor *rowpattern* especificado como `/ROOT/Customer` identifica los nodos `<Customers>` que se van a procesar.  
  
 El parámetro opcional *ColPattern* (patrón de columna) no se especifica porque el nombre de columna coincide con los nombres de atributos XML.  
  
 El proveedor del conjunto de filas `OPENXML` crea un conjunto de filas de 2 columnas (`CustomerID` y `ContactName`) desde el que la instrucción `SELECT` recupera las columnas necesarias (en este caso, todas las columnas).  
  
```sql  
DECLARE @idoc INT, @doc VARCHAR(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;  
-- Execute a SELECT statement that uses the OPENXML rowset provider.  
SELECT    *  
FROM       OPENXML (@idoc, '/ROOT/Customer',1)  
            WITH (CustomerID  VARCHAR(10),  
                  ContactName VARCHAR(20));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Si se ejecuta la misma instrucción `SELECT` con el parámetro *flags* establecido en `2`, lo que indica una asignación **centrada en elementos**, los valores de `CustomerID` y `ContactName` para los dos clientes en el documento XML se devuelven como NULL, ya que no hay ningún elemento con el nombre `CustomerID` o `ContactName` en el documento XML.  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CustomerID ContactName  
---------- -----------  
NULL       NULL  
NULL       NULL  
```  
  
### <a name="b-specifying-colpattern-for-mapping-between-columns-and-the-xml-attributes"></a>B. Especificar ColPattern para la asignación entre columnas y los atributos XML  
 En la siguiente consulta se devuelven los atributos de CustomerID, OrderDate, ProductID y Quantity del documento XML. *rowpattern* identifica los elementos `<OrderDetails>`. `ProductID` y `Quantity` son los atributos del elemento `<OrderDetails>`. No obstante, `OrderID`, `CustomerID` y `OrderDate` son los atributos del elemento primario (`<Orders>`).  
  
 El elemento opcional *ColPattern* se especifica para las siguientes asignaciones:  
  
-   Los valores `OrderID`, `CustomerID` y `OrderDate` del conjunto de filas se asignan a los atributos del elemento primario de los nodos identificados por *rowpattern* en el documento XML.  
  
-   La columna `ProdID` del conjunto de filas se asigna al atributo `ProductID`, y la columna `Qty` del conjunto de filas se asigna al atributo `Quantity` de los nodos identificados en *rowpattern*.  
  
 Aunque el parámetro *flags* especifica la asignación **centrada en elementos**, la asignación especificada en *ColPattern* sobrescribe la anterior.  
  
```sql  
DECLARE @idoc INT, @doc VARCHAR(1000);   
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">v  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';   
  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;   
  
-- SELECT stmt using OPENXML rowset provider  
SELECT *  
FROM   OPENXML (@idoc, '/ROOT/Customer/Order/OrderDetail',2)   
         WITH (OrderID       int         '../@OrderID',
               CustomerID  varchar(10) '../@CustomerID',
               OrderDate   datetime    '../@OrderDate',
               ProdID      int         '@ProductID',
               Qty         int         '@Quantity');
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
OrderID CustomerID           OrderDate                 ProdID    Qty  
------------------------------------------------------------------------  
10248      VINET       1996-07-04 00:00:00.000   11      12  
10248      VINET       1996-07-04 00:00:00.000   42      10  
10283      LILAS       1996-08-16 00:00:00.000   72      3  
```  
  
### <a name="c-obtaining-results-in-an-edge-table-format"></a>C. Obtener el resultado en formato de tabla irregular  
 El documento XML del ejemplo siguiente está formado por los elementos `<Customers>`, `<Orders>` y `<Order_0020_Details>`. Primero se llama a **sp_xml_preparedocument** para obtener un identificador de documentos. Este identificador de documentos se pasa a `OPENXML`.  
  
 En la instrucción `OPENXML`, el valor *rowpattern* (`/ROOT/Customers`) identifica los nodos `<Customers>` que se van a procesar. Puesto que no se proporciona la cláusula WITH, `OPENXML` devuelve el conjunto de filas en un formato de tabla **irregular**.  
  
 Por último, la instrucción `SELECT` recupera todas las columnas de la tabla **irregular**.  
  
```sql  
DECLARE @idoc INT, @doc VARCHAR(1000);   
SET @doc ='  
<ROOT>  
<Customers CustomerID="VINET" ContactName="Paul Henriot">  
   <Orders CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <Order_x0020_Details OrderID="10248" ProductID="11" Quantity="12"/>  
      <Order_x0020_Details OrderID="10248" ProductID="42" Quantity="10"/>  
   </Orders>  
</Customers>  
<Customers CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Orders CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <Order_x0020_Details OrderID="10283" ProductID="72" Quantity="3"/>  
   </Orders>  
</Customers>  
</ROOT>';  
  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;   
  
-- SELECT statement that uses the OPENXML rowset provider.  
SELECT    *  
FROM       OPENXML (@idoc, '/ROOT/Customers')   
EXEC sp_xml_removedocument @idoc;   
```  
  
## <a name="see-also"></a>Vea también  
 [Ejemplos: Uso de OPENXML](../../relational-databases/xml/examples-using-openxml.md)  
  
