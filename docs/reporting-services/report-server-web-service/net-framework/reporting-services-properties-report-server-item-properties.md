---
title: Propiedades de los elementos del servidor de informes | Microsoft Docs
description: Las propiedades de elemento son específicas de los elementos de la base de datos del servidor de informes. Los elementos incluyen informes, informes vinculados, carpetas, recursos, modelos y orígenes de datos.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- report servers [Reporting Services], properties
- item-specific properties [Reporting Services]
- report items [Reporting Services], properties
- items [Reporting Services], properties
ms.assetid: 21edec6d-9897-48fb-8c75-182305b1dbdb
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4d4db0c4bdef91630cd2bf20b215f7a6b26d714b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063663"
---
# <a name="reporting-services-properties---report-server-item-properties"></a>Propiedades de Reporting Services: propiedades de los elementos del servidor de informes
  Las propiedades de los elementos son específicas de los elementos de la base de datos del servidor de informes. Tales elementos incluyen informes, informes vinculados, carpetas, recursos, modelos y orígenes de datos.  
  
 Los nombres de propiedad de los elementos siguientes están reservados. No puede crear propiedades definidas por el usuario con el mismo nombre. Puede leer o modificar muchas de estas propiedades utilizando los métodos de servicio web del servidor de informes.  
  
## <a name="item-properties"></a>Propiedades de los elementos  
 Las propiedades siguientes se aplican a todos los elementos de la base de datos del servidor de informes.  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|**CreatedBy**|Nombre del usuario que agregó originalmente el elemento para la base de datos del servidor de informes.|  
|**CreationDate**|Fecha y hora cuando el elemento se agregó a la base de datos del servidor de informes.|  
|**Descripción**|Descripción del elemento.|  
|**Oculto**|Valor que indica si el elemento está visible y disponible para los usuarios.|  
|**Id**|Identificador de un elemento de la base de datos del servidor de informes.|  
|**ModifiedBy**|Nombre del usuario que modificó el elemento de la base de datos del servidor de informes en último lugar.|  
|**ModifiedDate**|Fecha y hora cuando el usuario modificó el elemento por última vez.|  
|**Nombre**|Nombre de un elemento de la base de datos del servidor de informes.|  
|**Path**|Nombre de la ruta de acceso completa del elemento. La ruta de acceso de cualquier elemento de la base de datos del servidor de informes tiene una longitud máxima de 260 caracteres.|  
|**Tamaño**|El tamaño, en bytes, de un elemento en la base de datos del servidor de informes.|  
|**Tipo**|Tipo de un elemento de la base de datos del servidor de informes.|  
|**VirtualPath**|Ruta de acceso virtual de un elemento de la base de datos del servidor de informes. El valor de la propiedad <xref:ReportService2010.CatalogItem.VirtualPath%2A> es la ruta de acceso en la que un usuario espera ver el elemento. Por ejemplo, un informe denominado informe1, que se encuentra en la carpeta personal Mis informes, tiene la ruta de acceso virtual /Mis informes. La ruta de acceso real del elemento es /Usuarios/nombredeusuario/Mis informes.|  
  
## <a name="folder-properties"></a>Propiedades de carpeta  
 Además de las propiedades de los elementos enumerados previamente, la propiedad siguiente se aplica a las carpetas en la base de datos del servidor de informes.  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|**Reserved**|Valor que devuelve el método <xref:ReportService2010.ReportingService2010.GetProperties%2A> para las carpetas que reserva el servidor de informes. Las carpetas reservadas incluyen Usuarios, Mis informes y /. Las carpetas reservadas no se pueden modificar ni quitar.|  
  
## <a name="report-properties"></a>Propiedades de informe  
 Además de las propiedades de los elementos enumerados previamente, las propiedades siguientes se aplican a las carpetas en la base de datos del servidor de informes.  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|**Lenguaje**|Lenguaje utilizado en un informe. El valor es un código de idioma definido en la especificación del Grupo de trabajo de ingeniería de Internet (IETF) RFC1766. La primera parte es una designación de dos caracteres del lenguaje de bajo nivel. La segunda parte se separa con un guión y designa la variación o dialecto del lenguaje. Si no se especifica un valor en el elemento **Style** asociado al elemento **Body** en la definición de informe, el valor predeterminado es el lenguaje del servidor de informes.|  
|**ReportProcessingTimeout**|Tiempo de espera, en segundos, para un informe individual. Si se establece este valor, el servidor de informes intenta detener el procesamiento de un informe cuando ha transcurrido el tiempo especificado. Los valores válidos para esta propiedad van desde **-1** a **2** **147** **483** **647**. Si el valor es **-1**, no se agota el tiempo de espera del informe durante el procesamiento. Si el valor es **null**, el valor de propiedad del sistema **ReportProcessingTimeout** se usa para el tiempo de espera del procesamiento del informe. El valor predeterminado es **null**. Para más información, vea [Propiedades del sistema del servidor de informes](../../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).|  
|**ExecutionDate**|Fecha y hora cuando una instantánea de informe se creó por última vez para un informe.|  
|**CanRunUnattended**|Valor que indica si un informe se puede ejecutar de forma desatendida. Si esta propiedad se establece en **true**, se definen valores predeterminados para los parámetros de informe y las credenciales del origen de datos se almacenan con el informe, o la opción de recuperación de credenciales se establece en **None**. Si esta propiedad se establece en **false**, no se cumplen los requisitos previos para ejecutar un informe desatendido. Para más información, vea [Configurar la cuenta de ejecución desatendida &#40;Administrador de configuración del servidor de informes&#41;](../../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).|  
|**HasParameterDefaultValues**|Valor que indica si el informe tiene establecidos los valores predeterminados válidos para todos los parámetros de informe. El valor también es **true** si un informe no tiene los parámetros de informe. Si esta propiedad se establece en **false**, uno o más parámetros de informe no tienen un valor predeterminado válido.|  
|**HasDataSourceCredentials**|Un valor que indica que la opción de recuperación de credenciales establecida para todos los orígenes de datos asociados al informe es **None** o **Store**. Si esta propiedad se establece en **false**, una opción de recuperación de credenciales establecida para uno de los orígenes de datos asociados al informe es **Integrated** o **Prompt**.|  
|**IsSnapshotExecution**|Valor que indica si el informe es una instantánea.|  
|**HasScheduleReadyDataSources**|Valor que indica si los orígenes de datos de un informe se configuran para admitir la ejecución programada. Si esta propiedad se establece en **false**, los usuarios no pueden suscribirse al informe.|  
  
## <a name="resource-properties"></a>Propiedades de los recursos  
 Además de las propiedades de los elementos enumerados previamente, la propiedad siguiente se aplica a los recursos en la base de datos del servidor de informes.  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|**MimeType**|Tipo MIME de un recurso en la base de datos del servidor de informes.|  
  
## <a name="see-also"></a>Consulte también  
 [Creación de aplicaciones con el servicio web y .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Servicio web del servidor de informes](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [Referencia técnica &#40;SSRS&#41;](../../../reporting-services/technical-reference-ssrs.md)  
  
  
