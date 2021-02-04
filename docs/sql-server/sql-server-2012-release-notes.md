---
title: Notas de la versión de SQL Server 2012 | Microsoft Docs
description: En estas notas de la versión se describen los problemas conocidos con los que familiarizarse antes de instalar Microsoft SQL Server 2012 o solucionar problemas en este.
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Release Notes, SQL Server
ms.assetid: 9ccb390a-67a9-4593-85ea-2b4c41c4620f
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 06294cb2dbf588ed5f7588d05ee5aa1c5af971bc
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076077"
---
# <a name="sql-server-2012-release-notes"></a>Notas de la versión de SQL Server 2012
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
En este documento Notas de la versión se describen los problemas conocidos que debe conocer antes de instalar [Microsoft SQL Server 2012](https://go.microsoft.com/fwlink/?LinkId=238647) o solucionar problemas en este. Este documento de notas de la versión solo está disponible en línea, no en el disco de instalación, y se actualiza periódicamente.  
  
Para obtener una introducción e información acerca de cómo instalar SQL Server 2012, vea el documento Léame de SQL Server 2012. El documento Léame está disponible en el disco de instalación y en la página de descarga [Léame](https://download.microsoft.com/download/3/B/D/3BD9DD65-D3E3-43C3-BB50-0ED850A82AD5/ENU/Readme.htm) . También puede encontrar más información en los [Libros en pantalla de SQL Server](/previous-versions/sql/sql-server-2012/ms130214(v=sql.110)) y en los foros de [SQL Server](https://go.microsoft.com/fwlink/?LinkId=213599).  
  
## <a name="10-before-you-install"></a><a name="Install"></a>1.0 Antes de la instalación  
Antes de instalar [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)], tenga en cuenta la siguiente información.  
  
### <a name="11-rules-documentation-for-sql-server-2012-setup"></a>1.1 Documentación de las reglas para la instalación de SQL Server 2012  
**Problema:** el programa de instalación de SQL Server valida la configuración del equipo antes de que se complete la operación de instalación. Las diversas reglas que se ejecutan durante la operación de instalación de SQL Server se capturan mediante el informe del Comprobador de configuración del sistema (SCC). La documentación de estas reglas de instalación ya no está disponible en MSDN Library.  
  
**Solución alternativa:** puede examinar el informe de comprobación de la configuración del sistema para obtener más información sobre estas reglas de instalación. La comprobación de la configuración del sistema genera un informe que contiene una descripción breve de cada regla ejecutada y el estado de ejecución. El informe de comprobación de la configuración del sistema se encuentra en %programfiles%\Microsoft SQL Server\110\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\.  
  
### <a name="12-adding-a-local-user-account-for-the-distributed-replay-controller-service-might-terminate-setup-unexpectedly"></a>1.2 Al agregar una cuenta de usuario local para el servicio del controlador de Distributed Replay, la instalación puede finalizar inesperadamente  
**Problema:** en la página **Distributed Replay Controller** de la instalación de SQL Server, al intentar agregar una cuenta de usuario local para el servicio Distributed Replay Controller, la instalación puede finalizar inesperadamente con el mensaje de error "Error del programa de instalación de SQL Server".  
  
**Solución alternativa:** durante la instalación de SQL, no agregue cuentas de usuario local mediante "Agregar usuario actual" o "Agregar…". Después de la instalación, agregue una cuenta de usuario local manualmente siguiendo estos pasos:  
  
1.  Detenga el servicio SQL Server Distributed Replay Controller  
  
2.  En el equipo del controlador en el que esté instalado el servicio del controlador, desde el símbolo del sistema, escriba dcomcnfg.  
  
3.  En la ventana Servicios de componentes, navegue a **Raíz de consola** -> **Servicios de componentes** -> **Equipos** -> **Mi PC** -> **Dconfig** ->**DReplayController**.  
  
4.  Haga clic con el botón derecho en **DReplayController** y luego haga clic en **Propiedades**.  
  
5.  En la ventana **Propiedades de DReplayController** , en la pestaña **Seguridad** , haga clic en **Editar** en la sección **Permisos de inicio y activación** .  
  
6.  Conceda a la cuenta de usuario local los permisos **Activación local y remota** y, a continuación, haga clic en **Aceptar**.  
  
7.  En la sección Permisos de acceso, haga clic en **Editar** y conceda a la cuenta de usuario local los permisos **Acceso local y remoto** ; a continuación, haga clic en **Aceptar**.  
  
8.  Haga clic en **Aceptar** para cerrar la ventana **Propiedades de DReplayController** .  
  
9. En el equipo controlador, agregue la cuenta de usuario local al grupo **Usuarios COM distribuidos** .  
  
10. Inicie el servicio SQL Server Distributed Replay Controller.  
  
### <a name="13-sql-server-setup-might-fail-while-trying-to-start-the-sql-server-browser-service"></a>1.3 El programa de instalación de SQL Server podría dar error al intentar iniciar el servicio SQL Server Browser  
**Problema:** el programa de instalación de SQL Server podría dar error al intentar iniciar el servicio SQL Server Browser, con errores similares a los siguientes:  
  
```
The following error has occurred:  
Service 'SQLBrowser' start request failed. Click 'Retry' to retry the failed action, or click 'Cancel' to cancel this action and continue setup.
```
  
or  
  
```
The following error has occurred:  
SQL Server Browser configuration for feature 'SQL_Browser_Redist_SqlBrowser_Cpu32' was cancelled by user after a previous installation failure. The last attempted step: Starting the SQL Server Browser service 'SQLBrowser', and waiting for up to '900' seconds for the process to complete.
```
  
**Solución alternativa:** esto puede ocurrir cuando el Motor de SQL Server o Analysis Services no se puede instalar. Para corregir este problema, consulte los registros del programa de instalación de SQL Server y solucione los errores del Motor de SQL Server y de Analysis Services. Para obtener más información, vea Ver y leer los archivos de registro de instalación de SQL Server. Para obtener más información, vea [View and Read SQL Server Setup Log Files](../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
### <a name="14-sql-server-2008-2008-r2-analysis-services-failover-cluster-upgrade-to-sql-server-2012-might-fail-after-renaming-the-network-name"></a>1.4 Se puede producir un error en la actualización del clúster de conmutación por error de SQL Server 2008 y 2008 R2 Analysis Services a SQL Server 2012 después de cambiar el nombre de red  
**Problema:** después de cambiar el nombre de red de una instancia de clúster de conmutación por error de Microsoft SQL Server 2008 o 2008 R2 Analysis Services mediante la herramienta Administrador de clústeres de Windows, es posible que se produzca un error en la operación de actualización.  
  
**Solución alternativa:** para resolver este problema, actualice la entrada del Registro ClusterName siguiendo las instrucciones incluidas en la sección Resolución de [este artículo de KB](https://support.microsoft.com/kb/955784).  
  
### <a name="15-installing-sql-server-2012-on-windows-server-2008-r2-server-core-service-pack-1"></a>1.5 Instalar SQL Server 2012 en Windows Server 2008 R2 Server Core Service Pack 1  
Puede instalar SQL Server en Windows Server 2008 R2 Server Core SP1 con las limitaciones siguientes:  
  
-   Microsoft SQL Server 2012 no admite la instalación con el Asistente para la instalación en el sistema operativo Server Core. Al realizar la instalación en Server Core, el programa de instalación de SQL Server admite el modo totalmente silencioso mediante el uso del parámetro /Q o el modo silencioso simple mediante el parámetro /QS.  
  
-   La actualización de una versión anterior de SQL Server a Microsoft SQL Server 2012 no se admite en un equipo que ejecute Windows Server 2008 R2 Server Core SP1.  
  
-   La instalación de una versión de 32 bits de la edición de Microsoft SQL Server 2012 no se admite en un equipo que ejecute Windows Server 2008 R2 Server Core SP1.  
  
-   Microsoft SQL Server 2012 no se puede instalar en paralelo con versiones anteriores de SQL Server en un equipo que ejecute Windows Server 2008 R2 Server Core SP1.  
  
-   No todas las características de SQL Server 2012 se admiten en el sistema operativo Server Core. Para obtener más información sobre las características admitidas y sobre cómo instalar SQL Server 2012 en Server Core, vea [Instalar SQL Server 2012 en Server Core](/previous-versions/sql/sql-server-2012/hh231669(v=sql.110)).  
  
### <a name="16-semantic-search-requires-you-to-install-an-additional-dependency"></a>1.6 La búsqueda semántica requiere que instale una dependencia adicional  
**Problema:** la búsqueda estadística semántica tiene un requisito previo adicional, la base de datos de estadísticas semánticas de idioma, que no instala el programa de instalación de SQL Server.  
  
**Solución alternativa:** para configurar la base de datos de estadísticas de lenguaje semántico como requisito previo para la indexación semántica, realice las tareas siguientes:  
  
1.  Busque y ejecute el paquete de Windows Installer denominado SemanticLanguageDatabase.msi en el disco de instalación de SQL Server para extraer la base de datos. Para SQL Server 2012 Express, descargue la base de datos de estadísticas de lenguaje semántico del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=52681) (https://www.microsoft.com/download/details.aspx?id=52681) ) y, después, ejecute el paquete de Windows Installer.  
  
2.  Mueva la base de datos a una carpeta de datos apropiada. Si deja la base de datos en la ubicación predeterminada, debe cambiar los permisos para poderla adjuntar correctamente.  
  
3.  Adjunte la base de datos extraída.  
  
4.  Registre la base de datos llamando al procedimiento almacenado **sp_fulltext_semantic_register_language_statistics_db** y proporcionando el nombre que dio a la base de datos cuando la adjuntó.  
  
Si estas tareas no se completan, verá un mensaje de error similar al siguiente al intentar crear un índice semántico.  
  
```
Msg 41209, Level 16, State 3, Line 1  
A semantic language statistics database is not registered. Full-text indexes using 'STATISTICAL_SEMANTICS' cannot be created or populated.
```
  
### <a name="17-installation-prerequisite-handling-during-sql-server-2012-setup"></a>1.7 Tratamiento de la instalación de requisitos previos durante la instalación de SQL Server 2012  
A continuación se describe el comportamiento de la instalación de requisitos previos durante la instalación de SQL Server 2012:  
  
-   La instalación de SQL Server 2012 solo se admite en Windows 7 SP1 o Windows Server 2008 R2 SP1. No obstante, el programa de instalación no impide la instalación de SQL Server 2012 en Windows 7 o Windows Server 2008 R2.  
  
-   .NET Framework 3.5 SP1 es un requisito para SQL Server 2012 al seleccionar Motor de base de datos, Replicación, Master Data Services, Reporting Services, Data Quality Services (DQS) o SQL Server Management Studio, y el programa de instalación de SQL Server ya no instala el marco de trabajo.  
  
    -   Si ejecuta el programa de instalación en un equipo con el sistema operativo Windows Vista SP2 o Windows Server 2008 SP2 y no tiene instalado .NET Framework 3.5 SP1, el programa de instalación de SQL Server le pedirá que descargue e instale .NET Framework 3.5 SP1 para poder continuar con la instalación de SQL Server. Puede descargar .NET Framework 3.5 SP1 desde Windows Update o directamente desde [aquí](https://www.microsoft.com/download/details.aspx?id=25150). Para evitar interrupciones durante la instalación de SQL Server, descargue e instale .NET 3.5 SP1 antes de ejecutar el programa de instalación de SQL Server.  
  
    -   Si el programa de instalación se ejecuta en un equipo con el sistema operativo Windows 7 SP1 o Windows Server 2008 R2 SP1, debe habilitar .NET Framework 3.5 SP1 antes de instalar SQL Server 2012.  
  
        **Use uno de los métodos siguientes para habilitar .NET Framework 3.5 SP1 en Windows Server 2008 R2 SP1:**  
  
        Método 1: uso de Administrador del servidor  
  
        1.  En el Administrador del servidor, haga clic en **Agregar características** para que se muestre una lista de las características posibles.  
  
        2.  En la interfaz **Seleccionar características** , expanda la entrada **Características de .NET Framework 3.5.1** .  
  
        3.  Después de expandir **Características de .NET Framework 3.5.1**, verá dos casillas. Una es para .NET Framework 3.5.1 y la otra para Activación WCF. Seleccione **.NET Framework 3.5.1** y, a continuación, haga clic en **Siguiente**. No puede instalar características de .NET Framework 3.5.1 a menos que también se instalen los servicios de rol y las características necesarios.  
  
        4.  En **Confirmar las selecciones de la instalación**, revise las selecciones y haga clic en Instalar.  
  
        5.  Deje que el proceso de instalación se complete y haga clic en **Cerrar**.  
  
        Método 2: Use Windows PowerShell  
  
        1.  Haga clic en **Inicio** | **Todos los programas** | **Accesorios**.  
  
        2.  Expanda **Windows PowerShell**, haga clic con el botón secundario en **Windows PowerShell** y haga clic en **Ejecutar como administrador**. Haga clic en **Sí** en el cuadro **Control de cuentas de usuario** .  
  
        3.  En el símbolo del sistema de PowerShell, escriba los comandos siguientes y presione ENTRAR después de cada uno:  
  
            ```  
            Import-Module ServerManager  
            Add-WindowsFeature as-net-framework  
            ```  
  
        **Use el método siguiente para habilitar .NET Framework 3.5 SP1 en Windows 7 SP1:**  
  
        1.  Haga clic en **Inicio** | **Panel de control** | **Programas** y, a continuación, haga clic en **Activar o desactivar las características de Windows**. Si se le solicita una contraseña de administrador o se le pide confirmación, escriba la contraseña o proporcione la confirmación.  
  
        2.  Para habilitar **Microsoft .NET Framework 3.5.1**, active la casilla situada junto a esa característica. Para desactivar una característica de Windows, desactive la casilla.  
  
        3.  Haga clic en **OK**.  
  
        **Use Administración y mantenimiento de imágenes de implementación (DISM.exe) para habilitar .NET Framework 3.5 SP1.**  
  
        También puede habilitar .NET Framework 3.5 SP1 mediante Administración y mantenimiento de imágenes de implementación (DISM.exe). Para obtener más información acerca de cómo habilitar características de Windows en línea, vea [Habilitar o deshabilitar características de Windows en línea](/previous-versions/windows/it-pro/windows-7/dd744582(v=ws.10)). A continuación se ofrecen las instrucciones para habilitar .NET Framework 3.5 SP1:  
  
        1.  En el símbolo del sistema, escriba el comando siguiente para enumerar todas las características disponibles en el sistema operativo.  
  
            ```  
            sm /online /Get-Features  
            ```  
  
        2.  Opcional: en el símbolo del sistema, escriba el comando siguiente para ver información sobre la característica concreta que le interese.  
  
            ```  
            Dism /online /Get-FeatureInfo /FeatureName:NetFx3  
            ```  
  
        3.  Escriba el comando siguiente para habilitar Microsoft .NET Framework 3.5.1.  
  
            ```  
            Dism /online /Enable-Feature /FeatureName:NetFx3  
            ```  
  
-   .NET Framework 4 es un requisito para SQL Server 2012. El programa de instalación de SQL Server instala .NET Framework 4 durante el paso de instalación de características.  
  
    SQL Server 2012 Express no instala .NET Framework 4.0 al realizar la instalación en el sistema operativo Windows Server 2008 R2 SP1 Server Core. Al instalar SQL Server 2012 Express (solo base de datos), .NET Framework 4 no es necesario si .NET Framework 3.5 SP1 ya está presente. Cuando .NET Framework 3.5 SP1 no está presente o al instalar SQL Server 2012 Management Studio Express, SQL Server 2012 Express con Tools o SQL Server 2012 Express con Advanced Services, debe instalar .NET Framework 4 antes de instalar SQL Server 2012 Express en un sistema operativo Windows Server 2008 R2 SP1 Server Core.  
  
-   SQL Server le exige que instale una actualización para asegurarse de que se puede instalar correctamente el componente de Visual Studio. El programa de instalación de SQL Server comprueba la presencia de esta actualización y, a continuación, le exige que descargue e instale la actualización antes de continuar con la instalación de SQL Server. Para evitar la interrupción durante la instalación de SQL Server, puede descargar e instalar la actualización, tal como se describe más abajo, antes de ejecutar el programa de instalación de SQL Server (o puede instalar todas las actualizaciones de .NET Framework 3.5 SP1 que están disponibles en Windows Update):  
  
    -   Si instala SQL Server 2012 en un equipo con el sistema operativo Windows Vista SP2 o Windows Server 2008 SP2, puede obtener la actualización requerida desde [aquí](https://support.microsoft.com/?kbid=956250).  
  
    -   Si instala SQL Server 2012 en un equipo con el sistema operativo Windows 7 SP1 o Windows Server 2008 R2 SP1, esta actualización ya está instalada en el equipo.  
  
-   Windows PowerShell 2.0 es un requisito previo para instalar los componentes del Motor de base de datos de SQL Server 2012 y SQL Server Management Studio, pero el programa de instalación de SQL Server ya no instala Windows PowerShell. Si PowerShell 2.0 no está presente en su equipo, puede habilitarlo siguiendo las instrucciones de la página [Marco de administración de Windows](https://mskb.pkisolutions.com/kb/968929) . La forma de obtener Windows PowerShell 2.0 depende del sistema operativo que esté ejecutando:  
  
    -   Windows Server 2008: Windows PowerShell 1.0 es una característica y se puede agregar. Las versiones de Windows PowerShell 2.0 se descargan e instalan (como una revisión del sistema operativo).  
  
    -   Windows 7/Windows Server 2008 R2: Windows PowerShell 2.0 se instala de forma predeterminada.  
  
-   Si piensa usar características de SQL Server 2012 en un entorno de SharePoint, se necesitan SharePoint Server 2010 Service Pack 1 (SP1) y la actualización acumulativa de agosto de SharePoint. Debe instalar SP1, la [actualización acumulativa de agosto](https://blogs.technet.com/b/stefan_gossner/archive/2010/09/02/august-2010-cumulative-update-for-sharepoint-has-been-released.aspx)de SharePoint y aplicar todas las revisiones a la granja de servidores antes de agregar características de SQL Server 2012 a la granja. Este requisito se aplica a las siguientes características de SQL Server 2012: uso de una instancia del Motor de base de datos como el servidor de base de datos de la granja de servidores, configuración de PowerPivot para SharePoint o implementación de Reporting Services en modo de SharePoint.  
  
### <a name="18-supported-operating-systems-for-sql-server-2012"></a>1.8 Sistemas operativos admitidos para SQL Server 2012  
SQL Server 2012 se admite en los sistemas operativos Windows Vista SP2, Windows Server 2008 SP2, Windows 2008 R2 SP1 y Windows 7 SP1.  
  
### <a name="19-sync-framework-is-not-included-in-the-installation-package"></a>1.9 Sync Framework no se incluye en el paquete de instalación  
**Problema:** Sync Framework no se incluye en el paquete de instalación de SQL Server 2012.  
  
**Solución alternativa:** descargue la versión adecuada de Sync Framework de [esta página del Centro de descarga de Microsoft](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=23217).  
  
### <a name="110-if-visual-studio-2010-service-pack-1-is-uninstalled-the-sql-server-2012-instance-must-be-repaired-to-restore-certain-components"></a>1.10 Si se desinstala Visual Studio 2010 Service Pack 1, se debe reparar la instancia de SQL Server 2012 para restaurar determinados componentes  
**Problema:** [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] la instalación depende de algunos componentes de Visual Studio 2010 Service Pack 1. Si desinstala el Service Pack 1, algunos de los componentes compartidos se degradan a sus versiones originales y otros se quitan por completo del equipo.  
  
**Solución alternativa:** repare la instancia de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] desde el disco de instalación de origen original o la ubicación de instalación de red.  
  
1.  Inicie el programa de instalación de SQL Server (setup.exe) desde el disco de instalación de SQL Server.  
  
2.  Después de comprobar los requisitos previos y el sistema, el programa de instalación mostrará la página **Centro de instalación de SQL Server** .  
  
3.  Haga clic en **Mantenimiento** en el área de navegación de la izquierda y, después, haga clic en **Reparar** para iniciar la operación de reparación. Si el Centro de instalación se ha iniciado con el menú **Inicio** , tendrá que proporcionar la ubicación del medio de instalación en este momento.  
  
4.  Configure las reglas auxiliares del programa de instalación y las rutinas de archivo para asegurarse de que el sistema tiene instalados los requisitos previos y de que el equipo pasa las reglas de validación del programa de instalación. Haga clic en **Aceptar** o en **Instalar** para continuar.  
  
5.  En la página **Seleccionar instancia** , seleccione la instancia que desea reparar y, a continuación, haga clic en **Siguiente** para continuar.  
  
6.  Las reglas de reparación se ejecutarán para validar la operación. Para continuar, haga clic en **Siguiente**.  
  
7.  La página **Listo para reparar** indica que la operación está lista para continuar. Para continuar, haga clic en **Reparar**.  
  
8.  La página **Progreso de la reparación** muestra el estado de la operación de reparación. La página **Operación completada** indica que la operación ha finalizado.  
  
Para obtener más información acerca de cómo reparar una instancia de SQL Server, vea [Reparar una instalación de SQL Server 2012 con error](../database-engine/install-windows/repair-a-failed-sql-server-installation.md).  
  
### <a name="111-an-instance-of-sql-server-2012-might-fail-after-an-os-upgrade"></a>1.11 Una instancia de SQL Server 2012 podría dar error después de actualizar el sistema operativo  
**Problema:** una instancia de SQL Server 2012 podría dar el error siguiente tras actualizar el sistema operativo a Windows 7 SP1 desde Windows Vista.  
  
`Setup has detected that the .NET Framework version 4 needs to be repaired. Do not restart your computer until Setup is complete.`  
  
**Solución alternativa**: repare la instalación de .NET Framework 4 después de actualizar el sistema operativo. Para obtener más información, vea [Reparar una instalación existente de .NET Framework](https://support.microsoft.com/kb/306160).  
  
### <a name="112-sql-server-edition-upgrade-requires-a-restart"></a>1.12 La actualización de SQL Server Edition necesita un reinicio  
**Problema:** cuando la edición actualiza una instancia de SQL Server 2012, es posible que algunas de las funcionalidades asociadas con la nueva edición no se activen inmediatamente.  
  
**Solución alternativa**: reinicie el equipo tras la actualización de edición de una instancia de SQL Server 2012. Para obtener más información sobre actualizaciones admitidas en SQL Server 2012, vea [Actualizaciones de versión y edición admitidas](../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md).  
  
### <a name="113-database-with-read-only-filegroup-or-files-cannot-be-upgraded"></a>1.13 No se puede actualizar una base de datos con archivos o un grupo de archivos de solo lectura  
**Problema:**  no puede actualizar una base de datos ya sea por adjuntar la base de datos o restaurarla de una copia de seguridad si la base de datos o sus archivos o grupos de archivos están configurados de solo lectura.  Se devuelve el error 3415.  Este problema también se aplica cuando se realiza una actualización en contexto de una instancia de SQL Server. Es decir, intenta reemplazar una instancia existente de SQL Server mediante la instalación de SQL Server 2012 y una o varias bases de datos existentes están configuradas de solo lectura.  
  
**Solución alternativa:** antes de actualizar, asegúrese de que la base de datos y sus archivos o grupos de archivos están configurados para lectura y escritura.  
  
### <a name="114-reinstalling-an-instance-of-sql-server-failover-custer-fails-if-you-use-the-same-ip-address"></a>1.14 Reinstalar una instancia de clúster de conmutación por error de SQL Server produce un error si utiliza la misma dirección IP  
**Problema:** Si especifica una dirección IP incorrecta durante la instalación de una instancia de clúster de conmutación por error de SQL Server, la instalación produce errores. Después de desinstalar la instancia con errores, y si intenta reinstalar la instancia de clúster de conmutación por error de SQL Server con el mismo nombre de instancia, y la dirección IP correcta, la instalación produce errores. El error se debe al grupo de recursos duplicados que deja atrás la instalación anterior.  
  
**Solución alternativa:** para resolver este problema, use otro nombre de instancia durante la reinstalación, o bien elimine manualmente el grupo de recursos antes de reinstalar. Para obtener más información, vea [Agregar o quitar nodos en un clúster de conmutación por error de SQL Server](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")  
  
## <a name="20-analysis-services"></a><a name="AS"></a>2.0 Analysis Services  
  
### <a name="21-sql-editor-and-as-editor-cannot-connect-to-their-respective-server-instances-in-the-same-ssms-instance"></a>2.1 El editor SQL y el editor de AS no pueden conectar con sus instancias de servidor respectivas en la misma instancia de SSMS  
**Problema:** no puede conectar con un servidor de Analysis Services con el editor MDX/DMX cuando el editor SQL ya está conectado.  
  
Cuando se usa SQL Server Management Studio 2012 (SSMS), si hay un archivo .sql abierto en el editor y está conectado a una instancia de SQL Server, cuando se abre un archivo DMX o DMX en la misma instancia de SSMS no se puede conectar con una instancia del servidor de AS. Asimismo, si ya hay un archivo MDX o DMX abierto en el editor en SSMS y está conectado a una instancia de servidor de AS, cuando se abre un archivo .sql en la misma instancia de SSMS no se puede conectar con una instancia de SQL Server.  
  
**Solución alternativa**:  use una de las opciones siguientes para resolver este problema.  
  
-   Inicie otra instancia de SSMS para abrir el archivo MDX/DMX.  
  
-   Desconecte el editor SQL y, después, conecte el editor MDX/DMX con un servidor de AS.  
  
### <a name="22-cannot-create-or-open-tabular-projects-when-builtinadministrators-group-name-cannot-be-resolved"></a>2.2 No se pueden crear o abrir proyectos tabulares cuando el nombre del grupo BUILTIN\Administrators no se puede resolver  
**Problema:** para poder crear o abrir proyectos tabulares, debe ser administrador de un servidor de base de datos de área de trabajo. Se puede agregar un usuario al grupo administradores de servidor agregando el nombre de usuario o de grupo. Si es miembro del grupo BUILTIN\Administrator, no puede crear o modificar los archivos BIM a menos que el servidor de base de datos de área de trabajo se una al dominio desde el que se aprovisionó originalmente. Si abre o crea el archivo BIM, se producirá un error y se mostrará el siguiente mensaje de error:  
  
`"The BIM file cannot be opened. The server connected to is not valid. Reason: You are not an administrator of server [server name]."`  
  
**Soluciones alternativas:**  
  
-   Vuelva a unir el servidor de base de datos del área de trabajo y el equipo Herramientas de datos de SQL Server (SSDT) al dominio.  
  
-   Si el servidor de bases de datos del área de trabajo y/o los equipos con SSDT no van a estar unidos al dominio en todo momento, agregue nombres de usuario individuales en lugar del grupo BUILTIN\Administrators como administradores del servidor de bases de datos del área de trabajo.  
  
### <a name="23-ssis-components-for-as-tabular-models-do-not-work-as-expected"></a>2.3 Los componentes SSIS para modelos tabulares AS no funcionan como se esperaba  
Los componentes de SQL Server Integration Services (SSIS) para Analysis Services (AS) no funcionan de la manera esperada con los modelos tabulares. A continuación se indican problemas conocidos que pueden producirse al intentar escribir un paquete SSIS para que funcione con modelos tabulares.  
  
**Problema:** el Administrador de conexiones AS no puede usar un modelo tabular en la misma solución que un origen de datos.  
  
**Solución alternativa:** debe conectarse de modo explícito al servidor de AS antes de continuar la Tarea de procesamiento de AS o la tarea Ejecutar DDL de AS.  
  
Se producen problemas con la Tarea de procesamiento de AS cuando se trabaja con modelos tabulares:  
  
**Problema:** en lugar de bases de datos, tablas y particiones, ve cubos, grupos de medida y dimensiones. Se trata de una limitación de la tarea.  
  
**Solución alternativa:** aún puede procesar el modelo tabular mediante la estructura de cubo, grupo de medida y dimensión.  
  
**Problema:** algunas opciones de procesamiento admitidas por AS en modo tabular no se exponen en la Tarea de procesamiento de AS, como Procesar desfragmentación.  
  
**Solución alternativa:** use la tarea Ejecutar DDL de Analysis Services en lugar de ejecutar un script XMLA que contenga el comando ProcessDefrag.  
  
**Problema:** algunas opciones de configuración de la herramienta no son aplicables. Por ejemplo,"Procesar objetos relacionados" no se debe usar al procesar particiones y la opción de configuración "Procesamiento paralelo" contiene un mensaje de error no válido que indica que el procesamiento paralelo no se admite en la SKU Standard.  
  
**Solución alternativa:** None  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")  
  
## <a name="30-books-online"></a><a name="BOL"></a>3.0 Libros en pantalla  
  
### <a name="31-help-viewer-for-sql-server-crashes-in-environments-configured-to-run-only-ipv6"></a>3.1 El Visor de Ayuda para SQL Server se bloquea en entornos configurados para ejecutar solo IPv6  
**Problema:** si el entorno está configurado para ejecutar IPv6 únicamente, el Visor de la Ayuda para SQL Server 2012 se bloqueará y aparecerá el mensaje de error siguiente:  
  
`HelpLibAgent.exe has stopped working.`  
  
> [!IMPORTANT]  
> Se aplica a todos los entornos que se ejecuten solo con IPv6 habilitado. Los entornos habilitados para IPv4 (e IPv4 con IPv6) no están afectados.  
  
**Solución alternativa**: para evitar este problema, habilite IPv4 o siga estos pasos para agregar una entrada del Registro y cree una ACL para habilitar el Visor de la Ayuda para IPv6:  
  
1.  Cree una clave del Registro con el nombre "IPv6" y el valor "1 (DWORD (32 bits))" en HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Help\v1.0.  
  
2.  Establezca las ACL de seguridad del puerto para IPv6; para ello, ejecute el siguiente comando en una ventana CMD de administrador:  
  
    ```  
    netsh http add urlacl url=https://[::1]:47873/help/ sddl=D:(A;;GX;;;WD)  
    ```  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")  
  
## <a name="40-data-quality-services"></a><a name="DQS"></a>4.0 Data Quality Services  
  
### <a name="41-dqs-not-supported-in-a-cluster"></a>4.1 DQS no se admite en un clúster  
**Problema:** DQS no se admite en una instalación de clúster de SQL Server. Si va a instalar una instancia de clúster de SQL Server, no debe activar las casillas **Data Quality Services** y **Data Quality Client** en la página **Selección de características** . Si estas casillas están activadas durante la instalación de la instancia del clúster (y completa la instalación de Data Quality Server ejecutando el archivo DQSInstaller.exe), DQS se instalará en este nodo, pero no estará disponible en otros nodos adicionales cuando se agreguen más nodos al clúster, por lo que no funcionará en nodos adicionales.  
  
**Solución alternativa:** instale la Actualización acumulativa 1 de SQL Server 2012 para resolver este problema. Para obtener instrucciones, vea [https://support.microsoft.com/kb/2674817](https://support.microsoft.com/kb/2674817).  
  
### <a name="42-to-reinstall-data-quality-server-delete-the-dqs-objects-after-uninstalling-data-quality-server"></a>4.2 Para volver a instalar Data Quality Server, elimine los objetos de DQS después de desinstalar Data Quality Server  
**Problema:** si desinstala Data Quality Server, los objetos de DQS (bases de datos de DQS, inicios de sesión de DQS y un procedimiento almacenado de DQS) no se eliminan de la instancia de SQL Server.  
  
**Solución alternativa:** para volver a instalar Data Quality Server en el mismo equipo y en la misma instancia de SQL Server, debe eliminar manualmente los objetos de DQS de la instancia de SQL Server. Además, debe eliminar también del equipo los archivos de las bases de datos de DQS (DQS_MAIN, DQS_PROJECTS y DQS_STAGING_DATA) de la carpeta C:\Archivos de programa\Microsoft SQL Server\MSSQL11.<Instancia_SQL_Server>\MSSQL\DATA antes de volver a instalar Data Quality Server. De lo contrario, se producirá un error en la instalación de Data Quality Server. Mueva los archivos de base de datos en lugar de eliminarlos si desea conservar los datos, por ejemplo las bases de conocimiento o los proyectos de calidad de datos. Para obtener más información acerca de cómo quitar los objetos de DQS una vez completado el proceso de desinstalación, vea [Quitar objetos de Data Quality Server](./install/remove-data-quality-server-objects.md).  
  
### <a name="43-indication-of-a-terminated-knowledge-discovery-or-interactive-cleansing-activity-is-delayed"></a>4.3 Indicación de que una actividad de limpieza interactiva o de detección de conocimiento terminada se ha retrasado  
**Problema:** si un administrador termina una actividad en la pantalla Supervisión de actividad, un usuario interactivo que ejecute la detección del conocimiento, la administración del dominio o la actividad de borrado interactivo no recibirá ninguna indicación de que su actividad ha terminado hasta que realice la siguiente operación.  
  
**Solución alternativa:** None  
  
### <a name="44-a-cancel-operation-discards-work-from-multiple-activities"></a>4.4 Una operación de cancelación descarta el trabajo de varias actividades  
**Problema:** si hace clic en **Cancelar** para una actividad de administración de dominio o de detección de conocimiento en ejecución y se han completado otras actividades anteriormente sin realizar una operación de publicación mientras la actividad está ejecutándose, se descartará el trabajo de todas las actividades realizadas desde la última publicación y no solo la actual.  
  
**Solución alternativa:** para evitar esto, publique el trabajo que necesite conservar en la base de conocimiento antes de comenzar una nueva actividad.  
  
### <a name="45-controls-do-not-scale-properly-on-large-font-sizes"></a>4.5 Los controles no se adaptan correctamente en los tamaños de fuente grandes  
**Problema:** si cambia el tamaño del texto a "Más grande: 150%" (en Windows Server 2008 o Windows 7), o bien cambia el valor de Configuración de ppp personalizada a 200% (en Windows 7), los botones **Cancelar** y **Crear** de la página **Nueva base de conocimiento** no están accesibles.  
  
**Solución alternativa:** para resolver el problema, establezca la fuente en un tamaño menor.  
  
### <a name="46-screen-resolution-of-800x600-is-not-supported"></a>4.6 No se admite la resolución de pantalla de 800x600  
**Problema:** la aplicación Data Quality Client no se muestra correctamente si la resolución de pantalla se establece en 800x600.  
  
**Solución alternativa:** para resolver este problema, configure la resolución de pantalla en un valor más alto.  
  
### <a name="47-map-bigint-column-in-the-source-data-to-a-decimal-domain-to-prevent-data-loss"></a>4.7 Asignar la columna bigint en los datos de origen a un dominio decimal para evitar la pérdida de datos  
**Problema:** si una columna de los datos de origen es del tipo de datos **bigint**, debe asignarla a un dominio del tipo de datos **decimal** en vez de al tipo de datos **integer** en DQS. Esto se debe a que el tipo de datos **decimal** representa un intervalo de datos mayor que el tipo de datos **int** y, por tanto, puede contener valores de mayor tamaño.  
  
### <a name="48-nvarcharmax-and-varcharmax-data-types-are-not-supported-in-the-dqs-cleansing-component-in-integration-services"></a>4.8 Los tipos de datos NVARCHAR(MAX) y VARCHAR(MAX) no se admiten en el componente de limpieza DQS en Integration Services  
**Problema:** las columnas de datos de los tipos de datos **nvarchar(max)** y **varchar(max)** no se admiten en el componente de limpieza DQS en Integration Services. Como tal, estas columnas de datos no están disponibles para asignarse en la pestaña Asignación del Editor de transformación de limpieza DQS y, por lo tanto, no se pueden limpiar.  
  
**Solución alternativa:** antes de procesar estas columnas de datos con el componente de limpieza DQS, debe convertirlos al tipo de datos **DT_STR** o **DT_WSTR** usando la transformación Conversión de datos.  
  
### <a name="49-the-item-to-run-dqsinstallerexe-on-the-start-menu-is-overwritten-on-new-sql-server-instance-installation"></a>4.9 El elemento para ejecutar el archivo DQSInstaller.exe en el menú Inicio se sobrescribe en una instalación de una nueva instancia de SQL Server.  
**Problema:** si elige instalar Data Quality Services en una instancia de SQL Server, se crea un elemento en el menú **Inicio**, debajo del grupo de programas **Data Quality Services** denominado Instalador de **Data Quality Server** tras completar la instalación de SQL Server. Sin embargo, si instala varias instancias de SQL Server en el mismo equipo, todavía hay un solo elemento **Instalador de Data Quality Server** en el menú **Inicio** . Al hacer clic en este elemento, se ejecuta el archivo DQSInstaller.exe en la instancia de SQL Server instalada más reciente.  
  
### <a name="410-activity-monitoring-displays-incorrect-status-for-failed-integration-services-cleansing-activities"></a>4.10 La supervisión de actividad muestra un estado incorrecto para las actividades de limpieza de Integration Services con error  
La pantalla de supervisión de actividad muestra **Correcto** incluso para las actividades de limpieza de Integration Services en la columna **Estado actual** .  
  
### <a name="411-schema-name-is-not-displayed-as-part-of-tableview-name"></a>4.11 El nombre del esquema no se muestra como parte del nombre de la tabla o vista  
Al seleccionar un origen de datos de SQL Server en cualquiera de las actividades de DQS durante la fase de asignación en Data Quality Client, la lista de tablas y vistas se muestra sin el nombre de esquema. Por lo tanto, si hay varias tablas o vistas con el mismo nombre pero un esquema diferente, pueden distinguirse únicamente mirando en la vista previa de datos o seleccionándolas y después mirando en los campos disponibles para la asignación.  
  
### <a name="412-issue-with-cleansing-output-and-export-if-data-source-is-mapped-to-a-composite-domain-containing-a-child-domain-of-date-type"></a>4.12 Problema con el resultado de la limpieza y exportación si el origen de datos se asigna a un dominio compuesto que contiene un dominio secundario de tipo de fecha  
En un proyecto de calidad de datos de limpieza, si ha asignado un campo en los datos de origen con un dominio compuesto que tiene un dominio secundario de un tipo de datos de fecha, el resultado del dominio secundario en el resultado de la limpieza tiene un formato de fecha incorrecto y la exportación en la operación de base de datos da error.  
  
### <a name="413-error-when-mapping-to-an-excel-sheet-that-contains-a--semicolon-in-its-name"></a>4.13 Error al asignar una hoja de Excel que contiene un punto y coma (;) en su nombre  
**Problema:** en la página **Asignar** de cualquier actividad de DQS en Data Quality Client, si asigna la hoja de Excel de origen que contiene un punto y coma (;) en su nombre, se muestra un mensaje de excepción no controlada al hacer clic en **Siguiente** en la página **Asignar**.  
  
**Solución alternativa:** quite el punto y coma (;) del nombre de la hoja en el archivo de Excel que contenga los datos de origen que se van a asignar y vuelva a intentarlo.  
  
### <a name="414-issue-with-date-or-datetime-values-in-unmapped-source-fields-in-excel-during-cleansing-and-matching"></a>4.14 Problema con los valores Date o DateTime en los campos de origen sin asignar en Excel durante la limpieza y coincidencia  
**Problema:** si los datos de origen son de Excel y no ha asignado los campos de origen que contienen valores de un tipo de datos **Date** o **DateTime**, ocurre lo siguiente durante las actividades de limpieza y coincidencia:  
  
-   Los valores **Date** sin asignar se muestran y se exportan en el formato aaaa-mm-dd.  
  
-   El valor de tiempo se pierde para los valores **DateTime** sin asignar y se muestran y exportan en el formato aaaa-mm-dd.  
  
**Solución alternativa:** puede ver los valores de campos sin asignar en el panel inferior derecho de la página **Administrar y ver resultados** de la actividad de limpieza y en la página **Coincidencia** en la actividad de coincidencia.  
  
### <a name="415-cannot-import-domain-values-from-an-excel-file-xls-containing-more-than-255-columns-of-data"></a>4.15 No se pueden importar valores de dominio de un archivo de Excel (.xls) que contiene más de 255 columnas de datos  
**Problema:** si importa valores en un dominio desde un archivo de Excel 97-2003 (.xls) que contenga más de 255 columnas de datos, aparece un mensaje de excepción y se produce un error en la importación.  
  
**Solución alternativa:** para corregir este problema, puede elegir alguna de las opciones siguientes:  
  
-   Guarde el archivo .xls como archivo .xlsx y después importe los valores del archivo .xlsx en un dominio.  
  
-   Quite los datos de todas las columnas situadas más allá de la columna 255 en el archivo .xls, guarde el archivo e importe los valores del archivo .xls en un dominio.  
  
### <a name="416-activity-monitoring-feature-is-unavailable-for-roles-other-than-dqs_administrator"></a>4.16 La característica Supervisión de actividades no está disponible para otros roles distintos de dqs_administrator  
La característica Supervisión de actividades solo está disponible para los usuarios que tienen el rol dqs_administrator. Si su cuenta de usuario tiene el rol dqs_kb_editor o dqs_kb_operator, la característica Supervisión de actividades no estará disponible en la aplicación Data Quality Client.  
  
### <a name="417-error-on-opening-a-knowledge-base-in-the-recent-knowledge-base-list-for-domain-management"></a>4.17 Error al abrir una base de conocimiento en la lista de bases de conocimiento recientes para la administración de dominios  
Problema: puede recibir el siguiente error si abre una base de conocimiento en la lista **Base de conocimiento** reciente para la actividad Supervisión de dominios en la pantalla principal de Data Quality Client:  
  
`"A configuration with name 'RecentList:KB:<domain>\<username>' already exists in the database."`  
  
Esto ocurre porque DQS compara de distinto modo las cadenas en la base de datos de SQL Server y en C#. La comparación de cadenas en la base de datos de SQL Server no distingue mayúsculas de minúsculas, mientras que en C# sí se distingue entre mayúsculas y minúsculas.  
  
Vamos a ilustrarlo con un ejemplo. Imagine un usuario, Dominio\usuario1. El usuario inicia sesión en el equipo con Data Quality Client mediante la cuenta "usuario1" y trabaja en una base de conocimiento. DQS almacena la base de conocimiento reciente para cada usuario como un registro en la tabla A_CONFIGURATION de la base de datos DQS_MAIN. En este caso, el registro se almacenará con el nombre siguiente: RecentList:KB:Domain\user1. Más tarde, el usuario inicia sesión en el equipo con Data Quality Client como "Usuario1" (observe que la U está en mayúsculas) e intenta abrir la base de conocimiento en la lista **Base de conocimiento reciente** para la actividad de administración de dominios. El código subyacente de DQS comparará las dos cadenas, RecentList:KB:DOMINIO\usuario1 y DOMINIO\Usuario1, y teniendo en cuenta la comparación de cadenas con distinción de mayúsculas y minúsculas de C#, las cadenas no coincidirán y por tanto DQS intentará insertar un registro nuevo para el usuario (Usuario1) en la tabla A_CONFIGURATION de la base de datos DQS_MAIN. Sin embargo, debido a la comparación de cadenas sin distinción entre mayúsculas y minúsculas de la base de datos de SQL, la cadena ya existe en la tabla A_CONFIGURATION de la base de datos DQS_MAIN y se producirá un error en la operación de inserción.  
  
**Solución alternativa:** para corregir este problema, puede elegir alguna de las opciones siguientes:  
  
-   Compruebe si existen entradas duplicadas ejecutando la instrucción siguiente:  
  
    ```  
    SELECT * FROM DQS_MAIN.dbo.A_CONFIGURATION WHERE NAME like 'RecentList%'  
    ```  
  
    Después, puede ejecutar la instrucción siguiente para eliminar el registro solo para el usuario afectado cambiando el valor de la cláusula WHERE para que coincida con el dominio y el nombre de usuario afectados.  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%<domain>\<username>'  
    ```  
  
    Como alternativa, puede quitar todos los elementos recientes para todos los usuarios de DQS:  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%'  
    ```  
  
-   Use el mismo modelo de mayúsculas y minúsculas que la última vez para especificar su cuenta de usuario mientras inicia sesión en el equipo con Data Quality Client.  
  
> [!NOTE]  
> Para evitar este problema, use reglas coherentes de mayúsculas y minúsculas para especificar su cuenta de usuario mientras inicia sesión en el equipo con Data Quality Client.  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")  
  
## <a name="50-database-engine"></a><a name="DE"></a>5.0 Motor de base de datos  
  
### <a name="51-use-of-distributed-replay-controller-and-distributed-replay-client-features"></a>5.1 Uso de las características Distributed Replay Controller y Distributed Replay Client  
**Problema:** las características Distributed Replay Controller y Distributed Replay Client están disponibles en la SKU Server Core de Windows Server 2008, Windows Server 2008 R2 y Windows Server 7, aunque no se admiten en la SKU Server Core.  
  
**Solución alternativa:** no instale ni use estas dos características en la SKU Server Core de Windows Server 2008, Windows Server 2008 R2 y Windows Server 7.  
  
### <a name="52-sql-server-management-studio-depends-on-visual-studio-2010-sp1"></a>5.2 SQL Server Management Studio depende de Visual Studio 2010 SP1  
**Problema:**  SQL Server 2012 Management Studio depende de Visual Studio 2010 SP1 para funcionar correctamente. La desinstalación de Visual Studio 2010 SP1 puede producir pérdida de funcionalidad en SQL Server Management Studio y dejará Management Studio en un estado no admitido. En este caso se pueden producir los problemas siguientes:  
  
-   Los parámetros de línea de comandos de ssms.exe no funcionarán correctamente.  
  
-   La información de Ayuda mostrada al intentar ejecutar ssms.exe con el modificador /? será incorrecta.  
  
-   Por cada archivo que se abra haciendo doble clic en él en el Explorador de Windows, se iniciará una nueva instancia de SSMS para abrir el archivo.  
  
-   No se pueden depurar consultas en el modo de usuario normal.  
  
**Solución alternativa**: instale de nuevo Visual Studio 2010 SP1 y reinicie Management Studio.  
  
### <a name="53-x64-operating-systems-require-64-bit-powershell-20"></a>5.3 Los sistemas operativos x64 requieren PowerShell 2.0 de 64 bits  
**Problema:** las instalaciones de 32 bits de Extensiones de PowerShell para SQL Server de Windows no se admiten para instancias de SQL Server 2012 en sistemas operativos de 64 bits.  
  
**Soluciones alternativas:**  
  
-   Instale SQL Server 2012 de 64 bits con las herramientas de administración de 64 bits y Extensiones de Windows PowerShell para SQL Server de 64 bits.  
  
-   O bien, importe el módulo SQLPS desde un símbolo del sistema de Windows PowerShell 2.0 de 32 bits.  
  
### <a name="54-an-error-might-occur-when-navigating-in-the-generate-script-wizard"></a>5.4 Se podría producir un error al navegar por el Asistente para generar scripts  
**Problema:** después de generar un script en el Asistente para generar scripts al hacer clic en **Guardar o publicar scripts**, navegar haciendo clic en **Elegir opciones** o **Establecer opciones de scripting** y haciendo clic en **Guardar o publicar scripts** de nuevo se podría producir el siguiente error:  
  
```
An exception occurred while executing a Transact-SQL statement or batch. (Microsoft.SqlServer.ConnectionInfo)  
------------------------------  
ADDITIONAL INFORMATION:  
Invalid object name 'sys.federations'. (Microsoft SQL Server, Error: 208)
```
  
**Solución alternativa:** cierre y vuelva a abrir el Asistente para generar scripts.  
  
### <a name="55-new-maintenance-plan-layout-not-compatible-with-earlier-sql-server-tools"></a>5.5 Nuevo diseño para planes de mantenimiento no compatible con herramientas anteriores de SQL Server  
**Problema:** cuando se usan herramientas de administración de SQL Server 2012 para modificar un plan de mantenimiento existente creado en una versión anterior de las herramientas de administración de SQL Server (SQL Server 2008 R2, SQL Server 2008 o SQL Server 2005), el plan de mantenimiento se guarda en un formato nuevo. Las versiones anteriores de las herramientas de administración de SQL Server no admiten este nuevo formato.  
  
**Solución alternativa**: None  
  
### <a name="56-intellisense-has-limitations-when-logged-in-to-a-contained-database"></a>5.6 IntelliSense tiene limitaciones cuando se inicia sesión en una base de datos independiente  
Problema: Intellisense en SQL Server Management Studio (SSMS) y SQL Server Data Tools (SSDT) no funciona de la manera esperada cuando usuarios contenidos inician sesión en bases de datos independientes. En esos casos se produce el comportamiento siguiente:  
  
1.  El subrayado de los objetos no válidos no aparece.  
  
2.  La lista Autocompletar no aparece.  
  
3.  La Ayuda de información sobre herramientas para las funciones integradas no funciona.  
  
**Solución alternativa**:  None  
  
### <a name="57-alwayson-availability-groups"></a>5.7 Grupos de disponibilidad AlwaysOn  
Antes de intentar crear un grupo de disponibilidad, vea [Requisitos previos, restricciones y recomendaciones para los grupos de disponibilidad AlwaysOn (SQL Server)](https://go.microsoft.com/?linkid=9753168) en los Libros en pantalla. Para obtener una introducción a los grupos de disponibilidad AlwaysOn, vea [Grupos de disponibilidad AlwaysOn (SQL Server)](https://go.microsoft.com/?linkid=9753166)en los Libros en pantalla.  
  
#### <a name="571-client-connectivity-for-alwayson-availability-groups"></a>5.7.1 Conectividad de cliente para Grupos de disponibilidad AlwaysOn  
**Fecha de actualización:** 13 de agosto de 2012  
  
En esta sección se describe la compatibilidad con controladores de los grupos de disponibilidad AlwaysOn y las soluciones alternativas para los controladores no admitidos.  
  
**Compatibilidad de controlador**  
  
En la tabla siguiente se resume la compatibilidad de controlador para Grupos de disponibilidad AlwaysOn:  
  
|Controlador|Conmutación por error de múltiples subredes|Intención de aplicaciones|Enrutamiento de solo lectura|Conmutación por error de varias subredes: Conmutación por error más rápida del punto de conexión de una sola subred|Conmutación por error de varias subredes: Resolución de instancias con nombre para las instancias en clúster SQL|  
|----------|--------------------------|----------------------|----------------------|------------------------------------------------------------------|---------------------------------------------------------------------------------|  
|SQL Native Client 11.0 ODBC|Sí|Sí|Sí|Sí|Sí|  
|SQL Native Client 11.0 OLEDB|No|Sí|Sí|No|No|  
|ADO.NET con .NET Framework 4.0 con revisión de conectividad **\&#42;**|Sí|Sí|Sí|Sí|Sí|  
|ADO.NET con .NET Framework 3.5 SP1 con revisión de conectividad **\&#42;\&#42;**|Sí|Sí|Sí|Sí|Sí|  
|Controlador JDBC 4.0 de Microsoft para SQL Server|Sí|Sí|Sí|Sí|Sí|  
  
**\&#42;** Descargar la revisión de conectividad para ADO .NET con .NET Framework 4.0: [https://support.microsoft.com/kb/2600211](https://support.microsoft.com/kb/2600211).  
  
**\&#42;\&#42;** Descargar la revisión de conectividad para ADO.NET con .NET Framework 3.5 SP1: [https://support.microsoft.com/kb/2654347](https://support.microsoft.com/kb/2654347).  
  
**Palabra clave MultiSubnetFailover y características asociadas**  
  
MultiSubnetFailover es una nueva palabra clave de la cadena de conexión que se usa para habilitar la conmutación por error más rápida con los grupos de disponibilidad AlwaysOn y las instancias de clúster de conmutación por error AlwaysOn en SQL Server 2012. Las tres características siguientes se habilitan cuando se establece MultiSubnetFailover=True en la cadena de conexión:  
  
-   Conmutación por error de varias subredes más rápida a un agente de escucha de varias subredes para instancias del clúster de conmutación por error o un grupo de disponibilidad AlwaysOn.  
  
    -   Resolución de una instancia con nombre en una instancia de clúster por conmutación por error AlwaysOn de varias subredes.  
  
-   Conmutación por error de una sola subred más rápida a un agente de escucha de una sola subred para instancias del clúster de conmutación por error o un grupo de disponibilidad AlwaysOn.  
  
    -   Esta característica se usa al conectarse a un agente de escucha que solo tiene una dirección IP en una sola subred. Así, se realizan más reintentos de conexión TCP para acelerar las conmutaciones por error en una sola subred.  
  
-   Resolución de una instancia con nombre en una instancia de clúster por conmutación por error AlwaysOn de varias subredes.  
  
    -   Esto sirve para agregar compatibilidad con la resolución de una instancia con nombre para instancias del clúster de conmutación por error AlwaysOn con varios extremos de subred.  
  
**MultiSubnetFailover=True no se admite en .NET Framework 3.5 u OLEDB**  
  
**Problema:** si la instancia del clúster de conmutación por error o el grupo de disponibilidad tiene un nombre de escucha (denominado nombre de red o punto de acceso cliente en el Administrador de clústeres de WSFC) que depende de varias direcciones IP de subredes diferentes y usa ADO.NET con .NET Framework 3.5 SP1 o SQL Native Client 11.0 OLEDB, puede que se agote el tiempo de espera de conexión en el 50 % de las solicitudes de conexión de cliente al agente de escucha del grupo de disponibilidad.  
  
**Soluciones alternativas:** le recomendamos que realice una de las tareas siguientes.  
  
-   Si no tiene permiso para manipular recursos de clúster, cambie el tiempo de espera de la conexión a 30 segundos (este valor produce un tiempo de espera de TCP de 20 segundos además de un búfer de 10 segundos).  
  
    **Ventajas**: Si se produce una conmutación por error entre subredes, el tiempo de recuperación del cliente es breve.  
  
    **Inconvenientes**: La mitad de las conexiones cliente tardarán más de 20 segundos  
  
-   Si tiene el permiso para manipular recursos de clúster, el enfoque más recomendado consiste en establecer el nombre de red del agente de escucha del grupo de disponibilidad en **RegisterAllProvidersIP**=0. Para obtener más información, vea "Script de PowerShell de ejemplo para deshabilitar RegisterAllProvidersIP y reducir TTL", más adelante en esta sección.  
  
    **Ventajas:** No es necesario aumentar el valor del tiempo de espera de la conexión de cliente.  
  
    **Inconvenientes:** Si se produce una conmutación por error entre subredes, el tiempo de recuperación del cliente podría ser de 15 minutos o más, según la configuración de HostRecordTTL y la configuración de la programación de replicación DNS/AD entre sitios.  
  
**Script de PowerShell de ejemplo para deshabilitar RegisterAllProvidersIP y reducir TTL**  
  
En el siguiente script de PowerShell de ejemplo se muestra cómo deshabilitar `RegisterAllProvidersIP` y reducir TTL. Reemplace `yourListenerName` por el nombre del agente de escucha que va a cambiar.  
  
```  
Import-Module FailoverClusters  
Get-ClusterResource yourListenerName|Set-ClusterParameter RegisterAllProvidersIP 0  
Get-ClusterResource yourListenerName|Set-ClusterParameter HostRecordTTL 300  
```  
  
#### <a name="572-upgrading-from-ctp3-with-availability-group-configured-is-not-supported"></a>5.7.2 No se permite actualizar desde CTP3 con el grupo de disponibilidad configurado  
Quite el grupo de disponibilidad y vuelva crearlo antes de actualizar. Esto se debe a una limitación de la compilación CTP3. Las compilaciones futuras no tendrán esta restricción.  
  
#### <a name="573-side-by-side-installation-of-ctp3-with-later-versions-is-not-supported-if-you-have-an-availability-group-configured-in-your-instance"></a>5.7.3 La instalación en paralelo de CTP3 con versiones posteriores no se admite si tiene un grupo de disponibilidad configurado en su instancia  
Esto se debe a una limitación de la compilación CTP3. Las compilaciones futuras no tendrán esta restricción.  
  
#### <a name="574-side-by-side-installation-of-ctp3-with-later-versions-of-failover-cluster-instances-is-not-supported"></a>5.7.4 No se admite la instalación en paralelo de CTP3 con versiones posteriores de las instancias de clúster de conmutación por error.  
Esto se debe a una limitación de la compilación CTP3. Las compilaciones futuras no tendrán esta restricción. Para actualizar las instancias del clúster de conmutación por error desde CTP3, asegúrese de actualizar todas las instancias de un nodo al mismo tiempo.  
  
#### <a name="575--timeouts-may-occur-when-using-multi-ips-in-the-same-subnet-with-alwayson"></a>5.7.5 Se puede agotar el tiempo de espera cuando se usan varias IP en la misma subred con AlwaysOn  
**Problema:** cuando se usan varias IP en la misma subred con Always On, se puede agotar el tiempo de espera. Esto ocurre si la dirección IP que aparece en primer lugar en la lista es errónea.  
  
**Solución alternativa:** use "multisubnetfailover = true" en la cadena de conexión.  
  
#### <a name="576-failure-to-create-new-availability-group-listeners-because-of-active-directory-quotas"></a>5.7.6 Error al crear nuevos agentes de escucha del grupo de disponibilidad debido a las cuotas de Active Directory  
**Problema:** La creación de un nuevo agente de escucha del grupo de disponibilidad puede producir un error en la creación porque se ha alcanzado una cuota de Active Directory para la cuenta de equipo del nodo de clúster que participa. Para obtener más información, vea [Cómo solucionar problemas de la cuenta del Servicio de Clúster Server cuando modifica objetos de equipo](https://support.microsoft.com/kb/307532) y [Cuotas de Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc904295(v=ws.10)).  
  
#### <a name="577-netbios-conflicts-because-availability-group-listener-names-use-an-identical-15-character-prefix"></a>5.7.7 NetBIOS está en conflicto porque los nombres de los agentes de escucha del grupo de disponibilidad usan un prefijo idéntico de 15 caracteres  
Si tiene dos clústeres de WSFC controlados por la misma instancia de Active Directory e intenta crear agentes de escucha del grupo de disponibilidad en los dos clústeres utilizando nombres con más de 15 caracteres y un prefijo idéntico de 15 caracteres, obtendrá un error notificando que el recurso de nombre de red virtual no se pudo poner en línea. Para obtener información acerca de las reglas de nomenclatura de prefijos para los nombres DNS, vea [Asignación de nombres de dominio](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731265(v=ws.10)).  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")  
  
## <a name="60-integration-services"></a><a name="IS"></a>6.0 Integration Services  
  
### <a name="61-the-change-data-capture-service-for-oracle-and-the-change-data-capture-designer-console-for-oracle"></a>6.1 Servicio de captura de datos modificados para Oracle y consola del diseñador de captura de datos modificados para Oracle  
El servicio CDC para Oracle es un servicio de Windows que examina los registros de transacciones de Oracle y captura los cambios a las tablas de Oracle deseadas en tablas de cambios de SQL Server. La consola del diseñador CDC se emplea para desarrollar y mantener instancias CDC de Oracle. La consola del diseñador CDC es un complemento de Microsoft Management Console (MMC).  
  
#### <a name="611-install-the-cdc-service-for-oracle-and-the-cdc-designer-for-oracle"></a>6.1.1 Instalar el servicio CDC para Oracle y la consola del diseñador CDC para Oracle  
**Problema:** el programa de instalación de SQL Server no instala el servicio CDC y el diseñador CDC. Debe instalar manualmente el servicio CDC o el diseñador CDC en un equipo que cumpla los requisitos y los requisitos previos que se describen en los archivos de Ayuda actualizados.  
  
**Solución alternativa:** para instalar el servicio CDC para Oracle, ejecute manualmente AttunityOracleCdcService.msi desde el disco de instalación de SQL Server. Para instalar la Consola del diseñador CDC, ejecute manualmente AttunityOracleCdcDesigner.msi desde el disco de instalación de SQL Server.  Los paquetes de instalación para las versiones x86 y x64 se encuentran en .\Tools\AttunityCDCOracle\ en el disco de instalación de SQL Server.  
  
#### <a name="612-f1-help-functionality-points-to-incorrect-documentation-files"></a>6.1.2 La funcionalidad de Ayuda de F1 apunta a archivos de documentación incorrectos  
**Problema:** no puede acceder a la documentación de Ayuda correcta mediante la lista desplegable Ayuda de F1 ni haciendo clic en "?" en la consola de Attunity. Estos métodos apuntan a archivos chm incorrectos.  
  
**Solución alternativa:** los archivos chm correctos se instalan cuando se instalan el servicio CDC para Oracle y el diseñador CDC para Oracle. Para ver el contenido de Ayuda correcto, inicie los archivos chm directamente desde esta ubicación: `%Program Files%\Change Data Capture for Oracle by Attunity\*.chm`.  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")  
  
## <a name="70-master-data-services"></a><a name="MDS"></a>7.0 Master Data Services  
  
### <a name="71-fixing-an-mds-installation-in-a-cluster"></a>7.1 Corrección de una instalación de MDS en un clúster  
**Problema:** si instala una instancia en clúster de la versión RTM de SQL Server 2012 con la casilla **Master Data Services** activada, MDS se instalará en un solo nodo, pero no estará disponible y no funcionará en los nodos adicionales que agregue al clúster.  
  
**Solución alternativa**: para resolver este problema, debe instalar la Versión acumulativa 1 (CU1) de SQL Server 2012 y realizar los pasos siguientes:  
  
1.  Asegúrese de que no hay ninguna instalación existente de SQL/MDS.  
  
2.  Descargue SQL Server 2012 CU1 en un directorio local.  
  
3.  Instale SQL Server 2012 con la función MDS en el nodo del clúster principal y luego instale SQL Server 2012 con la característica MDS en los nodos de clúster adicionales.  
  
Para obtener más información sobre los problemas y cómo realizar los pasos anteriores, vea [https://support.microsoft.com/kb/2683467](https://support.microsoft.com/kb/2683467).  
  
### <a name="72-microsoft-silverlight-5-required"></a>7.2 Se necesita Microsoft Silverlight 5  
Para trabajar en la aplicación web Master Data Manager, Silverlight 5.0 debe estar instalado en el equipo cliente. Si no tiene la versión necesaria de Silverlight, se le pedirá que la instale cuando navegue a un área de la aplicación web que la necesite. Puede instalar Silverlight 5 desde [https://go.microsoft.com/fwlink/?LinkId=243096](https://go.microsoft.com/fwlink/?LinkId=243096).  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")  
  
## <a name="80-reporting-services"></a><a name="RS"></a>8.0 Reporting Services  
  
### <a name="81-reporting-services-connectivity-to-sql-server-pdw-requires-updated-drivers"></a>8.1 La conectividad de Reporting Services con PDW de SQL Server necesita controladores actualizados  
La conectividad desde SQL Server 2012 Reporting Services con Microsoft SQL Server PDW Appliance Update 2 y posterior necesita una actualización de los controladores de conectividad de PDW. Para obtener más información, los clientes de PDW de SQL Server deben ponerse en contacto con el soporte técnico de Microsoft.  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")  
  
## <a name="90-streaminsight"></a><a name="SI"></a>9.0 StreamInsight  
SQL Server 2012 incluye StreamInsight 2.1. StreamInsight 2.1 necesita una licencia de Microsoft SQL Server 2012 y .NET Framework 4.0. Incluye una serie de mejoras de rendimiento y algunas correcciones de errores. Para más información, consulte las [Notas de la versión de Microsoft StreamInsight 2.1](https://social.technet.microsoft.com/wiki/contents/articles/6539.aspx). Para descargar StreamInsight 2.1 por separado, visite la [página de descarga de Microsoft StreamInsight 2.1](https://www.microsoft.com/download/details.aspx?id=30149) en el Centro de descarga de Microsoft.  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")  
  
## <a name="100-upgrade-advisor"></a><a name="UA"></a>10.0 Asesor de actualizaciones  
  
### <a name="101-link-to-install-upgrade-advisor-is-not-enabled-on-chinese-hk-operating-systems"></a>10.1 El vínculo para instalar el Asesor de actualizaciones no está habilitado en los sistemas operativos en chino (HK)  
Problema: al intentar instalar el Asesor de actualizaciones en cualquier versión admitida de Windows en sistemas operativos en chino (RAE de Hong Kong), es posible que descubra que el vínculo para instalarlo no está habilitado.  
  
**Solución alternativa**: busque los archivos **SQLUA.msi** en el disco de SQL Server 2012 en `\1028_CHT_LP\x64\redist\Upgrade Advisor` o en `\1028_CHT_LP\x86\redist\Upgrade Advisor`, en función de la arquitectura del sistema operativo.  
  
![barra_horizontal](media/horizontal-bar.png "barra_horizontal")
