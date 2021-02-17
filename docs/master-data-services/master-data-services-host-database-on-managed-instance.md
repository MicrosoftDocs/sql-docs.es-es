---
title: Hospedar una base de datos en una instancia administrada
description: Aprenda a crear y configurar una base de datos de Master Data Services (MDS) y a hospedarla en una Instancia administrada de Azure SQL.
ms.custom: ''
ms.date: 07/01/2019
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 19519697-c219-44a8-9339-ee1b02545445
author: v-redu
ms.author: lle
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 0ae7d5ad7bdb7874e4f6a99a1a99ea08c228fced
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339487"
---
# <a name="host-an-mds-database-on-a-managed-instance"></a>Hospedar una base de datos de MDS en una instancia administrada

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En este artículo se explica cómo configurar una base de datos de Master Data Services (MDS) en una instancia administrada de.
  
## <a name="preparation"></a>Preparación

Para prepararse, debe crear y configurar una Instancia administrada de Azure SQL y configurar el equipo de la aplicación Web.

### <a name="create-and-configure-the-database"></a>Crear y configurar la base de datos

1. Cree una instancia administrada con una red virtual. Consulte [Inicio rápido: creación de un instancia administrada de SQL](/azure/sql-database/sql-database-managed-instance-get-started) para obtener más información.

1. Configure una conexión de punto a sitio. Consulte [configuración de una conexión de punto a sitio a una red virtual mediante la autenticación de certificados de Azure nativa: Azure portal](/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) para obtener instrucciones.

1. Configurar la autenticación de Azure Active Directory con SQL Instancia administrada. Consulte [configuración y administración de la autenticación de Azure Active Directory con SQL](/azure/sql-database/sql-database-aad-authentication-configure) para obtener más información.

### <a name="configure-web-application-machine"></a>Configurar equipo de aplicación Web

1. Instale un certificado de conexión de punto a sitio y una VPN para asegurarse de que el equipo pueda tener acceso a la instancia administrada. Consulte [configuración de una conexión de punto a sitio a una red virtual mediante la autenticación de certificados de Azure nativa: Azure portal](/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) para obtener instrucciones.

1. Instale los siguientes roles y características:
   - Funciones:
     - Internet Information Services
     - Herramientas de administración web
     - Consola de administración de IIS
     - Servicios de World Wide Web
     - Desarrollo de aplicaciones
     - Extensibilidad de .NET 3.5
     - Extensibilidad de .NET 4.5
     - ASP.NET 3.5
     - ASP.NET 4.5
     - Extensiones ISAPI
     - Filtros de ISAPI
     - Características HTTP comunes
     - Documento predeterminado
     - Examen de directorios
     - Errores HTTP
     - Contenido estático
     - Estado y diagnóstico
     - Registro HTTP
     - Monitor de solicitudes
     - Rendimiento
     - Compresión de contenido estático
     - Seguridad
     - Filtrado de solicitudes
     - Autenticación de Windows
       > [!NOTE]
       > No instalar publicación de WebDAV

   - Características:
     - .NET Framework 3.5 (incluye .NET 2.0 y 3.0)
     - Servicios avanzados de .NET Framework 4.5
     - ASP.NET 4.5
     - WCF Services
     - Activación HTTP (obligatorio)
     - Uso compartido de puertos TCP
     - Servicio de activación de procesos de Windows
     - Modelo de proceso
     - Entorno .NET
     - API de configuración
     - compresión de contenido dinámico

## <a name="install-and-configure-an-mds-web-application"></a>Instalación y configuración de una aplicación Web de MDS

A continuación, instale y configure [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] .

### <a name="install-sql-server-2019"></a>Instalación de SQL Server 2019

Use el Asistente para la instalación de SQL Server o el símbolo del sistema para instalar [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] .

1. Abra `Setup.exe` y siga los pasos del Asistente para la instalación de.

2. Seleccione [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] en la página **Selección de características** en **Características compartidas**.
Esta acción instala:
   - [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]
   - Ensamblados
   - Un complemento de Windows PowerShell
   - Carpetas y archivos para servicios y aplicaciones Web.

   ![Captura de pantalla que muestra la página selección de características.](../master-data-services/media/mds-sqlserver2019-config-mi-sqlfeatureselection.png "MDS-SQLServer2019-config-MI_SQLFeatureSelection")  

### <a name="set-up-the-database-and-website"></a>Configurar la base de datos y el sitio web

1. Conecte el Virtual Network de Azure para asegurarse de que puede conectarse a la instancia administrada.

   ![Captura de pantalla de la VPN de prueba de MI que se conecta a Azure Virtual Network.](../master-data-services/media/mds-sqlserver2019-config-mi-p2svpnconnect.png "MDS-SQLServer2019-config-MI_P2SVPNConnect")

