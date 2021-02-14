---
title: Instalación de SQL Server 2016 R Services
titleSuffix: ''
description: Obtenga información sobre cómo instalar SQL Server 2016 R Services en Windows. Puede usar R Services para ejecutar scripts de R en la base de datos.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: contperf-fy20q4
monikerRange: =sql-server-2016
ms.openlocfilehash: 56efa5151e4d72019cb126c69e3d3138829dbbd2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082486"
---
# <a name="install-sql-server-2016-r-services"></a>Instalación de SQL Server 2016 R Services

[!INCLUDE[SQL Server 2016 only](../../includes/applies-to-version/sqlserver2016-only.md)]

Obtenga información sobre cómo instalar SQL Server 2016 R Services en Windows. Puede usar R Services para ejecutar scripts de R en la base de datos.

> [!NOTE]
> En SQL Server 2017 y versiones posteriores, R se incluye en [Machine Learning Services](../sql-server-machine-learning-services.md) junto con Python. Si quiere R y tiene SQL Server 2017 o versiones posteriores, consulte [Instalación de SQL Server Machine Learning Services](sql-machine-learning-services-windows-install.md) para agregar la característica.

<a name="bkmk_prereqs"></a>

## <a name="pre-install-checklist"></a>Lista de comprobación previa a la instalación

+ Se necesita una instancia del motor de base de datos. No se puede instalar solo R, aunque se puede agregar incrementalmente a una instancia existente.

+ De cara a la continuidad empresarial, se admiten [Grupos de disponibilidad AlwaysOn](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) para R Services. Debe instalar R Services y configurar los paquetes en cada nodo.

+ No instale R Services en una Instancia de clúster de conmutación por error (FCI) de SQL Server Always On. El mecanismo de seguridad que se usa para aislar los procesos de R no es compatible con un entorno de instancia de clúster de conmutación por error (FCI) de SQL Server Always On.

+ No instale R Services en un controlador de dominio. Se producirá un error en la parte de la instalación de R Services.

+ No instale **Características compartidas** > **R Server (independiente)** en el mismo equipo en el que se ejecuta una instancia en base de datos.

+ Se admite la instalación en paralelo con otras versiones de R, pero no se recomienda. Se admite porque la instancia de SQL Server usa sus propias copias de las distribuciones de R de código abierto. Sin embargo, la ejecución de código que usa R en el equipo con SQL Server fuera de SQL Server puede provocar varios problemas:

  + Se usa una biblioteca y un ejecutable diferentes y se obtienen resultados diferentes que cuando se ejecuta en SQL Server.
  + SQL Server no puede administrar los scripts de R que se ejecutan en bibliotecas externas, lo que produce la contención de recursos.
  
> [!IMPORTANT]
> Una vez finalizada la instalación, asegúrese de completar los pasos adicionales posteriores a la configuración que se describen en este artículo. En estos pasos se incluye la habilitación de SQL Server para usar scripts externos y la adición de cuentas necesarias para que SQL Server ejecute trabajos de R en su nombre. Normalmente, si se realizan cambios en la configuración, es necesario reiniciar la instancia o el servicio Launchpad.

## <a name="get-the-installation-media"></a>Obtener los medios de instalación

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

<a name="bkmk_ga_instalpatch"></a>

### <a name="install-patch-requirement"></a>Requisito de instalación de revisión

Microsoft ha identificado un problema con la versión concreta de los archivos binarios en tiempo de ejecución de Microsoft VC++ 2013 que instala como requisito previo SQL Server. Si esta actualización de los archivos binarios en tiempo de ejecución de VC++ no se instala, puede que SQL Server experimente problemas de estabilidad en determinados escenarios. Antes de instalar SQL Server, siga las instrucciones de [Notas de la versión de SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) para ver si el equipo necesita una revisión para los archivos binarios en tiempo de ejecución de VC.  

<a name="bkmk2016top"></a>

## <a name="run-setup"></a>Ejecución de la configuración

En instalaciones locales, debe ejecutar el programa de instalación como administrador. Si instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde un recurso compartido remoto, deberá usar una cuenta de dominio que tenga permisos de lectura y ejecución para dicho recurso.

