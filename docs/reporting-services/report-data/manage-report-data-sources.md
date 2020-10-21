---
title: Administrar orígenes de datos de informe | Microsoft Docs
description: Obtenga información sobre la administración de orígenes de datos de informe, incluida la forma de conectarse a los orígenes de datos externos a los que se hace referencia en un informe.
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services], data
- published reports [Reporting Services], data source connections
- shared data sources [Reporting Services]
- data sources [Reporting Services], managing
ms.assetid: 0475aded-c8fe-4337-a2b5-4df0ec4c46af
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 85f40936b589ac5a1f90ec47a81756998c5e9db7
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934926"
---
# <a name="manage-report-data-sources"></a>Administrar orígenes de datos de informe
  En [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], los informes, los modelos de informe y las suscripciones controladas por datos recuperan datos desde orígenes de datos externos. Para conectarse a un origen de datos externo, un servidor de informes utiliza la información de conexión de origen de datos cuya definición o referencia se halla en el informe, el modelo o la suscripción. Las propiedades de conexión de origen de datos siempre se definen con el informe o modelo al crearlo, pero se pueden administrar independientemente una vez que el informe o el modelo se publican en un servidor de informes.  
  
 Si implementó el servidor de informes en modo integrado de SharePoint, para administrar los orígenes de datos de informe, puede usar el portal web en el servidor de informes en modo nativo o las páginas de aplicación en un sitio de SharePoint.  
  
 Las tareas siguientes, que se describen en este tema, caracterizan la administración de las conexiones de un origen de datos:  
  
-   Cambiar las cadenas de conexión.  
  
-   Cambiar credenciales.  
  
-   Crear y usar orígenes de datos compartidos en un servidor de informes, lo que incluye cambiar un origen de datos incrustado para un origen de datos compartido.  
  
-   Controlar el acceso a las propiedades del origen de datos estableciendo permisos en el informe, modelo o cualquier origen de datos compartido que se use.  
  
 Observe que la modificación de consultas no forma parte de la administración de conexiones de origen de datos. Para modificar una consulta de un informe o modelo, debe utilizar una herramienta de creación y realizar los cambios en la definición del modelo o del informe.  
  
## <a name="managed-properties-data-source-type-connection-strings-and-credentials"></a>Propiedades administradas: Tipo de origen de datos, Cadenas de conexión y Credenciales  
 Las propiedades de origen de datos que puede administrar en un servidor de informes son:  
  
|Propiedad|Descripción|Cómo administrarla|  
|--------------|-----------------|----------------------|  
|Tipo de origen de datos|Determina qué extensión de procesamiento de datos del servidor de informes se ha de utilizar en los datos externos. Algunos ejemplos de procesadores de datos son SQL Server, Analysis Services y Oracle.|El tipo de origen de datos es una propiedad administrada porque es configurable. Sin embargo, si va a crear un nuevo origen de datos compartido, solo debe configurar un tipo de origen de datos.<br /><br /> No cambie el tipo de origen de datos en las páginas de propiedades de un modelo o informe publicado, ya que, si lo hace, invalidará la conexión casi con seguridad. No es probable que las estructuras de datos requeridas por un informe o un modelo sean idénticas en una plataforma de datos diferente.|  
|Cadena de conexión|Establece la conexión inicial a un origen de datos externo. Un informe puede utilizar cadenas de conexión dinámica o estática.<br /><br /> Una *cadena de conexión estática* es un conjunto de valores que el informe utiliza siempre para conectarse al mismo origen de datos cada vez que se ejecuta.<br /><br /> Una *cadena de conexión dinámica* es una expresión que se genera en el informe y que permite al usuario seleccionar qué origen de datos ha de utilizar en tiempo de ejecución. Se deben generar la expresión y la lista de selección de orígenes de datos en el informe al crearlo en el Diseñador de informes.|Cambiar una cadena de conexión es útil si mueve un origen de datos a otro equipo, o si ha creado los informes con datos de prueba pero desea implementarlos con una base de datos de producción.<br /><br /> Puede administrar una cadena de conexión estática reemplazando la cadena original por otra diferente.<br /><br /> Para administrar una cadena de conexión dinámica en el portal web o en un sitio de SharePoint, está limitado a reemplazarla por una estática. No puede modificar la propia expresión, ni cambiar la lista de selección de orígenes de datos. Para cambiar la expresión o la lista de valores válidos, debe modificar la definición de informe y volver a publicarla en el servidor de informes. Para más información, vea [Creación de cadenas de conexión de datos - Generador de informes y SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).|  
|Credenciales|Proporciona el nombre y la contraseña de un usuario que tiene permiso para leer los datos del origen de datos.<br /><br /> Si un origen de datos no admite la autenticación (por ejemplo, si el origen de datos es un archivo XML en el sistema de archivos), puede configurar la cuenta de ejecución desatendida para permitir que el servidor de informes se conecte al origen de datos externo sin pasar credenciales.|Puede administrar credenciales actualizando la cuenta de usuario o una contraseña, si expiró.<br /><br /> También puede cambiar la forma de obtener credenciales (por ejemplo, pidiendo a los usuarios que las escriban en tiempo de ejecución).<br /><br /> Si desea que los usuarios puedan suscribirse a un informe, debe configurarlo de modo que se usen credenciales almacenadas.|  
  
