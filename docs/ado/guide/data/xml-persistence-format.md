---
description: Formato de persistencia de XML
title: Formato de persistencia XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML persistence [ADO], persistence format
ms.assetid: 6e146738-ac4d-47bb-b6cd-d87b2260aead
author: rothja
ms.author: jroth
ms.openlocfilehash: 3a1a1b832f9e34cbf99e5fd01c424bf54c50c3bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036725"
---
# <a name="xml-persistence-format"></a>Formato de persistencia de XML
ADO utiliza la codificación UTF-8 para la secuencia XML que persiste.  
  
 El formato XML de ADO se divide en dos secciones, una sección de esquema seguida de la sección de datos. A continuación se encuentra un archivo XML de ejemplo para la tabla Shippers de la base de datos Northwind. A continuación del ejemplo se explican varias partes del XML.  
  
## <a name="remarks"></a>Observaciones  
  
```  
<xml xmlns:s="uuid:BDC6E3F0-6DA3-11d1-A2A3-00AA00C14882"   
xmlns:dt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882"   
xmlns:rs="urn:schemas-microsoft-com:rowset"   
xmlns:z="#RowsetSchema">   
  <s:Schema id="RowsetSchema">   
    <s:ElementType name="row" content="eltOnly" rs:updatable="true">   
      <s:AttributeType name="ShipperID" rs:number="1"   
        rs:basetable="shippers" rs:basecolumn="ShipperID"  
        rs:keycolumn="true">   
        <s:datatype dt:type="int" dt:maxLength="4" rs:precision="10"   
          rs:fixedlength="true" rs:maybenull="false"/>   
      </s:AttributeType>   
      <s:AttributeType name="CompanyName" rs:number="2"   
        rs:nullable="true" rs:write="true" rs:basetable="shippers"   
        rs:basecolumn="CompanyName">   
        <s:datatype dt:type="string" dt:maxLength="40" />   
      </s:AttributeType>   
      <s:AttributeType name="Phone" rs:number="3" rs:nullable="true"   
        rs:write="true" rs:basetable="shippers"   
        rs:basecolumn="Phone">   
        <s:datatype dt:type="string" dt:maxLength="24"/>   
      </s:AttributeType>   
      <s:extends type="rs:rowbase"/>   
    </s:ElementType>   
  </s:Schema>   
  
  <rs:data>   
    <z:row ShipperID="1" CompanyName="Speedy Express"   
      Phone="(503) 555-9831"/>   
    <z:row ShipperID="2" CompanyName="United Package"   
      Phone="(503) 555-3199"/>   
    <z:row ShipperID="3" CompanyName="Federal Shipping"   
      Phone="(503) 555-9931"/>   
  </rs:data>   
</xml>  
```  
  
 El esquema muestra las declaraciones de espacios de nombres, la sección de esquema y la sección de datos. La sección Schema contiene las definiciones de Row, ShipperID, CompanyName y Phone.  
  
 Las definiciones de esquema se ajustan a la [especificación del XML-Data del W3C](http://www.w3.org/TR/1998/NOTE-XML-data/) y se pueden validar completamente (aunque no se producirá la validación en Internet Explorer 5). XML-Data es actualmente el único formato de esquema admitido para la persistencia del conjunto de registros.  
  
 La sección de datos tiene tres filas que contienen información acerca de los distribuidores. Para un conjunto de filas vacío, la sección de datos puede estar vacía, pero las \<rs:data> etiquetas deben estar presentes. Sin datos, puede escribir la abreviatura de la etiqueta como simplemente \<rs:data/> . Cualquier etiqueta con el prefijo "RS" indica que se encuentra en el espacio de nombres definido por urn: schemas-microsoft-com: RowSet.  
  
## <a name="see-also"></a>Consulte también  
 [Almacenar registros en formato XML](./persisting-records-in-xml-format.md)