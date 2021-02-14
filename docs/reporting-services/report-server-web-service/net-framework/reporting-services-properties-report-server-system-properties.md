---
title: Propiedades del sistema del servidor de informes | Microsoft Docs
description: Las propiedades del sistema son globales para la base de datos del servidor de informes. Puede leer o modificar muchas de estas propiedades utilizando los métodos de servicio web.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- report servers [Reporting Services], properties
- system-specific properties [Reporting Services]
ms.assetid: cd874117-00e5-4ae6-8629-eb9ba9f40478
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 17cfa96da435821c4f4dbba0f2493570f1ddcf27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063651"
---
# <a name="reporting-services-properties---report-server-system-properties"></a>Propiedades de Reporting Services: propiedades del sistema del servidor de informes
  Los siguientes nombres de propiedades de sistema están reservados. No puede crear propiedades definidas por el usuario con el mismo nombre. Puede leer o modificar muchas de estas propiedades utilizando los métodos de servicio web.  
  
## <a name="properties"></a>Propiedades  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|SiteName|Nombre del sitio del servidor de informes mostrado en la interfaz de usuario. El valor predeterminado es **Microsoft Report Server**. Esta propiedad puede ser una cadena vacía. La longitud máxima es de 8000 caracteres.|  
|SystemSnapshotLimit|Número máximo de instantáneas almacenadas para un informe. Los valores válidos para esta propiedad van desde **-1** a **2** **147** **483** **647**. Si el valor es **-1**, no hay ningún límite de instantáneas.|  
|SystemReportTimeout|El valor de tiempo de espera de procesamiento de informes predeterminado, en segundos, para todos los informes administrados en el espacio de nombres del servidor de informes. Este valor se puede invalidar en el nivel de informe. Si se establece esta propiedad, el servidor de informes intenta detener el procesamiento de un informe cuando ha expirado el tiempo especificado. Los valores válidos para esta propiedad van desde **-1** a **2** **147** **483** **647**. Si el valor es **-1**, no se agota el tiempo de espera de los informes del espacio de nombres durante el procesamiento. El valor predeterminado es **1800**.|  
|UseSessionCookies|Indica si el servidor de informes debería usar cookies de sesión al comunicarse con exploradores cliente. El valor predeterminado es **true**.|  
|SessionTimeout|El período, en segundos, que una sesión permanece activa. El valor predeterminado es **600**.|  
|EnableMyReports|Indica si la característica Mis informes está habilitada. El valor **true** indica que la característica está habilitada.|  
|MyReportsRole|El nombre del rol que se usa al crear directivas de seguridad en las carpetas Mis informes del usuario. El valor predeterminado es **My Reports Role**.|  
|EnableExecutionLogging|Indica si el registro de ejecución de informes está habilitado. El valor predeterminado es **true**.|  
|ExecutionLogDaysKept|Número de días que mantener la información de ejecución de informes en el registro de ejecución. Los valores válidos de esta propiedad son de **0** a **2**,**147**,**483**,**647**. Si el valor es **0** no se eliminan las entradas de la tabla del registro de ejecución. El valor predeterminado es **60**.|  
|SnapshotCompression|Define la manera en la que se comprimen las instantáneas. El valor predeterminado es **SQL**. Los valores válidos son los siguientes:<br /><br /> **SQL** = las instantáneas se comprimen cuando se almacenan en la base de datos del servidor de informes. Éste es el comportamiento actual.<br /><br /> **None =** no se comprimen las instantáneas.<br /><br /> **All** = las instantáneas se comprimen para todas las opciones de almacenamiento, entre las que se incluyen la base de datos del servidor de informes o el sistema de archivos.|  
|EnableClientPrinting|Determina si el control ActiveX RSClientPrint está disponible para descargarlo del servidor de informes. Los valores válidos son **true** y **false**. El valor predeterminado es **true**. Para más información sobre opciones de configuración adicionales necesarias para este control, vea [Habilitar y deshabilitar la impresión del lado cliente para Reporting Services](../../../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md).|  
|EnableIntegratedSecurity|Determina si se admite la seguridad integrada para las conexiones de origen de datos del informe. El valor predeterminado es **True**. Los valores válidos son los siguientes:<br /><br /> **True** = la seguridad integrada está habilitada.<br /><br /> **False** = la seguridad integrada no está habilitada. No se ejecutarán los orígenes de datos de informes que estén configurados para usar la seguridad integrada.|  
|EnableRemoteErrors|Incluye información de errores externa (por ejemplo, sobre los orígenes de datos de informe) con los mensajes de error que se devuelven para los usuarios que solicitan informes de los equipos remotos. Los valores válidos son **true** y **false**. El valor predeterminado es **false**. Para más información, vea [Habilitar errores remotos &#40;Reporting Services&#41;](../../../reporting-services/report-server/enable-remote-errors-reporting-services.md).|  
  
## <a name="see-also"></a>Consulte también  
 <xref:ReportService2010.ReportingService2010.GetSystemProperties%2A>   
 <xref:ReportService2010.ReportingService2010.SetSystemProperties%2A>   
 [Creación de aplicaciones con el servicio web y .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Servicio web del servidor de informes](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Referencia técnica &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
