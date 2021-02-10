---
description: Almacenar registros en formato XML
title: Guardar registros en formato XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisting data [ADO]
- data updates [ADO], persisting data
- data persistence [ADO]
- XML persistence [ADO]
- updating data [ADO], persisting data
ms.assetid: f3113ec4-ae31-428f-89c6-bc1024f128ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 6f29f624fe9f55287dcd8944fd04da0426d9552c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032627"
---
# <a name="persisting-records-in-xml-format"></a>Almacenar registros en formato XML
Al igual que el formato ADTG, la persistencia del **conjunto de registros** en formato XML se implementa con el proveedor de persistencia de Microsoft OLE DB. Este proveedor genera un conjunto de filas de solo avance y de solo lectura desde un archivo o secuencia XML guardado que contiene la información de esquema generada por ADO. Del mismo modo, puede tomar un **conjunto de registros** ADO, generar XML y guardarlo en un archivo o en cualquier objeto que implemente la interfaz **IStream** com. (De hecho, un archivo es simplemente otro ejemplo de un objeto que admite **IStream**). En las versiones 2,5 y posteriores, ADO se basa en el analizador de Microsoft XML (MSXML) para cargar el XML en el **conjunto de registros**; por consiguiente msxml.dll es necesario.  
  
> [!NOTE]
>  Se aplican algunas limitaciones al guardar **conjuntos de registros** jerárquicos (formas de datos) en formato XML. No se puede guardar en XML si el **conjunto de registros** jerárquico contiene actualizaciones pendientes y no se puede guardar un conjunto de **registros** jerárquico con parámetros. Para obtener más información, vea [Guardar conjuntos de registros jerárquicos y filtrados](../../../ado/guide/data/persisting-filtered-and-hierarchical-recordsets.md).  
  
 La forma más fácil de conservar los datos en XML y volver a cargarlos a través de ADO es con los métodos **Save** y **Open** , respectivamente. En el siguiente ejemplo de código ADO se muestra cómo guardar los datos de la tabla **titles** en un archivo denominado titles. sav.  
  
```  
Dim rs as new Recordset  
Dim rs2 as new Recordset  
Dim c as new Connection  
Dim s as new Stream  
  
' Query the Titles table.  
c.Open "provider=sqloledb;data source=MySQLServer;initial catalog=pubs;Integrated Security='SSPI'"  
rs.cursorlocation = adUseClient  
rs.Open "select * from titles", c, adOpenStatic  
  
' Save to the file in the XML format. Note that if you don't specify   
' adPersistXML, a binary format (ADTG) will be used by default.  
rs.Save "titles.sav", adPersistXML  
  
' Save the recordset into the ADO Stream object.  
rs.save s, adPersistXML  
rs.Close  
c.Close  
  
set rs = nothing  
  
' Reopen the file.  
rs.Open "titles.sav",,,,adCmdFile  
' Open the Stream back into a Recordset.  
rs2.open s  
```  
  
 ADO siempre conserva todo el objeto de **conjunto de registros** . Si desea conservar un subconjunto de filas del objeto de **conjunto de registros** , use el método **Filter** para restringir las filas o cambiar la cláusula de selección. Sin embargo, debe abrir un objeto de **conjunto de registros** con un cursor del lado cliente (**CursorLocation**  =  **adUseClient**) para utilizar el método de **filtro** para guardar un subconjunto de filas. Por ejemplo, para recuperar títulos que comienzan por la letra "b", puede aplicar un filtro a un objeto de **conjunto de registros** abierto:  
  
```  
rs.Filter "title_id like 'B*'"  
rs.Save "btitles.sav", adPersistXML  
```  
  
 ADO siempre usa el conjunto de filas del motor de cursor de cliente para generar un objeto de **conjunto de registros** desplazable y seleccionable sobre los datos de solo avance generados por el proveedor de persistencia.  
  
 Esta sección contiene los temas siguientes.  
  
-   [Formato de persistencia de XML](../../../ado/guide/data/xml-persistence-format.md)  
  
-   [Espacios de nombres](../../../ado/guide/data/namespaces.md)  
  
-   [Sección de esquema](../../../ado/guide/data/schema-section.md)  
  
-   [Sección de datos](../../../ado/guide/data/data-section.md)  
  
-   [Conjuntos de registros jerárquicos en XML](../../../ado/guide/data/hierarchical-recordsets-in-xml.md)  
  
-   [Propiedades del conjunto de registros dinámicos en XML](../../../ado/guide/data/recordset-dynamic-properties-in-xml.md)  
  
-   [Transformaciones XSLT](../../../ado/guide/data/xslt-transformations.md)  
  
-   [Guardar datos en el objeto de DOM de XML](../../../ado/guide/data/saving-to-the-xml-dom-object.md)  
  
-   [Consideraciones de seguridad XML](../../../ado/guide/data/xml-security-considerations.md)  
  
-   [Escenario de persistencia del conjunto de registros XML](../../../ado/guide/data/xml-recordset-persistence-scenario.md)
