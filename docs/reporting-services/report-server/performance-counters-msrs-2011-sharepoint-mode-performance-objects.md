---
title: Contadores de rendimiento para los objetos de rendimiento de modo de SharePoint de MSRS 2016 | Microsoft Docs
description: Obtenga información sobre los contadores de rendimiento para los objetos de rendimiento MSRS 2016 Web Service SharePoint Mode y MSRS 2016 Windows Service SharePoint Mode.
ms.date: 02/17/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- counters [Reporting Services]
- Report Server Windows service, performance counters
- RS Windows Service performance object
- Scheduling and Delivery Processor performance object [Reporting Services]
- performance [Reporting Services]
ms.assetid: 70bf6980-7845-4ab5-8b2a-ebf526d811a6
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: cfa6fbdf2a4f834accd2ec197a8933371a5a6501
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "100646347"
---
# <a name="performance-counters-msrs-2016-sharepoint-mode-performance-objects"></a>Contadores de rendimiento para los objetos de rendimiento de modo de SharePoint de MSRS 2016
  En este tema se describen los contadores de rendimiento para los objetos de rendimiento **MSRS 2016 Web Service SharePoint Mode** (Modo de SharePoint del servicio web de MSRS 2016) y **MSRS 2016 Windows Service SharePoint Mode** (Modo de SharePoint del servicio de Windows de MSRS 2016) que forman parte de una implementación en modo de SharePoint de SQL Server Reporting Services 2016.  
  
> [!NOTE]  
>  Estos objetos de rendimiento supervisan los eventos en el servidor de informes local. Si ejecuta un servidor de informes en una implementación escalada, los recuentos se aplican al servidor actual y no a la implementación escalada completa.  
  
 Los objetos de rendimiento están disponibles en el Monitor de rendimiento de Windows (**Perfmon.exe**). Para obtener más información, consulte la documentación de Windows. [Generar perfiles en tiempo de ejecución](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx).  
  
 Para obtener información relacionada con los contadores de rendimiento y los servidores de informes en modo nativo, vea el artículo sobre los [contadores de rendimiento para los objetos de rendimiento MSRS 2016 Web Service y MSRS 2016 Windows Service (modo nativo)](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md).
  
 En este tema:  
  
