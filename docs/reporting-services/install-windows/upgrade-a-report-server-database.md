---
description: Actualizar una base de datos del servidor de informes
title: Actualizar una base de datos del servidor de informes | Microsoft Docs
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- upgrading databases
- report server database
- upgrading Reporting Services
ms.assetid: 4091cf87-9d97-4048-a393-67f1f9207401
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cf6cc7dc672e14a0fd988354ba4900fb49c206e3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075906"
---
# <a name="upgrade-a-report-server-database"></a>Actualizar una base de datos del servidor de informes

La base de datos del servidor de informes proporciona almacenamiento para una o varias instancias del servidor de informes. Dado que el esquema de la base de datos del servidor de informes puede cambiar con cada versión nueva de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], es necesario que la versión de base de datos coincida con la versión de la instancia del servidor de informes que esté utilizando. En la mayoría de los casos, una base de datos del servidor de informes se puede actualizar automáticamente sin ninguna acción específica de su parte.  
  
 **Modo nativo:** en el modo nativo de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], la base de datos del servidor de informes consta realmente de dos bases de datos que tienen los nombres predeterminados ReportServer y ReportServerTempDB.  

::: moniker range="=sql-server-2016"
  
 **Modo de SharePoint:** en el modo de SharePoint de SQL Server 2016 Reporting Services, la base de datos del servidor de informes es realmente una colección de bases de datos que se crea para cada instancia de la aplicación de servicio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  

::: moniker-end

## <a name="ways-to-upgrade-a-native-mode-report-server-database"></a>Formas de actualizar una base de datos del servidor de informes en modo nativo

 En la lista siguiente se identifican las condiciones en las que se actualiza una base de datos del servidor de informes:  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actualiza una única instancia de un servidor de informes. El esquema de la base de datos del servidor de informes se actualiza automáticamente después del inicio del servicio y el servidor de informes determina que la versión del esquema de la base de datos no coincide con la versión del servidor.  
  
     Durante el inicio del servicio, el servidor de informes examina la versión del esquema de la base de datos para comprobar si coincide con la del servidor. Si la versión del esquema de la base de datos es anterior, se actualiza automáticamente a la versión que requiere el servidor de informes. La actualización automática es especialmente útil si ha restaurado o adjuntado una base de datos del servidor de informes anterior. Se escribe un mensaje en el archivo de registro de seguimiento del servidor de informes para indicar que se ha actualizado la versión del esquema de la base de datos.  
  
-   El Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] actualiza una base de datos del servidor de informes local o remota al seleccionar una versión anterior para usarla con una instancia más reciente del servidor de informes. En este caso, debe confirmar la acción de actualización antes de que tenga lugar.  
  
     El Administrador de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ya no dispone de un botón Actualizar independiente ni de un script de actualización. Esas características están obsoletas a partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] debido a la característica de actualización automática del servicio del servidor de informes.  
  
 Una vez actualizado el esquema, no se puede revertir la actualización a una versión anterior. Realice siempre una copia de seguridad de la base de datos del servidor de informes, por si necesita volver a crear una instalación previa.  
  
## <a name="how-the-schema-metadata-and-report-server-content-is-updated"></a>Cómo se actualizan el esquema, los metadatos y el contenido del servidor de informes  
 La base de datos del servidor de informes se actualiza en tres etapas:  
  
1.  El esquema se actualiza automáticamente después de la instalación y del inicio del servicio, o al seleccionar una base de datos del servidor de informes en modo nativo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que es de una versión anterior. Además, el servicio del servidor de informes comprueba la versión de la base de datos en el inicio. Si el servidor de informes está conectado a una base de datos que es de una versión anterior, la actualizará durante el inicio.  
  
2.  Los descriptores de seguridad se actualizan la primera vez que se usa la base de datos del servidor de informes después de haber actualizado el esquema.  
  
3.  Los informes publicados y las instantáneas de informes compilados se actualizan la primera vez que se utilizan. Para más información, consulte [Upgrade Reports](../../reporting-services/install-windows/upgrade-reports.md).  
  
 Además de la base de datos del servidor de informes, un servidor de informes utiliza también una base de datos temporal. La base de datos temporal se actualiza automáticamente al actualizar la base de datos del servidor de informes.  
  
## <a name="permissions-required-to-upgrade-a-report-server-database"></a>Permisos necesarios para actualizar una base de datos del servidor de informes  
 Si va a actualizar una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que incluye una base de datos del servidor de informes, puede aparecer un mensaje de error si la actualización de la base de datos se realiza sin los permisos adecuados. De manera predeterminada, el programa de instalación utiliza el token de seguridad del usuario que ejecuta el programa de instalación para conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] remota y actualizar el esquema. Si tiene permisos  **sysadmin** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servidor de bases de datos donde se hospedan las bases de datos del servidor de informes, la actualización de la base de datos se realizará correctamente. De manera similar, si ejecuta el programa de instalación desde el símbolo del sistema y se especifican los argumentos RSUPGRADEDATABASEACCOUNT y RSUPGRADEPASSWORD para una cuenta que tiene permiso **sysadmin** para modificar el esquema en el equipo remoto, la base de datos se actualizará correctamente.  
  
 Sin embargo, si no tiene permiso **sysadmin** para la base de datos del equipo remoto, se rechazará la conexión y aparecerá un error que indica lo siguiente:  
  
 `"Setup was not able to upgrade the report server database schema. You must update the database schema manually after setup is finished. To update the schema, run the Report Server Configuration Manager, open the Database Setup page, re-select the database, and click Apply. The database will be upgraded automatically."`  
  
 En este momento, los archivos de programa del servidor de informes se actualizarán, pero la base de datos del servidor de informes tendrá el formato de la versión anterior. El servidor de informes no estará disponible hasta que finalice el proceso de actualización actualizando la base de datos manualmente.  
  
#### <a name="to-upgrade-a-native-mode-database-with-scripts"></a>Para actualizar una base de datos en modo nativo con scripts  
 Puede utilizar scripts de WMI para actualizar una base de datos del servidor de informes. Para obtener más información, vea [Método GenerateDatabaseUpgradeScript &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaseupgradescript.md).  
  
## <a name="next-steps"></a>Pasos siguientes

[Administrador de configuración del servidor de informes](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
[Crear una base de datos del servidor de informes](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
[Actualizar y migrar Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Migrar una instalación de Reporting Services](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
