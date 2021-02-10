---
description: Más información acerca de la persistencia de conjunto de registros
title: Más información sobre la persistencia de conjuntos de registros | Microsoft Docs
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
- updating data [ADO], persisting data
ms.assetid: a9b287f5-04b0-4514-8143-f67879ca9842
author: rothja
ms.author: jroth
ms.openlocfilehash: ce3b22ecd5913db0045f82e72aa20b0833a5109b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037265"
---
# <a name="more-about-recordset-persistence"></a>Más información acerca de la persistencia de conjunto de registros
El objeto ADO Recordset permite almacenar el contenido de un objeto de **conjunto de registros** en un archivo mediante el método [Save](../../reference/ado-api/save-method.md) . El archivo almacenado de forma persistente puede existir en una unidad local, en un servidor o como una dirección URL en un sitio Web. Más adelante, el archivo se puede restaurar con el método [Open](../../reference/ado-api/open-method-ado-recordset.md) del objeto **Recordset** o el método [Execute](../../reference/ado-api/execute-method-ado-connection.md) del objeto [Connection](../../reference/ado-api/connection-object-ado.md) .  
  
 Además, el método [GetString](../../reference/ado-api/getstring-method-ado.md) convierte un objeto de **conjunto de registros** en un formulario en el que las columnas y filas se delimitan con los caracteres que se especifiquen.  
  
 Para conservar un **conjunto de registros**, empiece convirtiéndolo en un formulario que pueda almacenarse en un archivo. Los objetos de **conjunto de registros** se pueden almacenar en el formato de TableGram de datos avanzados (ADTG) propietario o en el formato Open lenguaje de marcado extensible (XML). Los ejemplos de ADTG se muestran en la sección siguiente. Para obtener más información sobre la persistencia de XML, vea [guardar registros en formato XML](./persisting-records-in-xml-format.md).  
  
 Guarde los cambios pendientes en el archivo guardado. Esto permite emitir una consulta que devuelve un objeto de **conjunto de registros** , edita el **conjunto de registros**, lo guarda y los cambios pendientes, restaura posteriormente el conjunto de **registros** y, a continuación, actualiza el origen de datos con los cambios pendientes guardados.  
  
 Para obtener información sobre el almacenamiento persistente de objetos de **flujo** , vea [secuencias y persistencia](./streams-and-persistence.md).  
  
 Para obtener un ejemplo de persistencia del **conjunto de registros** , vea el escenario de persistencia del conjunto de registros XML.  
  
## <a name="example"></a>Ejemplo  
  
### <a name="save-a-recordset"></a>Guardar un conjunto de registros:  
  
```  
Dim rs as New ADODB.Recordset  
rs.Save "c:\yourFile.adtg", adPersistADTG  
```  
  
### <a name="open-a-persisted-file-with-recordsetopen"></a>Abra un archivo persistente con Recordset. Open:  
  
```  
Dim rs as New ADODB.Recordset  
rs.Open "c:\yourFile.adtg", "Provider=MSPersist",,,adCmdFile  
```  
  
 Opcionalmente, si el **conjunto de registros** no tiene una conexión activa, puede aceptar todos los valores predeterminados y codificar lo siguiente:  
  
```  
Dim rs as New ADODB.Recordset  
rs.Open "c:\yourFile.adtg"  
```  
  
### <a name="open-a-persisted-file-with-connectionexecute"></a>Abra un archivo guardado con Connection.Exe.  
  
```  
Dim conn as New ADODB.Connection  
Dim rs as ADODB.Recordset  
conn.Open "Provider=MSPersist"  
Set rs = conn.execute("c:\yourFile.adtg")  
```  
  
### <a name="open-a-persisted-file-with-rdsdatacontrol"></a>Abra un archivo persistente con RDS. DataControl  
 En este caso, no se establece la propiedad del **servidor** .  
  
```  
Dim dc as New RDS.DataControl  
dc.Connection = "Provider=MSPersist"  
dc.SQL = "c:\yourFile.adtg"  
dc.Refresh  
```  
  
## <a name="see-also"></a>Consulte también  
 [GetString (método) (ADO)](../../reference/ado-api/getstring-method-ado.md)   
 [Proveedor de persistencia de Microsoft OLE DB (proveedor de servicios ADO)](../appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md)   
 [Objeto de conjunto de registros (ADO)](../../reference/ado-api/recordset-object-ado.md)   
 [Secuencias y persistencia](./streams-and-persistence.md)