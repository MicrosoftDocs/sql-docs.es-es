---
description: Trabajos del Agente SQL Server para paquetes
title: Trabajos del Agente SQL Server para paquetes | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- jobs [Integration Services]
- automatic package execution
- scheduling packages [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: ecf7a5f9-b8a7-47f1-9ac0-bac07cb89e31
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 353acdd28f3611bcc387bd4122ca377a2d8d0ca5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346862"
---
# <a name="sql-server-agent-jobs-for-packages"></a>Trabajos del Agente SQL Server para paquetes

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Puede automatizar y programar la ejecución de paquetes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] mediante el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Puede programar paquetes que se implementan en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y se almacena en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el almacén de paquetes de [!INCLUDE[ssIS](../../includes/ssis-md.md)] y el sistema de archivos.  
 
> [!NOTE]
> En este artículo se describe cómo programar paquetes SSIS en general y cómo programar paquetes de forma local. Los paquetes SSIS también se pueden ejecutar y programar en las plataformas siguientes:
> - **La nube de Microsoft Azure**. Para obtener más información, vea [Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md) y [Schedule the execution of an SSIS package in Azure (Programar la ejecución de un paquete SSIS en Azure)](../lift-shift/ssis-azure-schedule-packages.md).
> - **Linux**. Para obtener más información, vea [Extraer, transformar y cargar datos en Linux con SSIS](../../linux/sql-server-linux-migrate-ssis.md) y [Schedule SQL Server Integration Services package execution on Linux with cron](../../linux/sql-server-linux-schedule-ssis-packages.md) (Programar la ejecución de paquetes de SQL Server Integration Services en Linux con cron).

## <a name="sections-in-this-topic"></a>Secciones de este tema  
 Este tema contiene las siguientes secciones:  
  
