---
description: Editor de tareas de generación de perfiles de datos (página General)
title: Editor de la tarea de generación de perfiles de datos (página General) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataprofilingtask.general.f1
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: eec15906-d757-4079-b2f6-aca4e52b3b4c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d8a30c9fe99378aa06ebf9bfc69dca24ab09028a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352107"
---
# <a name="data-profiling-task-editor-general-page"></a>Editor de tareas de generación de perfiles de datos (página General)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilice la página **General** del **Editor de tareas de generación de perfiles de datos** para configurar las opciones siguientes:  
  
-   Especifique el destino para el perfil generado.  
  
-   Utilice la configuración predeterminada para simplificar la tarea de generar perfiles de una sola tabla o vista.  
  
 Para más información sobre cómo usar la tarea de generación de perfiles de datos, vea [Configuración de la Tarea de generación de perfiles de datos](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Para más información sobre cómo usar el Visor de perfil de datos para analizar la salida de la tarea de generación de perfiles de datos, vea [Visor de perfil de datos](../../integration-services/control-flow/data-profile-viewer.md).  
  
 **Para abrir la página General del Editor de tareas de generación de perfiles de datos**  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que tiene la tarea de generación de perfiles de datos.  
  
2.  En la pestaña **Flujo de control** , haga doble clic en la tarea de generación de perfiles de datos.  
  
3.  En el **Editor de tareas de generación de perfiles de datos**, haga clic en **General**.  
  
## <a name="data-profiling-options"></a>Opciones de la generación de perfiles de datos  
 **Tiempo de espera**  
 Especifique el número de segundos transcurridos los cuales la tarea de generación de datos debe agotar el tiempo de espera y dejar de ejecutarse. El valor predeterminado es 0, lo que no indica ningún tiempo de espera.  
  
## <a name="destination-options"></a>Opciones de destino  
  
> [!IMPORTANT]  
>  El archivo de salida podría contener datos confidenciales acerca de la base de datos y de los datos que contiene. Para conocer sugerencias sobre cómo hacer que este archivo sea más seguro, vea [Acceso a los archivos usados por los paquetes](../../integration-services/security/security-overview-integration-services.md#files).  
  
 **DestinationType**  
 Especifique si guardar el perfil de los datos generado en un archivo o una variable:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**FileConnection**|Guarde el perfil generado en un archivo de la ubicación que se especifica en un administrador de conexiones de archivos.<br /><br /> Nota: Para especificar qué administrador de conexiones de archivos utilizar, use la opción **Destino** .|  
|**Variable**|Guarde el perfil generado en una variable de paquete.<br /><br /> Para especificar qué variable de paquete utilizar, use la opción **Destino** .|  
  
 **Destino**  
 Especifique qué administrador de conexiones de archivos o variable de paquete contiene el perfil de datos generado:  
  
-   Si la opción **DestinationType** está establecida en **FileConnection**, la opción **Destination** muestra los administradores de conexión de archivos disponibles. Seleccione uno de estos administradores de conexiones, o bien seleccione \<New File connection> para crear un administrador de conexiones de archivos nuevo.  
  
-   Si la opción **DestinationType** está establecida en **Variable**, la opción **Destination** muestra las variables de paquete disponibles en la lista **Destination** . Seleccione una de estas variables, o bien seleccione \<New Variable> para crear una variable nueva.  
  
 **OverwriteDestination**  
 Especifique si sobrescribir el archivo de salida si ya existe. El valor predeterminado es **False**. El valor de esta propiedad se usa cuando la opción DestinationType está establecida en FileConnection. Cuando la opción DestinationType está establecida en Variable, la tarea siempre sobrescribe el valor anterior de la variable.  
  
> [!IMPORTANT]  
>  Si intenta ejecutar la tarea de generación de perfiles de datos más de una vez sin cambiar el nombre del archivo de salida o el valor de la propiedad **OverwriteDestination** a **True**, se produce un error en la tarea con un mensaje que indica que el archivo de salida ya existe.  
  
## <a name="other-options"></a>Otras opciones  
 **Perfil rápido**  
 Muestre el **Formulario de perfil rápido de tabla única**. Este formulario simplifica la tarea de generar perfiles de una tabla o vista únicas con la configuración predeterminada. Para más información, vea [Formulario de perfil rápido de tabla única &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md).  
  
 **Abrir el visor de perfil**  
 Abre el Visor de perfil de datos. El Visor de perfil de datos independiente muestra el resultado del perfil de datos de la tarea Generación de perfiles de datos. El resultado del perfil de datos puede verse después de ejecutar la tarea Generación de perfiles de datos dentro del paquete [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y de calcular los perfiles de datos.  
  
> [!NOTE]  
>  También puede abrir el Visor de perfil de datos si ejecuta el archivo DataProfileViewer.exe en la carpeta *\<drive>* :\Archivos de programa (x86) | Archivos de programa\Microsoft SQL Server\110\DTS\Binn.  
  
## <a name="see-also"></a>Consulte también  
 [Formulario de perfil rápido de tabla única &#40;tarea de generación de perfiles de datos&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)   
 [Editor de tareas de generación de perfiles de datos &#40;página Solicitudes de perfil&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md)  
  
  
