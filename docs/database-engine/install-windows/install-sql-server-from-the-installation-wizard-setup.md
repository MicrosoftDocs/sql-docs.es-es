---
title: Instalación mediante la interfaz gráfica de usuario
description: En este artículo se proporciona un procedimiento paso a paso para instalar una nueva instancia de SQL Server con el Asistente para la instalación de SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- installing SQL Server, steps
- Setup [SQL Server], steps
- SQL Server, installing
ms.assetid: 6ad23de1-2bab-4933-9122-c09f5565028d
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8bb4b3b0e70850702c34ff1c69b9c2fcd5f3c85b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342319"
---
# <a name="install-sql-server-from-the-installation-wizard-setup"></a>Instalar SQL Server desde el Asistente para la instalación (programa de instalación)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

En este artículo se describe cómo instalar SQL Server con el Asistente para la instalación. Se aplica a [!INCLUDE[SQLServer2016](../../includes/sssql16-md.md)] y a [!INCLUDE[SQLServer2017](../../includes/sssql17-md.md)].

En este artículo se proporciona un procedimiento paso a paso para instalar una nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el Asistente para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El Asistente para la instalación proporciona un único árbol de características para la instalación de todos los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para que no tenga que instalarlos individualmente. Para instalar los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por separado, consulte [Instalar SQL Server](../../database-engine/install-windows/install-sql-server.md#individual-component-installation).  

Para conocer otras formas de instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consulte:  

* [Instalación de SQL Server desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md)  
* [Instalación de SQL Server mediante un archivo de configuración](./install-sql-server-using-a-configuration-file.md)  
* [Instalar SQL Server con SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md)  
* [Crear un nuevo clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
* [Actualizar SQL Server con el Asistente para instalación &#40;programa de instalación&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

## <a name="get-the-installation-media"></a>Obtener los medios de instalación

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]
  
## <a name="prerequisites"></a>Requisitos previos

Antes de instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], revise [Planear una instalación de SQL Server](../../sql-server/install/planning-a-sql-server-installation.md).  
  
> [!NOTE]  
> En instalaciones locales, debe ejecutar el programa de instalación como administrador. Si instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde un recurso compartido remoto, deberá usar una cuenta de dominio que tenga permisos de lectura y ejecución para dicho recurso.  

::: moniker range=">=sql-server-2016 <=sql-server-2017"

###  <a name="install-patch-requirement"></a><a name="bkmk_ga_instalpatch"></a> Requisito de instalación de revisión

Microsoft ha identificado un problema con los archivos binarios de entorno de ejecución de Microsoft Visual C++ 2013 que se instalan como requisito previo con SQL Server 2016 y 2017. Hay disponible una actualización para corregir este problema. Si esta actualización a los archivos binarios de tiempo de ejecución de Visual C++ no se instala, puede que SQL Server experimente problemas de estabilidad en determinados escenarios. Antes de instalar SQL Server, siga las instrucciones que se indican en [Notas de la versión de SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) para ver si el equipo necesita una revisión para los archivos binarios de tiempo de ejecución de Visual C++. 

Esto no se aplica a [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)].

## <a name="to-install-sql-server-2016-and-2017"></a>Para instalar SQL Server 2016 y 2017:  

1. Inserte el medio de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Desde la carpeta raíz, haga doble clic en **Setup.exe**. Para realizar la instalación desde un recurso compartido de red, localice la carpeta raíz de dicho recurso y, a continuación, haga doble clic en **Setup.exe**.  
  
1. El Asistente para instalación ejecuta el Centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para crear una nueva instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], seleccione **Instalación** en el área de navegación izquierda y, luego, seleccione **Nueva instalación independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o agregar características a una instalación existente**.  

