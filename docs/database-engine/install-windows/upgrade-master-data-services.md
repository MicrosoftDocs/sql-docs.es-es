---
title: Actualizar Master Data Services | Microsoft Docs
description: Descubra los cuatro escenarios para actualizar Microsoft SQL Server Master Data Services. Obtenga información sobre las ubicaciones de archivos y la solución de problemas de las actualizaciones.
ms.custom: ''
ms.date: 07/21/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 9c3543f3-3eb9-455d-a9bf-f17e9506ad21
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 89905788ffe1b5f62f2c5a49daeee52261e00e73
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344943"
---
# <a name="upgrade-master-data-services"></a>Actualizar Master Data Services

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  Estos son los escenarios para actualizar Microsoft [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Master Data Services.  
  
-   [Actualizar sin actualización del motor de base de datos](../../database-engine/install-windows/upgrade-master-data-services.md#noengine)  
  
-   [Actualizar con actualización del motor de base de datos](../../database-engine/install-windows/upgrade-master-data-services.md#engine)  
  
-   [Actualización en un escenario de dos equipos](../../database-engine/install-windows/upgrade-master-data-services.md#twocomputer)  
  
-   [Actualización con restauración de una base de datos desde la copia de seguridad](../../database-engine/install-windows/upgrade-master-data-services.md#restore)  
  
> [!IMPORTANT]  
> -   No se admite la actualización desde la versión [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] CTP1 a la versión CTP2.  
> -   Realice una copia de la base de datos antes de realizar cualquier actualización.  
> -   El proceso de actualización vuelve a crear los procedimientos almacenados y actualiza las tablas que usa [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Cualquier personalización que haya realizado en alguno de estos componentes se podría perder.  
> -   Los paquetes de implementación de modelos solo se pueden usar en la edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que se crearon. No puede implementar los paquetes de implementación de modelos que se crearon en [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].  
> -   Después de actualizar Quality Data Services y Master Data Services a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], ya no funcionará ninguna versión anterior del complemento Master Data Services para Excel. Puede descargar el complemento de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Master Data Services para Excel desde [Complemento Master Data Services para Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md).  
  
##  <a name="file-location"></a><a name="fileLocation"></a> Ubicación del archivo  
  
-   En [!INCLUDE[ss2017](../../includes/sssql17-md.md)], los archivos se instalan de forma predeterminada en *unidad*:\Archivos de programa\Microsoft SQL Server\140\Master Data Services.  

-   En [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], los archivos se instalan de forma predeterminada en *unidad*:\Archivos de programa\Microsoft SQL Server\130\Master Data Services.  
  
-   En [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], los archivos se instalan de forma predeterminada en *unidad*:\Archivos de programa\Microsoft SQL Server\120\Master Data Services.  
  
-   En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], los archivos se instalan de forma predeterminada en *unidad*:\Archivos de programa\Microsoft SQL Server\110\Master Data Services.  
  
-   En [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], los archivos se instalan de forma predeterminada en *unidad*:\Archivos de programa\Microsoft SQL Server\Master Data Services.  
  
##  <a name="upgrade-without-database-engine-upgrade"></a><a name="noengine"></a> Actualizar sin actualización del motor de base de datos  
 En este escenario se sigue usando [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] para hospedar la base de datos de MDS. No obstante, hay que actualizar el esquema de la base de datos de MDS y, posteriormente, crear una aplicación web de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] actual para tener acceso a la base de datos de MDS. Tras la actualización, la aplicación web anterior ya no podrá tener acceso a la base de datos de MDS.  
  
 Puede instalar la versión actual de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] y una versión anterior de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en el mismo equipo. Los archivos se instalan en distintas ubicaciones, como se muestra en [Ubicación del archivo](#fileLocation).  
  
 **Actualizar sin actualización del motor de base de datos**  
  
1.  Instale [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] y cualquier otra característica que desee.  
  
    1.  Abra el Asistente para la instalación de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)].  
  
    2.  En el panel izquierdo, haga clic en **Instalación**.  
  
    3.  En el panel derecho, haga clic en **Nueva instalación independiente de SQL Server o agregar características a una instalación existente**.  
  
    4.  En la página de **Selección de características** , seleccione **[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]** y cualquier otra característica que desee instalar.  
  
    5.  Finalice el asistente.  
  
2.  Actualice el esquema de la base de datos de MDS.  
  
    1.  Abra la versión actual de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
        > [!IMPORTANT]  
        >  Para actualizar el esquema de base de datos de MDS, debe haber iniciado sesión con la cuenta de administrador que se especificó cuando se creó la base de datos de MDS. En la base de datos de MDS, en mdm.tblUser, el usuario tiene el valor de **Id.** establecido en **1**.  
  
    2.  En el panel izquierdo, haga clic en **Configuración de base de datos**.  
  
    3.  En el panel derecho, haga clic en **Seleccionar base de datos** e indique la información de la instancia de base de datos de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].  
  
    4.  Haga clic en **Actualizar base de datos** para iniciar el **Asistente para actualizar base de datos**. Para más información, vea [Asistente para actualizar base de datos &#40;Administrador de configuración de Master Data Services&#41;](../../master-data-services/upgrade-database-wizard-master-data-services-configuration-manager.md).  
  
3.  Cree una aplicación web.  
  
    1.  Abra la versión actual de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
    2.  En el panel izquierdo, haga clic en **Configuración web**.  
  
    3.  En el panel derecho, en la lista de **Sitio web** , seleccione una de las opciones siguientes:  
  
        -   **Sitio web predeterminado** y, después, haga clic en **Crear aplicación**.  
  
        -   **Crear nuevo sitio**. Se creará automáticamente una aplicación web cuando se cree el sitio web.  
  
        > [!IMPORTANT]  
        >  La aplicación web de MDS existente de una versión anterior de SQL Server ([!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) se puede seleccionar en la versión de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] del Administrador de configuración de Master Data Services. No debe seleccionar la aplicación web existente y en su lugar debe crear una aplicación web de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] para MDS. De lo contrario, al intentar asociar la aplicación web a la base de datos actualizada de MDS recibirá un error que indica que no se puede tener acceso a la página solicitada porque los datos de configuración relacionados para la página no son válidos.  
        >   
        >  Si quiere usar el mismo nombre (alias) para la aplicación web de MDS que la aplicación web existente ([!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]), debe eliminar primero la aplicación web y el grupo de aplicaciones asociado de IIS y crear después una aplicación web con el mismo nombre por medio de la versión de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] del Administrador de configuración de Master Data Services. Para obtener más información sobre cómo quitar una aplicación web y grupos de aplicaciones de IIS, vea [Quitar una aplicación (IIS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771205(v=ws.10)) y [Quitar un grupo de aplicaciones (IIS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772406(v=ws.10)).  
  
4.  Asocie la nueva aplicación web a la base de datos de MDS actualizada.  
  
    1.  En la sección **Asociar aplicación a base de datos** , haga clic en **Seleccionar**.  
  
    2.  Seleccione la base de datos de MDS.  
  
    3.  Haga clic en **Aplicar**.  
  
##  <a name="upgrade-with-database-engine-upgrade"></a><a name="engine"></a> Actualizar con actualización del motor de base de datos  
 En este escenario, actualizará tanto el motor de base de datos como la aplicación de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] desde una versión anterior a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] o a [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)].  
  
 **Actualizar con actualización del motor de base de datos**  
  
