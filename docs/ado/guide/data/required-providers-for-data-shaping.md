---
description: Proveedores deseados para dar forma a datos
title: Proveedores necesarios para el modelado de datos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], data shaping
- data shaping [ADO], providers required
ms.assetid: d49d48d2-ac2d-4c11-895c-5a149b444620
author: rothja
ms.author: jroth
ms.openlocfilehash: eb3d91f23666c900f59a6ffc8e6b94af9bcdb4d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032537"
---
# <a name="required-providers-for-data-shaping"></a>Proveedores deseados para dar forma a datos
La forma de los datos normalmente requiere dos proveedores. El proveedor de servicios, el [servicio de forma de datos para OLE DB](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md), proporciona la funcionalidad de forma de datos y un proveedor de datos, como el proveedor de OLE DB para SQL Server, proporciona filas de datos para rellenar el [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md)con forma.  
  
 El nombre del proveedor de servicios (MSDataShape) se puede especificar como el valor de la propiedad del [proveedor](../../../ado/reference/ado-api/provider-property-ado.md) de objetos de [conexión](../../../ado/reference/ado-api/connection-object-ado.md) o la palabra clave de cadena de conexión "Provider = MSDataShape;".  
  
 El nombre del proveedor de datos se puede especificar como el valor de la propiedad dinámica del **proveedor de datos** , que se agrega a la colección de [propiedades](../../../ado/reference/ado-api/properties-collection-ado.md) del objeto de **conexión** del servicio de forma de datos para OLE DB, o la palabra clave de cadena de conexión "**Provider =** _Provider_".  
  
 No se requiere ningún proveedor de datos si el **conjunto de registros** no está rellenado (por ejemplo, como en un **conjunto de registros** fabricado donde las columnas se crean con la palabra clave New). En ese caso, especifique "**Data Provider =** none;".  
  
## <a name="example"></a>Ejemplo  
  
```  
Dim cnn As New ADODB.Connection  
cnn.Provider = "MSDataShape"  
cnn.Open "Data Provider=SQLOLEDB;Integrated Security=SSPI;Database=Northwind"  
```  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de forma de datos](../../../ado/guide/data/data-shaping-example.md)   
 [Gramática de forma formal](../../../ado/guide/data/formal-shape-grammar.md)   
 [Comandos Shape en General](../../../ado/guide/data/shape-commands-in-general.md)
