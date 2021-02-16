---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 2a156ab122f0eadce71da9f4fcaf9e99584c8e32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072772"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de instalar un entorno de ejecución personalizado de R, instale lo siguiente:

+ Si usa una instancia de SQL Server existente, instale la [actualización acumulativa (CU) 3 o posterior](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) para SQL Server 2019.

## <a name="install-language-extensions"></a>Instalación de Extensiones de lenguaje

> [!NOTE]
> Si instaló [Machine Learning Services](../../sql-server-machine-learning-services.md) en SQL Server 2019, las Extensiones de lenguaje ya están instaladas y puede omitir este paso.

Siga los pasos que aparecen a continuación para instalar [Extensiones de lenguaje de SQL Server](../../../language-extensions/language-extensions-overview.md), que se usa para el entorno de ejecución personalizado de R.

1. Inicie el asistente para la instalación de SQL Server 2019.
  
1. En la pestaña **Instalación**, seleccione **Nueva instalación independiente de SQL Server o agregar características a una instalación existente**.

1. En la página **Selección de características** , seleccione estas opciones:
  
    + **Servicios de Motor de base de datos**
  
        Para usar las extensiones de lenguaje con SQL Server, debe instalar una instancia del motor de base de datos. Puede usar una instancia nueva o una existente.
  
    + **Machine Learning Services y extensiones de lenguaje**

        Seleccione **Machine Learning Services y extensiones de lenguaje**. No seleccione R, ya que va a instalar el entorno de ejecución de R personalizado más adelante.

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="Instalación de Extensiones de lenguaje de SQL Server 2019.":::

1. En la página **Listo para instalar**, confirme que estas selecciones se han realizado y haga clic en **Instalar**.
  
    + Servicios de Motor de base de datos
    + Machine Learning Services y extensiones de lenguaje

1. Una vez que se completa la instalación, reinicie la máquina si se le pide hacerlo.

> [!IMPORTANT]
> Si instala una instancia nueva de SQL Server 2019 con Extensiones de lenguaje, instale la [actualización acumulativa (CU) 3 o posterior](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) antes de ir al paso siguiente.

## <a name="install-r"></a>Instalar R

Descargue e instale la versión de R que utilizará como el entorno de ejecución personalizado. Se admite la versión 3.3 o posterior de R.