## <a name="creating-and-using-shared-data-sources"></a>Crear y usar orígenes de datos compartidos  
 Si publica un informe con propiedades de origen de datos incrustadas en él, considere la posibilidad de cambiar a propiedades de origen de datos compartido. Los orígenes de datos compartidos son más fáciles de administrar porque se pueden actualizar las credenciales y las cadenas de conexión en una página. Todos los informes, modelos y suscripciones controladas por datos que utilizan el origen de datos recopilan los cambios inmediatamente. También puede dejar sin conexión un origen de datos compartido, pausando de forma efectiva el informe o la suscripción para evitar que se ejecuten mientras soluciona o investiga los problemas surgidos.  
  
## <a name="controlling-access-data-source-properties"></a>Controlar el acceso a las propiedades del origen de datos  
 De forma predeterminada, cualquiera que tenga permiso para administrar informes puede establecer una propiedad en un informe, incluso las propiedades que determinan el tipo de origen de datos, la cadena de conexión, las credenciales y si el informe obtiene la información de conexión de un origen de datos incrustado o compartido. Para más información sobre qué tareas y permisos controlan el acceso a las propiedades del origen de datos en un servidor de informes en modo nativo, vea [Proteger elementos de orígenes de datos compartidos](../../reporting-services/security/secure-shared-data-source-items.md) y [Proteger informes y recursos](../../reporting-services/security/secure-reports-and-resources.md).  
  
 El administrador del sitio determina los permisos para ver y modificar las propiedades de los elementos de una biblioteca de SharePoint. Para más información sobre qué permisos controlan el acceso a las propiedades de conexión del origen de datos, vea [Referencia de permisos de sitio y lista de SharePoint para los elementos del servidor de informes](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md).  
  
## <a name="how-to-work-with-data-source-properties-on-a-report-server"></a>Cómo trabajar con las propiedades del origen de datos en un servidor de informes  
 Puede utilizar diversas herramientas para crear y modificar propiedades de origen de datos. En la tabla siguiente se resumen los enfoques y las herramientas, y se proporciona un vínculo a instrucciones adicionales.  
  
|Tarea|Herramienta|Vínculo|  
|----------|----------|----------|  
|Ver ejemplos de cadenas de conexión.||[Creación de cadenas de conexión de datos - Generador de informes y SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)|  
|Elegir un enfoque para obtener las credenciales para conectarse a un origen de datos.||[Especificación de información de credenciales y conexión para los orígenes de datos de informes](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)|  
|Agregar propiedades de conexión de origen de datos a un archivo de definición de informe (.rdl).|Diseñador de informes|[Crear un origen de datos incrustado o compartido &#40;SSRS&#41;](/previous-versions/sql/)|  
|Agregar y establecer un vínculo con un archivo origen de datos compartido (.rds) en el proyecto de informe.|Diseñador de informes|[Crear, modificar y eliminar orígenes de datos compartidos &#40;SSRS&#41;](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)|  
|Crear una lista predefinida de orígenes de datos que los usuarios pueden seleccionar en tiempo de ejecución. Cuando un usuario solicita un informe, el informe proporciona una lista de orígenes de datos. El usuario debe seleccionar qué origen de datos ha de utilizar antes de ejecutar el informe. Para agregar una lista de selección de orígenes de datos a un informe, ha de utilizar una expresión.<br /><br /> Esto se conoce como conexión dinámica de origen de datos.|Diseñador de informes|[Creación de cadenas de conexión de datos - Generador de informes y SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)|  
|Crear un elemento de origen de datos compartido en un servidor de informes.|[Creación, modificación y eliminación de orígenes de datos compartidos](create-modify-and-delete-shared-data-sources-ssrs.md) |  
|Almacenar credenciales como requisito previo para crear suscripciones o instantáneas de informe.|El portal web|[Almacenamiento de las credenciales en un origen de datos de Reporting Services](../../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md)|  
|Modificar las propiedades de conexión de origen de datos en un informe publicado.|El portal web|[Configuración de propiedades de origen de datos para un informe](../../reporting-services/report-data/configure-data-source-properties-for-a-report-report-manager.md)|  
|Crear un elemento de origen de datos compartido en un servidor de informes.|Sitio de SharePoint|[Crear y administrar orígenes de datos compartidos &#40;Reporting Services en el modo integrado de SharePoint&#41;](/previous-versions/sql/)|  
|Usar la información de conexión .odc existente con un informe.|Sitio de SharePoint|[Usar una conexión de datos de Office &#40;.odc&#41; con informes &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-data/use-an-office-data-connection-odc-with-reports.md)|  
  
> [!NOTE]  
>  Administrar las conexiones de un origen de datos con los orígenes de datos de informe no equivale a administrar la conexión del servidor de informes con la base de datos del servidor de informes. Para más información sobre la conexión de un servidor de informes con su almacén de datos interno, vea [Configurar una conexión a la base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
## <a name="see-also"></a>Consulte también  
 [Enlazar un informe o un modelo con un origen de datos compartido &#40;SSRS&#41;](../../reporting-services/report-data/bind-a-report-or-model-to-a-shared-data-source-ssrs.md)   
 [Almacenamiento de las credenciales en un origen de datos de Reporting Services](../../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md)   
 [Creación de cadenas de conexión de datos - Generador de informes y SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [Orígenes de datos admitidos por Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)   
 [Administración de contenido del servidor de informes &#40;Modo nativo de SSRS&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)  
  
