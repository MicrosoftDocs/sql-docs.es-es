---
title: Instalación en Windows
description: Obtenga información sobre cómo instalar SQL Server Machine Learning Services en Windows. Puede usar Machine Learning Services para ejecutar scripts de Python y R en la base de datos.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/17/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: 44993ae527e99d615a9be620e3d930aa520c8980
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839516"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-windows"></a>Instalación de SQL Server Machine Learning Services (Python y R) en Windows

[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

Obtenga información sobre cómo instalar SQL Server Machine Learning Services en Windows. Puede usar Machine Learning Services para ejecutar scripts de Python y R en la base de datos.

## <a name="pre-install-checklist"></a><a name="bkmk_prereqs"> </a> Lista de comprobación previa a la instalación

+ Se necesita una instancia del motor de base de datos. No se pueden instalar características solo de Python o de R, aunque se pueden agregar incrementalmente a una instancia existente.

+ Para la continuidad empresarial, se admiten [Grupos de disponibilidad AlwaysOn](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) para Machine Learning Services. Instale Machine Learning Services y configure los paquetes en cada nodo.

+ *No se admite* la instalación de Machine Learning Services en una [instancia de clúster de conmutación por error (FCI) de Always On](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md) en SQL Server 2017. Es compatible con SQL Server 2019 y versiones posteriores.
 
+ No instale Machine Learning Services en un controlador de dominio. Se producirá un error en la parte de la instalación de Machine Learning Services.

+ No instale **Características compartidas** > **Machine Learning Server (independiente)** en el mismo equipo en el que se ejecuta una instancia de base de datos. Un servidor independiente competirá por los mismos recursos, lo que disminuirá el rendimiento de ambas instalaciones.

+ Se admite la instalación en paralelo con otras versiones de Python y R, pero no se recomienda. Se admite porque la instancia de SQL Server usa sus propias copias de las distribuciones de R y Anaconda de código abierto. Sin embargo, no se recomienda porque la ejecución de código que usa Python y R en el equipo con SQL Server fuera de SQL°Server puede provocar varios problemas:
    
  + El uso de una biblioteca y archivos ejecutables distintos creará resultados incoherentes que los que se ejecutan en SQL Server.
  + SQL Server no puede administrar los scripts de R y Python que se ejecutan en bibliotecas externas, lo que produce la contención de recursos.

::: moniker range=">=sql-server-ver15"
> [!NOTE]
> Machine Learning Services se instala de forma predeterminada en **clústeres de macrodatos de SQL Server**. Si usa un **clúster de macrodatos**, no es necesario que siga los pasos de este artículo. Para más información, vea [Uso de Machine Learning Services (Python y R) en Clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

> [!IMPORTANT]
> Una vez finalizada la instalación, asegúrese de completar los pasos posteriores a la configuración que se describen en este artículo. En estos pasos se incluye la habilitación de SQL Server para usar scripts externos y la adición de cuentas necesarias para que SQL Server ejecute trabajos de R y Python en su nombre. Habitualmente, si se realizan cambios en la configuración, es necesario reiniciar la instancia o el servicio Launchpad.

## <a name="get-the-installation-media"></a>Obtener los medios de instalación

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

::: moniker range="=sql-server-2017"
Para más información sobre qué ediciones de SQL Server admiten la integración de Python y R con Machine Learning Services, consulte [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).
::: moniker-end

::: moniker range="=sql-server-ver15"
Para más información sobre qué ediciones de SQL Server admiten la integración de Python y R con Machine Learning Services, consulte [Ediciones y características admitidas de SQL Server 2019 (15.x)](../../sql-server/editions-and-components-of-sql-server-version-15.md).
::: moniker-end

## <a name="run-setup"></a>Ejecución de la configuración

En las instalaciones locales debe ejecutar el programa de instalación como administrador. Si instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde un recurso compartido remoto, deberá usar una cuenta de dominio que tenga permisos de lectura y ejecución para dicho recurso.

1. Inicie el asistente para la instalación de SQL Server.
  
1. En la pestaña **Instalación**, seleccione **Nueva instalación independiente de SQL Server o agregar características a una instalación existente**.

   ::: moniker range="=sql-server-2017"
   ![Nueva instalación independiente de SQL Server](media/2017setup-installation-page-mlsvcs.png)
   ::: moniker-end

   ::: moniker range="=sql-server-ver15"
   ![Nueva instalación independiente de SQL Server](media/2019setup-installation-page-mlsvcs.png)
   ::: moniker-end

1. En la página **Selección de características** , seleccione estas opciones:

   ::: moniker range="=sql-server-2017"

   - **Servicios de Motor de base de datos**
     
     Para usar R y Python con SQL Server, debe instalar una instancia del motor de base de datos. Puede usar una instancia predeterminada o una con nombre.

   - **Machine Learning Services (en base de datos)**
     
     Esta opción instala los servicios de base de datos que admiten la ejecución de scripts de R y Python.

   ::: moniker-end

   ::: moniker range="=sql-server-ver15"

   - **Servicios de Motor de base de datos**
     
     Para usar R o Python con SQL Server, debe instalar una instancia del motor de base de datos. Puede usar una instancia predeterminada o una con nombre.

   - **Machine Learning Services (en base de datos)**
     
     Esta opción instala los servicios de base de datos que admiten la ejecución de scripts de R y Python.

   ::: moniker-end

   - **R**
     
     Active esta opción para agregar los paquetes de Microsoft R, el intérprete y R de código abierto. 
     
   - **Python**
     
     Active esta opción para agregar los paquetes de Microsoft Python, el archivo ejecutable de Python 3.5 y bibliotecas seleccionadas de la distribución de Anaconda.
     
   ::: moniker range="=sql-server-ver15"
   Para información sobre la instalación y el uso de Java, consulte [Instalación de extensiones de lenguaje de SQL Server en Windows](../../language-extensions/install/windows-java.md).
   ::: moniker-end
   
   ::: moniker range="=sql-server-2017"
   ![Opciones de características para R y Python](media/2017setup-features-page-mls-rpy.PNG "Opciones de instalación para R y Python")
   ::: moniker-end
   
   ::: moniker range="=sql-server-ver15"
   ![Opciones de características para R y Python](media/2019setup-features-page-mls-rpy.png "Opciones de instalación para R y Python")
   ::: moniker-end
   
   > [!NOTE]
   > 
   > No seleccione la opción para **Machine Learning Server (independiente)** . La opción de instalar Machine Learning Server incluida en **Características compartidas** está pensada para su uso en un equipo independiente.

::: moniker range="=sql-server-2017"

4. En la página **Consentimiento para instalar Microsoft R Open**, seleccione **Aceptar** y **Siguiente**. 

El contrato de licencia abarca:
+ Microsoft R Open
+ Herramientas y paquetes base de R de código abierto
+ Paquetes de R y proveedores de conectividad mejorados del equipo de desarrollo de Microsoft.

1. En la página **Consentimiento de instalación de Python**, seleccione **Aceptar** y **Siguiente**. El contrato de licencia de código abierto de Python también cubre Anaconda y herramientas relacionadas, además de algunas bibliotecas nuevas de Python del equipo de desarrollo de Microsoft.

   > [!NOTE]
   >  Si el equipo que está usando no tiene acceso a Internet, puede pausar la instalación en este punto y descargar los instaladores por separado. Para obtener más información, consulte [Instalación de componentes de aprendizaje automático sin acceso a Internet](../install/sql-ml-component-install-without-internet-access.md).

1. En la página **Listo para instalar**, confirme que estas selecciones se han realizado y haga clic en **Instalar**.
  
   + Servicios de Motor de base de datos
   + Machine Learning Services (en base de datos)
   + R o Python, o ambos

   Tome nota de la ubicación de la carpeta en la ruta de acceso `..\Setup Bootstrap\Log` donde se almacenan los archivos de configuración. Una vez que se haya completado la instalación, podrá revisar los componentes instalados en el archivo de resumen.

1. Cuando finalice la instalación, si el programa indica que se reinicie el equipo, hágalo. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información, vea [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

::: moniker-end

::: moniker range="=sql-server-ver15"

1. En la página **Consentimiento para instalar Microsoft R Open**, seleccione **Aceptar** y **Siguiente**. Este contrato de licencia cubre Microsoft R Open, que incluye una distribución de las herramientas y paquetes base de R de código abierto, junto con proveedores de conectividad y paquetes de R mejorados del equipo de desarrollo de Microsoft.

2. En la página **Consentimiento de instalación de Python**, seleccione **Aceptar** y **Siguiente**. El contrato de licencia de código abierto de Python también cubre Anaconda y herramientas relacionadas, además de algunas bibliotecas nuevas de Python del equipo de desarrollo de Microsoft.

3. En la página **Listo para instalar**, confirme que estas selecciones se han realizado y haga clic en **Instalar**.
  
   + Servicios de Motor de base de datos
   + Machine Learning Services (en base de datos)
   + R o Python

   Tome nota de la ubicación de la carpeta en la ruta de acceso `..\Setup Bootstrap\Log` donde se almacenan los archivos de configuración. Una vez que se haya completado la instalación, podrá revisar los componentes instalados en el archivo de resumen.

4. Cuando finalice la instalación, si el programa indica que se reinicie el equipo, hágalo. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información, vea [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

::: moniker-end

## <a name="set-environment-variables"></a>Establecimiento de variables de entorno

Solo de cara a la integración de características de R, conviene establecer la variable de entorno **MKL_CBWR** para [garantizar una salida coherente](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) de los cálculos de la biblioteca Math Kernel Library (MKL) de Intel.

1. En el panel de control, haga clic en **Sistema y seguridad** > **Sistema** > **Configuración avanzada del sistema** > **Variables de entorno**.

2. Cree un usuario o una variable del sistema. 

   + Establezca el nombre de la variable en `MKL_CBWR`.
   + Establezca el valor de la variable en `AUTO`.

Este paso requiere el reinicio del servidor. Si va a habilitar la ejecución de scripts, puede posponer el reinicio hasta que se realice todo el trabajo de configuración.

<a name="bkmk_enableFeature"></a>

## <a name="enable-script-execution"></a>Habilitación de la ejecución de scripts

1. Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. 

    > [!TIP]
    > Puede descargar e instalar la versión adecuada desde esta página: [Descargue SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
    > 
    > También puede usar [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md), que admite tareas administrativas y consultas en SQL Server.
  
2. Conéctese a la instancia en la que instaló Machine Learning Services, haga clic en **Nueva consulta** para abrir una ventana de consulta y ejecute el comando siguiente:

    ```sql
    sp_configure
    ```

    El valor de la propiedad, `external scripts enabled`, debería ser **0** en este momento. La característica está desactivada de forma predeterminada. Un administrador debe habilitar explícitamente la característica para poder ejecutar scripts de R o Python.
    
3.  Para habilitar la característica de scripting externo, ejecute la siguiente instrucción:
    
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
    
    Si ya ha habilitado la característica para el lenguaje R, no ejecute RECONFIGURE una segunda vez para Python. La plataforma de extensibilidad subyacente admite ambos lenguajes.

## <a name="restart-the-service"></a>Reinicie el servicio.

Después de que la instalación se haya completado, reinicie el motor de base de datos.

Al reiniciar el servicio, también se reiniciará automáticamente el servicio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] relacionado.

Para reiniciar el servicio, puede hacer clic con el botón derecho en el comando **Reiniciar** de la instancia en SSMS, usar el panel **Servicios** del panel de control o emplear el [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md).

## <a name="verify-installation"></a>Comprobar la instalación

Haga lo siguiente para comprobar que se están ejecutando todos los componentes que se usan para iniciar el script externo.

1. En SQL Server Management Studio, abra una nueva ventana de consulta y ejecute el siguiente comando:
    
   ```sql
   EXECUTE sp_configure  'external scripts enabled'
   ```

   El valor **run_value** está establecido en 1.
    
2. Abra el panel de **Servicios** o el Administrador de configuración de SQL Server y compruebe que el **servicio SQL Server Launchpad** se está ejecutando. Debe tener un servicio para cada instancia del motor de base de datos que tenga instalado R o Python. Para más información sobre el servicio, vea [Marco de extensibilidad](../concepts/extensibility-framework.md). 
   
3. Si Launchpad se está ejecutando, puede ejecutar scripts sencillos de R y Python para comprobar que los tiempos de ejecución de scripting externo pueden comunicarse con SQL Server.

   Abra una ventana nueva de **consulta** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y, luego, ejecute un script como el siguiente:
   + Para R
   
     ```sql
     EXEC sp_execute_external_script  @language =N'R',
     @script=N'
     OutputDataSet <- InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
     
   + Para Python
     
     ```sql
     EXEC sp_execute_external_script  @language =N'Python',
     @script=N'
     OutputDataSet = InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
   
   **Resultados**

   El script puede tardar un poco en ejecutarse la primera vez que se carga el tiempo de ejecución del script externo. Los resultados deben tener un aspecto similar al siguiente:

   | hello |
   |----|
   | 1|

> [!NOTE]
> Las columnas o los encabezados usados en el script de Python no se devuelven de manera automática. Para agregar nombres de columna para la salida, debe especificar el esquema para el conjunto de datos devuelto. Para ello, use el parámetro WITH RESULTS del procedimiento almacenado, asigne un nombre a las columnas y especifique el tipo de datos SQL.
>
> Por ejemplo, puede agregar la línea siguiente para generar un nombre de columna arbitrario: `WITH RESULT SETS ((Col1 AS int))`.

::: moniker range=">=sql-server-2017"

<a name="apply-cu"></a>

## <a name="apply-updates"></a>Aplicación de actualizaciones

### <a name="existing-installation"></a>Instalación existente

Si ha agregado Machine Learning Services a una instancia de SQL Server existente y ha aplicado previamente una actualización acumulativa (CU), las versiones del motor de base de datos y la característica Machine Learning Services podrían ser diferentes. Esto puede producir un comportamiento o errores inesperados. 

Siga estos pasos para llevar Machine Learning Services a la misma versión que el motor de base de datos.

1. Determine la actualización acumulativa (CU) que tiene para el motor de base de datos. Ejecute esta instrucción T-SQL:

   ```sql
   SELECT @@VERSION
   ```
 
   A continuación se muestra un resultado de ejemplo de la actualización acumulativa (CU) 8 de SQL Server 2019:
 
   *Microsoft SQL Server 2019 (RTM-CU8-GDR) (KB4583459): 15.0.4083.2 (X64)   2 de noviembre de 2020 18:35:09   Copyright (C) 2019 Microsoft Corporation  Developer Edition (64 bits) en Windows 10 Enterprise 10.0 <X64> (Compilación 19042: ) (Hipervisor)*

   Para obtener más información, consulte [Determinar la versión, la edición y el nivel de actualización de SQL Server y sus componentes](https://docs.microsoft.com/troubleshoot/sql/general/determine-version-edition-update-level#machine-learning-services).

1. Descargue la [actualización acumulativa (CU)](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) que ha instalado para el motor de base de datos.

1. Ejecute la instalación de la actualización acumulativa (CU) y siga las instrucciones para instalar la CU para Machine Learning Services.

### <a name="new-installation"></a>Nueva instalación

Si instala Machine Learning Services con una nueva instalación del motor de base de datos de SQL Server, se recomienda aplicar la última actualización acumulativa tanto a los componentes de aprendizaje automático como a los de motor de base de datos.

En los dispositivos conectados a Internet, las actualizaciones acumulativas suelen aplicarse a través de Windows Update, pero también puede usar los pasos siguientes para las actualizaciones controladas. Al aplicar la actualización para el motor de base de datos, el programa de instalación extrae las actualizaciones acumulativas de las características de Python o R instaladas en la misma instancia. 

Los servidores desconectados requieren pasos adicionales. Para obtener más información, consulte [Instalación en equipos sin acceso a Internet > Aplicación de actualizaciones acumulativas](sql-ml-component-install-without-internet-access.md#apply-cu).

1. Comience con una instancia de línea de base ya instalada: versión inicial de SQL Server.

2. Vaya a la lista de actualizaciones acumulativas: [Actualizaciones más recientes de Microsoft SQL Server](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md).

3. Seleccione la actualización acumulativa más reciente. Se descarga un ejecutable que se extrae automáticamente.

4. Ejecute el programa de instalación. Acepte los términos de licencia y, en la página de selección de características, revise las características para las que se aplican las actualizaciones acumulativas. Debería ver todas las características instaladas para la instancia actual, incluidas las características de aprendizaje automático. El programa de instalación descarga los archivos CAB necesarios para actualizar todas las características.

   ![Resumen de las características instaladas](media/cumulative-update-feature-selection.png)

5. Continúe con los pasos del asistente y acepte los términos de licencia para las distribuciones de R y Python. 

::: moniker-end

## <a name="additional-configuration"></a>Configuración adicional

Si el paso de comprobación de scripts externos se ejecuta correctamente, puede ejecutar comandos de R o Python de SQL Server Management Studio, Visual Studio Code o cualquier otro cliente que pueda enviar instrucciones T-SQL al servidor.

Si se produjo un error al ejecutar el comando, revise los pasos de configuración adicional de esta sección. Es posible que tenga que crear otras configuraciones adicionales adecuadas para el servicio o la base de datos.

En el nivel de instancia, la configuración adicional podría incluir:

* [Configuración de firewall para SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
* [Habilitación de protocolos de red adicionales](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
* [Habilitación de conexiones remotas](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
* [Creación de un inicio de sesión para SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)
* [Administración de cuotas de disco](/windows/desktop/fileio/managing-disk-quotas) para evitar que los scripts externos ejecuten tareas que agoten el espacio en disco

::: moniker range=">=sql-server-ver15"
En SQL Server 2019 en Windows, el mecanismo de aislamiento ha cambiado. Este mecanismo afecta a **SQLRUserGroup**, las reglas de firewall, los permisos de archivo y la autenticación implícita. Para obtener más información, consulte [Cambios de aislamiento para Machine Learning Services](sql-server-machine-learning-services-2019.md).
::: moniker-end

<a name="bkmk_configureAccounts"></a> 
<a name="permissions-external-script"></a> 

En la base de datos, puede que necesite las siguientes actualizaciones de configuración:

* [Concesión de permiso a los usuarios para SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md)

> [!NOTE]
> El hecho de que se requiera una configuración adicional depende del esquema de seguridad, del lugar en el que se haya instalado SQL Server y de cómo se espera que los usuarios se conecten a la base de datos y ejecuten scripts externos.

## <a name="suggested-optimizations"></a>Optimizaciones sugeridas

Ahora que todo funciona, puede que también le interese optimizar el servidor para admitir el aprendizaje automático o instalar modelos de aprendizaje automático previamente entrenados.

::: moniker range="=sql-server-2017"

### <a name="add-more-worker-accounts"></a>Adición de más cuentas profesionales

Si prevé que muchos usuarios van a ejecutar scripts al mismo tiempo, puede aumentar el número de cuentas profesionales que están asignadas al servicio Launchpad. Para obtener más información, vea [Escalar la ejecución simultánea de scripts externos en SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).
::: moniker-end

### <a name="optimize-the-server-for-script-execution"></a>Optimización del servidor para la ejecución de scripts

La configuración predeterminada del programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está diseñada para optimizar el equilibrio del servidor para diversos servicios que el motor de base de datos admite, entre otros, procesos de extracción, transformación y carga de datos (ETL), creación de informes, auditoría y aplicaciones que usan datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En la configuración predeterminada, los recursos para el aprendizaje automático están restringidos a veces, especialmente en operaciones que usan mucha memoria.

Para asegurarse de que se asignen a los trabajos de aprendizaje automático la prioridad y los recursos correctos, se recomienda usar Resource Governor de SQL Server para configurar un grupo de recursos externos. Puede que también le interese cambiar la cantidad de memoria asignada al motor de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o aumentar el número de cuentas que se ejecutan en el servicio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)].

- Para configurar un grupo de recursos para la administración de recursos externos, vea [Creación de un grupo de recursos externos](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
- Para cambiar la cantidad de memoria reservada para la base de datos, vea [Opciones de configuración de memoria del servidor](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
- Para cambiar el número de cuentas de R que se pueden iniciar mediante [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)], consulte [Escalar la ejecución simultánea de scripts externos en SQL Server Machine Learning Services](../administration/scale-concurrent-execution-external-scripts.md).

Si usa la edición Standard Edition y no dispone de Resource Governor, puede usar vistas de administración dinámica (DMV) y eventos extendidos, así como la supervisión de eventos de Windows, como ayuda para administrar los recursos de servidor.

### <a name="install-additional-python-and-r-packages"></a>Instalación de paquetes adicionales de Python y R

Las soluciones de Python y R que cree para SQL Server pueden llamar a funciones básicas, funciones de los paquetes de su propiedad instalados con SQL Server y paquetes de terceros compatibles con la versión de Python y R de código abierto instalada por SQL Server.

Los paquetes que quiera usar de SQL Server deben estar instalados en la biblioteca predeterminada que la instancia usa. Si tiene una instalación independiente de Python o R en el equipo o si ha instalado paquetes en las bibliotecas de usuario, no puede usar esos paquetes desde T-SQL.

Para instalar y administrar paquetes adicionales, puede configurar grupos de usuarios para compartir paquetes en cada nivel de base de datos, o bien configurar roles de base de datos para permitir que los usuarios instalen sus propios paquetes. Para obtener más información, consulte [Instalación de paquetes de Python](../package-management/install-additional-python-packages-on-sql-server.md) e [Instalación de nuevos paquetes de R](../package-management/install-additional-r-packages-on-sql-server.md).

## <a name="next-steps"></a>Pasos siguientes

Los desarrolladores de Python pueden aprender a usar Python con SQL Server con estos tutoriales:

+ [Tutorial de Python: Predicción de alquileres de esquíes con regresión lineal en SQL Server Machine Learning Services](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Tutorial de Python: Clasificación de clientes por categorías mediante la agrupación en clústeres k-means con SQL Server Machine Learning Services](../tutorials/python-clustering-model.md)

Los desarrolladores de R pueden empezar con algunos ejemplos sencillos y conocer los aspectos básicos del funcionamiento de R con SQL Server. Para conocer el siguiente paso, vea los vínculos siguientes:

+ [Inicio rápido: Ejecutar R en T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Tutorial: Análisis en base de datos para desarrolladores de R](../tutorials/r-taxi-classification-introduction.md)
