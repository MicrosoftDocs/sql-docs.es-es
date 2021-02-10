---
description: Conjuntos de registros jerárquicos en XML
title: Conjuntos de registros jerárquicos en XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO], in XML
ms.assetid: 5d4b11c4-c94f-4910-b99b-5b9abc50d791
author: rothja
ms.author: jroth
ms.openlocfilehash: 9919489df58853a3fa8eea3711583cdf57a0849f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033715"
---
# <a name="hierarchical-recordsets-in-xml"></a>Conjuntos de registros jerárquicos en XML
ADO permite la persistencia de objetos de conjunto de registros jerárquicos en XML. Con los objetos de conjunto de registros jerárquicos, el valor de un campo del conjunto de registros primario es otro conjunto de registros. Estos campos se representan como elementos secundarios en la secuencia XML en lugar de un atributo.  
  
## <a name="remarks"></a>Observaciones  
 En el ejemplo siguiente se muestra este caso:  
  
```  
Rs.Open "SHAPE {select stor_id, stor_name, state from stores} APPEND ({select stor_id, ord_num, ord_date, qty from sales} AS rsSales RELATE stor_id TO stor_id)", "Provider=MSDataShape;DSN=pubs;Integrated Security=SSPI;"  
```  
  
 A continuación se encuentra el formato XML del conjunto de registros guardado:  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"     xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882"     xmlns:rs="urn:schemas-microsoft-com:rowset"   
    xmlns:z="#RowsetSchema">   
  <s:Schema id="RowsetSchema">   
    <s:ElementType name="row" content="eltOnly" rs:updatable="true">   
      <s:AttributeType name="stor_id" rs:number="1"   
        rs:writeunknown="true">   
        <s:datatype dt:type="string" dt:maxLength="4"   
          rs:fixedlength="true" rs:maybenull="false"/>   
      </s:AttributeType>   
      <s:AttributeType name="stor_name" rs:number="2" rs:nullable="true"   
        rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="40"/>   
      </s:AttributeType>   
      <s:AttributeType name="state" rs:number="3" rs:nullable="true"   
        rs:writeunknown="true">   
        <s:datatype dt:type="string" dt:maxLength="2"   
          rs:fixedlength="true"/>   
      </s:AttributeType>   
      <s:ElementType name="rsSales" content="eltOnly"   
        rs:updatable="true" rs:relation="010000000100000000000000">   
        <s:AttributeType name="stor_id" rs:number="1"   
          rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="4"   
            rs:fixedlength="true" rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="ord_num" rs:number="2"   
          rs:writeunknown="true">   
          <s:datatype dt:type="string" dt:maxLength="20"   
            rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="ord_date" rs:number="3"   
          rs:writeunknown="true">   
            <s:datatype dt:type="dateTime" dt:maxLength="16"   
              rs:scale="3" rs:precision="23" rs:fixedlength="true"   
              rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:AttributeType name="qty" rs:number="4" rs:writeunknown="true">   
          <s:datatype dt:type="i2" dt:maxLength="2" rs:precision="5"   
            rs:fixedlength="true" rs:maybenull="false"/>   
        </s:AttributeType>   
        <s:extends type="rs:rowbase"/>   
      </s:ElementType>   
      <s:extends type="rs:rowbase"/>   
    </s:ElementType>   
  </s:Schema>   
  <rs:data>   
    <z:row stor_id="6380" stor_name="Eric the Read Books" state="WA">   
      <rsSales stor_id="6380" ord_num="6871"   
        ord_date="1994-09-14T00:00:00" qty="5"/>   
      <rsSales stor_id="6380" ord_num="722a"   
        ord_date="1994-09-13T00:00:00" qty="3"/>   
    </z:row>   
    <z:row stor_id="7066" stor_name="Barnum's" state="CA">   
      <rsSales stor_id="7066" ord_num="A2976"   
        ord_date="1993-05-24T00:00:00" qty="50"/>   
      <rsSales stor_id="7066" ord_num="QA7442.3"   
        ord_date="1994-09-13T00:00:00" qty="75"/>   
    </z:row>   
    <z:row stor_id="7067" stor_name="News & Brews" state="CA">   
      <rsSales stor_id="7067" ord_num="D4482"   
        ord_date="1994-09-14T00:00:00" qty="10"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="40"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="20"/>   
      <rsSales stor_id="7067" ord_num="P2121"   
        ord_date="1992-06-15T00:00:00" qty="20"/>   
    </z:row>   
  </rs:data>   
</xml>   
```  
  
 El orden exacto de las columnas en el conjunto de registros primario no es obvio cuando se conserva de esta manera. Cualquier campo del elemento primario puede contener un conjunto de registros secundario. El proveedor de persistencia conserva todas las columnas escalares primero como atributos y, a continuación, conserva todas las "columnas" del conjunto de registros secundario como elementos secundarios de la fila primaria. La posición ordinal del campo en el conjunto de registros primario se puede obtener examinando la definición de esquema del conjunto de registros. Cada campo tiene una propiedad OLE DB, RS: Number, definida en el espacio de nombres del esquema de conjunto de registros que contiene el número ordinal de ese campo.  
  
 Los nombres de todos los campos del conjunto de registros secundario se concatenan con el nombre del campo en el conjunto de registros primario que contiene este elemento secundario. Esto es para asegurarse de que no hay conflictos de nombres en los casos en los que los conjuntos de registros primarios y secundarios contienen un campo que se obtiene de dos tablas diferentes, pero se denomina singularmente.  
  
 Al guardar conjuntos de registros jerárquicos en XML, debe tener en cuenta las siguientes restricciones en ADO:  
  
-   Un conjunto de registros jerárquico con actualizaciones pendientes no se puede almacenar en XML.  
  
-   Un conjunto de registros jerárquico creado con un comando de forma con parámetros no puede ser persistente (en formato XML o ADTG).  
  
-   ADO actualmente guarda la relación entre los conjuntos de registros primarios y secundarios como un objeto binario grande (BLOB). Las etiquetas XML para describir esta relación todavía no se han definido en el espacio de nombres del esquema del conjunto de filas.  
  
-   Cuando se guarda un conjunto de registros jerárquico, se guardan todos los conjuntos de registros secundarios junto con él. Si el conjunto de registros actual es un elemento secundario de otro conjunto de registros, no se guarda su elemento primario. Se guardan todos los conjuntos de registros secundarios que forman el subárbol del conjunto de registros actual.  
  
 Cuando se vuelve a abrir un conjunto de registros jerárquico desde su formato almacenado en XML, debe tener en cuenta las siguientes limitaciones:  
  
-   Si el registro secundario contiene registros para los que no hay registros primarios correspondientes, estas filas no se escriben en la representación XML del conjunto de registros jerárquico. Por lo tanto, estas filas se perderán cuando se vuelva a abrir el conjunto de registros desde su ubicación persistente.  
  
-   Si un registro secundario tiene referencias a más de un registro primario, al volver a abrir el conjunto de registros, el conjunto de registros secundario puede contener registros duplicados. Sin embargo, estos duplicados solo estarán visibles si el usuario trabaja directamente con el conjunto de filas secundario subyacente. Si un capítulo se utiliza para navegar por el conjunto de registros secundario (es decir, la única manera de navegar por ADO), los duplicados no estarán visibles.  
  
## <a name="see-also"></a>Consulte también  
 [Almacenar registros en formato XML](./persisting-records-in-xml-format.md)