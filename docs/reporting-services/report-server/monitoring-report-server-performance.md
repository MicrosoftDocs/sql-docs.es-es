---
title: Supervisar el rendimiento del servidor de informes | Microsoft Docs
description: Obtenga información sobre cómo supervisar el rendimiento del servidor de informes para evaluar su actividad, ver tendencias, diagnosticar cuellos de botella y recopilar datos sobre la configuración del sistema.
ms.date: 02/12/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- report servers [Reporting Services], performance
- counters [Reporting Services]
- monitoring performance [Reporting Services]
- SQL Server Reporting Services, performance
- performance [Reporting Services]
- Reporting Services, performance
ms.assetid: c1bc13d4-8297-4daf-bb19-4c1e5ba292a6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f24b3bb19150271532561d691a1ed1f4c89dfbc5
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838095"
---
# <a name="monitoring-report-server-performance"></a>Supervisar el rendimiento del servidor de informes

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

  Utilice las herramientas de supervisión del rendimiento para supervisar el rendimiento del servidor de informes a fin de evaluar la actividad del servidor, observar tendencias, diagnosticar cuellos de botella del sistema y recopilar datos que pueden ayudar a determinar si la configuración actual del sistema es suficiente. Para optimizar el rendimiento del servidor, puede especificar la frecuencia con que se recicla el dominio de aplicación del servidor de informes. Para obtener más información, vea [Configurar la memoria disponible para las aplicaciones del servidor de informes](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).  
  
## <a name="sources-of-performance-data"></a>Orígenes de datos de rendimiento  
 Utilice una combinación de tecnología y herramientas para obtener información completa sobre el rendimiento del sistema. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows Server proporcionan información de rendimiento mediante las siguientes herramientas:  
  
-   Administrador de tareas  
  
-   Visor de eventos  
  
-   Monitor de rendimiento  
  
 El Administrador de tareas brinda información sobre los programas y los procesos que se ejecutan en el equipo. Puede utilizar el Administrador de tareas para supervisar indicadores clave del rendimiento del servidor de informes. También puede evaluar la actividad de ejecutar procesos y visualizar gráficos y datos con relación al uso de la CPU y de la memoria. Para obtener información sobre cómo utilizar el Administrador de tareas, vea la documentación del producto de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 Puede usar el Monitor de rendimiento y el Visor de eventos para crear registros y alertas sobre el procesamiento de informes y el consumo de recursos. Para obtener información sobre los eventos de Windows generados por [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Registro de aplicación Windows](../../reporting-services/report-server/windows-application-log.md). Para obtener información sobre el Monitor de rendimiento, vea la sección "Contadores de rendimiento de Windows" más adelante en este artículo.  
  
 Las utilidades de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md) y [Eventos extendidos](../../relational-databases/extended-events/extended-events.md), también proporcionan información sobre la base de datos del servidor de informes y las bases de datos temporales que se usan para la administración de sesiones y el almacenamiento en caché.  
  
## <a name="windows-performance-counters"></a>Contadores de rendimiento de Windows  
 La supervisión de determinados contadores de rendimiento permite:  
  
-   Calcular los requisitos del sistema necesarios para admitir una carga de trabajo prevista.  
  
-   Crear una línea base de rendimiento para medir el efecto de los cambios de configuración o las actualizaciones de aplicaciones.  
  
-   Supervisar el rendimiento de la aplicación en determinadas cargas, independientemente de que éstas se hayan generado de forma real o artificial.  
  
-   Comprobar que las actualizaciones de hardware producen el efecto deseado en el rendimiento.  
  
-   Validar que los cambios realizados en la configuración del sistema producen el efecto deseado en el rendimiento.  

  
## <a name="reporting-services-performance-objects"></a>Objetos de rendimiento en Reporting Services  
SQL Server 2016 Reporting Services incluye los siguientes objetos de rendimiento:  
  
-   **MSRS 2016 Web Service** (Servicio web de MSRS 2016) y **MSRS 2016 Web Service SharePoint Mode** (Modo de SharePoint del servicio web de MSRS 2016) para supervisar el rendimiento del servidor de informes. Estos objetos de rendimiento incluyen una colección de contadores que se usan para realizar el seguimiento del procesamiento del servidor de informes que se suele iniciar mediante operaciones interactivas de visualización de informes. Estos contadores se restablecen siempre que el servicio web del servidor de informes se detiene o se recicla.  
  
