---
description: Tarea Lector de datos WMI
title: Tarea Lector de datos WMI | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.wmidatareadertask.f1
- sql13.dts.designer.wmidatareadertask.general.f1
- sql13.dts.designer.wmidatareadertask.wmiquery.f1
helpviewer_keywords:
- WQL [Integration Services]
- WMI Data Reader task [Integration Services]
ms.assetid: dae57067-0275-4ac3-8f34-1b9d169f1112
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d246e590cd54aca1b3a6ac911ec12d2835c8d91f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339232"
---
# <a name="wmi-data-reader-task"></a>Tarea Lector de datos WMI

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Lector de datos WMI ejecuta consultas mediante el Lenguaje de consulta del Instrumental de administración de Windows (WMI), que devuelve información de WMI sobre un sistema informático. Puede usar la tarea Lector de datos WMI para los siguientes fines:  
  
-   Realizar consultas de los registros de eventos de Windows en un equipo local o remoto y escribir la información en un archivo o variable.  
  
-   Obtener información sobre la presencia, estado o propiedades de componentes del hardware y usar posteriormente esta información para determinar si se deben ejecutar otras tareas en el flujo de control.  
  
-   Obtener una lista de las aplicaciones y determinar qué versión de cada aplicación está instalada.  
  
 Puede configurar la tarea Lector de datos WMI de las maneras siguientes:  
  
-   Especificar el administrador de conexiones WMI que se debe usar.  
  
-   Especificar el origen de la consulta WQL. La consulta se puede almacenar en una propiedad de tarea, o bien fuera de la tarea, en una variable o archivo.  
  
-   Definir el formato de los resultados de la consulta WQL. La tarea admite una tabla, par de nombre/valor de la propiedad o formato de valor de propiedad.  
  
-   Especificar el destino de la consulta. El destino puede ser una variable o un archivo.  
  