-   [Contadores de rendimiento para MSRS 2016 Web Service SharePoint Mode](#bkmk_webservice)  
  
-   [Contadores de rendimiento para MSRS 2016 Windows Service SharePoint Mode](#bkmk_windowsservice)  
  
-   [Usar cmdlets de PowerShell para devolver listas](#bkmk_powershell)  
  
##  <a name="msrs-2016-web-service-sharepoint-mode-performance-counters"></a><a name="bkmk_webservice"></a> Contadores de rendimiento para MSRS 2016 Web Service SharePoint Mode  
 El objeto de rendimiento **MSRS 2016 Web Service SharePoint Mode** supervisa el rendimiento del servidor de informes. Este objeto de rendimiento incluye una colección de contadores que se usan para realizar el seguimiento del procesamiento del servidor de informes que se suele iniciar mediante operaciones interactivas de visualización de informes. Cuando se configura este contador, se puede aplicar a todas las instancias de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o se pueden seleccionar instancias concretas. Estos contadores se restablecen siempre que [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] detiene el servicio web del servidor de informes.  
  
 En la tabla siguiente se enumeran los contadores que se incluyen con el objeto de rendimiento **MSRS 2016 Web Service SharePoint Mode**.  
  
|Contador|Descripción|  
|-------------|-----------------|  
|**Sesiones activas**|Número de sesiones activas. Este contador proporciona un recuento acumulativo de todas las sesiones del explorador que se generan a partir de las ejecuciones de informes, independientemente de si todavía están o no activas.<br /><br /> El contador se reduce cuando se quitan registros de sesiones. De forma predeterminada, las sesiones se quitan después de diez minutos de inactividad.|  
|**Aciertos de caché por segundo**|Número de solicitudes por segundo para informes en memoria caché. Se trata de solicitudes para informes que se vuelven a representar, no de solicitudes para informes procesados directamente desde la memoria caché. (Vea **Total de aciertos de caché** más adelante en este tema).|  
|**Aciertos de caché por segundo (modelos semánticos)**|Número de solicitudes por segundo para el modelo en memoria caché. Se trata de solicitudes para informes que se vuelven a representar, no de solicitudes para informes procesados directamente desde la memoria caché.|  
|**Errores de caché por segundo**|Número de solicitudes por segundo que no han podido devolver un informe de memoria caché. Utilice este contador para averiguar si los recursos utilizados para el almacenamiento en caché (disco o memoria) son suficientes.|  
|**Errores de caché por segundo (modelos semánticos)**|Número de solicitudes por segundo que no han podido devolver un modelo de memoria caché. Utilice este contador para averiguar si los recursos utilizados para el almacenamiento en caché (disco o memoria) son suficientes.|  
|**Solicitudes de primera sesión por segundo**|Número de nuevas sesiones de usuario que se inician desde la memoria caché del servidor de informes por segundo.|  
|**Aciertos de caché en memoria por segundo**|Número de veces por segundo que se recuperan informes de la memoria caché en memoria. La *caché en memoria* es una parte de la memoria caché que almacena informes en la memoria de la CPU. Cuando se usa la memoria caché en memoria, el servidor de informes no realiza una consulta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el contenido en memoria caché.|  
|**Errores de caché en memoria por segundo**|Número de veces por segundo que no se han podido recuperar informes de la memoria caché en memoria.|  
|**Solicitudes de siguiente sesión por segundo**|Número de solicitudes por segundo de informes que se abren en una sesión existente (como informes que se representan a partir de una instantánea de sesión).|  
|**Solicitudes de informe**|Número de informes actualmente activos que el servidor de informes está procesando.|  
|**Informes ejecutados por segundo**|Número de ejecuciones de informes logradas por segundo. Este contador proporciona estadísticas sobre el volumen de informes. Use este contador con **Solicitudes por segundo** para comparar la ejecución de informes con las solicitudes de informes que pueden devolverse desde la memoria caché.|  
|**Solicitudes por segundo**|Número de solicitudes por segundo realizadas al servidor de informes. Este contador realiza un seguimiento de todos los tipos de solicitudes que procesa el servidor de informes.|  
|**Total de aciertos de caché**|Número total de solicitudes de informes de la memoria caché después del inicio del servicio. Este contador se restablece siempre que [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] detiene el servicio web del servidor de informes.|  
|**Total de aciertos de caché (modelos semánticos)**|Número total de solicitudes del modelo de la memoria caché después del inicio del servicio. Este contador se restablece siempre que ASP.NET detiene el servicio web del servidor de informes.|  
|**Total de errores de caché**|Número total de veces que no se ha podido devolver un informe de la memoria caché después del inicio del servicio. Este contador se restablece siempre que [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] detiene el servicio web del servidor de informes. Utilice este contador para determinar si el espacio en disco y la memoria son suficientes.|  
|**Total de errores de caché (modelos semánticos)**|Número total de veces que no se ha podido devolver un modelo de la memoria caché después del inicio del servicio. Este contador se restablece siempre que ASP.NET detiene el servicio web del servidor de informes. Utilice este contador para determinar si el espacio en disco y la memoria son suficientes.|  
|**Total de aciertos de caché en memoria**|Número total de informes en memoria caché devueltos desde la memoria caché en memoria después del inicio del servicio. Este contador se restablece siempre que [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] detiene el servicio web del servidor de informes. La *caché en memoria* es una parte de la memoria caché que almacena informes en la memoria de la CPU. Cuando se usa la memoria caché en memoria, el servidor de informes no realiza una consulta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el contenido en memoria caché.|  
|**Total de errores de caché en memoria**|Número total de errores de memoria caché en la memoria caché en memoria después del inicio del servicio. Este contador se restablece siempre que [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] detiene el servicio web del servidor de informes.|  
|**Total de errores de procesamiento**|Número de errores del procesamiento de la solicitud de servicio web del servidor de informes.|  
|**Total de subprocesos rechazados**|Número total de subprocesos rechazados para el procesamiento asincrónico y tratados posteriormente como procesos sincrónicos en el mismo subproceso. Cada origen de datos se procesa en un subproceso. Si el volumen de subprocesos es superior a la capacidad, se rechazan subprocesos para el procesamiento asincrónico y, a continuación, se procesan en serie.|  
|**Total de informes ejecutados**|Número total de informes que se han ejecutado correctamente después del inicio del servicio. Este contador se restablece siempre que [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] detiene el servicio web del servidor de informes.|  
|**Total de solicitudes**|Número total de solicitudes efectuadas al servidor de informes después del inicio del servicio. Este contador se restablece siempre que [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] detiene el servicio web del servidor de informes.|  
  
##  <a name="msrs-2016-windows-service-sharepoint-mode-performance-counters"></a><a name="bkmk_windowsservice"></a> Contadores de rendimiento para MSRS 2016 Windows Service SharePoint Mode  
 El objeto de rendimiento **MSRS 2016 Windows Service SharePoint Mode** se usa para supervisar el servicio de Windows del servidor de informes. Este objeto de rendimiento incluye una colección de contadores que se usan para realizar el seguimiento del procesamiento de informes que se inicia mediante operaciones programadas. Entre las operaciones programadas se incluyen la suscripción y la entrega, las instantáneas de ejecución de informes y el historial del informe. Cuando se configura este contador, se puede aplicar a todas las instancias de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o se pueden seleccionar instancias concretas.  
  
 En la tabla siguiente se enumeran los contadores que se incluyen con el objeto de rendimiento **MSRS 2016 Windows Service SharePoint Mode**.  
  
|Contador|Descripción|  
|-------------|-----------------|  
|**Sesiones activas**|Número de sesiones activas almacenadas en la base de datos del servidor de informes. Este contador proporciona un recuento acumulativo de todas las sesiones del explorador que se pueden utilizar generadas a partir de suscripciones de informes, independientemente de si todavía están o no activas.|  
|**Alertas: longitud de la cola de eventos**||  
|**Alertas: eventos procesados - CreateSchedule**||  
|**Alertas: eventos procesados - DeleteSchedule**||  
|**Alertas: eventos procesados - DeliverAlert**||  
|**Alertas: eventos procesados - FireAlert**||  
|**Alertas: eventos procesados - FireSchedule**||  
|**Alertas: eventos procesados - GenerateAlert**||  
|**Alertas: eventos procesados - UpdateSchedule**||  
|**Vaciados de caché por segundo**|Número de vaciados de memoria caché por segundo.|  
|**Aciertos de caché por segundo**|Número de solicitudes por segundo para informes en memoria caché. Se trata de solicitudes para informes que se vuelven a representar, no de solicitudes para informes procesados directamente desde la memoria caché. (Vea **Total de aciertos de caché** más adelante en este tema).|  
|**Aciertos de caché por segundo (modelos semánticos)**|Número de solicitudes por segundo para los modelos en memoria caché.|  
|**Errores de caché por segundo**|Número de solicitudes por segundo que no han podido devolver un informe de memoria caché. Utilice este contador para averiguar si los recursos utilizados para el almacenamiento en caché (disco o memoria) son suficientes.|  
|**Errores de caché por segundo (modelos semánticos)**|Número de solicitudes por segundo que no han podido devolver un modelo de memoria caché. Utilice este contador para averiguar si los recursos utilizados para el almacenamiento en caché (disco o memoria) son suficientes.|  
|**Entregas por segundo**|Número de entregas de informes por segundo, desde cualquier extensión de entrega.|  
|**Eventos por segundo**|Número de eventos procesados por segundo. Entre los eventos que se supervisan se incluyen **SnapshotUpdated** y **TimedSubscription**.|  
|**Solicitudes de primera sesión por segundo**|Número de nuevas sesiones de ejecución de informe creadas por segundo.|  
|**Aciertos de caché en memoria por segundo**|Número de veces por segundo que se recuperan informes de la memoria caché en memoria. La *caché en memoria* es una parte de la memoria caché que almacena informes en la memoria de la CPU. Cuando se usa la memoria caché en memoria, el servidor de informes no realiza una consulta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el contenido en memoria caché.|  
|**Errores de caché en memoria por segundo**|Número de veces por segundo que no se pueden recuperar informes de la memoria caché en memoria.|  
|**Solicitudes de siguiente sesión por segundo**|Número de solicitudes por segundo de informes que se abren en una sesión existente (como informes que se representan a partir de una instantánea de sesión).|  
|**Solicitudes de informe**|Número de informes actualmente activos que el servidor de informes está procesando. Utilice este contador para evaluar la estrategia de almacenamiento en memoria caché. Es posible que haya bastantes más solicitudes que informes generados.|  
|**Informes ejecutados por segundo**|Número de informes generados correctamente por segundo.|  
|**Solicitudes por segundo**|Número total de solicitudes correctas que el servicio del servidor de informes procesó por segundo.|  
|**Actualizaciones de instantánea por segundo**|Número total de actualizaciones de instantánea de ejecución de informes por segundo.|  
|**Total de reciclados de dominio de aplicación**|Número total de ciclos de dominio de aplicación después del inicio del servicio Windows del servidor de informes.|  
|**Total de vaciados de caché**|Número total de actualizaciones de memoria caché del servidor de informes después del inicio del servicio. Este contador se restablece tras los reciclamientos de dominio de aplicación. Consulte **Vaciados de caché por segundo**.|  
|**Total de aciertos de caché**|Número total de solicitudes de informes procesadas directamente desde la memoria caché después del inicio del servicio Windows del servidor de informes. Este contador se restablece tras los reciclamientos de dominio de aplicación. Consulte **Aciertos de caché por segundo**.|  
|**Total de aciertos de caché (modelos semánticos)**|Número total de solicitudes de modelo procesadas directamente desde la memoria caché después del inicio del servicio Windows del servidor de informes. Este contador se restablece tras los reciclamientos de dominio de aplicación.|  
|**Total de errores de caché**|Número total de veces que no se ha podido devolver un informe de la memoria caché después del inicio del servicio Windows del servidor de informes. Este contador se restablece tras los reciclamientos de dominio de aplicación. Consulte **Errores de caché por segundo**.|  
|**Total de errores de caché (modelos semánticos)**|Número total de veces que no se ha podido devolver un modelo de la memoria caché después del inicio del servicio Windows del servidor de informes. Este contador se restablece tras los reciclamientos de dominio de aplicación.|  
|**Total de entregas**|Número total de informes entregados por el Procesador de entrega y programación para todas las extensiones de entrega. Este contador se restablece tras los reciclamientos de dominio de aplicación.|  
|**N.º total de eventos**|Número total de eventos después del inicio del servicio Windows del servidor de informes. Este contador se restablece tras los reciclamientos de dominio de aplicación.|  
|**Total de aciertos de caché en memoria**|Número total de informes en memoria caché devueltos desde la memoria caché en memoria después del inicio del servicio Windows del servidor de informes. Este contador se restablece tras los reciclamientos de dominio de aplicación.|  
|**Total de errores de caché en memoria**|Número total de errores de memoria caché en la memoria caché en memoria después del inicio del servicio. Este contador se restablece tras los reciclamientos de dominio de aplicación.|  
|**Total de errores de procesamiento**|Número de errores del procesamiento de la solicitud de servicio Windows del servidor de informes.|  
|**Total de subprocesos rechazados**|Número total de subprocesos rechazados para el procesamiento asincrónico y tratados posteriormente como un proceso sincrónico en el mismo subproceso. Con carga moderada o sobrecarga, este contador aumenta regularmente.|  
|**Total de informes ejecutados**|Número total de informes ejecutados.|  
|**Total de solicitudes**|Número total de informes que se han ejecutado correctamente después del inicio del servicio. Este contador se restablece tras los reciclamientos de dominio de aplicación.|  
|**Total de actualizaciones de instantánea**|Número total de actualizaciones de instantánea de ejecución de informes.|  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> Usar cmdlets de PowerShell para devolver listas  
 ![Contenido relacionado con PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenido relacionado con PowerShell")El script de Windows PowerShell siguiente devolverá los conjuntos de contadores en los que CounterSetName comienza con "msr".  
  
```  
get-counter -listset msr*  
Returns a list with the following information  
CounterSetName     : MSRS 2016 Windows Service SharePoint Mode  
CounterSetName     : MSRS 2016 Web Service SharePoint Mode  
```  
  
 El siguiente script de Windows PowerShell devolverá la lista de contadores de rendimiento para CounterSetName "MSRS 2016 Windows Service SharePoint Mode".  
  
```  
(get-counter -listset "MSRS 2016 Windows Service SharePoint Mode").paths  
```  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar el rendimiento del servidor de informes](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [Contadores de rendimiento para los objetos de rendimiento MSRS 2016 Web Service y MSRS 2016 Windows Service (modo nativo)](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [Contadores de rendimiento de los objetos ReportServer:Service y ReportServerSharePoint:Service](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
  
