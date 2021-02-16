---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 1303df98c60212c13a233d1e386c60109308e981
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072922"
---
## <a name="custom-installation-of-python"></a>Instalación personalizada de Python

> [!NOTE]
> Si instaló Python 3.7 en la ubicación predeterminada de `/usr/lib/python3.7`, puede omitir esta sección y pasar a la sección [Registro de la extensión del lenguaje](#register-language-extension-linux).

Si compiló su propia versión de Python 3.7, use los comandos siguientes para que SQL Server sepa de su instalación personalizada.

### <a name="add-environment-variable"></a>Incorporación de una variable de entorno

Primero, edite el servicio **mssql-launchpadd** para agregar la variable de entorno **PYTHONHOME** al archivo `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`.

1. Abra el archivo con systemctl.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Inserte el texto siguiente en el archivo `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` que se abre. Establezca el valor de **PYTHONHOME** en la ruta de instalación personalizada de Python.

    ```
    [Service]
    Environment="PYTHONHOME=/path/to/installation/of/python3.7"
    ```

1. Guarde el archivo y cierre el editor.

A continuación, asegúrese de que se puede cargar `libpython3.7m.so.1.0`.

1. Cree el archivo custom-python.conf en `/etc/ld.so.conf.d`.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-python.conf
    ```

1. En el archivo que se abre, agregue la ruta de acceso a **libpython3.7m.so.1.0** de la instalación personalizada de Python.

    ```
    /path/to/installation/of/python3.7/lib
    ```

1. Guarde el archivo nuevo y cierre el editor.

1. Ejecute `ldconfig` y compruebe que se puede cargar `libpython3.7m.so.1.0`; para ello, ejecute los comandos siguientes y compruebe que se pueden encontrar todas las bibliotecas dependientes.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
    ```

### <a name="grant-access-to-python-folder"></a>Concesión de acceso a la carpeta de Python

Establezca la opción `datadirectories` de la sección de extensibilidad del archivo `/var/opt/mssql/mssql.conf` en la instalación personalizada de Python.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>Reinicio de mssql-launchpadd

Ejecute el comando siguiente para reiniciar **mssql-launchpadd**.

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Registro de la extensión del lenguaje

Siga estos pasos para descargar y registrar la extensión del lenguaje Python, que se usa para el entorno de ejecución personalizado de Python.

1. Descargue el archivo **python-lang-extension-linux-release.zip** del [repositorio de GitHub de Extensiones de lenguaje de SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    También puede usar la versión de depuración (**python-lang-extension-linux-debug.zip**) en un entorno de desarrollo o prueba. La versión de depuración proporciona información de registro detallada para investigar los errores y no se recomienda para los entornos de producción.

1. Use [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) para conectarse a la instancia de SQL Server y ejecute el comando de T-SQL siguiente para registrar la extensión del lenguaje Python con [CREAR UN LENGUAJE EXTERNO](../../../t-sql/statements/create-external-language-transact-sql.md). 

    Modifique la ruta de acceso de esta instrucción para reflejar la ubicación del archivo ZIP descargado de la extensión del lenguaje (**python-lang-extension-linux-release.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux-release.zip', FILE_NAME = 'libPythonExtension.so.1.1');
    GO
    ```

    Ejecute la instrucción para cada base de datos en la que quiera usar la extensión del lenguaje Python.

    > [!NOTE]
    > **Python** es una palabra reservada y no se puede usar como nombre de un lenguaje externo nuevo. Use otro nombre. Por ejemplo, la instrucción anterior usa **myPython**.
