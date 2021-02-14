---
title: Métodos de procesamiento por lotes | Microsoft Docs
description: Obtenga información sobre cómo usar encabezados SOAP en Reporting Services para incluir varios métodos de servicio web en una única operación.
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-soap-headers
ms.topic: reference
helpviewer_keywords:
- methods [Reporting Services], batches
- BatchHeader SOAP header
- Web service [Reporting Services], methods
- Report Server Web service, batching
- batches [Reporting Services]
- XML Web service [Reporting Services], methods
- locking [Reporting Services]
- multiple Web service methods
ms.assetid: 86435534-c9fe-4b49-b88c-7fb6d21976b0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 145a7ad079827ca3392eb0bde0761f9f8f70a9f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081686"
---
# <a name="batching-methods"></a>Métodos de procesamiento por lotes
  El uso de encabezados SOAP en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] le permite incluir varios métodos de servicio web en una única operación. Los métodos se ejecutan dentro del ámbito de una única transacción de base de datos, en el orden en el que se llaman.  
  
 La reversión es una ventaja de la utilización de operaciones por lotes con varios métodos. Si se produce un error en alguna de las llamadas a los métodos mientras se está ejecutando un lote, el servidor de informes deja de ejecutarlo y revierte cualquier operación anterior. Esto es útil cuando una llamada a un método depende de que otras llamadas a métodos de ese lote se completen correctamente.  
  
 El servicio web no proporciona semántica de bloqueo para las operaciones por lotes con varios métodos. Las filas de la base de datos del servidor de informes no se bloquean para actualizar hasta que el mensaje se envía al servidor y se llama al comando Execute.  
  
 No hay tampoco ningún control de la simultaneidad para garantizar que la base de datos no ha cambiado desde que se leyeron los últimos datos. Si dos clientes modifican el mismo elemento, la última actualización tiene éxito si los parámetros todavía son válidos (por ejemplo, el nombre del elemento no se ha cambiado).  
  
 El ejemplo siguiente llama al método <xref:ReportService2005.ReportingService2005.CreateFolder%2A> tres veces y ejecuta estas llamadas como un único lote. Si cualquiera de las llamadas a <xref:ReportService2005.ReportingService2005.CreateFolder%2A> no puede realizarse, se cancela el lote completo.  
  
```vb  
Imports System  
Imports System.Web.Services.Protocols  
Imports myNamespace.MyReferenceName  
  
Class Sample  
    Sub Main(args() As String)  
        Dim rs As New ReportingService2005()  
        rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
      ' Set the base Web service URL of the source server  
      rs.Url = "https://<Server Name>/reportserver/ReportService2005.asmx"  
  
        Dim bh As New BatchHeader()  
  
        bh.BatchId = service.CreateBatch()  
        rs.BatchHeaderValue = bh  
        rs.CreateFolder("New Folder1", "/", Nothing)  
        rs.CreateFolder("New Folder2", "/", Nothing)  
        rs.CreateFolder("New Folder3", "/", Nothing)  
  
        Console.WriteLine("Creating folders...")  
        rs.BatchHeaderValue = bh  
        rs.ExecuteBatch()  
        Console.WriteLine("Folders created successfully.")  
  
        rs.BatchHeaderValue = Nothing  
    End Sub  
End Class  
```  
  
```csharp  
using System;  
using System.Web.Services.Protocols;   
using myNamespace.MyReferenceName;  
  
class Sample  
{  
    static void Main(string[] args)  
    {  
        ReportingService2005 rs = new ReportingService2005();  
        rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
      // Set the base Web service URL of the source server  
      rs.Url = "https://<Server Name>/reportserver/ReportService2005.asmx"  
  
        BatchHeader bh = new BatchHeader();  
  
        bh1.BatchID = service.CreateBatch();  
        rs.BatchHeaderValue = bh;  
        rs.CreateFolder("New Folder1", "/", null);  
        rs.CreateFolder("New Folder2", "/", null);  
        rs.CreateFolder("New Folder3", "/", null);  
  
        Console.WriteLine("Creating folders...");  
        rs.BatchHeaderValue = bh1;  
        rs.ExecuteBatch();  
        Console.WriteLine("Folders created successfully.");  
  
        rs.BatchHeaderValue = null;  
    }  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 <xref:ReportService2005.ReportingService2005.CancelBatch%2A>   
 <xref:ReportService2005.ReportingService2005.CreateBatch%2A>   
 [Referencia técnica &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)   
 [Uso de encabezados SOAP de Reporting Services](../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)  
  
  