-   **MSRS 2016 Windows Service** (Servicio de Windows de MSRS 2016) y **MSRS 2016 Windows Service SharePoint Mode** (Modo de SharePoint del servicio de Windows de MSRS 2016) para supervisar las operaciones programadas y la entrega de informes. Estos objetos de rendimiento incluyen una colección de contadores que se usan para realizar el seguimiento del procesamiento de informes que se inicia mediante operaciones programadas. Entre las operaciones programadas se incluyen la suscripción y la entrega, las instantáneas de ejecución de informes y el historial del informe.  
  
-   **ReportServer:Service** y **ReportServerSharePoint:Service** para supervisar los eventos relacionados con HTTP y la administración de memoria. Estos contadores son específicos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y realizan el seguimiento de los eventos relacionados con HTTP para el servidor de informes, como solicitudes, conexiones e intentos de inicio de sesión. Este objeto de rendimiento también incluye contadores relacionados con la administración de memoria.  
  
 Si tiene varias instancias de servidor de informes en un solo equipo, puede supervisarlas conjuntamente o por separado. Elija las instancias que desea incluir al agregar un contador. Para obtener más información sobre cómo usar el Monitor de rendimiento (perfmon.msc) y agregar contadores, vea la documentación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] sobre el [Monitor de rendimiento de Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749249(v=ws.11)).  
  
## <a name="other-performance-counters"></a>Otros contadores de rendimiento  
 Los contadores de rendimiento personalizados de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] solo se proporcionan para los objetos de rendimiento de Reporting Services enumerados anteriormente. Los siguientes objetos de rendimiento de .NET Framework proporcionan datos adicionales de supervisión de rendimiento para el servidor de informes.
 
 > [!NOTE]
 > Power BI Report Server y SQL Server Reporting Services 2017 (y versiones posteriores) no incluyen objetos de rendimiento de Reporting Services. Hay [contadores de rendimiento de.NET Framework](/dotnet/framework/debug-trace-profile/performance-counters) disponibles para proporcionar datos de supervisión de rendimiento para el servidor de informes. 
 
|Objeto de rendimiento|Notas|  
|------------------------|-----------|  
|**.NET CLR Data** y **.NET CLR Memory**|El portal web utiliza los contadores de rendimiento de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Para obtener más información, descargue la guía sobre cómo [mejorar el rendimiento y la escalabilidad de aplicaciones de .NET](https://www.microsoft.com/download/details.aspx?id=11711).|  
|**Proceso**|Agregue los contadores de rendimiento **Elapsed Time** y **ID Process** para que una instancia de ReportingServicesService realice el seguimiento del tiempo de funcionamiento de un proceso mediante el identificador del proceso.|  
  
## <a name="sharepoint-events"></a>Eventos de SharePoint  
 Además de los objetos de rendimiento de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , también puede configurar eventos de SharePoint si está ejecutando un servidor de informes en modo integrado de SharePoint y ha configurado el entorno de informes para utilizar un producto de SharePoint. En esta sección, utilice Eventos para un servidor de informes en modo integrado de SharePoint para revisar los eventos de diagnóstico que pueden proporcionar información útil si el entorno de informes está integrado en SharePoint.  
  
## <a name="in-this-section"></a>En esta sección  
 [Contadores de rendimiento para los objetos de rendimiento MSRS 2016 Web Service y MSRS 2016 Windows Service (modo nativo)](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)  
 Describe los contadores de rendimiento que utiliza el servicio web del servidor de informes.  
  
 [Contadores de rendimiento para los objetos de rendimiento MSRS 2016 Web Service SharePoint Mode y MSRS 2016 Windows Service SharePoint Mode (modo de SharePoint)](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
 Describe los contadores de rendimiento que utiliza el servicio de Windows del servidor de informes.  
  
 [Contadores de rendimiento de los objetos ReportServer:Service y ReportServerSharePoint:Service](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
 Describe los contadores de rendimiento de eventos relacionados con HTTP y la administración de memoria en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Configurar la memoria disponible para las aplicaciones del servidor de informes](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)   
 [Servidor de informes de Reporting Services &#40;modo nativo&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Herramientas de Reporting Services](../../reporting-services/tools/reporting-services-tools.md)  
