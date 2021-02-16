---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 07bd68eaee05893174813ed43dc5358104016e4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072773"
---
## <a name="custom-installation-of-r"></a>Instalación personalizada de R

> [!NOTE]
> Si ha instalado R en la ubicación predeterminada de **/usr/lib/R**, puede omitir esta sección y pasar a [Instalación del paquete Rcpp](#install-rcpp-package-linux).

### <a name="update-the-environment-variables"></a>Actualización de las variables de entorno

En primer lugar, edite el servicio **mssql-launchpadd** para agregar la variable de entorno **R_HOME** al archivo `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`.

1. Abra el archivo con systemctl.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Inserte el texto siguiente en el archivo `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` que se abre. Establezca el valor de **R_HOME** en la ruta de instalación personalizada de R.

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

1. Guarde y cierre el archivo.

A continuación, asegúrese de que se puede cargar `libR.so`.

1. Cree el archivo custom-r.conf en **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

1. En el archivo que se abre, agregue la ruta de acceso a **libR.so** desde la instalación personalizada de R.

    ```
    /path/to/installation/of/R/lib
    ```

1. Guarde el archivo nuevo y cierre el editor.

1. Ejecute `ldconfig` y compruebe que se puede cargar **libR.so**; para ello, ejecute el comando siguiente y compruebe que se pueden encontrar todas las bibliotecas dependientes.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Concesión de acceso a la carpeta de instalación personalizada de R

Establezca la opción `datadirectories` de la sección de extensibilidad del archivo `/var/opt/mssql/mssql.conf` en la instalación personalizada de R.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>Reinicio del servicio mssql-launchpadd

Ejecute el comando siguiente para reiniciar **mssql-launchpadd**.

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="install-rcpp-package-linux"></a>

## <a name="install-rcpp-package"></a>Instalación del paquete Rcpp

Siga estos pasos para instalar el paquete **Rcpp**.

1. Inicie R desde un shell:

    ```bash
    sudo ${R_HOME}/bin/R
    ```

1. Ejecute el script siguiente para instalar el paquete Rcpp en la carpeta ${R_HOME}\library.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="register-language-extension"></a>Registro de la extensión del lenguaje

Siga estos pasos para descargar y registrar la extensión del lenguaje R, que se usa para el entorno de ejecución personalizado de R.

1. Descargue el archivo **R-lang-extension-linux-release.zip** del [repositorio de GitHub de Extensiones de lenguaje de SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    También puede usar la versión de depuración (**R-lang-extension-linux-debug.zip**) en un entorno de desarrollo o prueba. La versión de depuración proporciona información de registro detallada para investigar los errores y no se recomienda para los entornos de producción.

1. Use [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) para conectarse a la instancia de SQL Server y ejecute el comando de T-SQL siguiente para registrar la extensión del lenguaje R con [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md). 

    Modifique la ruta de acceso de esta instrucción para reflejar la ubicación del archivo ZIP descargado de la extensión del lenguaje (**R-lang-extension-linux-release.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'/path/to/R-lang-extension-linux-release.zip', FILE_NAME = 'libRExtension.so.1.0');
    GO
    ```

    Ejecute la instrucción para cada base de datos en la que quiera usar la extensión del lenguaje R.

    > [!NOTE]
    > **R** es una palabra reservada y no se puede usar como nombre de un lenguaje externo nuevo. Use otro nombre. Por ejemplo, en la instrucción anterior se usa **myR**.
