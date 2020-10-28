---
title: Instalación de extensiones de lenguaje Java de SQL Server en Windows
titleSuffix: ''
description: Obtenga información sobre cómo instalar la característica de extensiones de lenguaje Java en SQL Server en Windows mediante la ejecución del Asistente para la instalación de SQL Server.
author: cawrites
ms.author: chadam
ms.date: 09/17/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 5da3226bc95866b35228b129ddaf790705cea54b
ms.sourcegitcommit: 43b92518c5848489d03c68505bd9905f8686cbc0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155572"
---
# <a name="install-sql-server-language-java-extensions-on-windows"></a>Instalación de extensiones de lenguaje Java de SQL Server en Windows

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Obtenga información sobre cómo instalar el componente de extensiones de lenguaje en SQL Server ejecutando el asistente para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> Este artículo trata la instalación de extensiones de lenguaje de SQL Server en Windows. Para Linux, vea [Instalación de Extensiones de lenguaje (Java) de SQL Server 2019 en Linux](../../linux/sql-server-linux-setup-language-extensions-java.md).

<a name="prerequisites"></a> 

## <a name="pre-install-checklist"></a>Lista de comprobación previa a la instalación

+ Se necesita el programa de instalación de SQL Server 2019 si quiere instalar la compatibilidad con extensiones de lenguaje.

+ Se necesita una instancia del motor de base de datos. No se pueden instalar únicamente las características de extensiones de lenguaje, aunque se pueden agregar incrementalmente a una instancia existente.

+ De cara a la continuidad empresarial, se admiten [Grupos de disponibilidad AlwaysOn](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) en las extensiones de lenguaje. Debe instalar las extensiones de lenguaje y configurar los paquetes en cada nodo.

+ La instalación de extensiones de lenguaje es posible en un clúster de conmutación por error en SQL Server 2019.

+ No instale extensiones de lenguaje de SQL Server en un controlador de dominio, ya que se producirá un error en la parte de las extensiones de lenguaje del programa de instalación.

+ Las extensiones de lenguaje y [Machine Learning Services](../../machine-learning/index.yml) se instalan de forma predeterminada en Clústeres de macrodatos de SQL Server. Si usa Clústeres de macrodatos, no es necesario que siga los pasos de este artículo. Para más información, vea [Uso de Machine Learning Services (Python y R) en Clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md).

> [!IMPORTANT]
> Una vez finalizada la instalación, asegúrese de completar los pasos posteriores a la configuración que se describen en este artículo. Estos pasos incluyen cómo habilitar SQL Server para usar código externo y cómo agregar las cuentas necesarias para que SQL Server ejecute código Java en su nombre. Normalmente, si se realizan cambios en la configuración, es necesario reiniciar la instancia o el servicio Launchpad.

<a name="java-jre-jdk"></a>

## <a name="java-jre-or-jdk"></a>JRE o JDK de Java

En la versión candidata para lanzamiento 1 de SQL Server 2019 existen dos formas de instalar y usar Java con SQL Server:

1. Usar el runtime predeterminado de Java, Zulu Open JRE versión 11.0.3. Este runtime es compatible y se incluye en la instalación de SQL Server.