-   [Programar trabajos en el Agente SQL Server](#jobs)  
  
-   [Programar paquetes de Integration Services](#packages)  
  
-   [Solucionar problemas de los paquetes programados](#trouble)  
  
##  <a name="scheduling-jobs-in-sql-server-agent"></a><a name="jobs"></a> Scheduling Jobs in SQL Server Agent  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un servicio instalado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que permite automatizar y programar tareas ejecutando trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe ejecutarse antes de que los trabajos pueden ejecutarse automáticamente. Para obtener más información, consulte [Configure SQL Server Agent](../../ssms/agent/configure-sql-server-agent.md).  
  
 El nodo **Agente SQL Server** aparece en el Explorador de objetos de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] al establecer conexión con una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
 Para automatizar una tarea periódica, se crea un trabajo a través del cuadro de diálogo **Nuevo trabajo** . Para más información, vea [Implementar trabajos](../../ssms/agent/implement-jobs.md).  
  
 Después de crear el trabajo, debe agregar al menos un paso. Un trabajo puede incluir varios pasos, y cada paso puede realizar una tarea distinta. Para más información, consulte [Manage Job Steps](../../ssms/agent/manage-job-steps.md).  
  
 Después de crear el trabajo y los pasos del trabajo, puede crear una programación para ejecutar el trabajo. No obstante, también puede crear un trabajo sin programar que se ejecute manualmente. Para obtener más información, vea [Crear y adjuntar programaciones a trabajos](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
 Puede mejorar el trabajo estableciendo opciones de notificación; por ejemplo, especificando un operador que envíe un mensaje de correo electrónico cuando finalice el trabajo o agregando alertas. Para más información, consulte [Alertas](../../ssms/agent/alerts.md).  
  
##  <a name="scheduling-integration-services-packages"></a><a name="packages"></a> Scheduling Integration Services Packages  
 Después de crear un trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para programar paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , debe agregar al menos un paso y definir el tipo de paso en **Paquete SQL Server Integration Services**. Un trabajo puede incluir varios pasos, y cada paso puede ejecutar un paquete diferente.  
  
 Ejecutar un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] desde un paso de trabajo es como ejecutar un paquete con las utilidades **dtexec** (dtexec.exe) y **DTExecUI** (dtexecui.exe). En lugar de establecer las opciones de tiempo de ejecución de un paquete con opciones de la línea de comandos o con el cuadro de diálogo **Utilidad de ejecución de paquetes** , establezca las opciones de tiempo de ejecución en el cuadro de diálogo **Nuevo paso de trabajo** . Para obtener más información acerca de las opciones para ejecutar un paquete, vea [dtexec Utility](../../integration-services/packages/dtexec-utility.md).  
  
 Para más información, vea [Programar un paquete mediante el Agente SQL Server](#schedule).  
  
 Para ver un vídeo que muestra cómo usar el agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ejecutar un paquete, vea la página principal del vídeo [ Cómo automatizar la ejecución de paquetes SSIS usando el Agente SQL Server (vídeo de SQL Server)](/previous-versions/sql/sql-server-2008/dd440761(v=sql.100)) en MSDN Library.  
  
##  <a name="troubleshooting"></a><a name="trouble"></a> Solucionar problemas  
 Es posible que un paso de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no pueda iniciar un paquete aunque el paquete se ejecute correctamente en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] y desde la línea de comandos. Hay algunos motivos frecuentes para este problema y varias soluciones recomendadas. Para obtener más información, vea los recursos siguientes.  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base, [Un paquete SSIS no se ejecuta al llamarlo desde un paso de trabajo del Agente SQL Server](https://support.microsoft.com/kb/918760)  
  
-   Vídeo [Solucionar problemas de ejecución de paquetes SSIS usando el Agente SQL Server (vídeo de SQL Server)](/previous-versions/sql/sql-server-2008/dd440760(v=sql.100)) en MSDN Library.  
  
 Una vez que un paso de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inicia un paquete, se puede producir un error en la ejecución del paquete o el paquete se puede ejecutar correctamente pero con resultados inesperados. Puede usar las herramientas siguientes para solucionar estos problemas.  
  
-   Para los paquetes almacenados en la base de datos MSDB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el almacén de paquetes de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o una carpeta del equipo local, puede usar el **Visor del archivo de registros** , así como los registros y los archivos de volcado de depuración generados durante la ejecución del paquete.  
  
     **Para usar el Visor del archivo de registros, haga lo siguiente.**  
  
    1.  Haga clic con el botón derecho en el trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el Explorador de objetos y, después, haga clic en **Ver historial**.  
  
    2.  Busque la ejecución del trabajo en el cuadro **Resumen de archivos del registro** que tengan el mensaje **error del trabajo** en la columna **Mensaje** .  
  
    3.  Expanda el nodo de trabajo y haga clic en el paso de trabajo para ver los detalles del mensaje en el área situada debajo del cuadro **Resumen de archivos del registro** .  
  
-   Para los paquetes almacenados en la base de datos de SSISDB, puede usar el **Visor del archivo de registros** , así como los registros y los archivos de volcado de depuración generados durante la ejecución del paquete. También puede usar los informes del servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     **Para buscar información en los informes sobre la ejecución del paquete asociada a la ejecución de un trabajo, haga lo siguiente.**  
  
    1.  Siga los pasos anteriores para ver los detalles del mensaje para el paso de trabajo.  
  
    2.  Busque el identificador de ejecución que aparece en el mensaje.  
  
    3.  Expanda el nodo Catálogos de Integration Services del Explorador de objetos.  
  
    4.  Haga clic con el botón secundario en SSISDB, apunte a Informes y a Informes estándar, y haga clic en Todas las ejecuciones.  
  
    5.  En el informe **Todas las ejecuciones** , busque el identificador de ejecución en la columna **Id.** Haga clic en **Información general**, en **Todos los mensajes** o en **Rendimiento de la ejecución** para ver información sobre la ejecución de este paquete.  
  
    Para obtener más información acerca de los informes Información general, Todos los mensajes y Rendimiento de la ejecución, vea [Reports for the Integration Services Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).  

## <a name="schedule-a-package-by-using-sql-server-agent"></a><a name="schedule"></a> Programar un paquete mediante el Agente SQL Server
  En el procedimiento siguiente se indican los pasos para automatizar la ejecución de un paquete usando un paso de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ejecutar el paquete.  
  
### <a name="to-automate-package-execution-by-using-sql-server-agent"></a>Para automatizar la ejecución de paquetes mediante el Agente SQL Server  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], conéctese a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que desee crear un trabajo o la instancia que contiene el trabajo al que desee agregar un paso.  
  
2.  Expanda el nodo Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el Explorador de objetos y realice una de las tareas siguientes:  
  
    -   Para crear un nuevo trabajo, haga clic con el botón derecho en **Trabajos** y luego haga clic en **Nuevo trabajo**.  
  
    -   Para agregar un paso a un trabajo existente, expanda **Trabajos**, haga clic con el botón derecho en el trabajo y luego haga clic en **Propiedades**.  
  
3.  En la página **General** , si está creando un trabajo, indique el nombre del trabajo, seleccione un propietario y una categoría de trabajo y, opcionalmente, proporcione una descripción del trabajo.  
  
4.  Para hacer que el trabajo esté disponible para programación, seleccione **Habilitado**.  
  
5.  Para crear un paso de trabajo para el paquete que desea programar, haga clic en **Pasos** y, a continuación, haga clic en **Nuevo**.  
  
6.  Seleccione **Paquete de Integración Services** en el tipo de paso de trabajo.  
  
7.  En la lista **Ejecutar como** , seleccione **Cuenta de servicio del Agente SQL Server** o seleccione una cuenta de proxy que tenga las credenciales que el paso de trabajo usará. Para obtener información acerca de cómo crear una cuenta de proxy, vea [Create a SQL Server Agent Proxy](../../ssms/agent/create-a-sql-server-agent-proxy.md).  

    El uso de una cuenta de proxy en lugar la **Cuenta de servicio del Agente SQL Server** puede resolver problemas frecuentes que pueden surgir al ejecutar un paquete mediante el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información sobre estos problemas, vea el artículo de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base [Un paquete SSIS no se ejecuta cuando se le llama desde un paso de trabajo del Agente SQL Server](https://support.microsoft.com/kb/918760). 
     
    - Al ejecutar el trabajo con un proxy, uno debe tener los siguientes elementos de seguridad preparados para que dicho trabajo se ejecute correctamente.

        Inicio de sesión mediante credenciales utilizado por el proxy, la cuenta que ejecuta el Agente SQL Server y la cuenta que ejecuta el servicio SQL Server requieren los permisos siguientes:

        - Atributo de directiva de seguridad local: reemplazo de un token de nivel de proceso
        - Control total sobre %SYSTEMROOT%\Temp 

        Si no pone los elementos de seguridad, se producirá un error en el trabajo y un mensaje de error similar al siguiente: Se produjo un error en el trabajo. El cliente no dispone de un privilegio requerido.

        > **NOTA:** Si cambia la contraseña para la credencial que la cuenta de proxy usa, necesita actualizar la contraseña de la credencial. De lo contrario, se producirá un error en el paso de trabajo.  

        Para más información sobre cómo configurar la cuenta del servicio del Agente SQL Server, vea [Establecer la cuenta de inicio del servicio para el Agente SQL Server &#40;Administrador de configuración de SQL Server &#41;](../../ssms/agent/set-service-startup-account-sql-server-agent-sql-server-configuration-manager.md).  

8.  En el cuadro de lista **Origen del paquete** , haga clic en el origen del paquete y configure después las opciones del paso de trabajo.  
  
    **En la tabla siguiente se describen los orígenes de paquete posibles.**  
  
    |Origen del paquete|Descripción|  
    |--------------------|-----------------|  
    |**Catálogo de SSIS**|Paquetes almacenados en la base de datos de SSISDB. Los paquetes se encuentran en proyectos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] implementados en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
    |**SQL Server**|Paquetes almacenados en la base de datos MSDB. Use el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para administrar estos paquetes.|  
    |**Almacén de paquetes SSIS**|Paquetes almacenados en la carpeta predeterminada del equipo. La carpeta predeterminada es *\<drive>* :\Archivos de programa\Microsoft SQL Server\110\DTS\packages. Use el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para administrar estos paquetes.<br /><br /> Nota: Puede especificar una carpeta diferente o especificar carpetas adicionales del sistema de archivos que se administrarán mediante el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] si modifica el archivo de configuración de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Para más información, vea [Servicio Integration Services &#40;servicio SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).|  
    |**Sistema de archivos**|Paquetes almacenados en cualquier carpeta del equipo local.|  
    |||
  
    **En las tablas siguientes se describen las opciones de configuración disponibles para el paso de trabajo en función del origen del paquete que seleccione.**  
  
    > **IMPORTANTE:** Si el paquete está protegido por contraseña, al hacer clic en cualquiera de las pestañas de la página **General** del cuadro de diálogo **Nuevo paso de trabajo** , salvo la pestaña **Paquete** , necesita escribir la contraseña en el cuadro de diálogo **Contraseña del paquete** que aparecerá. De lo contrario, el trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no podrá ejecutar el paquete.  
  
    **Origen del paquete**: Catálogo de SSIS  
  
    |Pestaña|Opciones|  
    |---------|-------------|  
    |**Package**|**Server**<br /><br /> Escriba o seleccione el nombre de la instancia del servidor de bases de datos que hospeda el catálogo de SSISDB.<br /><br /> Cuando **Catálogo de SSIS** es el origen del paquete, puede iniciar sesión en el servidor usando solo una cuenta de usuario de Microsoft Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no está disponible.|  
    ||**Package**<br /><br /> Haga clic en el botón de puntos suspensivos y seleccione un paquete.<br /><br /> Está seleccionando un paquete de una carpeta bajo el nodo **Catálogos de Integration Services** del **Explorador de objetos**.|  
    |**Parámetros**<br /><br /> Se encuentra en la pestaña **Configuration** .|El **Asistente para conversión de proyectos de Integration Services** permite reemplazar configuraciones de paquetes con parámetros.<br /><br /> La pestaña **Parámetros** muestra los parámetros que agregó al diseñar el paquete, por ejemplo con [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. La pestaña también muestra los parámetros que se agregaron al paquete cuando convirtió el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] del modelo de implementación de paquetes al modelo de implementación del proyecto. Escriba nuevos valores para los parámetros incluidos en el paquete. Puede especificar un valor literal o usar el valor contenido en una variable de entorno del servidor que ya haya asignado al parámetro.<br /><br /> Para escribir el valor literal, haga clic en el botón de puntos suspensivos situado junto a un parámetro. Aparecerá el cuadro de diálogo **Editar valor literal para la ejecución** .<br /><br /> Para usar una variable de entorno, haga clic en **Entorno** y seleccione el entorno que contiene la variable que desea usar.<br /><br /> **\*\* Importante \*\*** Si ha asignado varios parámetros o propiedades del administrador de conexiones a variables contenidas en varios entornos, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mostrará un mensaje de error. Para una ejecución determinada, un paquete solo puede ejecutarse con los valores contenidos en un único entorno de servidor.<br /><br /> Para más información sobre cómo crear un entorno de servidor y asignar una variable a un parámetro, vea [Deploy Integration Services (SSIS) Projects and Packages](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md) (Implementación de proyectos y paquetes de Integration Services [SSIS]).|  
    |**Administradores de conexión**<br /><br /> Se encuentra en la pestaña **Configuration** .|Cambie los valores de las propiedades del administrador de conexiones. Por ejemplo, puede cambiar el nombre del servidor. Se generan automáticamente parámetros en el servidor SSIS para las propiedades del administrador de conexiones. Para cambiar un valor de propiedad, puede especificar un valor literal o usar el valor contenido en una variable de entorno del servidor que ya haya asignado a la propiedad del administrador de conexiones.<br /><br /> Para escribir el valor literal, haga clic en el botón de puntos suspensivos situado junto a un parámetro. Aparecerá el cuadro de diálogo **Editar valor literal para la ejecución** .<br /><br /> Para usar una variable de entorno, haga clic en **Entorno** y seleccione el entorno que contiene la variable que desea usar.<br /><br /> **\*\* Importante \*\*** Si ha asignado varios parámetros o propiedades del administrador de conexiones a variables contenidas en varios entornos, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mostrará un mensaje de error. Para una ejecución determinada, un paquete solo puede ejecutarse con los valores contenidos en un único entorno de servidor.<br /><br /> Para más información sobre cómo crear un entorno de servidor y asignar una variable a una propiedad de un administrador de conexiones, vea [Deploy Integration Services (SSIS) Projects and Packages](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md) (Implementación de proyectos y paquetes de Integration Services [SSIS]).|  
    |**Avanzadas**<br /><br /> Se encuentra en la pestaña **Configuration** .|Configure los siguientes valores adicionales para la ejecución del paquete:|  
    ||**Invalidaciones de propiedad**:<br /><br /> Haga clic en **Agregar** para especificar un nuevo valor para una propiedad del paquete, especifique la ruta de acceso de la propiedad e indique si el valor de propiedad es importante. El servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] cifra la información confidencial. Para editar o quitar la configuración de una propiedad, haga clic en una fila en el cuadro **Invalidaciones de propiedad** y, a continuación, haga clic en **Editar** o en **Quitar**. Puede averiguar la ruta de acceso de la propiedad realizando una de las siguientes acciones.<br /><br /> \- Copie la ruta de acceso de propiedad del archivo de configuración XML (\*.dtsconfig). La ruta de acceso aparece en la sección Configuration, como un valor del atributo Path. A continuación se muestra un ejemplo de la ruta de acceso para la propiedad MaximumErrorCount: \Package.Properties[MaximumErrorCount]<br /><br /> \- Ejecute el **Asistente para la configuración de paquetes** y copie las rutas de acceso de propiedad de la página final **Finalización del asistente** . Después puede cancelar el asistente.<br /><br /> <br /><br /> Nota: La opción **Invalidaciones de propiedad** está pensada para paquetes cuyas configuraciones actualizó desde una versión anterior de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Los paquetes que se crean con [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] y se implementan en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usan parámetros en lugar de configuraciones.|  
    ||**Nivel de registro**<br /><br /> Seleccione uno de los niveles de registro siguientes para la ejecución del paquete. Tenga en cuenta que la selección del nivel de registro **Rendimiento** o **Detallado** puede afectar al rendimiento de la ejecución del paquete.<br /><br /> **Ninguna**:<br />                          El registro está desactivado. Solo se registra el estado de ejecución del paquete.<br /><br /> **Básico**:<br />                          Se registran todos los eventos, excepto los eventos personalizados y de diagnóstico. Es el valor predeterminado para el nivel de registro.<br /><br /> **Rendimiento**:<br />                          Solo se registran las estadísticas de rendimiento, y los eventos OnError y OnWarning.<br /><br /> **Detallado**:<br />                          Se registran todos los eventos, incluidos los eventos personalizados y de diagnóstico.<br /><br /> El nivel de registro que seleccione determinará la información que se muestra en las vistas de SSISDB y en los informes para el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Para más información, consulte [Registro de Integration Services (SSIS)](../../integration-services/performance/integration-services-ssis-logging.md).|  
    ||**Volcado de errores**<br /><br /> Especifique si se generarán archivos de volcado de depuración cuando se produzca algún error durante la ejecución del paquete. Los archivos contienen información sobre la ejecución del paquete que puede ayudarle a solucionar problemas. Si se selecciona esta opción y se produce un error durante la ejecución, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea un archivo .mdmp (archivo binario) y un archivo .tmp (archivo de texto). De forma predeterminada, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] almacena los archivos en la carpeta *\<drive>* :\Archivos de programa\Microsoft SQL Server\110\Shared\ErrorDumps.|  
    ||**Tiempo de ejecución de 32 bits**<br /><br /> Indique si quiere ejecutar el paquete mediante una versión de 32 bits de la utilidad dtexec en un equipo de 64 bits que tenga la versión de 64 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instalado.<br /><br /> Quizás necesite ejecutar el paquete con la versión de 32 bits dtexec si por ejemplo el paquete usa un proveedor OLE DB nativo que no está disponible en una versión de 64 bits. Para obtener más información, vea [Consideraciones sobre 64 bits para Integration Services](https://msdn.microsoft.com/library/ms141766\(SQL.105\).aspx).<br /><br /> De forma predeterminada, al seleccionar el tipo de paso de trabajo **Paquete SQL Server Integration Services** , el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ejecuta el paquete con la versión de la utilidad dtexec que el sistema invoca automáticamente. El sistema invoca la versión de 32 o de 64 bits de la utilidad en función del procesador del equipo, y la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecuta en el equipo.|  
  
     **Origen del paquete**:  SQL Server, el almacén de paquetes SSIS o sistema de archivos  
  
     Muchas de las opciones que se pueden establecer para los paquetes almacenados en SQL Server, el Almacén de paquetes SSIS o el sistema de archivos corresponden a las opciones de la línea de comandos para la utilidad **dtexec** de símbolo del sistema. Para más información sobre la utilidad y las opciones de la línea de comandos, vea [Utilidad dtexec](../../integration-services/packages/dtexec-utility.md).  
  
    |Pestaña|Opciones|  
    |---------|-------------|  
    |**Package**<br /><br /> Estas son las opciones de la pestaña para los paquetes almacenados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en el Almacén de paquetes [!INCLUDE[ssIS](../../includes/ssis-md.md)] .|**Server**<br /><br /> Escriba o seleccione el nombre de la instancia de servidor de bases de datos para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
    ||**Utilizar autenticación de Windows**<br /><br /> Seleccione esta opción para iniciar sesión en el servidor con una cuenta de usuario de Microsoft Windows.|  
    ||**Utilizar autenticación de SQL Server**<br /><br /> Cuando un usuario se conecta con un nombre y una contraseña de inicio de sesión determinados desde una conexión no de confianza, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza la autenticación y comprueba si se ha configurado una cuenta de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y si la contraseña especificada coincide con la almacenada anteriormente. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no encuentra la cuenta de inicio de sesión, la autenticación no se realizará correctamente y el usuario recibirá un mensaje de error.|  
    ||**Nombre de usuario**|  
    ||**Contraseña**|  
    ||**Package**<br /><br /> Haga clic en el botón de puntos suspensivos y seleccione el paquete.<br /><br /> Está seleccionando un paquete de una carpeta bajo el nodo **Paquetes almacenados** del **Explorador de objetos**.|  
    |**Package**<br /><br /> Estas son las opciones de la pestaña para los paquetes almacenados en el sistema de archivos.|**Package**<br /><br /> Escriba la ruta de acceso completa para el archivo de paquete o haga clic en el botón de puntos suspensivos para seleccionar el paquete.|  
    |**Configuraciones**|Agregue un archivo de configuración XML para ejecutar el paquete con una configuración concreta. Use una configuración de paquete para actualizar los valores de las propiedades de los paquetes en tiempo de ejecución.<br /><br /> Esta opción corresponde a la opción **/ConfigFile** para **dtexec**.<br /><br /> Para entender cómo se aplican las configuraciones de paquete, vea [Package Configurations](./legacy-package-deployment-ssis.md). Para obtener información sobre cómo crear una configuración de paquete, vea [Create Package Configurations](./legacy-package-deployment-ssis.md).|  
    |**Archivos de comandos**|Especifique opciones adicionales que desee ejecutar con **dtexec**, en un archivo independiente.<br /><br /> Por ejemplo, puede incluir un archivo que contenga la opción /Dump *errorcode* para generar archivos de volcado de depuración cuando se produzcan uno o varios eventos especificados mientras se ejecuta el paquete.<br /><br /> Puede ejecutar un paquete con diferentes conjuntos de opciones si crea varios archivos y después especifica el archivo adecuado mediante la opción **Archivos de comandos** .<br /><br /> La opción **Archivos de comandos** corresponde a la opción **/CommandFile** para **dtexec**.|  
    |**Orígenes de datos**|Vea los administradores de conexiones incluidos en el paquete. Para modificar una cadena de conexión, haga clic en el administrador de conexiones y, después, haga clic en la cadena de conexión.<br /><br /> Esta opción corresponde a la opción **/Connection** para **dtexec**.|  
    |**Opciones de ejecución**|**Rechazar el paquete cuando haya advertencias de validación**<br /> Indica si un mensaje de advertencia se considera un error. Si selecciona esta opción y se produce una advertencia durante la validación, se producirá un error en el paquete durante la validación. Esta opción corresponde a la opción **/WarnAsError** para **dtexec**.<br /><br /> **Validar el paquete sin ejecutarlo**<br /> Indica si se detiene la ejecución del paquete después de la fase de validación sin ejecutar realmente el paquete. Esta opción corresponde a la opción **/Validate** para **dtexec**.<br /><br /> **Invalidar propiedad MacConcurrentExecutables**<br /> Especifica el número de archivos ejecutables que el paquete puede ejecutar simultáneamente. El valor -1 significa que el paquete puede ejecutar un número máximo de archivos ejecutables igual al número total de procesadores del equipo que ejecuta el paquete, más dos. Esta opción corresponde a la opción **/MaxConcurrent** para **dtexec**.<br /><br /> **Habilitar puntos de comprobación de paquetes**<br /> Indica si el paquete usará puntos de comprobación durante la ejecución del paquete. Para obtener más información, vea [Restart Packages by Using Checkpoints](../../integration-services/packages/restart-packages-by-using-checkpoints.md).<br /><br /> Esta opción corresponde a la opción **/CheckPointing** para **dtexec**.<br /><br /> **Omitir opciones de reinicio**<br /> Indica si se establece un nuevo valor para la propiedad **CheckpointUsage** del paquete. Seleccione un valor del cuadro de lista **Opción de reinicio** .<br /><br /> Esta opción corresponde a la opción **/Restart** para **dtexec**.<br /><br /> **Usar motor en tiempo de ejecución de 32 bits**<br /> Indique si quiere ejecutar el paquete mediante una versión de 32 bits de la utilidad dtexec en un equipo de 64 bits que tenga la versión de 64 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instalado.<br /><br /> Quizás necesite ejecutar el paquete con la versión de 32 bits dtexec si por ejemplo el paquete usa un proveedor OLE DB nativo que no está disponible en una versión de 64 bits. Para obtener más información, vea [Consideraciones sobre 64 bits para Integration Services](https://msdn.microsoft.com/library/ms141766\(SQL.105\).aspx).<br /><br /> De forma predeterminada, al seleccionar el tipo de paso de trabajo **Paquete SQL Server Integration Services** , el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ejecuta el paquete con la versión de la utilidad dtexec que el sistema invoca automáticamente. El sistema invoca la versión de 32 o de 64 bits de la utilidad en función del procesador del equipo, y la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecuta en el equipo.|  
    |**Logging**|Asocie un proveedor de registro con la ejecución del paquete.<br /><br /> **Proveedor de registro SSIS para archivos de texto**<br /> Escribe entradas de registro en archivos de texto ASCII<br /><br /> **Proveedor de registro SSIS para SQL Server**<br /> Escribe entradas de registro en la tabla sysssislog de la base de datos MSDB.<br /><br /> **Proveedor de registro SSIS para SQL Server Profiler**<br /> Escribe seguimientos que puede ver mediante SQL Server Profiler.<br /><br /> **Proveedor de registro SSIS para el Registro de eventos de Windows**<br /> Escribe entradas de registro en el registro de la aplicación del registro de eventos de Windows.<br /><br /> **Proveedor de registro SSIS para archivos XML**<br /> Escribe archivos de registro en un archivo XML.<br /><br /> Para el archivo de texto, el archivo XML y los proveedores de registro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler, está seleccionando los administradores de conexiones de archivos incluidos en el paquete. Para el proveedor de registro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , está seleccionando un administrador de conexiones OLE DB incluidos en el paquete.<br /><br /> Esta opción corresponde a la opción **/Logger** para **dtexec**.|  
    |**Valores establecidos**|Invalide una configuración de propiedad del paquete. En el cuadro **Propiedades** , escriba valores en las columnas **Ruta de acceso de la propiedad** y **Valor** . Después de especificar los valores para una propiedad, aparece una fila vacía en el cuadro **Propiedades** para que pueda especificar valores para otra propiedad.<br /><br /> Para quitar una propiedad del cuadro Propiedades, haga clic en la fila y, a continuación, haga clic en **Quitar**.<br /><br /> Puede averiguar la ruta de acceso de la propiedad realizando una de las siguientes acciones.<br /><br /> \- Copie la ruta de acceso de propiedad del archivo de configuración XML (\*.dtsconfig). La ruta de acceso aparece en la sección Configuration, como un valor del atributo Path. A continuación se muestra un ejemplo de la ruta de acceso para la propiedad MaximumErrorCount: \Package.Properties[MaximumErrorCount]<br /><br /> \- Ejecute el **Asistente para la configuración de paquetes** y copie las rutas de acceso de propiedad de la página final **Finalización del asistente** . Después puede cancelar el asistente.|  
    |**Comprobación**|**Ejecutar solo los paquetes firmados**<br /> Indica si se comprueba la firma del paquete. Si el paquete no está firmado o la firma no es válida, se produce un error en el paquete. Esta opción corresponde a la opción **/VerifySigned** para **dtexec**.<br /><br /> **Comprobar la compilación del paquete**<br /> Indica si el número de compilación del paquete se comprueba con el número de compilación especificado en el cuadro **Compilación** situado junto a esta opción. Si se produce una discrepancia, el paquete no se ejecuta. Esta opción corresponde a la opción **/VerifyBuild** para **dtexec**.<br /><br /> **Comprobar el Id. del paquete**<br /> Indica si se comprueba el GUID del paquete comparándolo con el identificador de paquete especificado en el cuadro **Id. del paquete** situado junto a esta opción. Esta opción corresponde a la opción **/VerifyPackageID** para **dtexec**.<br /><br /> **Comprobar el Id. de versión**<br /> Indica si se comprueba el GUID de versión del paquete comparándolo con el identificador de versión especificado en el cuadro **Id. de versión** situado junto a esta opción. Esta opción corresponde a la opción **/VerifyVersionID** para **dtexec**.|  
    |**Línea de comandos**|Modifique las opciones de línea de comandos para dtexec. Para obtener más información acerca de las opciones, vea [dtexec Utility](../../integration-services/packages/dtexec-utility.md).<br /><br /> **Restaurar las opciones originales**<br /> Use las opciones de la línea de comandos que ha establecido en las pestañas **Paquete**, **Configuraciones**, **Archivos de comandos**, **Orígenes de datos**, **Opciones de ejecución**, **Registro**, **Valores establecidos** y **Comprobación** del cuadro de diálogo **Propiedades del paso de trabajo** .<br /><br /> **Editar el comando manualmente**<br /> Escriba opciones de la línea de comandos adicionales en el cuadro **Línea de comandos** .<br /><br /> Antes de hacer clic en **Aceptar** para guardar los cambios en el paso de trabajo, puede quitar todas las opciones adicionales que ha escrito en el cuadro **Línea de comandos** si hace clic en **Restaurar las opciones originales**.<br /><br /> **\*\* Sugerencia \*\*** Puede copiar la línea de comandos en una ventana de símbolo del sistema, agregar `dtexec`y ejecutar el paquete desde la línea de comandos. Esta es una forma sencilla de generar el texto de la línea de comandos.|  
  
9. Haga clic en **Aceptar** para guardar la configuración y cierre el cuadro de diálogo **Nuevo paso de trabajo** .  
  
    > [!NOTE]
    > En los paquetes almacenados en el **Catálogo de SSIS**, el botón **Aceptar** está deshabilitado cuando hay un parámetro o una configuración de propiedad del administrador de conexiones sin resolver. Se produce una configuración sin resolver cuando se usa un valor contenido en una variable de entorno del servidor para establecer el parámetro o la propiedad y se cumple una de las condiciones siguientes:  
    >   
    >  La casilla **Entorno** de la pestaña **Configuración** no está activada.  
    >   
    >  El entorno del servidor que contiene la variable no está seleccionado en el cuadro de lista de la pestaña **Configuración** .  
  
10. Para crear una programación para un paso trabajo, haga clic en **Programaciones** en el panel **Seleccionar una página** . Para obtener información sobre cómo configurar una programación, vea [Schedule a Job](../../ssms/agent/schedule-a-job.md).  
  
    > [!TIP]  
    >  Cuando asigne nombre a la programación, considere la posibilidad de usar un nombre que sea único y descriptivo para que pueda distinguir más fácilmente la programación de otras programación del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

## <a name="see-also"></a>Consulte también  
 [Execution of Projects and Packages](deploy-integration-services-ssis-projects-and-packages.md) (Ejecución de proyectos y paquetes)  

## <a name="external-resources"></a>Recursos externos  
  
-   Artículo de Knowledge Base, [Un paquete SSIS no se ejecuta al llamarlo desde un paso de trabajo del Agente SQL Server](https://support.microsoft.com/kb/918760), en el sitio web de [!INCLUDE[msCoName](../../includes/msconame-md.md)]  
  
-   Vídeo [Solucionar problemas de ejecución de paquetes SSIS usando el Agente SQL Server (vídeo de SQL Server)](/previous-versions/sql/sql-server-2008/dd440760(v=sql.100)) en MSDN Library  
  
-   Vídeo [ Cómo automatizar la ejecución de paquetes SSIS usando el Agente SQL Server (vídeo de SQL Server)](/previous-versions/sql/sql-server-2008/dd440761(v=sql.100)) en MSDN Library  
  
-   Artículo técnico relativo a la [comprobación de trabajos del Agente SQL Server mediante Windows PowerShell](https://go.microsoft.com/fwlink/?LinkId=165675), en mssqltips.com  
  
-   Artículo técnico relativo a la [alerta automática de los trabajos del Agente SQL cuando están habilitados o deshabilitados](https://go.microsoft.com/fwlink/?LinkId=165676), en mssqltips.com  
  
-   Entrada de blog, [Configuring SQL Agent Jobs to Write to Windows Event Log](https://go.microsoft.com/fwlink/?LinkId=220745), en mssqltips.com.  
  
