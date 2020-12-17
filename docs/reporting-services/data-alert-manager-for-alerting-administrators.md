---
title: Administrador de alertas de datos para administradores de alertas | Microsoft Docs
description: Obtenga información sobre cómo administrar las alertas a través del Administrador de alertas de datos para SharePoint en SQL Server Reporting Services.
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- managing, alerts
- managing, data alerts
ms.assetid: 32fd968f-1c0c-4ba8-851c-8a3b5e1fbbf2
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 7bd656cc4b543ecb2c291adcd9745f3c4b1c79bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484347"
---
# <a name="data-alert-manager-for-alerting-administrators"></a>Administrador de alertas de datos para administradores de alertas

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE [ssrs-appliesto-not-2017](../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../includes/ssrs-previous-versions.md)]

SQL Server Reporting Services proporciona el Administrador de alertas de datos para SharePoint, que permite a los administradores de alertas administrar las alertas de datos. Los administradores de alertas pueden ver información sobre todas las alertas guardadas en el sitio y eliminarlas. En la imagen siguiente se muestran las características disponibles en el Administrador de alertas de datos para los administradores de alertas de SharePoint.

![Administrador de alertas para administradores de sitios de SharePoint](../reporting-services/media/rs-alertmanagersite.gif "Administrador de alertas para administradores de sitios de SharePoint")

> [!NOTE]
> La integración de Reporting Services con SharePoint ya no está disponible a partir de SQL Server 2016.

 Cuando se habilita el sitio para las alertas de datos, se crean dos páginas de SharePoint, MyDataAlerts.aspx y SiteDataAlerts.aspx, que se agregan al sitio de SharePoint. SiteDataAlerts.aspx es el Administrador de alertas de datos para los administradores de alertas. Los administradores de alertas pueden abrir el Administrador de alertas de datos desde la página Configuración del sitio de SharePoint. Los administradores de alertas deben tener el permiso Administrar alertas de SharePoint para abrir el Administrador de alertas de datos.  
  
 También se puede abrir el Administrador de alertas de datos directamente mediante una dirección URL. A continuación se muestra la sintaxis de la dirección URL:  
  
 `http: //<site name>/_layouts/ReportServer/ SiteDataAlerts.aspx`  
  
> [!NOTE]  
>  Como administrador de alertas, puede conceder permiso a los trabajadores de la información para tener acceso a las características de alertas de datos de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Para obtener más información sobre los permisos necesarios, vea [Alertas de datos de Reporting Services](../reporting-services/reporting-services-data-alerts.md).  
  
##  <a name="viewing-data-alert-information"></a><a name="ViewingAlerts"></a> Ver información de alertas de datos  
 Cuando se instala y se configura Reporting Services en SharePoint, la página Configuración del sitio de SharePoint incluye las opciones de **Reporting Services** . Los administradores de alertas hacen clic en la opción **Administrar alertas de datos** en Reporting Services para abrir el Administrador de alertas de datos. En la imagen siguiente se muestra en qué lugar de la página Configuración del sitio se abre el Administrador de alertas de datos.  
  
 ![Sección Reporting Services de la página Configuración del sitio](../reporting-services/media/rs-sitesettings.gif "Sección Reporting Services de la página Configuración del sitio")  
  
 El Administrador de alertas de datos incluye una tabla con el nombre de la alerta, el nombre del informe, el nombre del propietario de la alerta, el número de veces que se envió la alerta, la última vez que se ejecutó la alerta, la última vez que se modificó la definición de la alerta y el estado del mensaje de alerta. Si la alerta no se puede generar o enviar, la columna de estado contiene información sobre el error que le ayudará a solucionar los problemas relacionados con la alerta. Para más información, consulte [Administrar todas las alertas de datos de un sitio de SharePoint en el Administrador de alertas de datos](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md).  
  
 En la tabla siguiente se muestran datos de ejemplo de una tabla del Administrador de alertas de datos. Cuando se produce un error, el mensaje de error y el identificador de la entrada del registro (un GUID) se incluyen en el campo **Estado** de la tabla.  
  
|Nombre de la alerta|Nombre del informe|Creado por|Alertas enviadas|Última ejecución|Modificado por última vez|Estado|  
|----------------|-----------------|----------------|-----------------|--------------|-------------------|------------|  
|SalesQTR|SalesByTerritoryAndQTR|Lauren Johnson|4|6/12/2011|6/1/2011|La última alerta se ejecutó correctamente y se envió.|  
|UnitsSold|ProductsSalesByQTR|Michael Blythe|2|7/1/2011|6/28/2011|La última alerta se ejecutó correctamente, pero los datos no se modificaron y no se envió la alerta.|  
|InventoryCount|StockStatusByQTR|Lauren Johnson|7|7/10/2011|7/2/2011|\<error message>El archivo de registro contiene información detallada sobre el error. Consulte la entrada del registro con el identificador: \<GUID>.|  
|TopPromotion|PromotionTracking|Cristian Petculescu|0||5/23/2011|Alerta creada.|  
  
 Para obtener más información, vea [Administrar todas las alertas de datos de un sitio de SharePoint en el Administrador de alertas de datos](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md).  
  
 Puede ver todas las alertas creadas por los usuarios del sitio. Puede seleccionar un usuario y luego elegir si desea ver todas sus alertas o solo las alertas de un informe determinado.  
  
  
##  <a name="delete-data-alerts"></a><a name="DeleteAlerts"></a> Eliminar alertas de datos  
 Las definiciones de alertas se eliminan desde el Administrador de alertas de datos. Cada definición de alerta de datos tiene un propietario, que es el usuario de SharePoint que la creó. Los propietarios solo pueden eliminar las definiciones de alertas que han creado ellos mismos. Para obtener más información, vea [Administrar mis alertas de datos en el Administrador de alertas de datos](../reporting-services/manage-my-data-alerts-in-data-alert-manager.md).  
  
 Los administradores de alertas de SharePoint pueden ver la lista de las definiciones de alertas creadas por todos los usuarios del sitio y eliminarlas. Para obtener más información, vea [Administrar todas las alertas de datos de un sitio de SharePoint en el Administrador de alertas de datos](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md).  
  
 Después de eliminar una definición de alerta, no se envían más alertas. Sin embargo, si consulta la base de datos de alertas puede que todavía exista la definición de la alerta. El servicio de alertas realiza limpiezas según una programación, y la definición de la alerta se eliminará definitivamente durante la limpieza siguiente. El intervalo de limpieza predeterminado es de 20 minutos. Este y otros intervalos de limpieza son configurables. Para obtener más información, vea [Alertas de datos de Reporting Services](../reporting-services/reporting-services-data-alerts.md).  
  
  
##  <a name="related-tasks"></a><a name="HowTo"></a> Tareas relacionadas  
 En esta sección se enumera un procedimiento que muestra cómo administrar las alertas.  
  
-   [Administrar todas las alertas de datos de un sitio de SharePoint en el Administrador de alertas de datos](../reporting-services/manage-all-data-alerts-on-a-sharepoint-site-in-data-alert-manager.md)  

## <a name="see-also"></a>Consulte también

[Alertas de datos de Reporting Services](../reporting-services/reporting-services-data-alerts.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