1.  **Solo en el caso de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]** : abra el **Panel de control** > **Programas y características** y desinstale Microsoft [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)][!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
2.  Actualice el motor de base de datos a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] o [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)]. Para obtener más información, vea [Choose a Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
3.  Complete todos los pasos en [Actualizar sin actualización del motor de base de datos](#noengine) .  
  
##  <a name="upgrade-in-two-computer-scenario"></a><a name="twocomputer"></a> Actualización en un escenario de dos equipos  
 Este escenario conlleva que actualice un sistema en el que SQL Server está instalado en dos equipos: uno con [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] o [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] y otro con una versión anterior de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)].  
  
 Si hay instalada una versión anterior de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)], hay que seguir usando esa versión anterior para hospedar la base de datos MDS en un equipo. Pero debe actualizar el esquema de la base de datos de MDS y, posteriormente, usar la aplicación web de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] o [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] respectivamente para tener acceso a la base de datos de MDS. La aplicación web de la versión anterior ya no podrá tener acceso a la base de datos de MDS.  
  
 **Actualizar en un escenario de dos equipos**  
  
-   Complete todos los pasos en [Actualizar sin actualización del motor de base de datos](#noengine).  
  
##  <a name="upgrade-with-restoring-a-database-from-backup"></a><a name="restore"></a> Actualización con restauración de una base de datos desde la copia de seguridad  
 En este escenario, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] o [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] está instalado junto con una versión anterior en el mismo equipo o en dos equipos diferentes. Se hizo una copia de seguridad de una base de datos en una versión anterior a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] o [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)], antes de la actualización, y hay que restaurar la base de datos.  
  
 **Actualizar con restauración de una base de datos desde la copia de seguridad**  
  
1.  Instale [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] y cualquier otra característica que desee.  
  
    1.  Abra el Asistente para la instalación de [!INCLUDE[sssnoversion](../../includes/ssnoversion-md.md)].  
  
    2.  En el panel izquierdo, haga clic en **Instalación**.  
  
    3.  En el panel derecho, haga clic en **Nueva instalación independiente de SQL Server o agregar características a una instalación existente**.  
  
    4.  En la página de **Selección de características** , seleccione **[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]** y cualquier otra característica que desee instalar.  
  
    5.  Finalice el asistente.  
  
2.  Restaure la base de datos de la que se realizó la copia de seguridad.  
  
3.  Actualice el esquema de la base de datos de MDS, cree una aplicación web y asocie la nueva aplicación web con la base de datos de MDS actualizada. Para obtener instrucciones, vea los pasos 2-4 en [Actualizar sin actualización del motor de base de datos](#noengine).  
  
## <a name="troubleshooting"></a>Solución de problemas  
 **Problema:** al abrir la aplicación web de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], se muestra un mensaje de error similar a "La versión del cliente no es compatible con la versión de la base de datos".  
  
 **Solución:** este problema se produce cuando una aplicación web de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Master Data Manager intenta acceder a una base de datos que se ha actualizado a [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] Master Data Services. Debe usar una aplicación web de [!INCLUDE[ssSQL16](../../includes/sssql17-md.md)] en su lugar.  
  
 Este problema puede producirse también si no se ha detenido y reiniciado el **Grupo de aplicaciones de MDS** en IIS al actualizar el esquema de la base de datos de MDS. Reinicie el **Grupo de aplicaciones de MDS** para corregir el problema.  
  
## <a name="see-also"></a>Consulte también  
 [Instalar Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