1. En la página **Clave del producto**, seleccione una opción para indicar si va a instalar una edición gratuita de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o una versión de producción con una clave de PID. Para más información, vea [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
   Para continuar, seleccione **Siguiente**.

1. En la página **Términos de licencia**, revise el contrato de licencia. Si está de acuerdo, marque la casilla **Acepto los términos de licencia** y, luego, seleccione **Siguiente**.  
    
   > [!NOTE]
   > SQL Server transmite la información sobre su experiencia de instalación, así como otros datos de uso y rendimiento para ayudar a Microsoft a mejorar el producto. Para obtener más información acerca de los controles de privacidad y de procesamiento de datos de SQL Server, consulte la [declaración de privacidad](https://privacy.microsoft.com/privacystatement) y [Configuración de SQL Server para enviar comentarios a Microsoft](../../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md).

1. En la página **Reglas globales**, el procedimiento de instalación avanzará automáticamente hasta la página **Actualizaciones de productos** si no hay ningún error de regla.  
  
1. La página **Microsoft Update** aparecerá a continuación si la casilla **Microsoft Update** de **Panel de control** > **Todos los elementos de Panel de control** > **Windows Update** > **Cambiar configuración** no está activada. Al activar la casilla **Microsoft Update** cambia la configuración del equipo para incluir las actualizaciones más recientes de todos los productos de Microsoft al buscar actualizaciones de Windows.  

1. En la página **Actualizaciones del producto**, se muestran las actualizaciones del producto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] más recientes disponibles. Si no se detectan actualizaciones de producto, el programa de instalación no muestra esta página y pasa automáticamente a la página **Instalar archivos de configuración**.  

1. En la página **Instalar archivos de configuración**, el programa de instalación proporciona el progreso de la descarga, la extracción y la instalación de los archivos de configuración. Si se encuentra una actualización para la instalación y se especifica que debe incluirse, esa actualización también se instalará. Si no se encuentra ninguna actualización, el programa de instalación avanzará automáticamente.
  
1. En la página **Instalar reglas**, el programa de instalación busca posibles problemas que pueden producirse al ejecutar la instalación. Si se producen errores, seleccione un elemento de la columna **Estado** para obtener más información. De lo contrario, seleccione **Siguiente**.

1. Si es la primera instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la máquina, el programa de instalación omite la página **Tipo de instalación** y va directamente a la página **Selección de características**. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ya está instalado en el sistema, puede usar la página **Tipo de instalación** para seleccionar realizar una instalación nueva o agregar características a una instalación existente. Para continuar, seleccione **Siguiente**.
  
1. En la página **Selección de características**, seleccione los componentes de la instalación. Por ejemplo, para instalar una nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)], seleccione **Servicios de Motor de base de datos**.

    Después de seleccionar el nombre de la característica, aparece una descripción de cada grupo del componente en el panel **Descripción de característica** . Puede activar cualquier combinación de casillas. Para más información, consulte [Ediciones y componentes de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) y [Ediciones y componentes de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
  
     Los requisitos previos para las características seleccionadas se muestran en el panel **Requisitos previos de las características seleccionadas** . El programa de instalación instala los requisitos previos que no se hayan instalado todavía durante el paso de instalación que se describe más adelante en este procedimiento.  
  
     Si desea especificar un directorio personalizado para los componentes compartidos, use el campo situado en la parte inferior de la página **Selección de características**. Para cambiar la ruta de instalación de los componentes compartidos, actualice la ruta en el campo situado en la parte inferior del cuadro de diálogo o seleccione **Examinar** para moverse a un directorio de instalación. La ruta de instalación predeterminada es [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)].  
  
     > [!NOTE]
     > La ruta de acceso especificada para los componentes compartidos debe ser una ruta de acceso absoluta. La carpeta no se debe comprimir ni cifrar. No se admiten unidades asignadas.  
  
     SQL Server usa dos directorios para las características compartidas:
  
     * Directorio de características compartidas  
     * Directorio de características compartidas (x86)  
  
     > [!NOTE]
     > La ruta de acceso especificada para cada una de las opciones anteriores debe ser diferente.  
  
1. La página **Reglas de características** avanza automáticamente si se cumplen todas las reglas.  
  
1. En la página **Configuración de instancia**, especifique si desea instalar una instancia predeterminada o una instancia con nombre. Para obtener más información, consulte [Configuración de instancia](../../sql-server/install/instance-configuration.md#instance-configuration-page).  
  
     * **Id. de instancia**: de forma predeterminada, el nombre de instancia se usa como identificador de la instancia. Este identificador se usa para identificar los directorios de instalación y las claves del Registro de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El mismo comportamiento se produce para las instancias predeterminadas y las instancias con nombre. Para las instancias predeterminadas, el nombre y el identificador son MSSQLSERVER. Para usar un identificador de instancia no predeterminado, especifique un valor diferente en el cuadro de texto **Id. de instancia**.  
  
       > [!NOTE]  
       > Las instancias independientes típicas de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], tanto si son predeterminadas como si son instancias con nombre, no usan un valor no predeterminado para identificador de instancia.  
  
       Todos los Service Pack y actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se aplican a cada componente de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     * **Instancias instaladas**: la cuadrícula muestra las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que están en el equipo en el que se ejecuta el programa de instalación. Si ya hay una instancia predeterminada instalada en el equipo, debe instalar una instancia con nombre de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)].  
  
     El flujo de trabajo del resto de la instalación depende de las características que haya especificado en la instalación. Dependiendo de las selecciones, es posible que no vea todas las páginas. 

