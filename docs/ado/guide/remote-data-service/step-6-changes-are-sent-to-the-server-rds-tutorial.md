---
description: 'Paso 6: Envío de los cambios al servidor (Tutorial de RDS)'
title: 'Paso 6: los cambios se envían al servidor (tutorial de RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], changes sent to server
ms.assetid: b1e927d6-7d50-4978-9eef-045043cdce7a
author: rothja
ms.author: jroth
ms.openlocfilehash: d6a769a22a86e145727079af6fc067b1552f2bbf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031717"
---
# <a name="step-6-changes-are-sent-to-the-server-rds-tutorial"></a>Paso 6: Envío de los cambios al servidor (Tutorial de RDS)
Si se edita el objeto de **conjunto de registros** , los cambios (es decir, las filas que se agregan, se modifican o se eliminan) pueden enviarse de vuelta al servidor.  
  
> [!NOTE]
>  El comportamiento predeterminado de RDS se puede invocar implícitamente con objetos ADO y el proveedor de servicios remotos de Microsoft OLE DB. Las consultas pueden devolver **conjuntos de registros** y los conjuntos de **registros** editados pueden actualizar el origen de datos. En este tutorial no se invoca RDS con objetos ADO, pero este es el aspecto que tendría si lo hiciera:  
  
```vb
Dim rs as New ADODB.Recordset  
rs. "SELECT * FROM Authors","=MS Remote;=Pubs;" & _  
=https://yourServer;=SQLOLEDB;"  
...              ' Edit the Recordset.  
rs.   ' The equivalent of   
...  
```  
  
 **Parte A** En este caso, supongamos que solo ha usado [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md) y que un objeto de **conjunto de registros** ahora está asociado a **RDS. DataControl**. El método [SubmitChanges](../../reference/rds-api/submitchanges-method-rds.md) actualiza el origen de datos con los cambios realizados en el objeto de **conjunto de registros** si todavía se han establecido las propiedades [Server](../../reference/rds-api/server-property-rds.md) y [Connect](../../reference/rds-api/connect-property-rds.md) .  
  
```vb
Sub RDSTutorial6A()  
Dim DC as New RDS.DataControl  
Dim RS as ADODB.Recordset  
DC. = "https://yourServer"  
DC. = "DSN=Pubs"  
DC. = "SELECT * FROM Authors"  
DC.  
...  
Set RS = DC.  
   ' Edit the Recordset.  
...  
DC.  
...  
```  
  
 **Parte B** Como alternativa, puede actualizar el servidor con el objeto [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) , especificando una conexión y un objeto de **conjunto de registros** .  
  
```vb
Sub RDSTutorial6B()  
Dim DS As New RDS.DataSpace  
Dim RS As ADODB.Recordset  
Dim DC As New RDS.DataControl  
Dim DF As Object  
Dim blnStatus As Boolean  
Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
Set RS = DF. ("DSN=Pubs", "SELECT * FROM Authors")  
DC. = RS    ' Visual controls can now bind to DC.  
    ' Edit the Recordset.  
blnStatus = DF."DSN=Pubs", RS  
End Sub  
```  
  
 **Éste es el final del tutorial.**  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Consulte también  
 [Proveedor de servicios remotos de Microsoft OLE DB (proveedor de servicios ADO)](../appendixes/microsoft-ole-db-remoting-provider-ado-service-provider.md)   
 [Tutorial de RDS](./rds-tutorial.md)   
 [Tutorial de RDS (VBScript)](./rds-tutorial-vbscript.md)