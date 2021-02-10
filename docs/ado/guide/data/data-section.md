---
description: Sección de datos
title: Sección de datos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data section [ADO]
ms.assetid: 43dc42a8-7057-48e6-93d6-880d5c5c51a4
author: rothja
ms.author: jroth
ms.openlocfilehash: 09b79b0001ff448ecd333a4ec601c4ff42febf6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037665"
---
# <a name="data-section"></a>Sección de datos
La sección de datos define los datos del conjunto de filas junto con las actualizaciones, inserciones o eliminaciones pendientes. La sección de datos puede contener cero o más filas. Solo puede contener datos de un conjunto de filas donde la fila esté definida por el esquema. Además, como se indicó antes, se pueden omitir las columnas sin datos. Si se utiliza un atributo o subelemento en la sección de datos y esa construcción no se ha definido en la sección de esquema, se pasa por alto de forma silenciosa.  
  
## <a name="string"></a>String  
 Los caracteres XML reservados en los datos de texto deben reemplazarse por las entidades de caracteres apropiadas. Por ejemplo, en el nombre de la empresa "garaje de Joe", la comilla simple debe reemplazarse por una entidad. La fila real sería similar a la siguiente:  
  
```  
<z:row CompanyName="Joe's Garage"/>  
```  
  
 Los caracteres siguientes están reservados en XML y deben reemplazarse por entidades de caracteres: {', ', &, \<,> }.  
  
## <a name="binary"></a>Binary  
 Los datos binarios son bin. hex codificado (es decir, un byte se asigna a dos caracteres, un carácter por recorte).  
  
## <a name="datetime"></a>DateTime  
 El formato de la variante VT_DATE no es compatible directamente con los tipos de datos de XML-Data. El formato correcto para las fechas con un componente de datos y de hora es aaaa-mm-ddThh: mm: SS.  
  
 Para obtener más información acerca de los formatos de fecha especificados por XML, vea la [especificación del W3C XML-Data](https://go.microsoft.com/fwlink/?LinkId=5692).  
  
 Cuando la especificación de XML-Data define dos tipos de datos equivalentes (por ejemplo, I4 = = int), ADO escribirá el nombre descriptivo, pero leerá en ambos.  
  
## <a name="managing-pending-changes"></a>Administrar cambios pendientes  
 Un conjunto de registros se puede abrir en modo de actualización inmediata o por lotes. Cuando se abren en modo de actualización por lotes con cursores de cliente, todos los cambios realizados en el conjunto de registros están en estado pendiente hasta que se llama al método UpdateBatch. Los cambios pendientes también se conservan cuando se guarda el conjunto de registros. En XML, se representan mediante el uso de los elementos "Update" definidos en urn: schemas-microsoft-com: RowSet. Además, si se puede actualizar un conjunto de filas, la propiedad actualizable debe establecerse en true en la definición de la fila. Por ejemplo, para definir que la tabla Shippers contiene cambios pendientes, la definición de la fila tendría un aspecto similar al siguiente.  
  
```  
<s:ElementType name="row" content="eltOnly" updatable="true">  
  <s:attribute type="ShipperID"/>  
  <s:attribute type="CompanyName"/>  
  <s:attribute type="Phone"/>  
  <s:extends type="rs:rowbase"/>  
</s:ElementType>  
```  
  
 Esto indica al proveedor de persistencia que muestre los datos para que ADO pueda construir un objeto de conjunto de registros actualizable.  
  
 Los datos de ejemplo siguientes muestran cómo las inserciones, cambios y eliminaciones se buscan en el archivo guardado.  
  
```  
<rs:data>  
  <z:row ShipperID="2" CompanyName="United Package"   
    Phone="(503) 555-3199"/>  
<rs:update>  
  <rs:original>  
    <z:row ShipperID="3" CompanyName="Federal Shipping"   
      Phone="(503) 555-9931"/>  
  </rs:original>  
  <z:row Phone="(503) 552-7134"/>  
</rs:update>  
<rs:insert>  
  <z:row ShipperID="12" CompanyName="Lightning Shipping"   
    Phone="(505) 111-2222"/>  
  <z:row ShipperID="13" CompanyName="Thunder Overnight"   
    Phone="(505) 111-2222"/>  
  <z:row ShipperID="14" CompanyName="Blue Angel Air Delivery"   
    Phone="(505) 111-2222"/>  
</rs:insert>  
<rs:delete>  
  <z:row ShipperID="1" CompanyName="Speedy Express" Phone="(503) 555-9831"/>  
</rs:delete>  
</rs:data>  
```  
  
 Una actualización siempre contiene los datos de fila originales completos seguidos de los datos de fila modificados. La fila modificada puede contener todas las columnas o solo las columnas que realmente han cambiado. En el ejemplo anterior, no se cambia la fila del remitente 2 y solo la columna Phone ha cambiado los valores para el remitente 3 y, por lo tanto, la única columna incluida en la fila modificada. Las filas insertadas para los distribuidores 12, 13 y 14 se agrupan por lotes en una etiqueta RS: INSERT. Tenga en cuenta que las filas eliminadas también se pueden agrupar por lotes, aunque esto no se muestra en el ejemplo anterior.  
  
## <a name="see-also"></a>Consulte también  
 [Almacenar registros en formato XML](./persisting-records-in-xml-format.md)