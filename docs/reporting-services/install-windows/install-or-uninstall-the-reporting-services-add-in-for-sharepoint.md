---
title: Instalar o desinstalar el complemento de Reporting Services para SharePoint | Microsoft Docs
description: Aprenda a ejecutar el paquete de instalación complemento Microsoft SQL Server Reporting Services para productos de SharePoint (rsSharePoint.msi) en los servidores de SharePoint para habilitar características de Reporting Services dentro de una implementación de SharePoint.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
ms.assetid: c2804a9a-08ea-4f4a-805d-a2c19c68733d
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: fc01d1323d3145b16c9a6317509e72715f5a12ce
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91890862"
---
# <a name="install-or-uninstall-the-reporting-services-add-in-for-sharepoint-ssrs"></a>Instalación o desinstalación del complemento Reporting Services para SharePoint (SSRS)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  Ejecute el paquete de instalación del complemento de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para productos de SharePoint (rsSharePoint.msi) en los servidores de SharePoint para proporcionar características de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dentro de una implementación de SharePoint. Entre las características se incluyen Power View, un elemento web del Visor de informes, un punto de conexión proxy URL, tipos de contenido y páginas de aplicación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], para que pueda crear, ver y administrar informes, orígenes de datos y otro contenido de un servidor de informes en un sitio de SharePoint. El complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para productos de SharePoint es un componente necesario para un servidor de informes que se ejecute en modo de SharePoint. El complemento se puede instalar desde el Asistente para instalación de SQL Server 2016 o mediante la descarga del archivo rsSharePoint.msi desde el Feature Pack de SQL Server 2016. Para obtener una lista de las versiones del complemento, así como de las páginas de descarga, vea [Dónde encontrar el complemento Reporting Services para Productos de SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md).  
  
> [!NOTE]
> La integración de Reporting Services con SharePoint ya no está disponible a partir de SQL Server 2016.
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a> Requisitos previos  
 La instalación del complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] es uno de los diversos pasos necesarios para integrar un servidor de informes con una instancia de un producto de SharePoint. Para obtener más información sobre cómo instalar y configurar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Instalación del primer servidor de informes en modo de SharePoint](install-the-first-report-server-in-sharepoint-mode.md).  
  
-   Si va a integrar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] con una granja de SharePoint que tenga varias aplicaciones front-end web, instale el complemento en cada equipo de la granja que tenga un servidor front-end web. Haga esto únicamente para los front-end web que se vayan a usar para tener acceso al contenido del servidor de informes.  
  
-   Para instalar el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] debe ser administrador en el equipo. Por ejemplo, si va a ejecutar rsSharePoint.msi en el símbolo del sistema, debe abrir el símbolo del sistema con privilegios de administrador mediante la opción **Ejecutar como administrador** .  
  
-   Para instalar el complemento de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , debe ser miembro del grupo Administradores de la granja de SharePoint.  
  
-   Debe ser administrador de la colección de sitios para activar la característica de integración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .   
  
##  <a name="what-does-the-add-in-install"></a><a name="bkmk_whatinstalled"></a> ¿Qué instala el complemento?  
 El proceso de instalación del complemento consta de dos fases, las cuales se completan automáticamente al completar una instalación estándar:  
  
-   La primera fase es instalar los archivos en las carpetas correspondientes. Las carpetas son estándar para las implementaciones de SharePoint. Uno de los archivos que se instala es rsCustomAction.exe.  
  
-   La segunda parte de la instalación es ejecutar un conjunto de acciones personalizadas para registrar los archivos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] con SharePoint. Las acciones personalizadas se ejecutan desde rsCustomAction.exe. El archivo .exe se quita cuando se completa toda la instalación en dos fases. Puede ejecutar una instalación de **solo archivos** y rsCustomAction.exe no se ejecutará al final de la instalación permaneciendo en la unidad de disco.  
  
## <a name="the-reporting-services-installation-order"></a>Orden de instalación de Reporting Services  
 El complemento se puede instalar antes de instalar SharePoint o después de la instalación de SharePoint. El complemento sigue las normas previas a la implementación de SharePoint e instala los archivos en las ubicaciones utilizadas por la instalación de SharePoint.  
  