1. Si selecciona instalar la característica de PolyBase, se agregará la página **Configuración de PolyBase** al programa de instalación de SQL Server, que se muestra después de la página **Configuración de instancia**. PolyBase requiere, como mínimo, la actualización 51 de Oracle JRE 7 y, si todavía no está instalada, se bloqueará la instalación. En la página **Configuración de PolyBase**, puede usar SQL Server como una instancia habilitada para PolyBase independiente, o bien puede usar esta instancia de SQL Server como parte de un grupo de escalado horizontal de PolyBase. Si decide usar el grupo de escalado horizontal, deberá especificar un intervalo de puertos de hasta 6 o más puertos. 


1. Use la página **Configuración del servidor - Cuentas de servicio** para especificar las cuentas de inicio de sesión para los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los servicios reales que se configuran en esta página dependen de las características que ha elegido instalar. Para obtener más información acerca de los valores de configuración, consulte [Ayuda del Asistente para instalación](../../sql-server/install/instance-configuration.md#serverconfig).
  
     Puede asignar la misma cuenta de inicio de sesión a todos los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], o configurar cada cuenta de servicio individualmente. También puede especificar si los servicios se inician de forma automática o manual, o si están deshabilitados. Se recomienda configurar cuentas de servicio de forma individual para proporcionar los privilegios mínimos para cada servicio. Asegúrese de que a los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se les concedan los permisos mínimos que deben tener para completar sus tareas. Para obtener más información, consulte [Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Para especificar la misma cuenta de inicio de sesión para todas las cuentas de servicio de esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], proporcione credenciales en los campos de la parte inferior de la página.  
  
    > [!IMPORTANT]  
    > [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  

    > [!NOTE]
    > A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], active la casilla **Conceder el privilegio de realización de tareas de mantenimiento de volumen al servicio Motor de base de datos de SQL Server** para permitir que la cuenta de servicio de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] pueda usar [inicialización instantánea de archivos de bases de datos](../../relational-databases/databases/database-instant-file-initialization.md).
  
1. Use la página **Configuración del servidor - Intercalación** para especificar intercalaciones no predeterminadas para [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].    

   La configuración de instalación predeterminada depende de la configuración regional del sistema operativo (SO). Las intercalaciones de nivel de servidor se pueden cambiar durante la instalación o modificando la configuración regional del SO antes de la instalación. La intercalación predeterminada se establece en la versión más antigua disponible que esté asociada a cada configuración regional concreta. Esto se debe a motivos de compatibilidad con versiones anteriores. Por consiguiente, esta no es siempre la intercalación recomendada. Para aprovechar al máximo las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cambie la configuración de instalación predeterminada para que use las intercalaciones de Windows. Por ejemplo, para la configuración regional del sistema operativo **Inglés (Estados Unidos)** (página de códigos 1252), la intercalación predeterminada durante la instalación es **SQL_LATIN1_GENERAL_CP1_CI_AS** y se puede cambiar a la intercalación de Windows homóloga más cercana: **Latin1_General_100_CI_AS_SC**.

   Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
