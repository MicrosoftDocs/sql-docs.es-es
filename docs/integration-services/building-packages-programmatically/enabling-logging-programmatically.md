---
description: Habilitar el registro mediante programación
title: Habilitar el registro mediante programación | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- Integration Services packages, logs
- SSIS logging
- log providers [Integration Services]
- logs [Integration Services], enabling
- LoggingMode property
- LogProvider object
- packages [Integration Services], logs
ms.assetid: 3222a1ed-83eb-421c-b299-a53b67bba740
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 35b731cdb854d24ee2c4759c9ce3f611dc98c104
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349168"
---
# <a name="enabling-logging-programmatically"></a>Habilitar el registro mediante programación

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El motor en tiempo de ejecución proporciona una colección de objetos <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider> que permiten capturar información específica del evento durante la validación y ejecución de paquetes. Los objetos <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider> están disponibles para los objetos <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer>, incluidos los objetos <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, <xref:Microsoft.SqlServer.Dts.Runtime.Package>, <xref:Microsoft.SqlServer.Dts.Runtime.ForLoop> y <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>. El registro se habilita en contenedores individuales o en el paquete completo.  
  
 Existen diferentes tipos de proveedores de registro disponibles para que los utilice un contenedor. Esto proporciona la flexibilidad necesaria para crear y almacenar información del registro en distintos formatos. Dar de alta un objeto contenedor en el registro es un proceso de dos pasos: en primer lugar se habilita el registro y, a continuación, se selecciona un proveedor de registro. Las propiedades <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer.LoggingOptions%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer.LoggingMode%2A> del contenedor se utilizan para especificar los eventos registrados y seleccionar el proveedor de registro.  
  
## <a name="enabling-logging"></a>Habilitar el registro  
 La propiedad <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer.LoggingMode%2A>, incluida en cada contenedor que puede realizar el registro, determina si la información de evento del contenedor se registra en el registro de eventos. El valor de esta propiedad se asigna desde la estructura <xref:Microsoft.SqlServer.Dts.Runtime.DTSLoggingMode> y se hereda de forma predeterminada del elemento primario del contenedor. Si el contenedor es un paquete y, por tanto, no dispone de elemento primario, la propiedad utiliza <xref:Microsoft.SqlServer.Dts.Runtime.DTSLoggingMode.UseParentSetting>, que tiene como valor predeterminado **Deshabilitado**.  
  
### <a name="selecting-a-log-provider"></a>Seleccionar un proveedor de registro  
 Una vez establecida la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.DtsContainer.LoggingMode%2A> en **Habilitado**, se agrega un proveedor de registro a la colección <xref:Microsoft.SqlServer.Dts.Runtime.SelectedLogProviders> del contenedor para completar el proceso. La colección <xref:Microsoft.SqlServer.Dts.Runtime.SelectedLogProviders> está disponible en el objeto <xref:Microsoft.SqlServer.Dts.Runtime.LoggingOptions> y contiene los proveedores de registro seleccionados para el contenedor. Se llama al método <xref:Microsoft.SqlServer.Dts.Runtime.SelectedLogProviders.Add%2A> para crear un proveedor y agregarlo a la colección. El método devuelve el proveedor de registro que se agregó a la colección. Cada proveedor incluye configuración única para dicho proveedor; estas propiedades se establecen mediante la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider.ConfigString%2A>.  
  
 En la tabla siguiente se enumeran los proveedores de registro disponibles, su descripción y su información <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider.ConfigString%2A>.  
  
|Proveedor|Descripción|Propiedad ConfigString|  
|--------------|-----------------|---------------------------|  
|SQL Server Profiler|Genera archivos de Seguimiento de SQL que se pueden capturar y ver en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler. La extensión predeterminada de los nombres de archivo de este proveedor es .trc.|No se requiere ninguna configuración.|  
|SQL Server|Escribe las entradas del registro de eventos en la tabla **sysssislog** de cualquier base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|El proveedor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere que se especifique la conexión a la base de datos y el nombre de la base de datos de destino.|  
|Archivo de texto|Escribe las entradas de registro de eventos en archivos de texto ASCII con un formato de valores separados por comas (CSV). La extensión predeterminada de los nombres de archivo de este proveedor es .log.|El nombre de un administrador de conexión de archivos.|  
|Registro de eventos de Windows|Escribe las entradas en el registro de eventos estándar de Windows en el equipo local, en el registro de aplicaciones.|No se requiere ninguna configuración.|  
|Archivo XML|Escribe las entradas del registro de eventos en archivos con formato XML. La extensión predeterminada de los nombres de archivo de este proveedor es .xml.|El nombre de un administrador de conexión de archivos.|  
  
 Los eventos se incluyen en el registro de eventos o se excluyen del mismo mediante las propiedades **EventFilterKind** y **EventFilter** del contenedor. La estructura **EventFilterKind** contiene dos valores, **ExclusionFilter** y **InclusionFilter**, que indican si los eventos que se agregan a **EventFilter** se incluyen en el registro de eventos. A continuación, se asigna una matriz de cadenas que contiene los nombres de los eventos sujetos del filtrado a la propiedad **EventFilter**.  
  
 El código siguiente habilita el registro en un paquete, agrega el proveedor de registro para archivos de texto a la colección <xref:Microsoft.SqlServer.Dts.Runtime.SelectedLogProviders> y especifica una lista de eventos para incluir en la salida del registro.  
  
## <a name="sample"></a>Ejemplo  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
  
      ConnectionManager loggingConnection = p.Connections.Add("FILE");  
      loggingConnection.ConnectionString = @"C:\SSISPackageLog.txt";  
  
      LogProvider provider = p.LogProviders.Add("DTS.LogProviderTextFile.2");  
      provider.ConfigString = loggingConnection.Name;  
      p.LoggingOptions.SelectedLogProviders.Add(provider);  
      p.LoggingOptions.EventFilterKind = DTSEventFilterKind.Inclusion;  
      p.LoggingOptions.EventFilter = new String[] { "OnPreExecute",   
         "OnPostExecute", "OnError", "OnWarning", "OnInformation" };  
      p.LoggingMode = DTSLoggingMode.Enabled;  
  
      // Add tasks and other objects to the package.  
  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
  
    Dim loggingConnection As ConnectionManager = p.Connections.Add("FILE")  
    loggingConnection.ConnectionString = "C:\SSISPackageLog.txt"  
  
    Dim provider As LogProvider = p.LogProviders.Add("DTS.LogProviderTextFile.2")  
    provider.ConfigString = loggingConnection.Name  
    p.LoggingOptions.SelectedLogProviders.Add(provider)  
    p.LoggingOptions.EventFilterKind = DTSEventFilterKind.Inclusion  
    p.LoggingOptions.EventFilter = New String() {"OnPreExecute", _  
       "OnPostExecute", "OnError", "OnWarning", "OnInformation"}  
    p.LoggingMode = DTSLoggingMode.Enabled  
  
    ' Add tasks and other objects to the package.  
  
  End Sub  
  
End Module  
```  
  
## <a name="see-also"></a>Consulte también  
 [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md)  
  
  