1. Inicie el asistente para la instalación de SQL Server 2016.

1. En la pestaña **Instalación**, seleccione **Nueva instalación independiente de SQL Server o agregar características a una instalación existente**.

    ![Instalación de R Services (en bases de datos)](media/2016-setup-installation-rsvcs.png "Inicio de la instalación del motor de base de datos con R Services")

1. En la página **Selección de características**, seleccione las siguientes opciones:

    + Seleccione **Servicios de Motor de base de datos**. El motor de base de datos es necesario en cada instancia que usa el aprendizaje automático.
    + Seleccione **R Services (en bases de datos)** . Instalar compatibilidad con el uso en la base de datos de R.

    ![Selección de características de R Services](media/2016setup-rsvcs-features.png "Selección de estas características para R Services en la base de datos")

    > [!IMPORTANT]
    > No instale R Server y R Services al mismo tiempo. 

1. En la página **Consentimiento para instalar Microsoft R Open**, haga clic en **Aceptar**.
  
    Este contrato de licencia es necesario para descargar Microsoft R Open, que incluye una distribución de las herramientas y paquetes base de R de código abierto, junto con proveedores de conectividad y paquetes de R mejorados del equipo de desarrollo de Microsoft R.
  
1. Una vez aceptado el contrato de licencia, habrá una breve pausa mientras se prepara el instalador. Haga clic en **Siguiente** cuando el botón se encuentre disponible.

1. En la página **Listo para instalar**, confirme que se incluyen los siguientes elementos y, a continuación, seleccione **Instalar**.

    + Servicios de Motor de base de datos
    + R Services (en bases de datos)

