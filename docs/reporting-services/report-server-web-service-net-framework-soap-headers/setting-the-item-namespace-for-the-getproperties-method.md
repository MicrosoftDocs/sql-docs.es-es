---
description: Establecer el espacio de nombres de elemento para el método GetProperties
title: Establecer el espacio de nombres de elemento para el método GetProperties | Microsoft Docs
Description: Obtenga información sobre cómo recuperar propiedades en función de la ruta o el Id. de un elemento con el método GetProperties y el encabezado SOAP ItemNamespaceHeader.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-soap-headers
ms.topic: reference
helpviewer_keywords:
- item properties [Reporting Services]
- ItemNamespaceHeader SOAP header
- GetProperties method
ms.assetid: b0a08639-3101-40a2-abe2-3a41753826d1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 45edea2b8f4b489f7681ded859a4f81a06047b5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044975"
---
# <a name="setting-the-item-namespace-for-the-getproperties-method"></a>Establecer el espacio de nombres de elemento para el método GetProperties
  Puede utilizar el encabezado SOAP <xref:ReportService2010.ItemNamespaceHeader> en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para recuperar propiedades de elementos basadas en dos identificadores de elemento diferentes: la ruta de acceso completa o el id. del elemento.  
  
 Al realizar una llamada al método <xref:ReportService2010.ReportingService2010.GetProperties%2A>, normalmente pasa como argumento la ruta de acceso completa del elemento para el que desea recuperar las propiedades. Usando <xref:ReportService2010.ItemNamespaceHeader>, Puede establecer el encabezado SOAP para que la llamada al método le permita utilizar <xref:ReportService2010.ReportingService2010.GetProperties%2A> pasando el identificador del elemento como un identificador.  
  
 El ejemplo de código siguiente recupera los valores de las propiedades de elementos basadas en el identificador del elemento.  
  
> [!NOTE]  
>  De forma predeterminada, no necesita establecer un valor para <xref:ReportService2010.ItemNamespaceHeader> si pasa al método <xref:ReportService2010.ReportingService2010.GetProperties%2A> el nombre de ruta de acceso completa como identificador del elemento.  
  
```vb  
Imports System  
Imports System.Collections  
  
Class Sample  
   Sub Main()  
      Dim rs As New ReportingService2010()  
      rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
      rs.Url = "https://<Server Name>/reportserver/ReportService2010.asmx"  
  
      Dim items() As CatalogItem  
  
      Try  
         ' Need the ID property of items. Normally, you would already have   
         ' this stored somewhere.  
         items = rs.ListChildren("/AdventureWorks Sample Reports", False)  
  
         ' Set the item namespace header to be GUID-based  
         rs.ItemNamespaceHeaderValue = New ItemNamespaceHeader()  
         rs.ItemNamespaceHeaderValue.ItemNamespace = ItemNamespaceEnum.GUIDBased  
  
         ' Call GetProperties with item ID.  
         If Not (items Is Nothing) Then  
            Dim item As CatalogItem  
            For Each item In  items  
               Dim properties As [Property]() = rs.GetProperties(item.ID, Nothing)  
               Dim property As [Property]  
               For Each property In  properties  
                  Console.WriteLine(([property].Name + ": " + [property].Value))  
               Next property  
               Console.WriteLine()  
            Next item  
         End If  
  
      Catch e As Exception  
         Console.WriteLine((e.Message + ": " + e.StackTrace))  
      End Try  
   End Sub 'Main  
End Class 'Sample  
```  
  
```csharp  
using System;  
using System.Collections;  
  
class Sample  
{  
   static void Main()  
   {  
   ReportingService2010 rs = new ReportingService2010();  
      rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
      rs.Url = "https://<Server Name>/reportserver/ReportService2010.asmx";  
  
      CatalogItem[] items;  
  
      try  
      {  
         // Need the ID property of items. Normally, you would already have   
         // this stored somewhere.  
         items = rs.ListChildren("/AdventureWorks Sample Reports", false);  
  
         // Set the item namespace header to be GUID-based  
         rs.ItemNamespaceHeaderValue = new ItemNamespaceHeader();  
         rs.ItemNamespaceHeaderValue.ItemNamespace = ItemNamespaceEnum.GUIDBased;  
  
         // Call GetProperties with item ID.  
         if (items != null)  
         {  
            foreach( CatalogItem item in items)  
            {  
               Property[] properties = rs.GetProperties(item.ID, null);  
               foreach (Property property in properties)  
               {  
                  Console.WriteLine(property.Name + ": " + property.Value);  
               }  
               Console.WriteLine();  
            }  
         }  
      }  
  
      catch (Exception e)  
      {  
         Console.WriteLine(e.Message);  
      }  
   }  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia técnica &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)   
 [Utilizar los encabezados SOAP de Reporting Services](../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)  
  
  