1. Usar la distribución Java de su preferencia en lugar del runtime de Java predeterminado.

    Actualmente, Java 11 es la versión admitida en Windows. Java Runtime Environment (JRE) es el requisito mínimo, pero Java Development Kit (JDK) es útil si necesita el compilador de Java y los paquetes de desarrollo. Dado que JDK lo incluye todo, si lo instala, JRE no será necesario. En Windows, se recomienda instalar JDK en la carpeta `/Program Files/` predeterminada, si es posible. De lo contrario, se necesitará configuración adicional para conceder permisos a los archivos ejecutables. Para más información, vea la sección sobre [concesión de permisos (Windows)](#perms-nonwindows) de este documento.

    > [!NOTE]
    > Dado que Java es compatible con versiones anteriores, las versiones anteriores podrían funcionar, pero la versión admitida y probada en la versión candidata para lanzamiento 1 es Java 11.
    
## <a name="get-the-installation-media"></a>Obtener los medios de instalación

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

## <a name="run-setup"></a>Ejecución del programa de instalación

En instalaciones locales, debe ejecutar el programa de instalación como administrador. Si instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde un recurso compartido remoto, deberá usar una cuenta de dominio que tenga permisos de lectura y ejecución para dicho recurso.

1. Inicie el asistente para la instalación de SQL Server 2019. 
  
2. En la pestaña **Instalación** , seleccione **Nueva instalación independiente de SQL Server o agregar características a una instalación existente** .

    ![Instalación de SQL Server 2019](../media/sql-install.png) 

3. En la página **Selección de características** , seleccione estas opciones:
  
    - **Servicios de Motor de base de datos**
  
        Para usar las extensiones de lenguaje con SQL Server, debe instalar una instancia del motor de base de datos. Puede usar una instancia predeterminada o una con nombre.
  
    - **Machine Learning Services y extensiones de lenguaje**
  
        Con esta opción se instala el componente de extensiones de lenguaje que admite la ejecución de código Java.

        - Si quiere instalar el runtime de Java predeterminado, Zulu Open JRE 11.0.3, seleccione **Machine Learning Services y extensiones de lenguaje** y **Java** .

        - Si quiere usar su propio runtime de Java, seleccione **Machine Learning Services y extensiones de lenguaje** . No seleccione Java.

        Si quiere usar R y Python, vea [Instalación de SQL Server Machine Learning Services (Python y R) en Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md).

    ![Opciones de características de extensiones de lenguaje](../media/sql-install-feature-selection.png)

4. Si elige **Java** en el paso anterior para instalar el runtime de Java predeterminado, se abrirá la página **Ubicación de instalación de Java** .

    Seleccione **Instalación de Open JRE 11.0.3 incluida con esta instalación** .

    ![Selección de la ubicación de instalación de Java](../media/sql-install-openjdk.png)

    > [!NOTE]
    > La opción **Proporcionar la ubicación de otra versión que se haya instalado en este equipo** no se usa en las extensiones de lenguaje.

5. En la página **Listo para instalar** , confirme que estas selecciones se han realizado y haga clic en **Instalar** .
  
    + Servicios de Motor de base de datos
    + Machine Learning Services y extensiones de lenguaje

    Tome nota de la ubicación de la carpeta en la ruta de acceso `..\Setup Bootstrap\Log` donde se almacenan los archivos de configuración. Una vez que se haya completado la instalación, podrá revisar los componentes instalados en el archivo de resumen.

6. Cuando finalice la instalación, si el programa indica que se reinicie el equipo, hágalo. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información, vea [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="add-the-jre_home-variable"></a>Adición de la variable JRE_HOME

`JRE_HOME` es una variable de entorno del sistema que especifica la ubicación del intérprete de Java. En este paso, crearemos una variable de entorno del sistema para el intérprete en Windows.

1. Busque y copie la ruta de acceso principal de JRE.

    Por ejemplo, la ruta de acceso principal de JRE del runtime predeterminado de Java Zulu JRE 11.0.3 es `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Binn\AZUL-OpenJDK-JRE\`.

    Según cuál sea la ruta de acceso de instalación de SQL Server, o si se elige otro runtime de Java, la ubicación de JDK o de JRE podría ser diferente de la ruta de acceso de ejemplo anterior. Incluso si tiene instalado JDK, a menudo se obtiene una subcarpeta de JRE como parte de esa instalación, por lo que deberá apuntar a la carpeta de JRE en ese caso. La extensión Java intentará cargar `jvm.dll` desde la ruta de acceso `%JRE_HOME%\bin\server`.

2. En el panel de control, abra **Sistema y seguridad** , abra **Sistema** y seleccione **Propiedades del sistema > Avanzadas** .

3. Seleccione **Variables de entorno** .

4. Cree una variable del sistema para `JRE_HOME` con el valor de la ruta de acceso de JDK/JRE (que encontrará en el paso 1).

5. Reinicie [Launchpad](../concepts/extensibility-framework.md#launchpad).

    1. Abra el [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md).

    2. En Servicios de SQL Server, haga clic con el botón derecho en Launchpad de SQL Server y elija **Reiniciar** .

<a name="perms-nonwindows"></a>

## <a name="grant-access-to-non-default-jre-folder"></a>Concesión de acceso a la carpeta de JRE no predeterminada

Si no instaló el runtime predeterminado Zulu Open JRE incluido con SQL Server ni tampoco JDK o JRE en Archivos de programa, deberá realizar los siguientes pasos. Ejecute los comandos **icacls** desde una línea *con privilegios elevados* para conceder acceso a las cuentas de servicio **SQLRUsergroup** y de SQL Server (en **ALL_APPLICATION_PACKAGES** ) para acceder a JRE. Estos comandos concederán acceso de forma recursiva a todos los archivos y carpetas en la ruta de acceso de directorio especificada.

1. Para conceder permisos de SQLRUserGroup:

    En el caso de una instancia con nombre, anexe el nombre de la instancia a SQLRUsergroup (por ejemplo, `SQLRUsergroupINSTANCENAME`).

    ```cmd
    icacls "<PATH to JRE>" /grant "SQLRUsergroup":(OI)(CI)RX /T
    ```
    
    Este paso se puede omitir si se ha instalado JDK/JRE en la carpeta predeterminada de Archivos de programa en Windows.

2. Para conceder permisos de AppContainer:

    ```cmd
    icacls “<PATH to JRE>” /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```
    
    > [!NOTE]
    > El comando anterior concede permisos al SID del equipo **S-1-15-2-1** , que es equivalente a **TODOS LOS PAQUETES DE APLICACIONES** en una versión en inglés de Windows. Como alternativa, puede usar `icacls "<PATH to JRE>" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` en una versión en inglés de Windows.
    
## <a name="enable-script-execution"></a>Habilitación de la ejecución de scripts

1. Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. 

    > [!TIP]
    > Puede descargar e instalar la versión adecuada desde esta página: [Descargue SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
    > 
    > También puede usar [Azure Data Studio](../../azure-data-studio/what-is.md), que admite tareas administrativas y consultas en SQL Server.
  
2. Conéctese a la instancia en la que instaló las extensiones de lenguaje, haga clic en **Nueva consulta** para abrir una ventana de consulta y ejecute el siguiente comando:

    ```sql
    sp_configure
    ```

    El valor de la propiedad, `external scripts enabled`, debería ser **0** en este momento. La característica está desactivada de forma predeterminada, y un administrador debe habilitarla expresamente para poder ejecutar código Java.
    
3.  Para habilitar la característica de scripting externo, ejecute la siguiente instrucción:
    
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
    
    Si ya ha habilitado la característica para Machine Learning Services, no ejecute RECONFIGURE una segunda vez para las extensiones de lenguaje. La plataforma de extensibilidad subyacente admite ambos.

## <a name="restart-the-service"></a>Reinicie el servicio.

Cuando se haya completado la instalación, reinicie el motor de base de datos antes de continuar con lo siguiente para habilitar la ejecución de scripts.

Al reiniciar el servicio, también se reiniciará automáticamente el servicio SQL Server Launchpad relacionado.

Para reiniciar el servicio, puede hacer clic con el botón derecho en el comando **Reiniciar** de la instancia en SSMS, usar el panel **Servicios** del panel de control o emplear el [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md).

<a name="register_external_language"></a>

## <a name="register-external-language"></a>Registrar lenguaje externo

Por cada base de datos en la que quiera usar Extensiones de lenguaje, debe registrar el lenguaje externo con [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md).

En el ejemplo siguiente se agrega un lenguaje externo denominada Java a una base de datos de SQL Server en Windows.

```SQL
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

Para obtener más información, vea [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md).

## <a name="verify-installation"></a>Comprobar la instalación

Compruebe el estado de instalación de la instancia en los registros de instalación.

Haga lo siguiente para comprobar que se están ejecutando todos los componentes que se usan para iniciar el script externo.

1. En SQL Server Management Studio o Azure Data Studio, abra una nueva ventana de consulta y ejecute la siguiente instrucción:
    
    ```sql
    EXEC sp_configure  'external scripts enabled'
    ```

    Ahora, **run_value** está establecido en 1.
    
2. Abra el panel de **Servicios** o el Administrador de configuración de SQL Server y compruebe que el **servicio SQL Server Launchpad** se está ejecutando. Debe tener un servicio por cada instancia del motor de base de datos que tenga instaladas extensiones de lenguaje. Para más información sobre el servicio, vea [Marco de extensibilidad](../concepts/extensibility-framework.md). 
   
## <a name="additional-configuration"></a>Configuración adicional

Si el paso de comprobación se ejecuta correctamente, puede ejecutar código Java en SQL Server Management Studio, Azure Data Studio, Visual Studio Code o cualquier otro cliente que pueda enviar instrucciones T-SQL al servidor.

Si se produjo un error al ejecutar el comando, revise los pasos de configuración adicional de esta sección. Es posible que tenga que crear otras configuraciones adicionales adecuadas para el servicio o la base de datos.

En el nivel de instancia, la configuración adicional podría incluir:

* [Configuración de firewall para SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md)
* [Habilitación de protocolos de red adicionales](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
* [Habilitación de conexiones remotas](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
* [Creación de un inicio de sesión para SQLRUserGroup](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

<a name="bkmk_configureAccounts"></a> 
<a name="permissions-external-script"></a> 

En la base de datos, puede que necesite las siguientes actualizaciones de configuración:

* [Concesión de permiso a los usuarios para SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md)
* [Concesión de permiso a los usuarios para ejecutar un lenguaje específico](../../t-sql/statements/create-external-language-transact-sql.md#permissions)

> [!NOTE]
> El hecho de que se requiera una configuración adicional depende del esquema de seguridad, del lugar en el que se haya instalado SQL Server y de cómo se espera que los usuarios se conecten a la base de datos y ejecuten scripts externos.

## <a name="suggested-optimizations"></a>Optimizaciones sugeridas

Ahora que todo funciona, puede que también le interese optimizar el servidor para admitir extensiones de lenguaje.

### <a name="optimize-the-server-for-language-extensions"></a>Optimización del servidor para extensiones de lenguaje

La configuración predeterminada del programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está diseñada para optimizar el equilibrio del servidor para diversos servicios que el motor de base de datos admite, entre otros, procesos de extracción, transformación y carga de datos (ETL), creación de informes, auditoría y aplicaciones que usan datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por lo tanto, en la configuración predeterminada podría ver que los recursos para extensiones de lenguaje a veces están restringidos, especialmente en operaciones que usan mucha memoria.

Para asegurarse de que se asignan a los trabajos de extensiones de lenguaje la prioridad y los recursos correctos, se recomienda usar Resource Governor de SQL Server para configurar un grupo de recursos externos. Puede que también le interese cambiar la cantidad de memoria asignada al motor de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o aumentar el número de cuentas que se ejecutan en el servicio [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)].

- Para configurar un grupo de recursos para la administración de recursos externos, vea [Creación de un grupo de recursos externos](../../t-sql/statements/create-external-resource-pool-transact-sql.md).
  
- Para cambiar la cantidad de memoria reservada para la base de datos, vea [Opciones de configuración de memoria del servidor](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
Si usa la edición Standard Edition y no dispone de Resource Governor, puede usar vistas de administración dinámica (DMV) y eventos extendidos, así como la supervisión de eventos de Windows, como ayuda para administrar los recursos de servidor.

## <a name="next-steps"></a>Pasos siguientes

Los desarrolladores de Java pueden empezar con algunos ejemplos sencillos y conocer los aspectos básicos del funcionamiento de Java con SQL Server. Para conocer el siguiente paso, vea el siguiente vínculo:

+ [Tutorial: Expresiones regulares con Java](../tutorials/search-for-string-using-regular-expressions-in-java.md)