1. Descargue la [versión 3.3 o posterior de R](https://cran.r-project.org/bin/windows/base/).

1. Ejecute el programa de instalación de R.

1. Anote la ruta de acceso donde se instala R. Por ejemplo, en este artículo es `C:\Program Files\R\R-4.0.3`.

    :::image type="content" source="../media/r-setup-path.png" alt-text="Instalación de R":::

## <a name="update-system-environment-variable"></a>Actualización de las variables de entorno del sistema

Siga estos pasos para modificar las variables de entorno del sistema **PATH**.

1. En el cuadro de búsqueda de Windows, busque **Editar las variables de entorno del sistema** y ábralo.

1. En **Opciones avanzadas**, seleccione **Variables de entorno**.

1. Modifique la variable de entorno del sistema **PATH**.

    Seleccione **PATH** y haga clic en **Editar**.

    Seleccione **Nueva** y agregue la ruta de acceso a la carpeta `\bin\x64` en la ruta de instalación de R. Por ejemplo, `C:\Program Files\R\R-4.0.3\bin\x64`.

## <a name="install-rcpp-package"></a>Instalación del paquete Rcpp

Siga estos pasos para instalar el paquete **Rcpp**.

1. Inicie un símbolo del sistema con *privilegios elevados* (Ejecutar como administrador).

1. Inicie R desde el símbolo del sistema. Ejecute `\bin\R.exe` en la carpeta de la ruta de instalación de R. Por ejemplo, `C:\Program Files\R\R-4.0.3\bin\R.exe`.

    ```CMD
    "C:\Program Files\R\R-4.0.3\bin\R.exe"
    ```

1. Ejecute el script siguiente para instalar el paquete Rcpp en la carpeta `\library` de la ruta de instalación de R. Por ejemplo, `C:\Program Files\R\R-4.0.3\library`.

    ```R
    install.packages("Rcpp", lib="C:\Program Files\R\R-4.0.3\library");
    ```

## <a name="grant-access-to-r-folder"></a>Concesión de acceso a la carpeta de R

> [!NOTE]
> Si ha instalado R en la ubicación predeterminada `C:\Program Files\R\R-version` (por ejemplo, `C:\Program Files\R\R-4.0.3`), puede omitir este paso.

Ejecute los comandos **icacls** siguientes desde un nuevo símbolo del sistema con *privilegios elevados* para conceder acceso de **LECTURA Y EJECUCIÓN** al **nombre de usuario del servicio SQL Server Launchpad** y al SID **S-1-15-2-1** (**ALL APPLICATION PACKAGES**). El nombre de usuario del servicio Launchpad tiene el formato `NT Service\MSSQLLAUNCHPAD$INSTANCENAME`, donde `INSTANCENAME` es el nombre de la instancia de SQL Server.

Estos comandos concederán acceso de forma recursiva a todos los archivos y carpetas en la ruta de acceso de directorio especificada.

1. Conceda permisos al **nombre de usuario del servicio SQL Server Launchpad** para la ruta de instalación de R. Por ejemplo, `C:\Program Files\R\R-4.0.3`.

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    En el caso de una instancia con nombre, el comando será `icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` para una instancia denominada **SQL01**.

2. Conceda permisos para **SID S-1-15-2-1** a la ruta de instalación de R. Por ejemplo, `C:\Program Files\R\R-4.0.3`.

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    El comando anterior concede permisos al SID del equipo **S-1-15-2-1**, que es equivalente a **TODOS LOS PAQUETES DE APLICACIONES** en una versión en inglés de Windows. Como alternativa, puede usar `icacls "C:\Program Files\R\R-4.0.3" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` en una versión en inglés de Windows.

## <a name="restart-sql-server-launchpad"></a>Reinicio de SQL Server Launchpad

Siga estos pasos para reiniciar el servicio SQL Server Launchpad.

1. Abra el [Administrador de configuración de SQL Server](../../../relational-databases/sql-server-configuration-manager.md).

1. En **Servicios de SQL Server**, haga clic con el botón derecho en **SQL Server Launchpad (MSSQLSERVER)** y seleccione **Reiniciar**. Si usa una instancia con nombre, se mostrará el nombre de la instancia en lugar de **(MSSQLSERVER)** .

## <a name="register-language-extension"></a>Registro de la extensión del lenguaje

Siga estos pasos para descargar y registrar la extensión del lenguaje R, que se usa para el entorno de ejecución personalizado de R.

1. Descargue el archivo **R-lang-extension-windows-release.zip** del [repositorio de GitHub de Extensiones de lenguaje de SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    También puede usar la versión de depuración (**R-lang-extension-windows-debug.zip**) en un entorno de desarrollo o prueba. La versión de depuración proporciona información de registro detallada para investigar los errores y no se recomienda para los entornos de producción.

1. Use [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) para conectarse a la instancia de SQL Server y ejecute el comando de T-SQL siguiente para registrar la extensión del lenguaje R con [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md).

    Modifique la ruta de acceso de esta instrucción para reflejar la ubicación del archivo ZIP descargado de la extensión del lenguaje (**R-lang-extension-windows-release.zip**) y la ubicación de la instalación de R (`C:\\Program Files\\R\\R-4.0.3`).

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'C:\path\to\R-lang-extension-windows-release.zip', 
        FILE_NAME = 'libRExtension.dll',
        ENVIRONMENT_VARIABLES = N'{"R_HOME": "C:\\Program Files\\R\\R-4.0.3"}'););
    GO
    ```

    Ejecute la instrucción para cada base de datos en la que quiera usar la extensión del lenguaje R.

    > [!NOTE]
    > **R** es una palabra reservada y no se puede usar como nombre de un lenguaje externo nuevo. Use otro nombre. Por ejemplo, en la instrucción anterior se usa **myR**.