1. Cuando finalice la instalación, si el programa indica que se reinicie el equipo, hágalo. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información, vea [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="set-environment-variables"></a>Establecimiento de variables de entorno

Solo de cara a la integración de características de R, conviene establecer la variable de entorno **MKL_CBWR** para [garantizar una salida coherente](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) de los cálculos de la biblioteca Math Kernel Library (MKL) de Intel.

1. En el panel de control, haga clic en **Sistema y seguridad** > **Sistema** > **Configuración avanzada del sistema** > **Variables de entorno**.

1. Cree un usuario o una variable del sistema.

    + Establezca el nombre de la variable en `MKL_CBWR`.
    + Establezca el valor de la variable en `AUTO`.

Este paso requiere el reinicio del servidor. Puede posponer el reinicio hasta que se realice todo el trabajo de configuración.

<a name="bkmk_enableFeature"></a>

##  <a name="enable-script-execution"></a>Habilitación de la ejecución de scripts

1. Abra [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) o [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

1. Conéctese a la instancia en la que se ha instalado R Services, haga clic en **Nueva consulta** para abrir una ventana de consulta y ejecute el comando siguiente:

   ```sql
   sp_configure
   ```

    El valor de la propiedad, `external scripts enabled`, debería ser **0** en este momento. Esto se debe a que la característica está desactivada de forma predeterminada. Un administrador debe habilitar explícitamente la característica para poder ejecutar scripts de R.

1. Para habilitar la característica de scripting externo, ejecute la siguiente instrucción:
  
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
  
## <a name="restart-the-service"></a>Reinicie el servicio.

Cuando se haya completado la instalación, reinicie el motor de base de datos antes de continuar con lo siguiente para habilitar la ejecución de scripts.

Al reiniciar el servicio, también se reiniciará automáticamente el servicio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] relacionado.

Para reiniciar el servicio, puede hacer clic con el botón derecho en el comando **Reiniciar** de la instancia en SSMS o emplear el [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md).

## <a name="verify-installation"></a>Comprobar la instalación

Haga lo siguiente para comprobar que se están ejecutando todos los componentes que se usan para iniciar el script externo.

1. En SQL Server Management Studio, abra una nueva ventana de consulta y ejecute el siguiente comando:

    ```sql
    EXEC sp_configure 'external scripts enabled'
    ```

    **run_value** debería estar establecido ahora en 1.

1. Abra el Administrador de configuración de SQL Server y compruebe que el **servicio SQL Server Launchpad** se está ejecutando. Debe tener un servicio para cada instancia del motor de base de datos que tenga instalado R. Para más información sobre el servicio, vea [Marco de extensibilidad](../concepts/extensibility-framework.md).

1. Si Launchpad se está ejecutando, debería poder ejecutar scripts sencillos de R para comprobar que los tiempos de ejecución de scripting externo pueden comunicarse con SQL Server.

    Abra una nueva ventana de **consulta** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o en Azure Data Studio y ejecute un script como el siguiente:

    ```sql
    EXEC sp_execute_external_script  @language =N'R',
    @script=N'
    OutputDataSet <- InputDataSet;
    ',
    @input_data_1 =N'SELECT 1 AS hello'
    WITH RESULT SETS (([hello] int not null));
    GO
    ```

    El script puede tardar un poco en ejecutarse la primera vez que se carga el tiempo de ejecución del script externo. Los resultados deben tener un aspecto similar al siguiente:

    | hello |
    |----|
    | 1|

<a name="apply-cu"></a>

## <a name="apply-updates"></a>Aplicación de actualizaciones

Se recomienda aplicar el último Service Pack y la última actualización acumulativa tanto en el motor de base de datos como en los componentes de aprendizaje automático.

En los dispositivos conectados a Internet, las actualizaciones acumulativas suelen aplicarse a través de Windows Update, pero también puede usar los pasos siguientes para las actualizaciones controladas. Al aplicar la actualización para el motor de base de datos, el programa de instalación extrae las actualizaciones acumulativas de las bibliotecas de R instaladas en la misma instancia. 

En los servidores desconectados, se requieren pasos extra. Para obtener más información, consulte [Instalación en equipos sin acceso a Internet > Aplicación de actualizaciones acumulativas](sql-ml-component-install-without-internet-access.md#apply-cu).

1. Comience con una instancia de línea base ya instalada: Versión inicial de SQL Server 2016, SQL Server 2016 SP1 o SQL Server 2016 SP2.

1. Vaya a la lista de actualizaciones acumulativas: [Actualizaciones más recientes de Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

1. Seleccione el Service Pack más reciente (si no está instalado todavía como instancia de línea de base) y la actualización acumulativa. Se descarga un ejecutable que se extrae automáticamente.

1. Ejecute el programa de instalación. Acepte los términos de licencia y, en la página de selección de características, revise las características para las que se aplican las actualizaciones acumulativas. Debería ver todas las características instaladas para la instancia actual, incluido R Services. El programa de instalación descarga los archivos CAB necesarios para actualizar todas las características.

1. Continúe con los pasos del asistente y acepte los términos de licencia para la distribuciones de R.

> [!NOTE]
> La actualización acumulativa (CU) 14 y versiones posteriores para SQL Server 2016 SP2 incluye una versión más reciente del entorno de ejecución de R. Para más información, consulte [Cambio a la versión predeterminada del entorno de ejecución del lenguaje](change-default-language-runtime-version.md).

<a name="bkmk_FollowUp"></a>

## <a name="additional-configuration"></a>Configuración adicional

Si el paso de comprobación de scripts externos se ha ejecutado correctamente, puede ejecutar comandos de R desde SQL Server Management Studio, Azure Data Studio o cualquier otro cliente que pueda enviar instrucciones T-SQL al servidor.

Si se produjo un error al ejecutar el comando, revise los pasos de configuración adicional de esta sección. Es posible que tenga que crear otras configuraciones adicionales adecuadas para el servicio o la base de datos.

En el nivel de instancia, la configuración adicional podría incluir:

* [Configuración de firewall para SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md).
* [Habilitación de protocolos de red adicionales](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).
* [Habilitación de conexiones remotas](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md).
* [Administración de cuotas de disco](/windows/desktop/fileio/managing-disk-quotas) para evitar que los scripts externos ejecuten tareas que agoten el espacio en disco.

<a name="bkmk_configureAccounts"></a>
<a name="bkmk_AllowLogon"></a>

En la base de datos, puede que necesite las siguientes actualizaciones de configuración:

* [Concesión de permiso a los usuarios para SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md)
* [Agregar SQLRUserGroup como usuario de base de datos](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

> [!NOTE]
> No se requieren todos los cambios enumerados y no es necesario ninguno de ellos. Los requisitos dependen del esquema de seguridad, del lugar en el que se haya instalado SQL Server y de cómo se espera que los usuarios se conecten a la base de datos y ejecuten scripts externos. Se pueden encontrar instrucciones de instalación adicionales aquí: [Instalación de SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md)

## <a name="suggested-optimizations"></a>Optimizaciones sugeridas

Puede que también le interese optimizar el servidor para admitir el aprendizaje automático con R o instalar modelos previamente entrenados.

### <a name="add-more-worker-accounts"></a>Adición de más cuentas profesionales

Si cree que puede hacer un uso intensivo de R, o si prevé que muchos usuarios ejecuten scripts al mismo tiempo, puede aumentar el número de cuentas de trabajo que están asignadas al servicio Launchpad. Para obtener más información, vea [Escalar la ejecución simultánea de scripts externos en SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

<a name="bkmk_optimize"></a>

### <a name="optimize-the-server-for-external-script-execution"></a>Optimización del servidor para la ejecución de scripts externos

La configuración predeterminada del programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está diseñada para optimizar el equilibrio del servidor para diversos servicios que el motor de base de datos admite, entre otros, procesos de extracción, transformación y carga de datos (ETL), creación de informes, auditoría y aplicaciones que usan datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por lo tanto, en la configuración predeterminada, podría ver que los recursos para el aprendizaje automático están restringidos a veces, especialmente en operaciones que usan mucha memoria.

Para asegurarse de que se asignen a los trabajos de aprendizaje automático la prioridad y los recursos correctos, se recomienda usar Resource Governor de SQL Server para configurar un grupo de recursos externos. Puede que también le interese cambiar la cantidad de memoria asignada al motor de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o aumentar el número de cuentas que se ejecutan en el servicio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)].

- Para configurar un grupo de recursos para la administración de recursos externos, vea [Creación de un grupo de recursos externos](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
- Para cambiar la cantidad de memoria reservada para la base de datos, vea [Opciones de configuración de memoria del servidor](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
- Para cambiar el número de cuentas de R que se pueden iniciar mediante [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)], consulte [Escalar la ejecución simultánea de scripts externos en SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

Si usa la edición Standard Edition y no dispone de Resource Governor, puede usar vistas de administración dinámica (DMV) y eventos extendidos, así como la supervisión de eventos de Windows, como ayuda para administrar los recursos de servidor que usa R.

### <a name="install-additional-r-packages"></a>Instalación de paquetes adicionales de R

Las soluciones de R que cree para SQL Server pueden llamar a funciones básicas de R, funciones de los paquetes de su propiedad instalados con SQL Server y paquetes de R de terceros compatibles con la versión de R de código abierto instalada por SQL Server.

Los paquetes que quiera usar de SQL Server deben estar instalados en la biblioteca predeterminada que la instancia usa. Si tiene una instalación independiente de R en el equipo o si ha instalado paquetes en las bibliotecas de usuario, no podrá usar esos paquetes desde T-SQL.

El proceso de instalación y administración de paquetes de R es diferente en SQL Server 2016 y SQL Server 2017. En SQL Server 2016, un administrador de bases de datos debe instalar los paquetes de R que necesitan los usuarios. En SQL Server 2017 puede configurar grupos de usuarios para compartir paquetes en cada nivel de base de datos, o bien configurar roles de base de datos para permitir que los usuarios instalen sus propios paquetes. Para obtener más información, vea [Instalación de paquetes con herramientas de R](../package-management/install-r-packages-standard-tools.md).

## <a name="next-steps"></a>Pasos siguientes

Los desarrolladores de R pueden empezar con algunos ejemplos sencillos y conocer los aspectos básicos del funcionamiento de R con SQL Server. Para conocer el siguiente paso, vea los vínculos siguientes:

+ [Inicio rápido: Ejecutar R en T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Tutoriales de R para aprendizaje automático de SQL](../tutorials/r-tutorials.md)
+ [Documentación del aprendizaje automático en SQL](../index.yml)