---
title: Creación de una base de datos del servidor de informes (Administrador de configuración) | Microsoft Docs
description: El modo nativo de SQL Server Reporting Services usa dos bases de datos relacionales de SQL Server Reporting Services para almacenar metadatos y objetos del servidor de informes. Una base de datos se utiliza para el almacenamiento principal y la otra para almacenar datos temporales.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seodec18
ms.date: 9/2/2020
ms.openlocfilehash: 70a92bf37285e9af58dcef2e7a69abfb88b37178
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067112"
---
# <a name="create-a-report-server-database-report-server-configuration-manager"></a>Crear una base de datos del servidor de informes (Administrador de configuración del servidor de informes)  

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

El modo nativo de SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa dos bases de datos relacionales [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para almacenar metadatos y objetos del servidor de informes. Una base de datos se utiliza para el almacenamiento principal y la otra para almacenar datos temporales. 

Las bases de datos se crean juntas y se enlazan mediante el nombre. Con una instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , las bases de datos tienen los nombres **reportserver** y **reportservertempdb**. En conjunto, ambas se conocen como **base de datos del servidor de informes** o **catálogo del servidor de informes**.

::: moniker range="=sql-server-2016"

El [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]modo SharePoint **de SQL Server** incluye una tercera base de datos que se usa para los metadatos de alerta de datos. Las tres bases de datos se crean para cada aplicación de servicio de SSRS. Los nombres de base de datos incluyen de forma predeterminada un GUID que representa la aplicación de servicio. 

A continuación, se indican nombres de ejemplo de las tres bases de datos en modo de SharePoint:

- ReportingService_90a9f37075544f22953c4a62e4a9f370  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370TempDB  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370_Alerting  

::: moniker-end
  
> [!IMPORTANT]  
> No escriba aplicaciones que ejecuten consultas en la base de datos del servidor de informes. La base de datos del servidor de informes no es un esquema público. La estructura de tablas puede cambiar de una versión a la siguiente. Si escribe una aplicación que necesita acceso a la base de datos del servidor de informes, use siempre las API de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para obtener acceso.  
>
> Las vistas del registro de ejecución son excepciones a esta regla. Para obtener más información, vea [Registro de ejecución del servidor de informes y la vista ExecutionLog3](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
## <a name="ways-to-create-the-report-server-database"></a>Formas de crear la base de datos del servidor de informes

 ### <a name="native-mode"></a>en modo nativo
 Puede crear la base de datos del servidor de informes en modo nativo de las maneras siguientes:  
  
- **Automática**. Use el Asistente para instalación de SQL Server si elige para la instalación la opción de configuración predeterminada. En el Asistente para la instalación de SQL Server, esta opción es **Instalar y configurar** en la página **Report Server Installation Options** (Opciones de instalación del servidor de informes). Si elige la opción **Solo instalar**, debe usar el Administrador de configuración de SQL Server Reporting Services para crear la base de datos.  
  
- **Manual**. Use el Administrador de configuración de SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Si usa una instancia remota de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], cree la base de datos del servidor de informes manualmente para hospedarla. Para más información, consulte [Crear una base de datos del servidor de informes de modo nativo](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  

::: moniker range="=sql-server-2016"
  
### <a name="sharepoint-mode"></a>Modo de SharePoint 
La página **Report Server Installation Options** (Opciones de instalación del servidor de informes) solo tiene una opción para el modo SharePoint, **Install Only** (Solo instalar). Esta opción instala todos los archivos de SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y el servicio compartido de SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. El paso siguiente consiste en crear al menos una aplicación de servicio de SSRS de una de las siguientes maneras:  
  
- Vaya a Administración central de SharePoint Server para crear una aplicación de servicio de SSRS. Para más información, consulte la sección **Crear una aplicación de servicio** de [Instalar el primer servidor de informes en el modo de SharePoint](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md#bkmk_create_serrviceapplication).  
  
- Use cmdlets de PowerShell de SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para crear una aplicación de servicio y las bases de datos del servidor de informes. Para más información, consulte el ejemplo para crear aplicaciones de servicio en el tema [Cmdlets de PowerShell para el modo de SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  

::: moniker-end
  
## <a name="database-server-version-requirements"></a>Requisitos de versión del servidor de bases de datos

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se usa para hospedar las bases de datos del servidor de informes. La instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] puede ser local o remota. Las siguientes versiones admitidas de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] pueden hospedar las bases de datos del servidor de informes:  
::: moniker range=">=sql-server-ver15"

- Instancia administrada de Azure SQL

- SQL Server 2019

::: moniker-end
::: moniker range=">=sql-server-2017"

- SQL Server 2017  
::: moniker-end

- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
  
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  

Si crea la base de datos del servidor de informes en un equipo remoto, configure la conexión para usar una cuenta de usuario de dominio o una cuenta de servicio que tenga acceso a la red. Si usa una instancia remota de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], piense qué credenciales debe usar el servidor de informes para conectarse a la instancia. Para más información, vea [Configurar una conexión a la base de datos del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
> [!IMPORTANT]  
> El servidor de informes y la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que hospeda la base de datos del servidor de informes pueden estar en dominios diferentes. Para realizar la implementación en Internet, una práctica común es usar un servidor que esté detrás de un firewall. 
>
> Si configura un servidor de informes para el acceso a Internet, use las credenciales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que está detrás del firewall. Proteja la conexión mediante IPSEC.  
  
## <a name="edition-requirements-for-a-database-server"></a>Requisitos de edición de un servidor de bases de datos 

 Al crear una base de datos del servidor de informes, no se pueden usar todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para hospedarla. Para más información, consulte [Requisitos de edición de servidor de la base de datos del servidor de informes](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md#edition-requirements-for-the-report-server-database) en [Características de Reporting Services compatibles con las ediciones de SQL Server](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).  

## <a name="next-steps"></a>Pasos siguientes

Lea información sobre el [Administrador de configuración del servidor de informes](reporting-services-configuration-manager-native-mode.md).  

¿Tiene alguna pregunta más? Pregunte en el [foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).