1. Use la página **Configuración del Motor de base de datos- Configuración del servidor** para especificar las siguientes opciones:  
  
    * **Modo de seguridad**: seleccione **Autenticación de Windows** o **Autenticación de modo mixto** para su instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si selecciona **Autenticación de modo mixto**, debe proporcionar una contraseña segura para la cuenta de administrador del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] integrada.  
  
       Una vez que un dispositivo establece una conexión correcta con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el mecanismo de seguridad es el mismo para la autenticación de Windows y para la autenticación de modo mixto. Para obtener más información, consulte [Configuración del Motor de base de datos: configuración del servidor](../../sql-server/install/instance-configuration.md#serverconfig).
  
    * **Administradores de SQL Server**: debe especificar al menos un administrador del sistema para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para agregar la cuenta en la que se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], seleccione **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, haga clic en **Agregar** o en **Quitar** y, luego, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Use la página **Configuración del Motor de base de datos - Directorios de datos** para especificar los directorios de instalación no predeterminados. Para realizar la instalación en los directorios predeterminados, seleccione **Siguiente**.  
  
    > [!IMPORTANT]  
    > Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Para obtener más información, consulte la página [Configuración del Motor de base de datos - Directorios de datos](../../sql-server/install/instance-configuration.md#datadir).

     Use la página **Configuración del Motor de base de datos - TempDB** para configurar el tamaño de archivo, el número de archivos, los directorios de instalación no predeterminados y la configuración del crecimiento de archivos para **tempdb**. Para obtener más información, consulte la página [Configuración del Motor de base de datos: TempDB](../../sql-server/install/instance-configuration.md#tempdb). 

 
     Use la página **Configuración de Motor de base de datos - FILESTREAM** para habilitar FILESTREAM para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, consulte la página [Configuración del Motor de base de datos - FILESTREAM](../../sql-server/install/instance-configuration.md#database-engine-configuration---filestream-page).  
  
1. Use la página **Configuración de Analysis Services - Aprovisionamiento de cuentas** para especificar el modo de servidor y los usuarios o las cuentas que tendrán permisos de administrador para [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. El modo servidor determina los subsistemas de memoria y de almacenamiento que se utilizan en el servidor. Tipos diferentes de solución se ejecutan en modos servidor diferentes. Si tiene previsto ejecutar bases de datos multidimensionales de cubo en el servidor, elija la opción predeterminada, **Modo multidimensional y de minería de datos**.

    Debe especificar al menos un administrador del sistema para [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]:

    * Para agregar la cuenta en la que se ejecuta el programa de instalación de SQL Server, seleccione **Agregar usuario actual**.

    * Para agregar o quitar cuentas de la lista de administradores del sistema, seleccione **Agregar** o **Quitar** y, luego, modifique la lista de usuarios, grupos o equipos que tienen privilegios de administrador para [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].

    Para obtener más información sobre el modo servidor y los permisos de administrador, consulte la página [Configuración de Analysis Services - Aprovisionamiento de cuentas](../../sql-server/install/instance-configuration.md#analysis-services-configuration---account-provisioning-page).

    Cuando haya terminado de editar la lista, seleccione **Aceptar**. Compruebe la lista de administradores en el cuadro de diálogo de configuración. Una vez completada la lista, seleccione **Siguiente**.

    Use la página **Configuración de Analysis Services - Directorios de datos** para especificar los directorios de instalación no predeterminados. Para realizar la instalación en los directorios predeterminados, seleccione **Siguiente**.  

    > [!IMPORTANT]  
    > Si al instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se especifica la misma ruta de directorio para INSTANCEDIR y SQLUSERDBDIR, el Agente SQL Server y la búsqueda de texto completo no se iniciarán porque faltarán permisos.  
    >  
    > Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

    Para obtener más información, consulte la página [Configuración de Analysis Services - Directorios de datos](../../sql-server/install/instance-configuration.md#analysis-services-configuration---data-directories-page).  

1. Use la página **Configuración de Distributed Replay Controller** para especificar los usuarios a los que desee conceder permisos administrativos para el servicio Distributed Replay Controller. Los usuarios con permisos administrativos tendrán acceso ilimitado al servicio Distributed Replay Controller.  
  
     * Para conceder permisos de acceso al servicio Distributed Replay Controller para el usuario que ejecuta el programa de instalación de SQL Server, seleccione el botón **Agregar usuario actual**.

     * Para conceder a otros usuarios permisos de acceso al servicio Distributed Replay Controller, seleccione el botón **Agregar**.

     * Para quitar los permisos de acceso del servicio Distributed Replay Controller, seleccione el botón **Quitar**.

     * Para continuar, seleccione **Siguiente**.  
  
1. Use la página **Configuración de Distributed Replay Client** para especificar los usuarios a los que desee conceder permisos administrativos para el servicio Distributed Replay Client. Los usuarios con permisos administrativos tendrán acceso ilimitado al servicio Distributed Replay Client.  
  
     * El **nombre del controlador** es opcional. El valor predeterminado es \<*blank*>. Escriba el nombre del controlador con el que se comunicará el equipo cliente para el servicio Distributed Replay Client:  
  
       * Si ya ha configurado un controlador, escriba el nombre del controlador mientras configura cada cliente.  
  
       * Si aún no ha configurado ningún controlador, puede dejar el nombre del controlador en blanco. Sin embargo, debe escribir manualmente el nombre del controlador en el archivo de **configuración de cliente** .  
  
     * Especifique el **Directorio de trabajo** para el servicio Distributed Replay Client. El directorio de trabajo predeterminado es \<*drive letter*>:\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\.  
  
     * Especifique el **Directorio de resultados** para el servicio Distributed Replay Client. El directorio de resultados predeterminado es \<*drive letter*>:\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\.  
  
     * Para continuar, seleccione **Siguiente**.  
  
1. La página **Listo para instalar** muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. En esta página, el programa de instalación indica si la característica de **actualización de producto** está habilitada o deshabilitada y la versión final de actualización.  
  
     Seleccione **Instalar** para continuar. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación instala primero los requisitos previos necesarios para las características seleccionadas y después instala las características seleccionadas.  
  
1. La página **Progreso de la instalación** muestra las actualizaciones de estado para que pueda supervisar el progreso de la instalación durante la ejecución del programa de instalación.  
  
1. Después de la instalación, en la página **Operación completada** se proporciona un vínculo al archivo de registro de resumen de la instalación y a otras notas importantes.
  
    > [!IMPORTANT]
    > Asegúrese de leer el mensaje del Asistente para la instalación tras finalizar la instalación. Para obtener más información, consulte [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

    Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], seleccione **Cerrar**.  
  
1. Si el programa indica que se reinicie el equipo, hágalo ahora.

::: moniker-end

::: moniker range=">=sql-server-ver15"
## <a name="to-install-sql-server-2019"></a>Para instalar SQL Server 2019: 
  
1. Inserte el medio de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Desde la carpeta raíz, haga doble clic en **Setup.exe**. Para realizar la instalación desde un recurso compartido de red, localice la carpeta raíz de dicho recurso y, a continuación, haga doble clic en **Setup.exe**.  
  
1. El Asistente para instalación ejecuta el Centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para crear una nueva instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], seleccione **Instalación** en el área de navegación izquierda y, luego, seleccione **Nueva instalación independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o agregar características a una instalación existente**.  

1. En la página **Clave del producto**, seleccione una opción para indicar si va a instalar una edición gratuita de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o una versión de producción con una clave de PID. Para más información, vea [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
   Para continuar, seleccione **Siguiente**.
  
1. En la página **Términos de licencia**, revise el contrato de licencia. Si está de acuerdo, marque la casilla **Acepto los términos de licencia [declaración de privacidad](https://privacy.microsoft.com/privacystatement)** y, luego, seleccione **Siguiente**.  

   > [!NOTE]
   > Si se especifica una clave de producto de licencia Enterprise Server o CAL y el equipo tiene más de 20 núcleos físicos, o 40 núcleos lógicos si la tecnología Hyper-Threading está habilitada, se muestra una advertencia durante la instalación. Puede continuar con la instalación si activa la casilla **Para continuar, active esta casilla para confirmar esta limitación o haga clic en Atrás o Cancelar para escribir una licencia de producto Enterprise Core que admita hasta el máximo del sistema operativo** o hace clic en **Atrás** y escribe una clave de licencia que admita el número máximo de procesadores del sistema operativo.

   > [!NOTE]
   > SQL Server transmite la información sobre su experiencia de instalación, así como otros datos de uso y rendimiento para ayudar a Microsoft a mejorar el producto. Para obtener más información acerca de los controles de privacidad y de procesamiento de datos de SQL Server, consulte la [declaración de privacidad](https://privacy.microsoft.com/privacystatement) y [Configuración de SQL Server para enviar comentarios a Microsoft](../../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md).

1. En la página **Reglas globales**, el procedimiento de instalación avanzará automáticamente hasta la página **Actualizaciones de productos** si no hay ningún error de regla.  
  
1. La página **Microsoft Update** aparecerá a continuación si la casilla **Microsoft Update** de **Panel de control** > **Todos los elementos de Panel de control** > **Windows Update** > **Cambiar configuración** no está activada. Al activar la casilla **Microsoft Update** cambia la configuración del equipo para incluir las actualizaciones más recientes de todos los productos de Microsoft al buscar actualizaciones de Windows.  

1. En la página **Actualizaciones del producto**, se muestran las actualizaciones del producto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] más recientes disponibles. Si no se detectan actualizaciones de producto, el programa de instalación no muestra esta página y pasa automáticamente a la página **Instalar archivos de configuración**.  

1. En la página **Instalar archivos de configuración**, el programa de instalación proporciona el progreso de la descarga, la extracción y la instalación de los archivos de configuración. Si se encuentra una actualización para la instalación y se especifica que debe incluirse, esa actualización también se instalará. Si no se encuentra ninguna actualización, el programa de instalación avanzará automáticamente.
  
1. En la página **Instalar reglas**, el programa de instalación busca posibles problemas que pueden producirse al ejecutar la instalación. Si se producen errores, seleccione un elemento de la columna **Estado** para obtener más información. De lo contrario, seleccione **Siguiente**.

1. Si es la primera instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la máquina, el programa de instalación omite la página **Tipo de instalación** y va directamente a la página **Selección de características**. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ya está instalado en el sistema, puede usar la página **Tipo de instalación** para seleccionar realizar una instalación nueva o agregar características a una instalación existente. Para continuar, seleccione **Siguiente**.
  
1. En la página **Selección de características**, seleccione los componentes de la instalación. Por ejemplo, para instalar una nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)], seleccione **Servicios de Motor de base de datos**.

    Después de seleccionar el nombre de la característica, aparece una descripción de cada grupo del componente en el panel **Descripción de característica** . Puede activar cualquier combinación de casillas. Para más información, consulte [Ediciones y componentes de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) y [Ediciones y componentes de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
  
     Los requisitos previos para las características seleccionadas se muestran en el panel **Requisitos previos de las características seleccionadas** . El programa de instalación instala los requisitos previos que no se hayan instalado todavía durante el paso de instalación que se describe más adelante en este procedimiento.  
  
     Si desea especificar un directorio personalizado para los componentes compartidos, use el campo situado en la parte inferior de la página **Selección de características**. Para cambiar la ruta de instalación de los componentes compartidos, actualice la ruta en el campo situado en la parte inferior del cuadro de diálogo o seleccione **Examinar** para moverse a un directorio de instalación. La ruta de instalación predeterminada es [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)].  
  
     > [!NOTE]
     > La ruta de acceso especificada para los componentes compartidos debe ser una ruta de acceso absoluta. La carpeta no se debe comprimir ni cifrar. No se admiten unidades asignadas.  
  
     SQL Server usa dos directorios para las características compartidas:
  
     * Directorio de características compartidas  
     * Directorio de características compartidas (x86)  
  
     > [!NOTE]
     > La ruta de acceso especificada para cada una de las opciones anteriores debe ser diferente.  
  
1. La página **Reglas de características** avanza automáticamente si se cumplen todas las reglas.  
  
1. En la página **Configuración de instancia**, especifique si desea instalar una instancia predeterminada o una instancia con nombre. Para obtener más información, consulte [Configuración de instancia](../../sql-server/install/instance-configuration.md#instance-configuration-page).  
  
     * **Id. de instancia**: de forma predeterminada, el nombre de instancia se usa como identificador de la instancia. Este identificador se usa para identificar los directorios de instalación y las claves del Registro de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El mismo comportamiento se produce para las instancias predeterminadas y las instancias con nombre. Para las instancias predeterminadas, el nombre y el identificador son MSSQLSERVER. Para usar un identificador de instancia no predeterminado, especifique un valor diferente en el cuadro de texto **Id. de instancia**.  
  
       > [!NOTE]  
       > Las instancias independientes típicas de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], tanto si son predeterminadas como si son instancias con nombre, no usan un valor no predeterminado para identificador de instancia.  
  
       Todos los Service Pack y actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se aplican a cada componente de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     * **Instancias instaladas**: la cuadrícula muestra las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que están en el equipo en el que se ejecuta el programa de instalación. Si ya hay una instancia predeterminada instalada en el equipo, debe instalar una instancia con nombre de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)].  
  
     El flujo de trabajo del resto de la instalación depende de las características que haya especificado en la instalación. Dependiendo de las selecciones, es posible que no vea todas las páginas. 

1. A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], PolyBase ya no requiere que la actualización 51 (como mínimo) de Oracle JRE 7 esté preinstalada en el equipo antes de instalar la característica. Si selecciona instalar la característica de PolyBase, se agregará la página **Ubicación de instalación de Java** al programa de instalación de SQL Server, que se muestra después de la página **Configuración de instancia**. En la página Ubicación de instalación de Java, puede elegir instalar Open JRE de Azul Zulu, incluido en la instalación de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], o bien proporcionar una ubicación de otro JRE o JDK que ya se haya instalado en el equipo.

1. A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], se ha agregado Java con extensiones de lenguaje. Si selecciona instalar la característica de Java, se agregará la página **Ubicación de instalación de Java** a la ventana de diálogo de instalación de SQL Server, que se muestra después de la página **Configuración de instancia**. En la página **Ubicación de instalación de Java**, puede elegir instalar Open JRE de Azul Zulu, incluido en la instalación de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], o bien proporcionar una ubicación de otro JRE o JDK que ya se haya instalado en el equipo.

