---
title: Instalar SQL Server con SysPrep | Microsoft Docs
description: En este artículo se describe cómo preparar y completar imágenes mediante SysPrep en la instalación de SQL Server.
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 11f4ed8a-aaa9-417b-bdd5-204f551c6bb6
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8bbb1d668efb9a8531d6a93b084a7315e99072ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348314"
---
# <a name="install-sql-server-with-sysprep"></a>Instalar SQL Server con SysPrep

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] relacionadas con SysPrep. La página **Avanzadas** del **Centro de instalación** tiene dos opciones: **Preparar imagen de una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** y **Completar imagen de una instancia independiente preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Las secciones [Preparar](#prepare) y [Completar](#complete) describen el proceso de instalación en detalle. Para obtener más información, vea [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md). 
  
También puede preparar y completar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el símbolo del sistema o un archivo de configuración. Para más información, consulte:  
  
- [Instalar SQL Server desde el símbolo del sistema](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  
  
- [Instalar SQL Server mediante un archivo de configuración](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)  
  
## <a name="prerequisites"></a>Requisitos previos  
Antes de instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], revise los artículos de [Planear una instalación de SQL Server](../../sql-server/install/planning-a-sql-server-installation.md). 
  
Para más información sobre las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los requisitos de hardware y software, vea [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). 
    
##  <a name="ssnoversion-sysprep-cluster-support"></a><a name="sysprep"></a> Compatibilidad con clústeres de SysPrep de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], SysPrep admite instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en clúster en instalaciones desde la línea de comandos. Para obtener más información, vea [¿Qué es Sysprep?](https://msdn.microsoft.com/library/cc721940\(v=WS.10\).aspx). 
  
### <a name="to-prepare-a-ssnoversion-failover-cluster-unattended"></a>Para preparar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desatendido)  
  
1. Prepare la imagen (como se explica en [Consideraciones acerca de la instalación de SQL Server con SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)) y capture la imagen de Windows con SysPrep Generalization. En el siguiente ejemplo se prepara la imagen:  
  
    ```  
    Setup.exe /q /ACTION=PrepareImage l /FEATURES=SQLEngine /InstanceID =<MYINST> /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     A continuación, ejecute Windows SysPrep Generalization. 
  
2. Implemente la imagen mediante Windows SysPrep Specialize. 
  
3. Cree el clúster de conmutación por error de Windows. 
  
4. Ejecute setup.exe con **/ACTION=PrepareFailoverCluster** en todos los nodos. Por ejemplo:  
  
    ```  
    setup.exe /q /ACTION=PrepareFailoverCluster /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
### <a name="complete-a-ssnoversion-failover-cluster-unattended"></a>Completar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desatendido)  
  
1. Ejecute setup.exe con **/ACTION=CompleteFailoverCluster** en el nodo propietario del grupo de almacenamiento disponible:  
  
    ```  
    setup.exe /q /ACTION=CompleteFailoverCluster /InstanceName=<InstanceName>  /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'>:" /FAILOVERCLUSTERNETWORKNAME="<Insert FOI Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /INSTALLSQLDATADIR="<Drive>:\<Path>\MSSQLSERVER" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSYSADMINACCOUNTS="<DomainName\UserName>"  
    ```  
  
### <a name="adding-a-node-to-an-existing-ssnoversion-failover-cluster-unattended"></a>Agregar un nodo a un clúster de conmutación por error existente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desatendido)  
  
1. Implemente la imagen mediante Windows SysPrep Specialize. 
  
2. Únase al clúster de conmutación por error de Windows. 
  
3. Ejecute setup.exe con **/ACTION=AddNode** en todos los nodos:  
  
    ```  
    setup.exe /q /ACTION=AddNode /InstanceName=<InstanceName> /Features=SQLEngine  /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx"  /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
##  <a name="prepare-image"></a><a name="prepare"></a> Preparar imagen  
  
### <a name="prepare-a-stand-alone-instance-of-ssnoversion"></a>Preparar una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
1. Inserte el medio de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Desde la carpeta raíz, haga doble clic en Setup.exe. Para realizar la instalación desde un recurso compartido de red, localice la carpeta raíz de dicho recurso y, a continuación, haga doble clic en Setup.exe. 
  
2. El Asistente para instalación ejecuta el Centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para preparar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], haga clic en **Preparar imagen de una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** en la página **Avanzadas**. 
  
3. El Comprobador de configuración del sistema ejecuta una operación de detección en el equipo. Para continuar, haga clic en **Aceptar**. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
4. En la página Actualizaciones del producto se muestran las actualizaciones del producto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] más recientes disponibles. Si no quiere incluir las actualizaciones, desactive la casilla **Incluir actualizaciones de productos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Si no se detectan actualizaciones de producto, el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no muestra esta página y pasa automáticamente a la página **Instalar archivos de instalación** . 
  
5. En la página Instalar archivos de instalación, el programa de instalación proporciona el progreso de descarga, extracción e instalación de los archivos de instalación. Si se encuentra una actualización para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], y se especifica que debe incluirse, esa actualización también se instalará. 
  
6. El Comprobador de configuración del sistema comprueba el estado del sistema del equipo antes de seguir con la instalación. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
7. En la página **Tipo de imagen para preparar**, seleccione **Preparar una nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . 
  
     Solo se muestra la página **Tipo de imagen para preparar** cuando se tiene una instancia preparada sin configurar de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la máquina. Puede optar por preparar una nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o agregar las características compatibles de preparación del sistema en una instancia preparada existente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el equipo. Para obtener más información acerca de cómo agregar características a una instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Agregar características a una instancia preparada](#AddFeatures). 
  
8. En la página **Términos de licencia** , lea el contrato de licencia y active la casilla para aceptar los términos y condiciones de la licencia. Para ayudar a mejorar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], también puede habilitar la opción de uso de características y enviar informes a [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 
  
9. En la página **Selección de características** , seleccione los componentes de la instalación:  
  
    |Instalación|Componentes|  
    |-|-|  
    |[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] SysPrep|[!INCLUDE[ssDE](../../includes/ssde-md.md)]<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Replicación<br /><br /> Características de texto completo<br /><br /> Data Quality Services<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en modo nativo<br /><br /> [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]<br /><br /> Características redistribuibles<br /><br /> Características compartidas|  
  
     Al resaltar el nombre de la característica, se muestra una descripción de cada grupo de componentes en el panel derecho. Puede activar cualquier combinación de casillas. Para obtener más información, consulte [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) y [Ediciones y características admitidas de SQL Server 2019 (15.x)](../../sql-server/editions-and-components-of-sql-server-version-15.md).
  
     Los requisitos previos para las características seleccionadas se muestran en el recuadro del lado derecho. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación instalará los requisitos previos que no se hayan instalado todavía durante el paso de instalación que se describe más adelante en este procedimiento. 
  
10. En la página **Preparar reglas de imagen** , el Comprobador de configuración del sistema comprueba el estado del sistema del equipo antes de que continúe el programa de instalación. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
11. En la página Configuración de instancia, especifique el identificador de la instancia. Haga clic en **Siguiente** para continuar. 
  
     **Id. de instancia**: se usa para identificar los directorios de instalación y las claves del Registro para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Es así en las instancias predeterminadas y en las instancias con nombre. Si la instancia preparada se ha completado como una instancia predeterminada durante el paso Completar, el nombre se sobrescribe con MSSQLSERVER. El identificador de instancia es el mismo que se ha especificado. 
  
     **Directorio raíz de instancia**: de forma predeterminada, el directorio raíz de instancia es [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]. Para especificar un directorio raíz no predeterminado, use el campo proporcionado o haga clic en **Examinar** para buscar una carpeta de instalación. El directorio especificado en el paso de preparación se usará durante la configuración en el paso Completar. 
  
     Todos los Service Pack y actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se aplicarán a cada componente de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
     **Instancias instaladas**: en la cuadrícula se muestran las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que están en el equipo en el que se ejecuta el programa de instalación. 
  
12. La página **Requisitos de espacio en disco** calcula el espacio en disco necesario para las características que haya especificado. A continuación, compara el espacio necesario con el espacio en disco disponible. 
  
13. El Comprobador de configuración del sistema ejecutará las reglas de preparación de imagen para validar la configuración del equipo con las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ha especificado. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
14. La página **Listo para preparar la imagen** muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. En esta página, el programa de instalación indica si la característica de actualización de producto está habilitada o deshabilitada y la versión final de actualización. Para continuar, haga clic en **Preparar**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación instalará primero los requisitos previos necesarios para las características seleccionadas y a continuación realizará la instalación de características. 
  
15. La página **Progreso de preparación de imagen** muestra el estado para que pueda supervisar el progreso de la instalación durante la ejecución de la instalación. 
  
16. Después de la instalación, en la página **Operación completada** se proporciona un vínculo al archivo de registro de resumen de la instalación y a otras notas importantes. Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic en **Cerrar**. 
  
17. Si el programa indica que se reinicie el equipo, hágalo ahora. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información, vea [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
18. De este modo se completa el paso de preparación. Puede completar la imagen o implementar la imagen preparada tal como se describe en [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md). 
  
##  <a name="complete-image"></a><a name="complete"></a> Completar imagen  
  
### <a name="complete-a-prepared-instance-of-ssnoversion"></a>Completar una instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Si tiene una instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluida en la imagen del equipo, verá un acceso directo en el menú Inicio. También puede iniciar el Centro de instalación y hacer clic en **Completar imagen de una instancia independiente preparada** en la página **Avanzadas** . 
  
2. El Comprobador de configuración del sistema ejecuta una operación de detección en el equipo. Para continuar, haga clic en **Aceptar**. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
3. En la página **Archivos auxiliares del programa de instalación** , haga clic en **Instalar** para instalar los archivos auxiliares de la instalación. 
  
4. El Comprobador de configuración del sistema comprueba el estado del sistema del equipo antes de seguir con la instalación. Cuando se haya completado la comprobación, haga clic en **Siguiente** para continuar. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
5. En la página **Clave del producto** , seleccione un botón de opción para indicar si está instalando una edición gratuita de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o una versión de producción del producto con una clave de PID. Para más información, vea [Ediciones y características admitidas de SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md). Si está instalando una edición de evaluación, el período de prueba de 180 días comienza al completar este paso. 
  
6. En la página **Términos de licencia** , lea el contrato de licencia y active la casilla para aceptar los términos y condiciones de la licencia. Para ayudar a mejorar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], también puede habilitar la opción de uso de características y enviar informes a [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 
  
7. En la página **Seleccionar una instancia preparada** , seleccione la instancia preparada que desea completar en el cuadro desplegable. Seleccione la instancia sin configurar en la lista **Id. de instancia** . 
  
     **Instancias instaladas:** esta cuadrícula muestra todas las instancias, incluidas las instancias preparadas en este equipo. 
  
8. En la página **Revisión de características** , verá las características seleccionadas y los componentes incluidos en la instalación durante el paso de preparación. Si desea agregar a su instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] más características no incluidas en la instancia preparada, primero debe realizar este paso para completar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, a continuación, agregar las características desde **Agregar características** en el **Centro de instalación**. 
  
    > [!NOTE]  
    >  Puede agregar las características que estén disponibles para la versión de producto que está instalando. Para más información, vea [Ediciones y características admitidas de SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
9. En la página Configuración de instancia, especifique el nombre de la instancia preparada. Este es el nombre de la instancia después de haber completado la configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Haga clic en **Siguiente** para continuar. 
  
     **Id. de instancia**: se usa para identificar los directorios de instalación y las claves del Registro para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Es así en las instancias predeterminadas y en las instancias con nombre. Si la instancia preparada se ha completado como una instancia predeterminada durante el paso Completar, el nombre se sobrescribe con MSSQLSERVER. El identificador de instancia es el mismo que el especificado durante el paso Preparar. 
  
     **Directorio raíz de instancia**: se usará el directorio especificado en el paso de preparación y no se podrá modificar en este paso. 
  
     Todos los Service Pack y actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se aplicarán a cada componente de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
     **Instancias instaladas:** la cuadrícula muestra las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que están en el equipo en el que se ejecuta el programa de instalación. 
  
10. El flujo de trabajo en el resto del artículo depende de las características seleccionadas durante el paso de preparación. En función de las selecciones, es posible que no vea todas las páginas. 
  
11. En la página **Configuración del servidor** - Cuentas de servicio, especifique las cuentas de inicio de sesión para los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los servicios reales que se configuran en esta página dependen de las características que se van a instalar. 
  
     Puede asignar la misma cuenta de inicio de sesión a todos los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , o configurar cada cuenta de servicio individualmente. También puede especificar si los servicios se inician de forma automática o manual, o si están deshabilitados. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recomienda configurar las cuentas de servicio de forma individual para proporcionar a cada servicio los privilegios mínimos; así, los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] obtendrán los permisos mínimos que necesitan para completar sus tareas. Para obtener más información, vea [Configuración del servidor - Cuentas de servicio](./install-sql-server.md) y [Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md). 
  
     Para especificar la misma cuenta de inicio de sesión para todas las cuentas de servicio en esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], las credenciales se proporcionan en los campos de la parte inferior de la página. 
  
     **Nota de seguridad** [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     Cuando termine de especificar la información de inicio de sesión para los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic en **Siguiente**. 
  
12. Use la pestaña **Configurar servidor - Intercalación** para especificar intercalaciones no predeterminadas para [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Para obtener más información, vea [Configuración del servidor - Intercalación](./install-sql-server.md). 
  
13. Use la página Configuración de [!INCLUDE[ssDE](../../includes/ssde-md.md)] - Aprovisionamiento de cuentas para especificar lo siguiente:  
  
    - Modo de Seguridad: seleccione la autenticación de Windows o la autenticación de modo mixto para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si selecciona la autenticación de modo mixto, debe proporcionar una contraseña segura para la cuenta de administrador del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] integrada. 
  
         Una vez que un dispositivo establece una conexión correcta con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el mecanismo de seguridad es el mismo para la autenticación de Windows y para el modo mixto. Para obtener más información, vea [Configuración del motor de base de datos: configuración del servidor](./install-sql-server.md). 
  
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Administradores: debe especificar al menos un administrador del sistema para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para agregar la cuenta en la que se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic en **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, haga clic en **Agregar** o en **Quitar** y, a continuación, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Configuración del motor de base de datos: configuración del servidor](./install-sql-server.md). 
  
     Cuando haya terminado de modificar la lista, haga clic en **Aceptar**. Compruebe la lista de administradores en el cuadro de diálogo de configuración. Cuando la lista esté completa, haga clic en **Siguiente**. 
  
14. Use la página Configuración de [!INCLUDE[ssDE](../../includes/ssde-md.md)] - Directorios de datos para especificar los directorios de instalación no predeterminados. Para instalar en los directorios predeterminados, haga clic en **Siguiente**. 
  
    > [!IMPORTANT]  
    >  Si especifica los directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
     Para obtener más información, vea [Configuración del motor de base de datos - Directorios de datos](./install-sql-server.md). 
  
15. Use la página Configuración de [!INCLUDE[ssDE](../../includes/ssde-md.md)] - FILESTREAM para habilitar FILESTREAM para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Configuración del motor de base de datos - Secuencia de archivo](./install-sql-server.md). 
  
16. Use la página Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para especificar el tipo de instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que se creará. Para obtener más información sobre los modos de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Opciones de configuración de Reporting Services &#40;SSRS&#41;](./install-sql-server.md). 
  
17. En la página **Informes de errores** , especifique la información que desee enviar a [!INCLUDE[msCoName](../../includes/msconame-md.md)] y que ayudará a mejorar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. De forma predeterminada, se habilitan las opciones de informes de errores. 
  
18. En la página **Completar reglas de imagen** , el Comprobador de configuración del sistema ejecutará las reglas para completar imagen a fin de validar la configuración del equipo con los valores de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ha especificado. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
19. La página **Listo para completar la imagen** muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. Para continuar, haga clic en **Instalar**. 
  
20. La página **Progreso de compleción de la imagen** muestra el estado para que pueda supervisar el progreso de la instalación durante la ejecución del programa de instalación. 
  
21. Después de la instalación, en la página **Operación completada** se proporciona un vínculo al archivo de registro de resumen de la instalación y a otras notas importantes. Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic en **Cerrar**. 
  
22. Si el programa indica que se reinicie el equipo, hágalo ahora. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información, vea [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
23. Este paso completa la configuración de la instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y se finaliza la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
##  <a name="add-features-to-a-prepared-instance"></a><a name="AddFeatures"></a> Add Features to a Prepared Instance  
  
### <a name="add-features-to-a-prepared-instance-of-ssnoversion"></a>Agregar características a una instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Inserte el medio de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Desde la carpeta raíz, haga doble clic en Setup.exe. Para realizar la instalación desde un recurso compartido de red, localice la carpeta raíz de dicho recurso y, a continuación, haga doble clic en Setup.exe. 
  
2. El Asistente para instalación ejecuta el Centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para agregar características a una instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], haga clic en **Preparar imagen de una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** en la página **Avanzadas**. 
  
3. El Comprobador de configuración del sistema ejecuta una operación de detección en el equipo. Para continuar, haga clic en **Aceptar**. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
4. En la página Archivos auxiliares del programa de instalación, haga clic en **Instalar** para instalar los archivos auxiliares del programa de instalación. 
  
5. En la página **Tipo de imagen para preparar**, seleccione la opción **Agregar características a una instancia preparada existente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Seleccione la instancia preparada específica a la que desee agregar características en la lista desplegable de las instancias preparadas disponibles. 
  
6. En la página **Selección de características** , especifique las características que desee agregar a la instancia preparada especificada. 
  
     Los requisitos previos para las características seleccionadas se muestran en el recuadro del lado derecho. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación instalará los requisitos previos que no se hayan instalado todavía durante el paso de instalación que se describe más adelante en este procedimiento. 
  
7. En la página **Preparar reglas de imagen** , el Comprobador de configuración del sistema comprueba el estado del sistema del equipo antes de que continúe el programa de instalación. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
8. La página Requisitos de espacio en disco calcula el espacio en disco necesario para las características que haya especificado. A continuación, compara el espacio necesario con el espacio en disco disponible. 
  
9. En la página **Preparar reglas de imagen** , el Comprobador de configuración del sistema ejecutará las reglas de preparación de imagen para validar la configuración del equipo con las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ha especificado. Puede ver los detalles en la pantalla haciendo clic en **Mostrar detalles** o, como un informe HTML, haciendo clic en **Ver informe detallado**. 
  
10. La página **Listo para preparar la imagen** muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. Para continuar, haga clic en **Instalar**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación instalará primero los requisitos previos necesarios para las características seleccionadas y a continuación realizará la instalación de características. 
  
11. La página **Progreso de preparación de imagen** muestra el estado para que pueda supervisar el progreso de la instalación durante la ejecución de la instalación. 
  
12. Después de la instalación, en la página **Operación completada** se proporciona un vínculo al archivo de registro de resumen de la instalación y a otras notas importantes. Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic en **Cerrar**. 
  
13. Si el programa indica que se reinicie el equipo, hágalo ahora. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información, vea [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md). 
  
##  <a name="remove-features-from-a-prepare-instance"></a><a name="RemoveFeatures"></a> Quitar características de una instancia preparada  
  
### <a name="removing-features-from-a-prepared-instance-of-ssnoversion"></a>Quitar características de una instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Para comenzar el proceso de desinstalación, en el menú **Inicio** , haga clic en **Panel de control** y haga doble clic en **Programas y características**. 
  
2. Haga doble clic en el componente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que desea desinstalar y haga clic en **Quitar**. 
  
3. Se ejecutarán las reglas auxiliares del programa de instalación para comprobar la configuración del equipo. Haga clic en **Aceptar** para continuar. 
  
4. En la página **Seleccionar instancia** , seleccione la instancia preparada que desea modificar. El nombre de la instancia preparada se mostrará como "IdentificadorDeInstanciaPreparada sin configurar" donde IdentificadorDeInstanciaPreparada es la instancia que ha seleccionado. 
  
5. En la página **Seleccionar características** , especifique las características que desea quitar de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificada. Haga clic en **Siguiente** para continuar. 
  
6. Se ejecutarán las reglas de eliminación para comprobar que la operación se puede completar correctamente. 
  
7. En la página **Listo para eliminar** , revise la lista de componentes y características que se van a desinstalar. 
  
8. La página **Progreso de la desinstalación** mostrará el estado de la operación. 
  
9. En la página **Completar** puede revisar el estado de compleción de la operación. Haga clic en **cerrar** para salir del asistente para la instalación. 
  
##  <a name="uninstalling-a-prepared-instance"></a><a name="Uninstall"></a> Desinstalar una instancia preparada  
  
### <a name="uninstall-a-prepared-instance-of-ssnoversion"></a>Desinstalar una instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1. Para comenzar el proceso de desinstalación, en el menú **Inicio** , haga clic en **Panel de control** y haga doble clic en **Programas y características**. 
  
2. Haga doble clic en el componente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que desea desinstalar y haga clic en **Quitar**. 
  
3. Se ejecutarán las reglas auxiliares del programa de instalación para comprobar la configuración del equipo. Haga clic en **Aceptar** para continuar. 
  
4. En la página **Seleccionar instancia** , seleccione la instancia preparada que desea modificar. El nombre de la instancia preparada se mostrará como "IdentificadorDeInstanciaPreparada sin configurar" donde IdentificadorDeInstanciaPreparada es la instancia que ha seleccionado. 
  
5. En la página **Seleccionar características** , especifique las características que desea quitar de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificada. Haga clic en **Siguiente** para continuar. 
  
6. En la página **Reglas de eliminación** , el programa de instalación ejecutará las reglas para comprobar que la operación se puede completar correctamente. 
  
7. En la página **Listo para eliminar** , revise la lista de componentes y características que se van a desinstalar. 
  
8. La página **Progreso de la desinstalación** mostrará el estado de la operación. 
  
9. En la página **Completar** puede revisar el estado de compleción de la operación. Haga clic en **cerrar** para salir del asistente para la instalación. 
  
10. Repita los pasos 1 a 9 hasta que se hayan quitado todos los componentes de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . 
  
##  <a name="modifying-or-uninstalling-a-completed-instance-of-ssnoversion"></a><a name="bk_Modifying_Uninstalling"></a> Modificar o desinstalar una instancia completada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
 El proceso para agregar o quitar características, o bien para desinstalar una instancia completada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es similar al proceso de una instancia instalada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte los siguientes artículos.  
  
- [Agregar características a una instancia de SQL Server &#40;programa de instalación&#41;](./add-features-to-an-instance-of-sql-server-setup.md)  
  
- [Desinstalar una instancia existente de SQL Server &#40;programa de instalación&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)  
  
## <a name="see-also"></a>Consulte también  
 [Qué es Windows SysPrep](/previous-versions/windows/it-pro/windows-vista/cc721940(v=ws.10))   
 [Cómo funciona Windows SysPrep](/previous-versions/windows/it-pro/windows-vista/cc766514(v=ws.10))  
  
