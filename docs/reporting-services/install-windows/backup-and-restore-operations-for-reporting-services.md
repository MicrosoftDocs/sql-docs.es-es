---
description: Operaciones de copia de seguridad y restauración de Reporting Services
title: Operaciones de copia de seguridad y restauración de Reporting Services | Microsoft Docs
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 157bc376-ab72-4c99-8bde-7b12db70843a
ms.date: 05/08/2019
ms.openlocfilehash: 038f7bd7db0df60073ae887839eddd057b69cd58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065370"
---
# <a name="backup-and-restore-operations-for-reporting-services"></a>Operaciones de copia de seguridad y restauración de Reporting Services

  En este artículo se ofrece información general sobre todos los archivos de datos que se usan en una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y se describe cuándo y cómo se debe realizar una copia de seguridad de los archivos. La parte más importante de una estrategia de recuperación es desarrollar un plan de copias de seguridad y restauración de los archivos de base de datos del servidor de informes. Pero una estrategia de recuperación más completa incluiría copias de seguridad de las claves de cifrado, extensiones o ensamblados personalizados, archivos de configuración y archivos de origen para informes.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]** Modo nativo de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] | Modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  

> [!NOTE]
> La integración de Reporting Services con SharePoint ya no está disponible a partir de SQL Server 2016.
  
 A menudo se usan operaciones de copias de seguridad y restauración para mover la totalidad o una parte de la instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   Si se van a mover únicamente las bases de datos del servidor de informes, se pueden usar las operaciones de copias de seguridad y restauración, o de adjuntar y separar, para reubicar las bases de datos en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diferente. Para más información, vea [Mover las bases de datos del servidor de informes a otro equipo &#40;Modo nativo de SSRS&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
-   El traspaso de una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a otro equipo se conoce como migración. Cuando se migra una instalación, se ejecuta el programa de instalación para instalar una nueva instancia del servidor de informes y luego copiar datos de instancia al nuevo equipo. Para obtener más información sobre la migración de una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea los artículos siguientes:  
  
    - [Actualizar y migrar Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
    - [Migrar una instalación de Reporting Services &#40;modo nativo&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  

    ::: moniker range="=sql-server-2016"
  
    - [Migrar una instalación de Reporting Services &#40;modo de SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)  

    ::: moniker-end
  
## <a name="backing-up-the-report-server-databases"></a>Realizar copias de seguridad de bases de datos del servidor de informes  
 Puesto que un servidor de informes es un servidor sin estado, todos los datos de aplicación se almacenan en las bases de datos **reportserver** y **reportservertempdb** que se ejecutan en una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Se puede realizar una copia de seguridad de las bases de datos **reportserver** y **reportservertempdb** mediante uno de los métodos admitidos para copias de seguridad de bases de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Estas son algunas recomendaciones específicas de las bases de datos del servidor de informes:  
  
-   Use el modelo de recuperación completa para realizar la copia de seguridad de la base de datos **reportserver**.  
  
-   Use el modelo de recuperación simple para realizar la copia de seguridad de la base de datos **reportservertempdb**.  
  
-   Puede utilizar distintas programaciones de copia de seguridad para cada base de datos. La copia de seguridad de **reportservertempdb** solo se realiza para evitar tener que volver a crearla en caso de un error de hardware. Si se produce un error de hardware, no es necesario recuperar los datos de **reportservertempdb**, pero sí se requiere la estructura de la tabla. Si se pierde **reportservertempdb**, la única forma de recuperarla es volver a crear la base de datos del servidor de informes. Si se vuelve a crear **reportservertempdb**, es importante que su nombre sea el mismo que el de la base de datos primaria del servidor de informes.  
  
 Para obtener más información sobre la copia de seguridad y la recuperación de bases de datos relacionales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Realizar copias de seguridad y restaurar bases de datos de SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).  

::: moniker range="=sql-server-2016"

> [!IMPORTANT]  
>  Si el servidor de informes está en modo de SharePoint, hay que tener en cuenta otras bases de datos adicionales, incluidas las bases de datos de configuración de SharePoint y la base de datos de alertas de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. En el modo de SharePoint, se crean tres bases de datos para cada aplicación de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Las bases de datos **reportserver**, **reportservertempdb** y **dataalerting** . Para obtener más información, vea [Copias de seguridad y restauración de aplicaciones de servicio de SharePoint para Reporting Services](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md).  

::: moniker-end
  
## <a name="backing-up-the-encryption-keys"></a>Realizar copias de seguridad de claves de cifrado  
 La primera vez que se configura una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], se recomienda realizar una copia de seguridad de las claves de cifrado. También se debería realizar una copia de seguridad de las claves siempre que se cambie la identidad de las cuentas de servicio o el nombre del equipo. Para obtener más información, vea [Hacer copia de seguridad y restaurar claves de cifrado de Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md). 

::: moniker range="=sql-server-2016"

Para los servidores de informes en modo de SharePoint, vea la sección "Administración de claves" de [Administrar una aplicación de servicio de SharePoint para Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).  

::: moniker-end
  
## <a name="backing-up-the-configuration-files"></a>Realizar copias de seguridad de archivos de configuración  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa archivos de configuración para almacenar la configuración de aplicación. Se debe realizar una copia de seguridad de estos archivos la primera vez que se configura el servidor y después de implementar extensiones personalizadas. Debe realizar una copia de seguridad de los siguientes archivos:  
  
-   RSReportServer.config  
  
-   Rssvrpolicy.config  
  
-   Rsmgrpolicy.config  
  
-   Reportingservicesservice.exe.config  
  
-   Web.config para la aplicación [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] del servidor de informes.
  
-   Machine.config de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]  
  
## <a name="backing-up-data-files"></a>Realizar una copia de seguridad de archivos de datos  
 Realice una copia de seguridad de los archivos que cree y mantenga en el Diseñador de informes. Estos archivos incluyen archivos de definición de informe (.rdl), de orígenes de datos compartidos (.rds), de vista de datos (.dv), de origen de datos (.ds), de proyecto de servidor de informes (.rptproj) y de solución de informes (.sln).  
  
 No olvide realizar una copia de seguridad de los archivos de script (.rss) que haya creado para las tareas de administración o implementación.  
  
 Compruebe que dispone de una copia de seguridad de las extensiones personalizadas y de los ensamblados personalizados que utilice.  

## <a name="next-steps"></a>Pasos siguientes

[Base de datos del servidor de informes](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
[Archivos de configuración de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
[Utilidad rskeymgmt](../../reporting-services/tools/rskeymgmt-utility-ssrs.md)   
[Copiar bases de datos con Copias de seguridad y restauración](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
[Administrar una base de datos del servidor de informes](../../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md)   
[Configurar y administrar las claves de cifrado](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