1. Use la página **Configuración del servidor - Cuentas de servicio** para especificar las cuentas de inicio de sesión para los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los servicios reales que se configuran en esta página dependen de las características que ha elegido instalar. Para obtener más información acerca de los valores de configuración, consulte [Ayuda del Asistente para instalación](../../sql-server/install/instance-configuration.md#serverconfig).
  
     Puede asignar la misma cuenta de inicio de sesión a todos los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], o configurar cada cuenta de servicio individualmente. También puede especificar si los servicios se inician de forma automática o manual, o si están deshabilitados. Se recomienda configurar cuentas de servicio de forma individual para proporcionar los privilegios mínimos para cada servicio. Asegúrese de que a los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se les concedan los permisos mínimos que deben tener para completar sus tareas. Para obtener más información, consulte [Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Para especificar la misma cuenta de inicio de sesión para todas las cuentas de servicio de esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], proporcione credenciales en los campos de la parte inferior de la página.  
  
    > [!IMPORTANT]  
    > [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  

    > [!NOTE]
    > A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], active la casilla **Conceder el privilegio de realización de tareas de mantenimiento de volumen al servicio Motor de base de datos de SQL Server** para permitir que la cuenta de servicio de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] pueda usar [inicialización instantánea de archivos de bases de datos](../../relational-databases/databases/database-instant-file-initialization.md).
  
     Use la página **Configuración del servidor - Intercalación** para especificar intercalaciones no predeterminadas para [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Para más información, consulte [Compatibilidad con la intercalación y Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
1. Use la página **Configuración del Motor de base de datos- Configuración del servidor** para especificar las siguientes opciones:  
  
    * **Modo de seguridad**: seleccione **Autenticación de Windows** o **Autenticación de modo mixto** para su instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si selecciona **Autenticación de modo mixto**, debe proporcionar una contraseña segura para la cuenta de administrador del sistema (sa) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] integrada.  
  
       Una vez que un dispositivo establece una conexión correcta con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el mecanismo de seguridad es el mismo para la autenticación de Windows y para la autenticación de modo mixto. Para obtener más información, consulte [Configuración del Motor de base de datos: configuración del servidor](../../sql-server/install/instance-configuration.md#serverconfig).
  
    * **Administradores de SQL Server**: debe especificar al menos un administrador del sistema para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para agregar la cuenta en la que se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], seleccione **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, haga clic en **Agregar** o en **Quitar** y, luego, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  También puede agregar un grupo de dominio de Windows para establecer un grupo de administradores compartido de SQL en Active Directory con acceso de sysadmin a SQL Server. 
  
     Use la página **Configuración del Motor de base de datos - Directorios de datos** para especificar los directorios de instalación no predeterminados. Para realizar la instalación en los directorios predeterminados, seleccione **Siguiente**.  
  
    > [!IMPORTANT]  
    > Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Para obtener más información, consulte la página [Configuración del Motor de base de datos - Directorios de datos](../../sql-server/install/instance-configuration.md#datadir).

     Use la página **Configuración del Motor de base de datos - TempDB** para configurar el tamaño de archivo, el número de archivos, los directorios de instalación no predeterminados y la configuración del crecimiento de archivos para **tempdb**. Para obtener más información, consulte la página [Configuración del Motor de base de datos: TempDB](../../sql-server/install/instance-configuration.md#tempdb).

     Use la página **[!INCLUDE[ssDE](../../includes/ssde-md.md)] Configuración: MaxDOP** para especificar el grado máximo de paralelismo. Esta configuración determina cuántos procesadores puede utilizar una única instrucción durante la ejecución. El valor recomendado se calcula automáticamente durante la instalación. 
     
    > [!NOTE]  
    > Esta página solo está disponible en el programa de instalación a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. 
    
    Para obtener más información, consulte [Página Configuración del Motor de base de datos: MaxDOP](../../sql-server/install/instance-configuration.md?view=sql-server-ver15&preserve-view=true#maxdop). 

     Use la página **Configuración del Motor de base de datos: Memoria** para especificar los valores de **Memoria de servidor mínima** y **Memoria de servidor máxima** que usará esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] después del inicio. Puede usar los valores predeterminados o los valores calculados recomendados o especificar manualmente sus propios valores después de haber elegido la opción **Recomendado**.
     
    > [!NOTE]  
    > Esta página solo está disponible en el programa de instalación a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. 
    
    Para obtener más información, consulte [Página Configuración del Motor de base de datos: memoria](../../sql-server/install/instance-configuration.md?view=sql-server-ver15&preserve-view=true#memory). 

     Use la página **Configuración de Motor de base de datos - FILESTREAM** para habilitar FILESTREAM para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, consulte la página [Configuración del Motor de base de datos - FILESTREAM](../../sql-server/install/instance-configuration.md#database-engine-configuration---filestream-page).  
  
1. Use la página **Configuración de Analysis Services - Aprovisionamiento de cuentas** para especificar el modo de servidor y los usuarios o las cuentas que tendrán permisos de administrador para [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. El modo servidor determina los subsistemas de memoria y de almacenamiento que se utilizan en el servidor. Tipos diferentes de solución se ejecutan en modos servidor diferentes. Si tiene previsto ejecutar bases de datos multidimensionales de cubo en el servidor, elija la opción predeterminada, **Modo multidimensional y de minería de datos**.

    Debe especificar al menos un administrador del sistema para [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]:

    * Para agregar la cuenta en la que se ejecuta el programa de instalación de SQL Server, seleccione **Agregar usuario actual**.

    * Para agregar o quitar cuentas de la lista de administradores del sistema, seleccione **Agregar** o **Quitar** y, luego, modifique la lista de usuarios, grupos o equipos que tienen privilegios de administrador para [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].

    Para obtener más información sobre el modo servidor y los permisos de administrador, consulte la página [Configuración de Analysis Services - Aprovisionamiento de cuentas](../../sql-server/install/instance-configuration.md#analysis-services-configuration---account-provisioning-page).

    Cuando haya terminado de editar la lista, seleccione **Aceptar**. Compruebe la lista de administradores en el cuadro de diálogo de configuración. Una vez completada la lista, seleccione **Siguiente**.

    Use la página **Configuración de Analysis Services - Directorios de datos** para especificar los directorios de instalación no predeterminados. Para realizar la instalación en los directorios predeterminados, seleccione **Siguiente**.  

    > [!IMPORTANT]  
    > Si al instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se especifica la misma ruta de directorio para INSTANCEDIR y SQLUSERDBDIR, el Agente SQL Server y la búsqueda de texto completo no se iniciarán porque faltarán permisos.  
    >  
    > Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

    Para obtener más información, consulte la página [Configuración de Analysis Services - Directorios de datos](../../sql-server/install/instance-configuration.md#analysis-services-configuration---data-directories-page).  

1. Use la página **Configuración de Distributed Replay Controller** para especificar los usuarios a los que desee conceder permisos administrativos para el servicio Distributed Replay Controller. Los usuarios con permisos administrativos tendrán acceso ilimitado al servicio Distributed Replay Controller.  
  
     * Para conceder permisos de acceso al servicio Distributed Replay Controller para el usuario que ejecuta el programa de instalación de SQL Server, seleccione el botón **Agregar usuario actual**.

     * Para conceder a otros usuarios permisos de acceso al servicio Distributed Replay Controller, seleccione el botón **Agregar**.

     * Para quitar los permisos de acceso del servicio Distributed Replay Controller, seleccione el botón **Quitar**.

     * Para continuar, seleccione **Siguiente**.  
  
1. Use la página **Configuración de Distributed Replay Client** para especificar los usuarios a los que desee conceder permisos administrativos para el servicio Distributed Replay Client. Los usuarios con permisos administrativos tendrán acceso ilimitado al servicio Distributed Replay Client.  
  
     * El **nombre del controlador** es opcional. El valor predeterminado es \<*blank*>. Escriba el nombre del controlador con el que se comunicará el equipo cliente para el servicio Distributed Replay Client:  
  
       * Si ya ha configurado un controlador, escriba el nombre del controlador mientras configura cada cliente.  
  
       * Si aún no ha configurado ningún controlador, puede dejar el nombre del controlador en blanco. Sin embargo, debe escribir manualmente el nombre del controlador en el archivo de **configuración de cliente** .  
  
     * Especifique el **Directorio de trabajo** para el servicio Distributed Replay Client. El directorio de trabajo predeterminado es \<*drive letter*>:\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\.  
  
     * Especifique el **Directorio de resultados** para el servicio Distributed Replay Client. El directorio de resultados predeterminado es \<*drive letter*>:\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\.  
  
     * Para continuar, seleccione **Siguiente**.  
  
1. La página **Listo para instalar** muestra una vista de árbol de las opciones de instalación que se especificaron durante la instalación. En esta página, el programa de instalación indica si la característica de **actualización de producto** está habilitada o deshabilitada y la versión final de actualización.  
  
     Seleccione **Instalar** para continuar. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación instala primero los requisitos previos necesarios para las características seleccionadas y después instala las características seleccionadas.  
  
1. La página **Progreso de la instalación** muestra las actualizaciones de estado para que pueda supervisar el progreso de la instalación durante la ejecución del programa de instalación.  
  
1. Después de la instalación, en la página **Operación completada** se proporciona un vínculo al archivo de registro de resumen de la instalación y a otras notas importantes.
  
    > [!IMPORTANT]
    > Asegúrese de leer el mensaje del Asistente para la instalación tras finalizar la instalación. Para obtener más información, consulte [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

    Para completar el proceso de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], seleccione **Cerrar**.  
  
1. Si el programa indica que se reinicie el equipo, hágalo ahora.

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

[Configure la nueva instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]](../../database-engine/configure-windows/database-engine-instances-sql-server.md).  
  
Para reducir el área expuesta de un sistema susceptible de recibir ataques, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instala y habilita de manera selectiva los servicios y características clave. Para obtener más información, consulte [Configuración del área expuesta](../../relational-databases/security/surface-area-configuration.md).  
  
## <a name="see-also"></a>Consulte también
  
* [Validar una instalación de SQL Server](../../database-engine/install-windows/validate-a-sql-server-installation.md)  
* [Reparar una instalación de SQL Server con errores](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)
* [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
* [Actualizar SQL Server con el Asistente para instalación &#40;programa de instalación&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
* [Instalación de SQL Server desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md)