1. Abra [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] y, a continuación, seleccione **configuración de base de datos** en el panel izquierdo.

1. Seleccione **crear base de datos** para abrir el **Asistente para crear bases de datos**. Seleccione **Next** (Siguiente).

1. En la página **servidor de base de datos** , complete el campo instancia de **SQL Server** y, a continuación, elija el tipo de **autenticación**. Seleccione **probar conexión** para confirmar que puede usar sus credenciales para conectarse a la base de datos mediante el tipo de autenticación elegido. Seleccione **Next** (Siguiente).

   > [!NOTE]
   > - Una instancia de SQL Server tiene el siguiente aspecto `xxxxxxx.xxxxxxx.database.windows.net` .
   > - En el caso de una instancia administrada, elija entre los tipos de autenticación " **SQL Server cuenta"** y **"usuario actual – Active Directory integrado"** .
   > - Si selecciona **usuario actual: Active Directory integrado** como el tipo de autenticación, el campo **nombre de usuario** es de solo lectura y muestra la cuenta de usuario de Windows con la sesión iniciada actualmente. Si está ejecutando SQL Server 2019 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] en una máquina virtual (VM) de Azure, el campo **nombre de usuario** muestra el nombre de la máquina virtual y el nombre de usuario de la cuenta de administrador local en la máquina virtual.

   La autenticación debe contener la regla **"sysadmin"** para las instancias administradas.

   ![Captura de pantalla de la página servidor de base de datos del Asistente para crear bases de datos.](../master-data-services/media/mds-sqlserver2019-config-mi-createdbconnect.png "MDS-SQLServer2019-config-MI_CreateDBConnect")  

1. Escriba un nombre en el campo **Nombre de la base de datos** . Opcionalmente, para seleccionar una intercalación de Windows, desactive la casilla **SQL Server intercalación predeterminada** y seleccione una o varias de las opciones disponibles. Por ejemplo, **distingue mayúsculas de minúsculas**. Seleccione **Next** (Siguiente).

   ![Captura de pantalla de la página base de datos del Asistente para crear bases de datos.](../master-data-services/media/mds-sqlserver2019-config-mi-createddbname.png "MDS-SQLServer2019-config-MI_CreatedDBName")

1. En el campo **nombre de usuario** , especifique la cuenta de Windows del superusuario predeterminado para [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] . Un superusuario tiene acceso a todas las áreas funcionales y puede Agregar, eliminar y actualizar todos los modelos.

   ![Captura de pantalla de la página cuenta de administrador del Asistente para crear bases de datos.](../master-data-services/media/mds-sqlserver2019-config-mi-createdbusername.png "MDS-SQLServer2019-config-MI_createDBUserName")

1. Seleccione **siguiente** para ver un resumen de la configuración de la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de datos. Vuelva a seleccionar  **siguiente** para crear la base de datos. Verá la página **progreso y finalizar** .