-   Indicar si el destino de la consulta se sobrescribe, se conserva o anexa.  
  
 Si el origen o destino es un archivo, la tarea Lector de datos WMI usa un administrador de conexiones de archivo para conectarse al archivo. Para más información, consulte [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 La tarea Lector de datos WMI usa un administrador de conexiones WMI para conectarse al servidor desde el cual lee la información de WMI. Para más información, consulte [WMI Connection Manager](../../integration-services/connection-manager/wmi-connection-manager.md).  
  
## <a name="wql-query"></a>WQL Query  
 WQL es un dialecto de SQL con extensiones para admitir la notificación de eventos de WMI y otras características específicas de WMI. Para obtener más información sobre WQL, vea la documentación sobre Instrumental de administración de Windows en [MSDN Library](../../sql-server/index.yml).  
  
> [!NOTE]  
>  Las clases de WMI varían en las diferentes versiones de Windows.  
  
 La siguiente consulta WQL devuelve entradas en el evento de registro de la aplicación.  
  
```  
SELECT * FROM Win32_NTLogEvent WHERE LogFile = 'Application' AND (SourceName='SQLISService' OR SourceName='SQLISPackage') AND TimeGenerated > '20050117'  
```  
  
 La siguiente consulta WQL devuelve información de disco lógica.  
  
```  
SELECT FreeSpace, DeviceId, Size, SystemName, Description FROM Win32_LlogicalDisk  
```  
  
 La siguiente consulta WQL devuelve una lista de las actualizaciones de Ingeniería de corrección rápida (QFE) al sistema operativo.  
  
```  
Select * FROM Win32_QuickFixEngineering  
```  
  
## <a name="custom-logging-messages-available-on-the-wmi-data-reader-task"></a>Mensajes de registro personalizados disponibles en la tarea Lector de datos WMI  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Lector de datos WMI. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**WMIDataReaderGettingWMIData**|Indica que la tarea inició la lectura de datos WMI.|  
|**WMIDataReaderOperation**|Informa de la consulta WQL que ejecutó la tarea.|  
  
## <a name="configuration-of-the-wmi-data-reader-task"></a>Configuración de la tarea Lector de datos WMI  
 Puede establecer propiedades mediante programación o a través del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Para obtener información acerca de las propiedades que puede establecer en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el tema siguiente:  
  
-   [Página Expresiones](../../integration-services/expressions/expressions-page.md)  
  
 Para obtener información sobre cómo establecer estas propiedades mediante programación, haga clic en el tema siguiente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.WmiDataReaderTask.WmiDataReaderTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 Para obtener más información sobre cómo establecer estas propiedades en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el siguiente tema:  
  
-   [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="wmi-data-reader-task-editor-general-page"></a>Editor de la tarea Lector de datos WMI (página General)
  Utilice la página **General** del cuadro de diálogo **Editor de la tarea Lector de datos WMI** para asignar un nombre a la tarea Lector de datos WMI y describirla.  
  
  Para más información sobre el lenguaje de consulta de WMI (WQL), vea el tema sobre Instrumental de administración de Windows, [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)(Realizar consultas con WQL), en MSDN Library.  
  
### <a name="options"></a>Opciones  
 **Nombre**  
 Proporcione un nombre único para la tarea Lector de datos WMI. Este nombre se utiliza como etiqueta en el icono de tarea.  
  
> [!NOTE]  
>  Los nombres de tarea deben ser únicos en un paquete.  
  
 **Descripción**  
 Escriba una descripción de la tarea Lector de datos WMI.  
  
## <a name="wmi-data-reader-task-editor-wmi-options-page"></a>Editor de la tarea Lector de datos WMI (página Opciones WMI)
  Use la página **Opciones WMI** del cuadro de diálogo **Editor de la tarea Lector de datos WMI** para especificar el origen de la consulta WQL (Lenguaje de consulta de Instrumental de administración de Windows) y el destino del resultado de la consulta.  
  
 Para más información sobre el lenguaje de consulta de WMI (WQL), vea el tema sobre Instrumental de administración de Windows, [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)(Realizar consultas con WQL), en MSDN Library.  
  
### <a name="static-options"></a>Opciones estáticas  
 **WMIConnectionName**  
 Seleccione un administrador de conexiones de WMI de la lista o haga clic en \<**New WMI Connection...**> para crear uno.  
  
 **Temas relacionados:** [Administrador de conexiones WMI](../../integration-services/connection-manager/wmi-connection-manager.md), [Editor del administrador de conexiones WMI](../connection-manager/wmi-connection-manager.md)  
  
 **WQLQuerySourceType**  
 Seleccione el tipo de origen de la consulta WQL que ejecuta la tarea. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Entrada directa**|Establezca el origen en una consulta WQL. Al seleccionar este valor se muestra la opción dinámica **WQLQuerySourceType**.|  
|**Conexión de archivos**|Seleccione el archivo que contiene la consulta WQL. Al seleccionar este valor se muestra la opción dinámica **WQLQuerySourceType**.|  
|**Variable**|Establezca el origen en una variable que defina la consulta WQL. Al seleccionar este valor se muestra la opción dinámica **WQLQuerySourceType**.|  
  
 **OutputType**  
 Especifique si la salida debe ser una tabla de datos, un valor de propiedad o un valor y nombre de propiedad.  
  
 **OverwriteDestination**  
 Especifica si mantener, sobrescribir o anexar a los datos originales en el archivo de destino o variable.  
  
 **DestinationType**  
 Seleccione el tipo de destino de la consulta WQL que ejecuta la tarea. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Conexión de archivos**|Seleccione un archivo donde guardar los resultados de la consulta WQL. Al seleccionar este valor se muestra la opción dinámica **DestinationType**.|  
|**Variable**|Establezca la variable donde almacenar los resultados de la consulta WQL. Al seleccionar este valor se muestra la opción dinámica **DestinationType**.|  
  
### <a name="wqlquerysourcetype-dynamic-options"></a>Opciones dinámicas WQLQuerySourceType  
  
#### <a name="wqlquerysourcetype--direct-input"></a>WQLQuerySourceType = Entrada directa  
 **WQLQuerySource**  
 Proporcione una consulta, o bien haga clic en los puntos suspensivos (…) y escriba una consulta con el cuadro de diálogo **Consulta WQL**.  
  
#### <a name="wqlquerysourcetype--file-connection"></a>WQLQuerySourceType = Conexión de archivos  
 **WQLQuerySource**  
 Seleccione un administrador de conexiones de archivos de la lista o haga clic en \<**New connection...**> para crear uno.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
#### <a name="wqlquerysourcetype--variable"></a>WQLQuerySourceType = Variable  
 **WQLQuerySource**  
 Seleccione una variable de la lista o haga clic en \<**New variable...**> para crear una.  
  
 **Temas relacionados:** [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md)  
  
### <a name="destinationtype-dynamic-options"></a>Opciones dinámicas DestinationType  
  
#### <a name="destinationtype--file-connection"></a>DestinationType = Conexión de archivos  
 **Destino**  
 Seleccione un administrador de conexiones de archivos de la lista o haga clic en \<**New connection...**> para crear uno.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Editor de administrador de conexiones de archivos](../connection-manager/file-connection-manager.md)  
  
#### <a name="destinationtype--variable"></a>DestinationType = Variable  
 **Destino**  
 Seleccione una variable de la lista o haga clic en \<**New variable...**> para crear una.  
  
 **Temas relacionados:** [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Agregar variable](../integration-services-ssis-variables.md)  
  
## <a name="see-also"></a>Consulte también  
 [Tareas de Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flujo de control](../../integration-services/control-flow/control-flow.md)  
  
