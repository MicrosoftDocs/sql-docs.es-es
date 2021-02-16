---
description: Eventos registrados por el servicio Integration Services
title: Eventos registrados por el servicio Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- service [Integration Services], events
- events [Integration Services], service
- Integration Services service, events
ms.assetid: d4122dcf-f16f-47a0-93a2-ffa3d0d4f9cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 45612ea0bfdbda2b27e6cac8b4deb1e302eb9298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346557"
---
# <a name="events-logged-by-the-integration-services-service"></a>Eventos registrados por el servicio Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] registra diversos mensajes en el registro de eventos de aplicación de Windows. El servicio registra estos mensajes cuando el servicio se inicia, cuando se detiene y cuando se producen ciertos problemas.  
  
 Este tema proporciona información sobre los mensajes de eventos comunes que el servicio registra en el registro de eventos de aplicación. El servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] registra todos los mensajes descritos en este tema con un origen del evento de SQLISService.  
  
 Para obtener información general sobre el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vea [Servicio Integration Services &#40;servicio SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
## <a name="service-status-messages"></a>Mensajes de estado personalizados
 Cuando selecciona [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para la instalación, el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se instala y se inicia, y el tipo de inicio del servicio se establece como automático.  
  
|Id. de evento|Nombre simbólico|Texto|Notas|  
|--------------|-------------------|----------|-----------|  
|256|DTS_MSG_SERVER_STARTING|Iniciando el servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)].|El servicio está a punto de iniciarse.|  
|257|DTS_MSG_SERVER_STARTED|Se ha iniciado el servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)].|El servicio se ha iniciado.|  
|260|DTS_MSG_SERVER_START_FAILED|El servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] no se ha podido iniciar.%nError: %1|El servicio no se ha podido iniciar. Esta imposibilidad de iniciarse puede deberse a una instalación dañada o a una cuenta de servicio inapropiada.|  
|258|DTS_MSG_SERVER_STOPPING|Deteniendo el servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)].%n%nDetener todos los paquetes en ejecución al salir: %1|El servicio se está deteniendo, y si se configura el servicio para ello, detendrá todos los paquetes en ejecución. Puede establecer un valor de verdadero o falso en el archivo de configuración que determina si el servicio detiene los paquetes en ejecución cuando el propio servicio se detiene. El mensaje para este evento incluye el valor de esta configuración.|  
|259|DTS_MSG_SERVER_STOPPED|El servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] se ha detenido.%nVersión de servidor %1|El servicio se ha detenido.|  
  
## <a name="settings-file-messages"></a>Mensajes de archivo de configuración  
 La configuración para el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se almacena en un archivo XML que se puede modificar. Para más información, vea [Servicio Integration Services &#40;servicio SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
|Id. de evento|Nombre simbólico|Texto|Notas|  
|--------------|-------------------|----------|-----------|  
|274|DTS_MSG_SERVER_MISSING_CONFIG_REG|Servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]: %nLa configuración del Registro que especifica el archivo de configuración no existe. %nIntentando cargar el archivo de configuración.|La entrada del Registro que contiene la ruta de acceso del archivo de configuración no existe o esta vacía.|  
|272|DTS_MSG_SERVER_MISSING_CONFIG|El archivo de configuración de servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] no existe.%nCargando la configuración predeterminada.|El archivo de configuración no existe en la ubicación especificada.|  
|273|DTS_MSG_SERVER_BAD_CONFIG|El archivo de configuración de servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)] es incorrecto.%nError al leer el archivo de configuración: %1%n%nCargando el servidor con la configuración predeterminada.|El archivo de configuración no se puede leer o no es válido. Este error puede ser el resultado de un error de sintaxis XML en el archivo.|  
  
## <a name="other-messages"></a>Otros mensajes  
  
|Id. de evento|Nombre simbólico|Texto|Notas|  
|--------------|-------------------|----------|-----------|  
|336|DTS_MSG_SERVER_STOPPING_PACKAGE|Servicio [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssIS](../../includes/ssis-md.md)]: deteniendo el paquete en ejecución.%nId. de instancia del paquete: %1%nId. del paquete: %2%nNombre del paquete: %3%nDescripción del paquete: %4%nPaquete|El servicio está intentando detener un paquete en ejecución. Puede supervisar y detener los paquetes en ejecución en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Para obtener información sobre cómo administrar paquetes en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], vea [Administración de paquetes &#40;servicio SSIS&#41;](../../integration-services/service/package-management-ssis-service.md).|  

## <a name="view-events"></a>Ver eventos
  Hay dos herramientas en las que puede ver los eventos para el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] :  
  
-   El cuadro de diálogo **Visor del archivo de registros** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. El cuadro de diálogo **Visor del archivo de registros** incluye opciones para exportar, filtrar y buscar en el registro. Para obtener más información sobre las opciones del **Visor del archivo de registro**, vea [Ayuda F1 del Visor de archivos de registro](../../relational-databases/logs/log-file-viewer-f1-help.md).  
  
-   El Visor de eventos de Windows.  
  
 Para obtener descripciones de los eventos registrados por el servicio de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vea [Eventos registrados por el servicio Integration Services](../../integration-services/service/events-logged-by-the-integration-services-service.md).  
  
### <a name="to-view-service-events-for-integration-services-in-sql-server-management-studio"></a>Para ver los eventos del servicio para Integration Services en SQL Server Management Studio  
  
1.  Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  En el menú **Archivo** , haga clic en **Conectar Explorador de objetos**.  
  
3.  En el cuadro de diálogo **Conectar al servidor** , seleccione el tipo de servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , seleccione o localice el servidor al que desea conectarse y haga clic en **Conectar**.  
  
4.  En el Explorador de objetos, haga clic con el botón derecho en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y, después, haga clic en **Ver registros**.  
  
5.  Para ver los eventos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , seleccione **SQL Server Integration Services**. La opción **Eventos NT** se selecciona automáticamente y se borra con la opción **SQL Server Integration Services** .  
  
### <a name="to-view-service-events-for-integration-services-in-windows-event-viewer"></a>Para ver los registros de eventos para Integration Services en el Visor de eventos de Windows  
  
1.  En el **Panel de control**, si utiliza la Vista clásica, haga clic en **Herramientas administrativas** o bien, si utiliza la Vista por categorías, haga clic en **Rendimiento y mantenimiento** y, a continuación, en **Herramientas administrativas**.  
  
2.  Haga clic en **Visor de eventos**.  
  
3.  En el cuadro de diálogo **Visor de eventos** , haga clic en **Aplicación**.  
  
4.  En el complemento **Aplicación** , localice una entrada en la columna **Origen** que contenga el valor **SQLISService**, haga clic con el botón derecho en la entrada y, después, haga clic en **Propiedades**.  
  
5.  O bien, haga clic en la flecha arriba o abajo para ver el evento anterior o el siguiente.  
  
6.  Si lo desea, puede hacer clic en el icono Copiar al Portapapeles para copiar la información del evento.  
  
7.  Elija si desea ver los datos del evento en bytes o en palabras.  
  
8.  Haga clic en **OK**.  
  
9. En el menú **Archivo** , haga clic en **Salir** para salir del cuadro de diálogo **Visor de eventos** .  
 
## <a name="related-tasks"></a>Related Tasks  
 Para obtener información sobre cómo ver las entradas de registro, vea [Eventos registrados por un paquete de Integration Services](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
