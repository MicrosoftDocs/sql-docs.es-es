---
description: Sección de esquema
title: Sección de esquema | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Schema section [ADO]
ms.assetid: 4ac6e524-2c92-48e8-b871-0a4b5c8fda18
author: rothja
ms.author: jroth
ms.openlocfilehash: 346957b2b060bf990cf77bfe7784f67b13a37c37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037015"
---
# <a name="schema-section"></a>Sección de esquema
La sección Schema es obligatoria. Como se muestra en el ejemplo anterior, ADO escribe metadatos detallados sobre cada columna para conservar la semántica de los valores de datos tanto como sea posible para la actualización. Sin embargo, para cargar en el XML, ADO solo requiere los nombres de las columnas y el conjunto de filas al que pertenecen. A continuación se muestra un ejemplo de un esquema mínimo:  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"  
    xmlns:rs="urn:schemas-microsoft-com:rowset"  
    xmlns:z="#RowsetSchema">  
  <s:Schema id="RowsetSchema">  
    <s:ElementType name="row" content="eltOnly">  
      <s:AttributeType name="ShipperID"/>  
      <s:AttributeType name="CompanyName"/>  
      <s:AttributeType name="Phone"/>  
      <s:Extends type="rs:rowbase"/>  
    </s:ElementType>  
  </s:Schema>  
  <rs:data>  
...  
  </rs:data>  
</xml>  
```  
  
 En el ejemplo anterior, ADO tratará los datos como cadenas de longitud variable porque no se incluye ninguna información de tipo en el esquema.  
  
## <a name="creating-aliases-for-column-names"></a>Crear alias para nombres de columna  
 El atributo RS: Name permite crear un alias para un nombre de columna para que pueda aparecer un nombre descriptivo en la información de columna expuesta por el conjunto de filas y se puede usar un nombre más corto en la sección de datos. Por ejemplo, el esquema anterior podría modificarse para asignar ShipperID a S1, CompanyName a S2 y teléfono a S3 de la manera siguiente:  
  
```  
<s:Schema id="RowsetSchema">   
<s:ElementType name="row" content="eltOnly" rs:updatable="true">   
<s:AttributeType name="s1" rs:name="ShipperID" rs:number="1" ...>   
...  
</s:AttributeType>   
<s:AttributeType name="s2" rs:name="CompanyName" rs:number="2" ...>   
...  
</s:AttributeType>   
<s:AttributeType name="s3" rs:name="Phone" rs:number="3" ...>   
...  
</s:AttributeType>   
...  
</s:ElementType>   
</s:Schema>  
```  
  
 A continuación, en la sección de datos, la fila usaría el atributo name (no RS: Name) para hacer referencia a esa columna:  
  
```  
"<row s1="1" s2="Speedy Express" s3="(503) 555-9831"/>  
```  
  
 Es necesario crear alias para los nombres de columna cuando un nombre de columna no es un nombre de atributo o etiqueta válido en XML. Por ejemplo, "LastName" debe tener un alias porque los nombres con espacios insertados son atributos no válidos. El analizador XML no controlará correctamente la línea siguiente, por lo que debe crear un alias para otro nombre que no tenga un espacio incrustado.  
  
```  
<row last name="Jones"/>  
```  
  
 Cualquier valor que use para el atributo name debe usarse de forma coherente en cada lugar en el que se haga referencia a la columna en las secciones de esquema y de datos del documento XML. En el ejemplo siguiente se muestra el uso coherente de S1:  
  
```  
<s:Schema id="RowsetSchema">  
  <s:ElementType name="row" content="eltOnly">  
    <s:attribute type="s1"/>  
    <s:attribute type="CompanyName"/>  
    <s:attribute type="s3"/>  
    <s:extends type="rs:rowbase"/>  
  </s:ElementType>  
  <s:AttributeType name="s1" rs:name="ShipperID" rs:number="1"   
    rs:maydefer="true" rs:writeunknown="true">  
    <s:datatype dt:type="i4" dt:maxLength="4" rs:precision="10"   
      rs:fixedlength="true" rs:maybenull="true"/>  
  </s:AttributeType>  
</s:Schema>  
<rs:data>  
  <z:row s1="1" CompanyName="Speedy Express" s3="(503) 555-9831"/>  
