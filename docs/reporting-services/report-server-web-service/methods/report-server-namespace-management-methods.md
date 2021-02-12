---
title: Métodos de administración de los espacios de nombres del servidor de informes | Microsoft Docs
description: El servicio web de administración del servidor de informes contiene métodos que puede utilizar para administrar los informes, carpetas y recursos en la base de datos del servidor de informes.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- reports [Reporting Services], managing
- management methods [Reporting Services]
- methods [Reporting Services], about methods
- methods [Reporting Services]
ms.assetid: 2aa43ce9-f51e-408a-8ce0-b40d3dd62561
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: fab863f46d1b5707359c64577e6cffde4a6e2ea2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063341"
---
# <a name="report-server-namespace-management-methods"></a>Métodos de administración de los espacios de nombres del servidor de informes
  El servicio web de administración del servidor de informes contiene métodos que puede utilizar para administrar los informes, carpetas y recursos en la base de datos del servidor de informes.  
  
|Método|Acción|  
|------------|------------|  
|<xref:ReportService2010.ReportingService2010.CancelJob%2A>|Cancela la ejecución de un trabajo.|  
|<xref:ReportService2010.ReportingService2010.CreateFolder%2A>|Agrega una carpeta a la base de datos del servidor de informes o a la biblioteca de SharePoint.|  
|<xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A>|Agrega un nuevo elemento a una base de datos del servidor de informes o a la biblioteca de SharePoint. Este método se aplica a los tipos de elemento **Report**, **Model**, **Dataset**, **Component**, **Resource** y **DataSource**.|  
|M:ReportService2010.ReportingService2010.CreateReportEditSession(System.String, System.String,System.Byte[],ReportService2010.Warning[]@)|Crea una nueva sesión de edición de informes.|  
|<xref:ReportService2010.ReportingService2010.DeleteItem%2A>|Quita un elemento de la base de datos del servidor de informes o biblioteca de SharePoint.|  
|<xref:ReportService2010.ReportingService2010.FindItems%2A>|Devuelve los elementos de la base de datos del servidor de informes o biblioteca de SharePoint que coinciden con el criterio de búsqueda especificado.|  
|<xref:ReportService2010.ReportingService2010.FireEvent%2A>|Desencadena un evento basado en los parámetros proporcionados.|  
|<xref:ReportService2010.ReportingService2010.GetExtensionSettings%2A>|Devuelve una lista de valores para una extensión determinada.|  
|<xref:ReportService2010.ReportingService2010.GetItemType%2A>|Recupera el tipo de un elemento de la base de datos del servidor de informes o biblioteca de SharePoint, si el elemento existe.|  
|<xref:ReportService2010.ReportingService2010.GetProperties%2A>|Devuelve los valores de una o más propiedades en un elemento de la base de datos del servidor de informes o biblioteca de SharePoint.|  
|<xref:ReportService2010.ReportingService2010.GetItemDefinition%2A>|Recupera la definición o contenido para un elemento. Este método se aplica a los tipos de elemento **Report**, **Model**, **Dataset**, **Component**, **Resource** y **DataSource**.|  
|<xref:ReportService2010.ReportingService2010.GetItemReferences%2A>|Devuelve una lista de referencias de elemento de catálogo asociadas a un elemento.|  
|<xref:ReportService2010.ReportingService2010.GetReportServerConfigInfo%2A>|Devuelve información de la instancia del servidor de informes conectada o de todas las instancias del servidor de informes de una implementación escalada.|  
|<xref:ReportService2010.ReportingService2010.GetSystemProperties%2A>|Devuelve una o más propiedades del sistema.|  
|<xref:ReportService2010.ReportingService2010.ListChildren%2A>|Obtiene una lista de elementos secundarios de una carpeta especificada.|  
|<xref:ReportService2010.ReportingService2010.ListDatabaseCredentialRetrievalOptions%2A>|Devuelve una lista de opciones de recuperación de credenciales admitidas.|  
|<xref:ReportService2010.ReportingService2010.ListEvents%2A>|Devuelve una lista de extensiones de evento como aparecen en el archivo de configuración del servidor de informes.|  
|<xref:ReportService2010.ReportingService2010.ListJobs%2A>|Devuelve una lista de los trabajos que se ejecutan en el servidor de informes.|  
|<xref:ReportService2010.ReportingService2010.ListExtensions%2A>|Devuelve una lista de las extensiones que están configuradas para un tipo de extensión determinado.|  
|<xref:ReportService2010.ReportingService2010.ListExtensionTypes%2A>|Devuelve una lista de tipos de extensión admitidos.|  
|<xref:ReportService2010.ReportingService2010.ListItemTypes%2A>|Devuelve una lista de los tipos de elemento del catálogo admitidos.|  
|<xref:ReportService2010.ReportingService2010.ListJobActions%2A>|Devuelve una lista de acciones de trabajo admitidas.|  
|<xref:ReportService2010.ReportingService2010.ListJobStates%2A>|Devuelve una lista de estados de trabajo admitidos.|  
|<xref:ReportService2010.ReportingService2010.ListJobTypes%2A>|Devuelve una lista de tipos de trabajo admitidos.|  
|<xref:ReportService2010.ReportingService2010.ListParents%2A>|Recupera los elementos primarios para el elemento determinado.|  
|<xref:ReportService2010.ReportingService2010.ListSecurityScopes%2A>|Devuelve una lista de los ámbitos de seguridad admitidos.|  
|<xref:ReportService2010.ReportingService2010.Logoff%2A>|Cierra la sesión del usuario actual que realiza solicitudes de servicio web. Este método solo se aplica al modo nativo.|  
|<xref:ReportService2010.ReportingService2010.LogonUser%2A>|Inicia la sesión de un usuario y autentica una solicitud de usuario en el servicio web del servidor de informes. Este método solo se aplica al modo nativo.|  
|<xref:ReportService2010.ReportingService2010.SetItemReferences%2A>|Establece los elementos de catálogo asociados a un elemento.|  
|<xref:ReportService2010.ReportingService2010.MoveItem%2A>|Mueve y/o cambia el nombre de un elemento.|  
|<xref:ReportService2010.ReportingService2010.SetProperties%2A>|Establece una o más propiedades de un elemento.|  
|<xref:ReportService2010.ReportingService2010.SetItemDefinition%2A>|Establece la definición o el contenido para un elemento especificado. Este método se aplica a los tipos de elemento **Report**, **Model**, **Dataset**, **Component**, **Resource** y **DataSource**.|  
|<xref:ReportService2010.ReportingService2010.SetSystemProperties%2A>|Establece una o varias propiedades del sistema en el servidor de informes o la granja de SharePoint.|  
|<xref:ReportService2010.ReportingService2010.ValidateExtensionSettings%2A>|Valida la configuración de la extensión de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].|  
  
## <a name="see-also"></a>Consulte también  
 [Creación de aplicaciones con el servicio web y .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Servicio web del servidor de informes](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Métodos del servicio web del servidor de informes](../../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)   
 [Referencia técnica &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
