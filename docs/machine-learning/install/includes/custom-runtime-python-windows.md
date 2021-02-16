---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 9f58ddf6b58e32d40b2962b47255aba2e553acca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072958"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de instalar un entorno de ejecución personalizado de Python, instale lo siguiente:

+ Si usa una instancia de SQL Server existente, instale la [actualización acumulativa (CU) 3 o posterior](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) para SQL Server 2019.

## <a name="install-language-extensions"></a>Instalación de Extensiones de lenguaje

> [!NOTE]
> Si instaló [Machine Learning Services](../../sql-server-machine-learning-services.md) en SQL Server 2019, las Extensiones de lenguaje ya están instaladas y puede omitir este paso.

Siga los pasos que aparecen a continuación para instalar [Extensiones de lenguaje de SQL Server](../../../language-extensions/language-extensions-overview.md), que se usa para el entorno de ejecución personalizado de Python.

1. Inicie el asistente para la instalación de SQL Server 2019.
  
1. En la pestaña **Instalación**, seleccione **Nueva instalación independiente de SQL Server o agregar características a una instalación existente**.

1. En la página **Selección de características** , seleccione estas opciones:
  
    + **Servicios de Motor de base de datos**
  
        Para usar las extensiones de lenguaje con SQL Server, debe instalar una instancia del motor de base de datos. Puede usar una instancia nueva o una existente.
  
    + **Machine Learning Services y extensiones de lenguaje**

        Seleccione **Machine Learning Services y extensiones de lenguaje**. No seleccione Python, ya que va a instalar el entorno de ejecución de Python personalizado más adelante.

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="Instalación de Extensiones de lenguaje de SQL Server 2019.":::

1. En la página **Listo para instalar**, confirme que estas selecciones se han realizado y haga clic en **Instalar**.
  
    + Servicios de Motor de base de datos
    + Machine Learning Services y extensiones de lenguaje

1. Una vez que se completa la instalación, reinicie la máquina si se le pide hacerlo.

> [!IMPORTANT]
> Si instala una instancia nueva de SQL Server 2019 con Extensiones de lenguaje, instale la [actualización acumulativa (CU) 3 o posterior](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) antes de ir al paso siguiente.

## <a name="install-python"></a>Instalar Python

La extensión del lenguaje Python que se usa para el entorno de ejecución de Python actualmente admite solo Python 3.7. Si quiere usar una versión distinta de Python, siga la instrucción que aparece en el [repositorio de GitHub sobre Extensiones de lenguaje de Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) para modificar y recompilar la extensión.

1. Descargue [Python 3.7](https://www.python.org/downloads/windows/) para Windows y ejecute el programa de instalación en el servidor.

1. Seleccione **Agregar Python 3.7 a la ruta de acceso** y, después, **Personalizar instalación**.

    :::image type="content" source="../media/python-install-add-to-path.png" alt-text="Instalación de Python 3.7: adición de Python 3.7 a la ruta de acceso":::

1. En **Características opcionales**, deje los valores predeterminados y seleccione **Siguiente**.

1. Seleccione **Instalar para todos los usuarios** y anote la ubicación de instalación.

    :::image type="content" source="../media/python-install-for-all-users.png" alt-text="Instalación de Python 3.7: instalación para todos los usuarios":::

1. Seleccione **Instalar**.

## <a name="install-pandas"></a>Instalación de pandas

Instale el paquete [pandas](https://pandas.pydata.org/) para Python desde un símbolo del sistema con *privilegios elevados* (Ejecutar como administrador):

```bash
python.exe -m pip install pandas
```

## <a name="grant-access-to-python-folder"></a>Concesión de acceso a la carpeta de Python

Ejecute los comandos **icacls** siguientes desde un símbolo del sistema con *privilegios elevados* nuevo para conceder acceso de **LECTURA Y EJECUCIÓN** a la ubicación de la instalación de Python al **servicio SQL Server Launchpad** y al SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**).

En los ejemplos siguientes se usa la ubicación de instalación de Python como `C:\Program Files\Python37`. Si la ubicación es diferente, cámbiela en el comando.

1. Conceda permisos al **nombre de usuario del servicio SQL Server Launchpad**.

    ```cmd
    icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    En el caso de una instancia con nombre, el comando será `icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` para una instancia denominada **SQL01**.

2. Conceda permisos al **SID S-1-15-2-1**.

    ```cmd
    icacls "C:\Program Files\Python37" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    El comando anterior concede permisos al SID del equipo **S-1-15-2-1**, que es equivalente a **TODOS LOS PAQUETES DE APLICACIONES** en una versión en inglés de Windows. Como alternativa, puede usar `icacls "C:\Program Files\Python37" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` en una versión en inglés de Windows.

## <a name="restart-sql-server-launchpad"></a>Reinicio de SQL Server Launchpad

Siga estos pasos para reiniciar el servicio SQL Server Launchpad.

1. Abra el [Administrador de configuración de SQL Server](../../../relational-databases/sql-server-configuration-manager.md).

1. En **Servicios de SQL Server**, haga clic con el botón derecho en **SQL Server Launchpad (MSSQLSERVER)** y seleccione **Reiniciar**. Si va a usar una instancia con nombre, se mostrará el nombre de la instancia en lugar de **(MSSQLSERVER)** .

## <a name="register-language-extension"></a>Registro de la extensión del lenguaje

Siga estos pasos para descargar y registrar la extensión del lenguaje Python, que se usa para el entorno de ejecución personalizado de Python.

1. Descargue el archivo **python-lang-extension-windows-release.zip** del [repositorio de GitHub de Extensiones de lenguaje de SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    También puede usar la versión de depuración (**python-lang-extension-windows-debug.zip**) en un entorno de desarrollo o prueba. La versión de depuración proporciona información de registro detallada para investigar los errores y no se recomienda para los entornos de producción.

1. Use [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) para conectarse a la instancia de SQL Server y ejecute el comando de T-SQL siguiente para registrar la extensión del lenguaje Python con [CREAR UN LENGUAJE EXTERNO](../../../t-sql/statements/create-external-language-transact-sql.md).

    Modifique la ruta de acceso de esta instrucción para reflejar la ubicación del archivo ZIP descargado de la extensión del lenguaje (**python-lang-extension-windows-release.zip**) y la ubicación de la instalación de Python (`C:\\Program Files\\Python3.7`).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'C:\path\to\python-lang-extension-windows-release.zip', 
        FILE_NAME = 'pythonextension.dll', 
        ENVIRONMENT_VARIABLES = N'{"PYTHONHOME": "C:\\Program Files\\Python3.7"}');
    GO
    ```

    Ejecute la instrucción para cada base de datos en la que quiera usar la extensión del lenguaje Python.

    > [!NOTE]
    > **Python** es una palabra reservada y no se puede usar como nombre de un lenguaje externo nuevo. Use otro nombre. Por ejemplo, la instrucción anterior usa **myPython**.
