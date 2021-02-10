---
description: Conservar los datos
title: Persistencia de datos | Microsoft Docs
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
ms.assetid: 21c162ca-2845-4dd8-a49d-e715aba8c461
author: rothja
ms.author: jroth
ms.openlocfilehash: f9a2ad809efbc847a2dfaf2292a15c1c7376e9b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037215"
---
# <a name="persisting-data"></a>Conservar los datos
La informática portátil (por ejemplo, el uso de equipos portátiles) ha generado la necesidad de aplicaciones que se pueden ejecutar en un estado conectado y desconectado. ADO ha agregado compatibilidad con esto al ofrecer al desarrollador la capacidad de guardar un conjunto de **registros** de cursor de cliente en el disco y volver a cargarlo más adelante.  
  
 Hay varios escenarios en los que podría usar este tipo de característica, entre las que se incluyen las siguientes:  
  
-   **Viajes:** Al poner en marcha la aplicación, es fundamental proporcionar la capacidad de realizar cambios y agregar nuevos registros que, a continuación, se pueden volver a conectar a la base de datos posteriormente y confirmarse.  
  
-   **Búsquedas con poca frecuencia:** A menudo, en una aplicación, las tablas se usan como búsquedas, por ejemplo, las tablas de impuestos estatales. Se actualizan con poca frecuencia y son de solo lectura. En lugar de volver a leer estos datos del servidor cada vez que se inicia la aplicación, la aplicación puede simplemente cargar los datos de un **conjunto de registros** guardado localmente.  
  
 En ADO, para guardar y cargar **conjuntos de registros**, use los métodos **Recordset. Save** y **Recordset. Open (,,,, AdCmdFile)** en el objeto **Recordset** de ADO.  
  
 Puede usar el método **Save de conjunto de registros** para conservar el **conjunto de registros** ADO en un archivo de un disco. (También puede guardar un **conjunto de registros** en un objeto de **secuencia** de ADO. Los objetos de **secuencia** se describen más adelante en la guía). Más adelante, puede usar el método **Open** para volver a abrir el **conjunto de registros** cuando esté listo para usarlo. De forma predeterminada, ADO guarda el **conjunto de registros** en el formato de Microsoft Advanced Data TABLEGRAM (ADTG) propietario. Este formato binario se especifica mediante el valor de **PersistFormatEnum de adPersistADTG** . Como alternativa, puede optar por guardar el **conjunto de registros** como XML en su lugar mediante **adPersistXML**. Para obtener más información sobre cómo guardar conjuntos de registros como XML, vea guardar [registros en formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md).  
  
 La sintaxis del método **Save** es la siguiente:  
  
```  
  
recordset.  
Save  
Destination, PersistFormat  
  
```  
  
 La primera vez que se guarda el **conjunto de registros**, es opcional especificar el *destino*. Si omite *Destination*, se creará un nuevo archivo con un nombre establecido en el valor de la propiedad [source](../../../ado/reference/ado-api/source-property-ado-recordset.md) del conjunto de **registros**.  
  
 Omitir *destino* cuando se llama a **Save** después de la primera vez que se guarda o se produce un error en tiempo de ejecución. Si posteriormente llama a **Save** con un nuevo *destino*, el **conjunto de registros** se guarda en el nuevo destino. Sin embargo, el nuevo destino y el destino original se abrirán.  
  
 **Guardar** no cierra el **conjunto de registros** o el *destino*, por lo que puede continuar trabajando con el **conjunto de registros** y guardar los cambios más recientes. El *destino* permanece abierto hasta que se cierra el **conjunto de registros** , durante el cual otras aplicaciones pueden leer pero no escribir en el *destino*.  
  
 Por motivos de seguridad, el método **Save** solo permite el uso de la configuración de seguridad baja y personalizada de un script ejecutado por Microsoft Internet Explorer.  
  
 Si se llama al método **Save** mientras está en curso una operación asincrónica de captura de **conjunto de registros** , ejecución o actualización, **guarde** las esperas hasta que se complete la operación asincrónica.  
  
 Los registros se guardan empezando por la primera fila del **conjunto de registros**. Cuando finaliza el método **Save** , la posición de la fila actual se mueve a la primera fila del **conjunto de registros**.  
  
 Para obtener los mejores resultados, establezca la propiedad [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) en **adUseClient** con **Guardar**. Si el proveedor no admite toda la funcionalidad necesaria para guardar objetos de **conjunto de registros** , el servicio de cursor proporcionará esa funcionalidad.  
  
 Cuando un **conjunto de registros** se mantiene con la propiedad **CursorLocation** establecida en **adUseServer**, la capacidad de actualización del **conjunto de registros** es limitada. Normalmente, solo se permiten las actualizaciones de tabla única, las inserciones y las eliminaciones (en función de la funcionalidad del proveedor). El método [Resync](../../../ado/reference/ado-api/resync-method.md) también está disponible en esta configuración.  
  
 Dado que el parámetro de *destino* puede aceptar cualquier objeto que admita la interfaz OLE DB **IStream** , puede guardar un **conjunto de registros** directamente en el objeto de **respuesta** de ASP.  
  
 En el ejemplo siguiente, se usan los métodos **Save** y **Open** para conservar un **conjunto de registros** y, posteriormente, volver a abrirlo:  
  
```  
'BeginPersist  
   conn.ConnectionString = _  
   "Provider=SQLOLEDB;Data Source=MySQLServer;" _  
      & "Integrated Security=SSPI;Initial Catalog=pubs"  
   conn.Open  
  
   conn.Execute "create table testtable (dbkey int " & _  
      "primary key, field1 char(10))"  
   conn.Execute "insert into testtable values (1, 'string1')"  
  
   Set rst.ActiveConnection = conn  
   rst.CursorLocation = adUseClient  
  
   rst.Open "select * from testtable", conn, adOpenStatic, _  
      adLockBatchOptimistic  
  
   'Change the row on the client  
   rst!field1 = "NewValue"  
  
   'Save to a file--the .dat extension is an example; choose  
   'your own extension. The changes will be saved in the file  
   'as well as the original data.  
   MyFile = Dir("c:\temp\temptbl.dat")  
   If MyFile <> "" Then  
       Kill "c:\temp\temptbl.dat"  
   End If  
  
   rst.Save "c:\temp\temptbl.dat", adPersistADTG  
   Set rst = Nothing  
  
   'Now reload the data from the file  
   Set rst = New ADODB.Recordset  
   rst.Open "c:\temp\temptbl.dat", , adOpenStatic, _  
      adLockBatchOptimistic, adCmdFile  
  
   Debug.Print "After Loading the file from disk"  
   Debug.Print "   Current Edited Value: " & rst!field1.Value  
   Debug.Print "   Value Before Editing: " & rst!field1.OriginalValue  
  
   'Note that you can reconnect to a connection and  
   'submit the changes to the data source  
   Set rst.ActiveConnection = conn  
   rst.UpdateBatch  
'EndPersist  
```  
  
## <a name="remarks"></a>Observaciones  
 Esta sección contiene los temas siguientes.  
  
-   [Más información acerca de la persistencia de conjunto de registros](../../../ado/guide/data/more-about-recordset-persistence.md)  
  
-   [Almacenar conjuntos de registros filtrados y jerárquicos](../../../ado/guide/data/persisting-filtered-and-hierarchical-recordsets.md)  
  
-   [Almacenar registros en formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
