---
title: Traza
description: El archivo de Web.config contiene una sección de seguimiento, nueva en SQL Server 2016 Master Data Services. Obtenga información sobre el comportamiento de seguimiento predeterminado.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 45823fc8-723a-49f2-9a11-94d241245cfd
author: lrtoyou1223
ms.author: lle
manager: erikre
ms.openlocfilehash: eda1a559e9936deae31a5511dacd695e8ec6f24e
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92195044"
---
# <a name="tracing-master-data-services"></a>Seguimiento (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  El archivo Web.config contiene una sección de seguimiento, como se muestra a continuación. Esta sección es nueva en [!INCLUDE[ssSQL15](../includes/sssql15-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]  
  
```  
<sources>  
      <!-- Adjust the switch value to control the types of messages that should be logged.   
           https://msdn.microsoft.com/library/system.diagnostics.sourcelevels  
           Use the a switchValue of Verbose to generate a full log. Please be aware that   
           the trace file can get quite large very quickly -->  
      <source name="MDS" switchType="System.Diagnostics.SourceSwitch" switchValue="Warning, ActivityTracing">  
        <listeners>  
          <!-- Set a directory path where the service account you chose while setting up Master Data Services has read and write privileges.  
               Default path is Logs in WebApplication folder, for example C:\Program Files\Microsoft SQL Server\130\Master Data Services\WebApplication  
               New log file will be created every day or every 10 mb.  
               When directory size hits the 200mb limitation, the oldest file will be deleted.-->  
          <add name="FileTraceListener"  
               type="Microsoft.MasterDataServices.Core.Logging.FileTraceListener, Microsoft.MasterDataServices.Core"   
               initializeData="DirectoryPath = Logs; FileSizeInMb = 10; MaxDirectorySizeInMb = 200"/>  
          <remove name="Default"/>  
        </listeners>  
      </source>  
    </sources>  
  
```  
  
 Este es el comportamiento de seguimiento predeterminado.  
  
-   El seguimiento está habilitado para los mensajes de advertencia y ActivityTracing.  
  
     Para más información, consulte [SourceLevels (Enumeración)](/dotnet/api/system.diagnostics.sourcelevels).  
  
-   Los registros se guardan en la carpeta Registros en la carpeta WebApplication. La ubicación predeterminada es C:\Archivos de programa\Microsoft SQL Server\130\Master Data Services\WebApplication\Logs.  
  
-   El archivo se crea cada día o cada 10 MB.  
  
-   Cuando el tamaño del directorio llega a 200 MB, se elimina el registro más antiguo.  
  
-   El formato de registro es CSV. En la tabla siguiente se describe el formato de registro.  
  
    |Elemento|Descripción|  
    |-------------|-----------------|  
    |Time|Cuando se produce la entrada de seguimiento.|  
    |CorrelationID|Se asigna un identificador de correlación para cada solicitud. Todos los seguimientos desencadenados por esta solicitud compartirán el mismo identificador de correlación.<br /><br /> Cuando se produce un error en la interfaz de usuario, aparece el identificador de correlación en el mensaje de error.|  
    |Operación|Nombre de la operación de solicitud. Si la solicitud es una solicitud de interfaz de usuario web, el nombre de la operación es la dirección URL. Si la solicitud es una solicitud de API, el nombre de la operación es el nombre del servicio.|  
    |Nivel|Nivel de esta entrada de seguimiento.|  
    |Mensaje|Cuerpo del mensaje de seguimiento.|  
  
## <a name="external-resources"></a>Recursos externos  
 Entrada de blog [Troubleshooting Logging Improvement](https://techcommunity.microsoft.com/t5/sql-server-integration-services/troubleshooting-logging-improvement/ba-p/388214)(Solución de problemas de mejora del registro), en msdn.com.  
  
