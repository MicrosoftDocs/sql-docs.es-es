---
title: Instalación y configuración
description: Obtenga información acerca de cómo instalar Master Data Services en un equipo con Windows Server 2012 R2, configurar la base de datos y el sitio web de MDS e implementar los modelos y datos de ejemplo.
ms.custom: ''
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: quickstart
ms.assetid: f6cd850f-b01b-491f-972c-f966b9fe4190
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 630ee8396c3f4f64e7eee5335484299f5bebfd42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350703"
---
# <a name="master-data-services-installation-and-configuration"></a>Instalación y configuración de Master Data Services

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  En este artículo se describe cómo instalar [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] en una máquina con Windows Server 2012 R2, configurar la base de datos y el sitio web de MDS e implementar los modelos y datos de ejemplo. [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) le permite a su organización administrar una versión de datos de confianza.   
  
> [!NOTE] 
> Puede instalar [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] en un equipo con Windows 10 si usa la edición Developer, que ahora es compatible con [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]. 
>>Para más información sobre la compatibilidad de los sistemas operativos para las distintas ediciones de [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)], vea [Requisitos de hardware y software para instalar SQL Server 2016](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). 

Para obtener información general sobre cómo organizar los datos en [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], consulte [Introducción a Master Data Services (MDS)](../master-data-services/master-data-services-overview-mds.md).     
  
 Para obtener información sobre las nuevas características de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], consulte [Novedades en Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md).  
 
Para obtener vínculos a los vídeos y a otros recursos de aprendizaje de [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], consulte [Información sobre SQL Server Master Data Services](../master-data-services/learn-sql-server-master-data-services.md). 
  