1. Una vez creada y configurada la base de datos, seleccione **Finalizar**.

   Para obtener más información acerca de la configuración del **Asistente para crear bases de datos**, vea [Asistente para crear bases de datos &#40;[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Configuration Manager&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).

1. En la página **configuración de base de datos** de la [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] , elija **Seleccionar base de datos**.

1. Seleccione **conectar**, elija la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de datos y, a continuación, haga clic en **Aceptar**.

   ![Captura de pantalla del cuadro de diálogo conectar con base de datos.](../master-data-services/media/mds-sqlserver2019-config-mi-connectdbname.png "MDS-SQLServer2019-config-MI_connectDBName")

1. En [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] , seleccione **configuración Web** en el panel izquierdo.

1. En el cuadro de lista **sitio** Web, elija **sitio web predeterminado** y, a continuación, seleccione **crear** para crear una aplicación Web.

   ![Captura de pantalla del cuadro de diálogo Administrador de configuración de Master Data Services.](../master-data-services/media/mds-sqlserver2019-config-mi-webconfiguration.png "MDS-SQLServer2019-config-MI_WebConfiguration")

   > [!NOTE]
   > Si selecciona **sitio web predeterminado**, tendrá que crear una aplicación web por separado. Si elige **crear nuevo sitio web** en el cuadro de lista, la aplicación se crea automáticamente.

1. En la sección **grupo de aplicaciones** , escriba un nombre de usuario diferente, escriba la contraseña y, después, haga clic en **Aceptar**.

   ![Captura de pantalla del cuadro de diálogo Administración de aplicaciones.](../master-data-services/media/mds-sqlserver2019-config-mi-createwebapplication.png "MDS-SQLServer2019-config-MI_CreateWebApplication")

   > [!NOTE]
   > Asegúrese de que el usuario puede tener acceso a la base de datos con la Active Directory autenticación integrada que ha creado recientemente. Como alternativa, puede cambiar la conexión `web.config` más adelante.

   Para obtener más información sobre el cuadro de diálogo **crear aplicación web** , vea [cuadro de diálogo crear aplicación web &#40;[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] Configuration Manager&#41;](../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md).

1. En el panel **configuración Web** de la ventana de la **aplicación web** , seleccione la aplicación que ha creado y, a continuación, elija **seleccionar** en la sección **asociar aplicación a base de datos** .

1. Seleccione **conectar** y elija la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de datos que desea asociar a la aplicación Web. Seleccione **Aceptar**.

   Ha terminado de configurar el sitio Web. La página **configuración Web** muestra ahora el sitio Web seleccionado, la aplicación web que creó y la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de datos asociada a la aplicación.

   ![Captura de pantalla de la sección de configuración Web.](../master-data-services/media/mds-sqlserver2019-config-mi-webconfigselectdb.png "MDS-SQLServer2019-config-MI_WebConfigSelectDB")

1. Seleccione **Aplicar**. Verá el mensaje **configuración completa** . Seleccione **Aceptar** en el cuadro de mensaje para iniciar la aplicación Web. La dirección del sitio web es `http://server name/web application/`.

## <a name="configure-authentication"></a>Configurar la autenticación

Para conectar la base de datos de instancia administrada a la aplicación Web, debe cambiar el otro tipo de autenticación.

Busque el `web.config` archivo en `C:\Program Files\Microsoft SQL Server\150\Master Data Services\WebApplication` . Modifique la connectionString para cambiar el otro tipo de autenticación para conectarse a la base de datos de instancia administrada.

El tipo de autenticación predeterminado es `Active Directory Integrated` como se muestra en la siguiente cadena de conexión de ejemplo:

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;Authentication=&quot;Active Directory Integrated&quot;" />
   ```

MDS también admite la autenticación Active Directory contraseña y la autenticación de SQL Server, tal y como se muestra en las siguientes cadenas de conexión de ejemplo:

- Autenticación de contraseña de Active Directory

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;Authentication=&quot;Active Directory Password&quot; ; UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!" />
   ```

- Los inicios de sesión de autenticación

   ```xml
   <add name="MDS1" connectionString="Data Source=*****.*****.database.windows.net;Initial Catalog=MasterDataServices;Integrated Security=False;Connect Timeout=60;User ID=UserName;Password=MyPassword!;" />
   ```

## <a name="upgrade-ssmdsshort_md-and-sql-database-version"></a>Actualización [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] y SQL Database versión

### <a name="upgrade-ssmdsshort_md"></a>Actualización [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]

Instale la **actualización acumulativa SQL Server 2019**. [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] se actualizará automáticamente.

### <a name="upgrade-sql-server"></a>Actualizar SQL Server

Podría obtener el error: `The client version is incompatible with the database version` después de instalar **SQL Server actualización acumulativa 2019**.
![Captura de pantalla del error de Master Data Services.](../master-data-services/media/mds-sqlserver2019-config-mi-upgradedbpage.png "MDS-SQLServer2019-config-MI_UpgradeDBPage")

Para corregir este problema, debe actualizar la versión de la base de datos:

1. Abra [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] y, a continuación, seleccione  **configuración de base de datos** en el panel izquierdo.

1. En la página **configuración de base de datos** de la [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] , elija **Seleccionar base de datos**.

1. Elija la [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] base de datos asociada a la aplicación Web. Seleccione **conectar** y, después, haga clic en **Aceptar**.

   ![Captura de pantalla del cuadro de diálogo conectar con una base de datos de Master Data Service.](../master-data-services/media/mds-sqlserver2019-config-mi-connectdbname.png "MDS-SQLServer2019-config-MI_ConnectDBName")

1. Seleccionar **Actualizar base de datos..** . .

   ![Captura de pantalla de la opción Actualizar base de datos.](../master-data-services/media/mds-sqlserver2019-config-mi-selectupgradedb.png "MDS-SQLServer2019-config-MI_SelectUpgradeDB")

1. En el Asistente para actualizar base de datos, seleccione **siguiente** en la página de **bienvenida** y en la página  **revisión de actualización** .

   ![Captura de pantalla de la página revisión de actualización del Asistente para actualizar base de datos.](../master-data-services/media/mds-sqlserver2019-config-mi-upgradedbwizard.png "MDS-SQLServer2019-config-MI_UpgradeDBWizard")

1. Seleccione **Finalizar** una vez completadas todas las tareas.

## <a name="see-also"></a>Consulte también

- [Base de datos de Master Data Services](../master-data-services/master-data-services-database.md)
- [Aplicación web Master Data Services](../master-data-services/master-data-manager-web-application.md)
- [Página Configuración de base de datos &#40;Administrador de configuración de Master Data Services&#41;](../master-data-services/database-configuration-page-master-data-services-configuration-manager.md)
- [Novedades en Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md)