> [!NOTE]  
>  La ventaja de instalar el complemento antes que el producto de SharePoint es que, cuando se agreguen nuevos servidores a la granja, el complemento Reporting Services será configurado y activado por la granja de SharePoint.  
  
##  <a name="overview-of-the-installation-methods"></a><a name="bkmk_3ways_to_install"></a> Información general de los métodos de instalación  
 El complemento de SQL Server 2016 Reporting Services para productos de SharePoint se puede instalar con uno de los dos métodos siguientes:  
  
-   **Asistente para la instalación:** ![Nota:](/analysis-services/analysis-services/instances/install-windows/media/ssrs-fyi-note.png "nota") En SQL Server 2016, el complemento se puede instalar con el Asistente para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elija **Complemento de Reporting Services para productos de SharePoint** en la página **Selección de características** del asistente.  
  
-   **rsSharepoint.msi:** el complemento se puede instalar directamente desde el disco de instalación o descargarse e instalarse. rsSharepoint.msi admite la instalación desde una interfaz gráfica de usuario y desde una línea de comandos. Debe ejecutar el archivo .msi con privilegios de administrador; para ello, primero abra una ventana del símbolo del sistema con permisos elevados y, a continuación, ejecute rsSharepoint.msi desde la línea de comandos. Para obtener más información, vea [Dónde encontrar el complemento Reporting Services para Productos de SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md).  
  
    > [!NOTE]  
    >  Si usa el modificador **q** para una instalación silenciosa desde la línea de comandos, el contrato de licencia para el usuario final no se mostrará. Independientemente del método de instalación, el uso de este software se rige por un contrato de licencia y usted es responsable de su cumplimiento.  
  
##  <a name="install-the-add-in-using-the-installation-file-rssharepointmsi"></a><a name="bkmk_install_rssharepoint"></a> Instalar el complemento mediante el archivo de instalación rsSharePoint.msi  
 Esta sección está relacionada con la instalación directa de rssharepoint.msi, ya sea ejecutando el asistente para la instalación de .msi o una instalación desde la línea de comandos. Si ha instalado el complemento utilizando el Asistente para la instalación de SQL Server, no necesita seguir estos pasos.  
  
 Puede ver una lista completa de modificadores de la línea de comandos ejecutando el comando siguiente:  
  
```  
Rssharepoint.msi /?  
```  
  
1.  Descargue el programa de instalación (**rsSharepoint.msi**) del complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Para obtener más información, vea [Dónde encontrar el complemento Reporting Services para Productos de SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md).  
  