> **Descargar**  
> -   Para descargar [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], vaya al  **[Centro de evaluación](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-ctp/)** .  
> -   ¿Tiene una cuenta de Azure?  Si es así, vaya **[aquí](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)** para poner en marcha una máquina virtual con SQL Server ya instalado.  
> 
> **¿No puede crear un sitio web de MDS?**
> >Consulte este artículo de soporte técnico de Microsoft para obtener instrucciones sobre cómo resolver este problema.
> [No se puede crear un sitio web de MDS mediante una cuenta con pocos privilegios en SQL Server 2016](https://aka.ms/mdssupport) 

## <a name="internet-explorer-and-silverlight"></a>Internet Explorer y Silverlight
- Al instalar [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] en un equipo con Windows Server 2012, podría tener que configurar la seguridad mejorada de Internet Explorer para permitir el scripting para el sitio de la aplicación web. De lo contrario, la exploración al sitio en el equipo servidor producirá errores.
- Para trabajar en la aplicación web, Silverlight 5 debe estar instalado en el equipo cliente. Si no tiene la versión necesaria de Silverlight, se le pedirá que la instale cuando navegue a un área de la aplicación web que la necesite. Puede instalar Silverlight 5 **[aquí](https://www.microsoft.com/silverlight/)**.

## <a name="ssmdsshort_md-on-an-azure-virtual-machine"></a>[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] en una máquina virtual de Azure
De forma predeterminada, cuando se pone en marcha una máquina virtual de Azure con [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] ya instalado, [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] también se instala. 

El siguiente paso consiste en instalar Internet Information Services (IIS). Vea la sección [Instalación y configuración de IIS](#InstallIIS). 

Si le interesa hacer cambios en la instalación de [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)], encontrará el archivo setup.exe en la ubicación predeterminada, `<drive>`:\SQLServer_13.0_Full.
  
## <a name="installing-master-data-services"></a><a name="InstallMDS"></a> Instalación de Master Data Services  
 Use el asistente para la instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o un símbolo del sistema para instalar [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
 **Para instalar [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] con Instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en una máquina con Windows Server 2012 R2**  
  
1.  Haga doble clic en Setup.exe y siga los pasos del asistente para la instalación.  
  
2.  Seleccione [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] en la página **Selección de características** en **Características compartidas**.  
  
     Esto instala [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], ensamblados, un complemento de Windows PowerShell y carpetas y archivos para las aplicaciones y los servicios web.  
  
     ![mds_SQLServer2016Setup_FeatureSelection](../master-data-services/media/mds-sqlserver2016setup-featureselection.png "mds_SQLServer2016Setup_FeatureSelection")  
  
3.  Complete el asistente para la instalación.  

## <a name="installing-and-configuring-iis"></a><a name="InstallIIS"></a> Instalación y configuración de IIS
  
1.  En [!INCLUDE[winblue_server_2](../includes/winblue-server-2-md.md)], haga clic en el icono **Administrador del servidor** de la barra de tareas en el **Escritorio**.  
  
     ![Icono del Administrador del servidor en la barra de tareas de Windows Server 2012](../master-data-services/media/mds-windowsservertaskbar-servermanagericon.png "Icono del Administrador del servidor en la barra de tareas de Windows Server 2012")  
  
5.  En **Administrador del servidor**, haga clic en **Agregar roles y características** en el menú **Administrar** .  
   
     ![En administración del servidor, comando de menú Agregar roles y características](../master-data-services/media/mds-servermanagerdashboard-addrolesfeaturesmenu.png "En administración del servidor, comando de menú Agregar roles y características")  
  
6.  En la página **Tipo de instalación** del **Asistente para agregar roles y características**, acepte el valor predeterminado (**Instalación basada en características o en roles**) y haga clic en **Siguiente**.  
  
7.  Haga clic en **Seleccionar un servidor del grupo de servidores** y, después, haga clic en el servidor en el que ha instalado [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
     ![mds_AddRolesFeaturesWizard_ServerSelectionPage](../master-data-services/media/mds-addrolesfeatureswizard-serverselectionpage.png) 
  
8. En la página **Roles de servidor**, haga clic en **Servidor web** y en **Siguiente**. 

   ![mds_AddRolesFeaturesWizard_ServerRolesPage](../master-data-services/media/mds-addrolesfeatureswizard-serverrolespage.png)
   
9. En la página **Características**, compruebe que estén seleccionadas las siguientes características y haga clic en **Siguiente**. Estas características son necesarias para [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] en [!INCLUDE[winblue_server_2_md](../includes/winblue-server-2-md.md)].
  
    |Características|Características|  
    |--------------|--------------|  
    |![mds_AddRolesFeaturesWizard_FeaturesPage](../master-data-services/media/mds-addrolesfeatureswizard-featurespage.png)|![mds_AddRolesFeaturesWizard_FeaturesPage_WindowsProcActive](../master-data-services/media/mds-addrolesfeatureswizard-featurespage-windowsprocactive.png)|  

10. En el panel izquierdo, haga clic en **Rol de servidor web (IIS)** y en **Servicios de rol**.
11. En la página **Servicios de rol**, compruebe que estén seleccionados los siguientes servicios y haga clic en **Siguiente**. Estos servicios son necesarios para [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] en [!INCLUDE[winblue_server_2](../includes/winblue-server-2-md.md)].

    > [!WARNING]  
    >  No instale el servicio de rol Publicación en WebDAV. Publicación en WebDAV no es compatible con [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
     |Servicios de rol|Servicios de rol|  
    |-----------------------------|-----------------------------|  
    |![mds_AddRolesFeaturesWizard_RoleServicesPage](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage.png)|![mds_AddRolesFeaturesWizard_RoleServicesPage_PerformSecurity](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-performsecurity.png)|  
    |![mds_AddRolesFeaturesWizard_RoleServicesPage_AppDevsection](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-appdevsection.png)|![mds_AddRolesFeaturesWizard_RoleServicesPage_ManageToolssection](../master-data-services/media/mds-addrolesfeatureswizard-roleservicespage-managetoolssection.png)|  
    |||  
  
     Para obtener una lista de las características y servicios de rol necesarios en otros sistemas operativos, vea [Requisitos de la aplicación web &#40;Master Data Services&#41;](../master-data-services/install-windows/web-application-requirements-master-data-services.md).   
  
 Para obtener más información sobre la instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con el programa de instalación, consulte [Instalación de SQL Server 2016 desde el Asistente para la instalación &#40;programa de instalación&#41;](../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).  
  
 Para obtener más información sobre la instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mediante un símbolo del sistema, consulte [Instalar SQL Server 2016 desde el símbolo del sistema](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md). Si se usa un símbolo del sistema, [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] está disponible como parámetro de característica.  
  
 Para obtener una descripción breve con vínculos a información adicional sobre las tareas previas a la instalación, consulte [Instalar Master Data Services](../master-data-services/install-windows/install-master-data-services.md).  
  
##  <a name="setting-up-the-database-and-website"></a><a name="SetUpWeb"></a> Configurar la base de datos y el sitio web  
 **Para configurar la base de datos y el sitio web mediante [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]**  

 
> [!WARNING]
>  Debe [instalar IIS](#InstallIIS) antes de iniciar el Administrador de configuración de [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]. Si no lo hace, el Administrador de configuración mostrará un error de Internet Information Services y no podrá crear la aplicación web de [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)].  
> 
> **Requisitos del explorador**
> >La aplicación web de [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] solo funciona en Internet Explorer (IE) 9 o posterior. No se admite ni IE 8 ni versiones anteriores, así como tampoco Microsoft Edge ni Chrome.    
  
1.  Inicie el [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]y haga clic en **Configuración de base de datos** en el panel izquierdo.  
  
2.  Haga clic en **Crear base de datos** y, después, haga clic en **Siguiente** en el **Asistente para crear bases de datos**.  
  
3.  En la página **servidor de base de datos** , especifique la instancia de SQL Server. 

    >  [!INCLUDE[sqlv15](../includes/sssql19-md.md)] agrega compatibilidad con Instancia administrada de SQL Server. Establezca el valor de **SQL Server instancia** en el host de la instancia administrada. Por ejemplo, `xxxxxx.xxxxxx.database.windows.net`.

4. Seleccione el **tipo de autenticación** y, a continuación, haga clic en **conexión de prueba** para confirmar que puede conectarse a la base de datos con las credenciales del tipo de autenticación que ha seleccionado. Haga clic en **Next**.

    >Para [!INCLUDE[sqlv15](../includes/sssql19-md.md)] , para conectarse a la instancia administrada, use uno de los siguientes tipos de autenticación:
    >
    >- Autenticación integrada de Azure Active Directory: **usuario actual: Active Directory integrada**
    >- Autenticación SQL Server: **SQL Server cuenta**.
    >
    >En SQL Instancia administrada, el usuario debe ser miembro del `sysadmin` rol fijo de servidor.

    > [!NOTE]  
    >  Al seleccionar **Usuario actual: Seguridad integrada** como el tipo de autenticación, el cuadro **Nombre de usuario** es de solo lectura y muestra el nombre de la cuenta de usuario de Windows con la que se ha iniciado sesión en el equipo. Si ejecuta [!INCLUDE[ssCurrent_md](../includes/sscurrent-md.md)] [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] en una máquina virtual de Azure, el cuadro **Nombre de usuario** mostrará el nombre de la máquina virtual y el nombre de usuario de la cuenta de administrador local de la máquina virtual. 

    ![mds_2016ConfigManager_CreateDatabaseWizard_ServerPage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-serverpage.png)  
  
4.  Escriba un nombre en el campo **Nombre de la base de datos** . Opcionalmente, para seleccionar una intercalación de Windows, desmarque la casilla **Intercalación predeterminada de SQL Server** y haga clic en una o varias de las opciones disponibles, como **Distinguir mayúsculas de minúsculas**. Haga clic en **Next**.

    ![mds_2016ConfigManager_CreateDatabaseWizard_DatabasePage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-databasepage.png)  
  
     Para obtener más información sobre la intercalación de Windows, consulte [Nombre de intercalación de Windows (Transact-SQL)](../t-sql/statements/windows-collation-name-transact-sql.md).  
  
5.  En el campo **Nombre de usuario** , especifique la cuenta de Windows del usuario que será el superusuario predeterminado de Master Data Services. Un superusuario tiene acceso a todas las áreas funcionales y puede agregar, eliminar y actualizar todos los modelos.  

    ![mds_2016ConfigManager_CreateDatabaseWizard_AdminPage](../master-data-services/media/mds-2016configmanager-createdatabasewizard-adminpage.png)  
  
6.  Haga clic en **Siguiente** para ver un resumen de la configuración de la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] y, luego, haga clic en **Siguiente** para crear la base de datos. Aparecerá la página **Avanzar y finalizar**.

7. Cuando se haya creado y configurado la base de datos, haga clic en **Finalizar**.  
  
     Para obtener información sobre la configuración del **Asistente para crear bases de datos**, consulte [Asistente para crear bases de datos &#40;Administrador de configuración de Master Data Services&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).  
  
7.  En la página **Configuración de base de datos** del [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], haga clic en **Seleccionar base de datos**.  
  
8.  Haga clic en **Conectar**, seleccione la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] que ha creado en el paso 7 y, luego, haga clic en **Aceptar**. 

    ![mds_2016ConfigManager_SelectDatabaseButton_ConnectToDatabaseDialog](../master-data-services/media/mds-2016configmanager-selectdatabasebutton-connecttodatabasedialog.png)  
  
     Ha terminado de configurar la base de datos. En la página **Configuración de base de datos** se muestra ahora la instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a la que está conectado para [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], la base de datos que ha creado y la versión actual de la base de datos.  

    ![mds_2016ConfigManager_DatabaseConfig_Completed](../master-data-services/media/mds-2016configmanager-databaseconfig-completed.png)   
  
9. En [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)], haga clic en **Configuración web** en el panel izquierdo.  
  
10. En el cuadro de lista **Sitio web** , haga clic en **Sitio web predeterminado** y, luego, haga clic en **Crear** para crear una aplicación web.  
  
    > [!NOTE]  
    >  Al seleccionar **Sitio web predeterminado**, debe crear una aplicación web. Si selecciona **Crear nuevo sitio web** en el cuadro de lista, la aplicación se crea de forma automática.  

     ![mds_2016ConfigManager_WebConfig](../master-data-services/media/mds-2016configmanager-webconfig.png)  
  
11. En la sección **Grupo de aplicaciones** , realice una de las siguientes acciones.  
  
    -   Escriba el mismo nombre de usuario que ha escrito en el paso 5 para la **Cuenta de administrador** de la base de datos, escriba la contraseña y, después, haga clic en **Aceptar**.  
  
         **De**  
  
    -   Seleccione otro nombre de usuario, escriba la contraseña y, luego, haga clic en Aceptar.  
  
         No tiene que usar la misma cuenta al crear la base de datos y la aplicación web.  

        ![mds_2016ConfigManager_WebConfig_CreateWebApplication](../master-data-services/media/mds-2016configmanager-webconfig-createwebapplication.png)   
  
     Para obtener más información sobre el cuadro de diálogo **Crear aplicación web**, consulte [Cuadro de diálogo Crear aplicación web &#40;Administrador de configuración de Master Data Services&#41;](../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md).  

    > [!NOTE] 
    >  Si el dominio implementa [los requisitos de enlace de canal ldap 2020 y firma LDAP para Windows](https://support.microsoft.com/en-us/help/4520412/2020-ldap-channel-binding-and-ldap-signing-requirements-for-windows). Verá el problema "no se pudieron comprobar las credenciales en Active Directory". al usar una cuenta de dominio para crear el grupo de aplicaciones. Para obtener una solución alternativa, en lugar de usuario de dominio, use un **usuario del equipo local**. Esto puede omitir la comprobación de credenciales con Active Directory. Después de crear la aplicación Web, puede cambiar la identidad a usuario de dominio en el **Administrador de Internet Information Services (IIS)**.
  
12. En la página **Configuración web** en el cuadro **Aplicación web** , haga clic en la aplicación que ha creado y, luego, haga clic en **Seleccionar** en la sección  **Asociar aplicación a base de datos** .  
  
13. Haga clic en **Conectar**, seleccione la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] que quiera asociar a la aplicación web y, después, haga clic en **Aceptar**.  
  
     Ha terminado de configurar el sitio web. En la página **Configuración web** se muestra ahora el sitio web que ha seleccionado, la aplicación web que ha creado y la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] asociada a la aplicación.  

     ![mds_2016ConfigManager_WebConfig_Completed](../master-data-services/media/mds-2016configmanager-webconfig-completed.png)  
 
     
15. Haga clic en **Aplicar**. Aparecerá el cuadro de mensaje **Configuración completada**. Haga clic en **Aceptar** en el cuadro de mensaje para iniciar la aplicación web. La dirección del sitio web es https://*nombre del servidor* / *web aplicación*/. 


![mds_2016ConfigurationComplete_MessageBox](../master-data-services/media/mds-2016configurationcomplete-messagebox.png) 
  
Para obtener más información sobre la configuración de la página Configuración web, consulte [Página Configuración web &#40;Administrador de configuración de Master Data Services&#41;](../master-data-services/web-configuration-page-master-data-services-configuration-manager.md)  
  
 También puede usar [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] para especificar otra configuración de las aplicaciones web y servicios asociados a la base de datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Por ejemplo, puede especificar la frecuencia con la que se cargan los datos o con la que se envían correos electrónico de validación. Para obtener más información, vea [Configuración del sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
##  <a name="deploying-sample-models-and-data"></a><a name="deploySample"></a> Implementar modelos y datos de ejemplo  
 Los siguientes tres paquetes de modelo de ejemplo se incluyen con  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].   Estos modelos de ejemplo incluyen datos. **La ubicación predeterminada de los paquetes de modelo de ejemplo es %programfiles%\Microsoft SQL Server\140\Master Data Services\Samples\Packages.**
  
-   chartofaccounts_en.pkg  
-   customer_en.pkg  
-   product_en.pkg  
  
 Implemente estos paquetes mediante la herramienta MDSModelDeploy. La ubicación predeterminada de la herramienta MDSModelDeploy es *unidad*\Archivos de programa\Microsoft SQL Server\140\Master Data Services\Configuration.  
  
 Para obtener información sobre los requisitos previos para ejecutar esta herramienta, consulte [Implementar un paquete de implementación de modelo mediante MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md).  
  
 Para información sobre las actualizaciones efectuadas en los datos para admitir nuevas características en [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vea [Ejemplos de SQL Server: paquetes de implementación de modelos (MDS)](../master-data-services/sql-server-samples-model-deployment-packages-mds.md).  
  
 **Para implementar los modelos de ejemplo**  
  
1.  Copie los paquetes de modelo de ejemplo en *unidad*\Archivos de programa\Microsoft SQL Server\140\Master Data Services\Configuration.  
  
2.  Abra un símbolo del sistema de administrador y ejecute el siguiente comando para ir a MDSModelDeploy.exe.  
  
    ```  
    cd c:\Program Files\Microsoft SQL Server\140\Master Data Services\Configuration  
    ```  
  
3.  Implemente todos los modelos de ejemplo en [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ; para ello, ejecute los siguientes comandos.  
  
    > [!IMPORTANT]  
    >  En los ejemplos siguientes, se especifica el valor de servicio de `MDS1` . Use este valor si ha seleccionado  **Sitio web predeterminado** al configurar el sitio web de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  Consulte la sección [Configurar la base de datos y el sitio web](#SetUpWeb) .  
    >   
    >  Si ha creado un sitio web nuevo o ha seleccionado otro sitio web existente, ejecute primero el comando siguiente para determinar el valor de servicio correcto.  
    >   
    >  `MDSModelDeploy listservices`  
    >   
    >  El primer valor de servicio de la lista de valores devueltos es el que especifica para implementar un modelo.  

    > [!NOTE]
    > Para obtener más información sobre los metadatos de los modelos de ejemplo, consulte el archivo Léame disponible en esta ubicación: "c:\Archivos de programa\Microsoft SQL Server\140\Master Data Services\Configuration".
   
     **Para implementar el modelo de ejemplo chartofaccounts_en.pkg**  
  
    ```console
    MDSModelDeploy deploynew -package chartofaccounts_en.pkg -model ChartofAccounts -service MDS1  
    ```  
  
     **Para implementar el modelo de ejemplo customer_en.pkg**  
  
    ```console
    MDSModelDeploy deploynew -package customer_en.pkg -model Customer -service MDS1  
    ```  
  
     **Para implementar el modelo de ejemplo product_en.pkg**  
  
    ```console
    MDSModelDeploy deploynew -package product_en.pkg -model Product -service MDS1  
    ```  
  
     Al implementar correctamente un modelo, se muestra el mensaje **MDSModelDeploy finalizó correctamente** .  
  
     En la siguiente imagen se muestra el comando para implementar el modelo de ejemplo product_en.pkg.  
  
     ![Línea de comandos para implementar el modelo de ejemplo del producto](../master-data-services/media/mds-commandprompt-deployingsamplemodel-product.png "Línea de comandos para implementar el modelo de ejemplo del producto")  
  
4.  Para ver los modelos de ejemplo, haga lo siguiente.  
  
    1.  Vaya al sitio web de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] que ha configurado. Consulte la sección [Configurar la base de datos y el sitio web](#SetUpWeb) .  
  
         La dirección del sitio web es https://*nombre del servidor* / *web aplicación*/.  
  
    2.  Seleccione un modelo del cuadro de lista **Modelo** y haga clic en **Explorador**.  
  
         ![Sitio web de MDS, Página principal.](../master-data-services/media/mds-mdswebsite-homepage-selectsamplemodel.png "Sitio web de MDS, Página principal.")  
  
## <a name="next-step"></a>siguiente paso  
 Cree un nuevo modelo y entidades para los datos. Consulte [Crear un modelo &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md) y [Crear una entidad &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
 Para obtener información general sobre cómo usar un modelo y entidades para crear una estructura para los datos de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], consulte [Introducción a Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md)  
    
## <a name="see-also"></a>Consulte también  
 [Master Data Services base de datos](../master-data-services/master-data-services-database.md)   
 [Master Data Manager aplicación Web](../master-data-services/master-data-manager-web-application.md)   
 [Página configuración de base de datos &#40;Administrador de configuración de Master Data Services&#41;](../master-data-services/database-configuration-page-master-data-services-configuration-manager.md)   
 [Novedades en Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md)  
  