</rs:data>  
```  
  
 Del mismo modo, dado que no hay ningún alias definido para `CompanyName` en el ejemplo anterior, `CompanyName` se debe usar de forma coherente en todo el documento.  
  
## <a name="data-types"></a>Tipo de datos  
 Puede aplicar un tipo de datos a una columna con el atributo dt: Type. Para obtener la guía definitiva de los tipos XML permitidos, vea la sección tipos de datos de la [especificación del XML-Data del W3C](http://www.w3.org/TR/1998/NOTE-XML-data/). Puede especificar un tipo de datos de dos maneras: especifique el atributo dt: Type directamente en la propia definición de columna o utilice la construcción s:DataType como un elemento anidado de la definición de columna. Por ejemplo,  
  
```  
<s:AttributeType name="Phone" >  
  <s:datatype dt:type="string"/>  
</s:AttributeType>  
```  
  
 es equivalente a  
  
```  
<s:AttributeType name="Phone" dt:type="string"/>  
```  
  
 Si omite el atributo dt: Type completamente de la definición de fila, de forma predeterminada, el tipo de la columna será una cadena de longitud variable.  
  
 Si tiene más información de tipo que simplemente el nombre de tipo (por ejemplo, DT: maxLength), hace que sea más legible usar el elemento secundario s:DataType. Sin embargo, esto es solo una Convención, y no un requisito.  
  
 En los siguientes ejemplos se muestra cómo incluir información de tipo en el esquema.  
  
```  
<!-- 1. String with no max length -->  
<s:AttributeType name="title_id"/>  
<!-or -->  
<s:AttributeType name="title_id" dt:type="string"/>  
  
<!-- 2. Fixed length string with max length of 6 -->  
<s:AttributeType name="title_id">  
    <s:datatype dt:type="string" dt:maxLength="6" rs:fixedlength="true" />  
</s:AttributeType>  
  
<!-- 3. Variable length string with max length of 6 -->  
<s:AttributeType name="title_id">  
    <s:datatype dt:type="string" dt:maxLength="6" />  
</s:AttributeType>  
  
<!-- 4. Integer -->  
<s:AttributeType name="title_id" dt:type="int"/>  
```  
  
 Hay un uso sutil del atributo RS: fixedlength en el segundo ejemplo. Una columna con el atributo RS: fixedlength establecida en true significa que los datos deben tener la longitud definida en el esquema. En este caso, un valor válido para title_id es "123456", como es "123". Sin embargo, "123" no sería válido porque su longitud es 3, no 6. Vea la guía del programador de OLE DB para obtener una descripción más completa de la propiedad fixedlength.  
  
## <a name="handling-nulls"></a>Controlar valores NULL  
 Los valores NULL se controlan mediante el atributo RS: maybenull. Si este atributo se establece en true, el contenido de la columna puede contener un valor null. Además, si la columna no se encuentra en una fila de datos, el usuario que lea los datos del conjunto de filas obtendrá un estado null de IRowset:: GetData (). Considere las siguientes definiciones de columna de la tabla Shippers.  
  
```  
<s:AttributeType name="ShipperID">  
  <s:datatype dt:type="int" dt:maxLength="4"/>  
</s:AttributeType>  
<s:AttributeType name="CompanyName">  
  <s:datatype dt:type="string" dt:maxLength="40" rs:maybenull="true"/>  
</s:AttributeType>  
```  
  
 La definición permite `CompanyName` que sea NULL, pero `ShipperID` no puede contener un valor null. Si la sección de datos contiene la fila siguiente, el proveedor de persistencia establecería el estado de los datos de la `CompanyName` columna en la constante de estado OLE DB DBSTATUS_S_ISNULL:  
  
```  
<z:row ShipperID="1"/>  
```  
  
 Si la fila estaba completamente vacía, como se indica a continuación, el proveedor de persistencia devolvería un estado OLE DB de DBSTATUS_E_UNAVAILABLE para `ShipperID` y DBSTATUS_S_ISNULL para CompanyName.  
  
```  
<z:row/>   
```  
  
 Tenga en cuenta que una cadena de longitud cero no es igual que null.  
  
```  
<z:row ShipperID="1" CompanyName=""/>  
```  
  
 En el caso de la fila anterior, el proveedor de persistencia devolverá un OLE DB estado de DBSTATUS_S_OK para ambas columnas. `CompanyName`En este caso, es simplemente "" (una cadena de longitud cero).  
  
 Para obtener más información sobre las construcciones de OLE DB disponibles para su uso en el esquema de un documento XML para OLE DB, vea la definición de "urn: schemas-microsoft-com: Rowset" y la guía del programador de OLE DB.  
  
## <a name="see-also"></a>Consulte también  
 [Almacenar registros en formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