2.  Como administrador, ejecute **rsSharepoint.msi** para ejecutar el Asistente para la instalación. El asistente muestra una página de bienvenida, los términos de licencia del software y una página de información de registro. El programa de instalación crea carpetas en la ruta siguiente y copia los archivos en ellas:  
  
     `%program files%\common files\Microsoft Shared\Web Server Extensions\15\` (SharePoint 2013)
  
     or  
  
     `%program files%\common files\Microsoft Shared\Web Server Extensions\16\` (SharePoint 2016)  
  
3.  Configure los parámetros del servidor de informes y la activación de características en Administración central de SharePoint. . Para obtener más información sobre cómo instalar y configurar el modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Instalación del primer servidor de informes en modo de SharePoint](install-the-first-report-server-in-sharepoint-mode.md).  
  
###  <a name="files-only-installation"></a><a name="bkmk_files_only_installation"></a> Instalación de solo archivos  
 Para instalar los archivos, pero omitir la fase de instalación de acciones personalizadas, ejecute rssharepoint.msi desde la línea de comandos con la opción SKIPCA.  
  
1.  Abra un símbolo del sistema **con permisos de administrador**.  
  
2.  Ejecute el siguiente comando:  
  
    ```  
    Msiexec.exe /i rsSharePoint.msi SKIPCA=1  
    ```  
  
 La interfaz de usuario de instalación se abrirá y se ejecutará como normal y el archivo de **rsCustomAction.exe** se instala. Sin embargo, el archivo .exe no se ejecutará al final de la instalación y **rsCustomAction.exe** permanecerá en el equipo una vez completada la instalación.  
  
### <a name="use-a-two-step-installation-to-troubleshoot-installation-issues"></a>Usar una instalación de dos pasos para solucionar problemas de instalación  
 Si obtiene errores durante la instalación, puede ejecutar el programa de instalación como un proceso de dos pasos desde la línea de comandos:  
  
1.  Abra un símbolo del sistema **con permisos de administrador** y ejecute la instalación de solo archivos que se describe en la sección anterior.  
  
2.  Ejecute el archivo ejecutable de acciones personalizadas:  
  
    1.  Navegue hasta la carpeta que contiene el archivo **rsCustomAction.exe**. La instalación de solo archivos del complemento copiará este archivo al equipo. **rsCustomAction.exe** se encuentra en el directorio **%Temp%** . Para navegar hasta el archivo, escriba lo siguiente desde el símbolo del sistema:  
  
         **CD %temp%** .  
  
         El archivo debe estar en **\Usuarios\\<su nombre\>\AppData\Local\Temp**  
  
    2.  Escriba el siguiente comando. Este paso de configuración tardará varios minutos en completarse. El servicio W3SVC se reiniciará durante este proceso. Se mostrarán varios mensajes de estado cuando el programa copie archivos, registre componentes y ejecute el asistente para la configuración de SharePoint.  
  
        ```  
        rsCustomAction.exe /i  
        ```  
  
    3.  El tiempo necesario para que los cambios surtan efecto puede variar según el entorno de servidor. También puede ejecutar **iisreset** para forzar una actualización más rápida.  
  
### <a name="quiet-installation-for-scripting"></a>Instalación silenciosa para scripting  
 Puede usar los modificadores **/q** o **/quiet** para realizar una instalación "silenciosa" que no mostrará cuadros de diálogos ni advertencias. La instalación silenciosa es útil si desea incluir en el script la instalación del complemento.  
  
> [!NOTE]  
>  Si usa el modificador **q** para una instalación silenciosa desde la línea de comandos, el contrato de licencia para el usuario final no se mostrará. Independientemente del método de instalación, el uso de este software se rige por un contrato de licencia y usted es responsable de su cumplimiento.  
  
 Para realizar una instalación silenciosa:  
  
1.  Abra un símbolo del sistema **con permisos de administrador**.  
  
2.  Ejecute el siguiente comando:  
  
    ```  
    Msiexec.exe /i rsSharePoint.msi /q  
    ```  
  
##  <a name="how-to-remove-the-reporting-services-add-in"></a><a name="bkmk_remove_addin"></a> Quitar el complemento Reporting Services  
 Puede desinstalar el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para productos de SharePoint en el panel de control de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows o en la línea de comandos.  
  
1.  Con el panel de control se ejecutará una desinstalación completa de los archivos en el equipo actual **Y** se quitará el objeto y las características de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la granja de SharePoint. Cuando se quitan el objeto y las características de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , ya no se pueden revisar ni actualizar los informes.  
  
2.  El método de línea de comandos para desinstalar el complemento permite usar el parámetro LocalOnly para eliminar solo los archivos de complemento del equipo local y el objeto y las características de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la granja no se cambiarán.  
  
 Al desinstalar el complemento se quitarán las características de integración de servidor que se utilizan para procesar informes en un servidor de informes. También se quitarán las páginas de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de Administración central de SharePoint y otras páginas personalizadas de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . También es posible que desee quitar todos los informes y otros elementos del servidor de informes que ya no use en los sitios de SharePoint afectados. No se ejecutarán después de haber quitado el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Para desinstalar el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , debe tener una instalación de SharePoint que siga funcionando. Si desinstala primero SharePoint, debe volverlo a instalar para desinstalar el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Los pasos para desinstalar el complemento son los mismos tanto para los servidores independientes como para las granjas de servidores. El programa de instalación quitará los archivos de programa y todos los valores de configuración agregados durante la instalación.  
  
 Al desinstalar el complemento no se quitará lo siguiente:  
  
-   Los inicios de sesión creados para la cuenta de servicio del servidor de informes que se usan para tener acceso a la configuración y a las bases de datos de contenido de SharePoint. Debe eliminar cualquier inicio de sesión para la cuenta del servicio Servidor de informes de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] que se use para hospedar las bases de datos SharePoint.  
  
-   Los permisos o grupos que haya creado para los usuarios de informes. Si creó niveles de permisos personalizados o grupos de SharePoint para conceder acceso a las características del servidor de informes, debe revocar todos los permisos que ya no son necesarios.  
  
-   Los archivos de datos que haya cargado en una biblioteca de SharePoint, incluidos los archivos de definición de informe (.rdl), de origen de datos compartido (.rsds) y de elementos de informes publicados (.rsc). Estos archivos no se eliminarán, pero dejarán de ejecutarse. Deberá eliminar estos archivos manualmente.  
  
-   El programa de instalación no eliminará la base de datos del servidor de informes ni modificará la instancia del servidor de informes utilizada para las operaciones integradas.  
  
### <a name="to-uninstall-from-windows-control-panel"></a>Para desinstalar desde el Panel de control de Windows  
 Para iniciar el asistente desde el Panel de control de [!INCLUDE[msCoName](../../includes/msconame-md.md)] y quitar el complemento:  
  
1.  En el Panel de control, en **Programas**, seleccione **Desinstalar un programa**  
  
2.  Seleccione **Complemento RS de Microsoft SQL Server para SharePoint**. También puede iniciar el asistente de desinstalación mediante la ejecución de **rssharepoint.msi** desde el símbolo del sistema sin los modificadores.  
  
3.  Haga clic en **Quitar**.  
  
### <a name="uninstall-from-the-command-line"></a>Desinstalar desde la línea de comandos  
 Para desinstalar el complemento desde la línea de comandos:  
  
1.  Abra un símbolo del sistema **con permisos de administrador**.  
  
2.  Ejecute el siguiente comando:  
  
    ```  
    msiexec.exe /uninstall rsSharePoint.msi  
    ```  
  
3.  Aparecerá un cuadro de mensaje de confirmación. Haga clic en **Sí**.  
  
### <a name="uninstall-the-add-in-from-the-local-server-only"></a>Desinstalar el complemento solamente del servidor local  
 Los métodos anteriores de desinstalación del complemento quitarán las características y el objeto de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la granja. Si tiene una granja con varios servidores y desea desinstalar el complemento solo del equipo local y dejar la granja de SharePoint en un estado funcional, complete los siguientes pasos:  
  
1.  Abra un símbolo del sistema **con permisos de administrador**.  
  
2.  Ejecute el siguiente comando:  
  
    ```  
    Msiexec.exe /uninstall rsSharePoint.msi LocalOnly=1  
    ```  
  
 De este modo, se eliminarán del Registro los componentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de SharePoint y se quitarán los archivos, pero solo para el equipo local.  
  
 Si desea eliminar del Registro las características de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de SharePoint pero que los archivos permanezcan en el disco para su uso posterior, complete los siguientes pasos:  
  
1.  Abra un símbolo del sistema **con permisos de administrador**.  
  
2.  Ejecute el siguiente comando:  
  
    ```  
    rsCustomAction.exe /p  
    ```  
  
 En los pasos anteriores se supone que completó una instalación de .msi con SkipCA=1 y que el archivo rscusstomaction.exe está disponible. Para obtener más información, vea la sección que describe la instalación de solo archivos.  
  
##  <a name="how-to-repair-rssharepointmsi-from-the-command-line"></a><a name="bkmk_repair"></a> Reparar rssharepoint.msi desde la línea de comandos  
 Para reparar o desinstalar el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] mediante la línea de comandos, complete los siguientes pasos:  
  
1.  Abra un símbolo del sistema **con permisos de administrador**.  
  
2.  Ejecute el siguiente comando:  
  
    ```  
    msiexec.exe /f rssharepoint.msi  
    ```  
  
##  <a name="setup-log-files"></a><a name="bkmk_logfiles"></a> Archivos de registro del programa de instalación  
 Cuando se ejecuta el programa de instalación, la información se guarda en un archivo de registro en la carpeta **%temp%** del usuario que instaló el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Por ejemplo, **c:\Usuarios\\<nombreDeUsuario\>\AppData\Local\Temp**. El nombre de archivo es **RS_SP_\<number>.log** (por ejemplo, **RS_SP_0.log**). Los errores que aparecen en el registro empiezan con la cadena "SSRSCustomActionError".  
  
> [!NOTE]  
>  AppData es una carpeta oculta en el sistema operativo Windows. Puede que necesite modificar la configuración de carpetas del Explorador de Windows para poder ver carpetas y archivos ocultos.  
  
#### <a name="view-a-log-file-with-windows-notepad"></a>Ver un archivo de registro con el Bloc de notas de Windows  
  
1.  Los comandos siguientes cambiarán la ruta de acceso del símbolo del sistema, enumerarán los archivos de registro rs y abrirán uno de los archivos con el Bloc de notas de Windows:  
  
    ```  
    cd %temp%  
    ```  
  
    ```  
    Dir rs_sp*.log  
    ```  
  
    ```  
    notepad rs_sp_3.log  
    ```  
  
#### <a name="view-a-log-file-with-powershell"></a>Ver un archivo de registro con PowerShell  
  
1.  Escriba el comando siguiente desde el Shell de administración de SharePoint para devolver una lista filtrada de filas del archivo que contengan "ssrscustomactionerror":  
  
    ```  
    Get-content -path C:\Users\<UserName\AppData\Local\Temp\rs_sp_0.log | select-string "ssrscustomactionerror"  
    ```  
  
2.  La salida tendrá una apariencia parecida a la siguiente:  
  
     `2011-05-23 12:40:12: SSRSCustomActionError: SharePoint is installed, but not configured`.  
  
##  <a name="upgrade"></a><a name="bkmk_upgrade"></a> Actualización  
 Si ya dispone de una instalación del complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puede actualizarla a la versión actual. El programa de instalación del complemento detectará la versión existente y solicitará que confirme la actualización. El mensaje será similar al siguiente:  
  
 **Se ha detectado en el sistema una versión anterior de este producto. ¿Desea actualizar la instalación existente?**  
  
 Si lo confirma, se quitará la versión anterior del complemento y, a continuación, se instalará la nueva.  
  
 Tenga en cuenta que el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no reconoce las instancias. Solo puede tener una instancia del complemento en cada equipo. No puede ejecutar versiones diferentes en paralelo con la versión actual.  
  
##  <a name="rscustomactionexe"></a><a name="bkmk_rscustomaction"></a> RsCustomAction.exe  
 En la siguiente tabla se resumen los modificadores de rscustomaction.exe:  
  
|Switch|Descripción|  
|------------|-----------------|  
|i|Instalación de las acciones personalizadas. Se registrarán los componentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en SharePoint. Se reiniciará W3SVCservice.|  
|r|Reparación|  
|u|Desinstalación. Se eliminarán del Registro los componentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de toda la granja de servidores de SharePoint pero los archivos permanecerán en el disco. Se reiniciará W3SVCservice.|  
|p|Desinstalación local. Se eliminarán del Registro los componentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] solamente del equipo local. Los archivos permanecerán en el disco. Se reiniciará W3SVCservice.|  
|t|Solo SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 2005. El modificador comprueba si el servidor de informes tiene una conexión activa con la base de datos del servidor de informes.|  
  
## <a name="configuring-reporting-services"></a>Configurar Reporting Services  
 Una vez instalado el complemento en todos los equipos necesarios, es preciso configurar el servidor de informes desde Administración central de SharePoint. Los pasos necesarios dependerán del orden en que se instalaron las distintas tecnologías. Para obtener más información, vea [Instalación del primer servidor de informes en modo de SharePoint](install-the-first-report-server-in-sharepoint-mode.md) y [Servidor de informes de Reporting Services &#40;modo de SharePoint&#41;](../../reporting-services/report-server-sharepoint/reporting-services-report-server-sharepoint-mode.md)  
  
## <a name="see-also"></a>Consulte también

[Instalación del primer servidor de informes en modo de SharePoint](install-the-first-report-server-in-sharepoint-mode.md)   
[Servidor de informes de Reporting Services &#40;modo de SharePoint&#41;](../../reporting-services/report-server-sharepoint/reporting-services-report-server-sharepoint-mode.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).