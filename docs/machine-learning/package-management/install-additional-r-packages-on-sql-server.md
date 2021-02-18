---
title: Instalación de paquetes de R con sqlmlutils
description: Obtenga más información sobre cómo usar sqlmlutils para instalar nuevos paquetes de R en una instancia de Machine Learning Services de SQL Server.
ms.prod: sql
ms.technology: machine-learning
ms.date: 12/15/2020
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 489bea391897c0924690719343bdf5febc37f35f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350685"
---
# <a name="install-r-packages-with-sqlmlutils"></a>Instalación de paquetes de R con sqlmlutils

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En este artículo se describe cómo usar las funciones del paquete [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) para instalar paquetes de R en una instancia de [Machine Learning Services en SQL Server](../sql-server-machine-learning-services.md) y en [Clústeres de macrodatos](../../big-data-cluster/machine-learning-services.md). Los paquetes que se instalen podrán usarse en los scripts de R que se ejecutan en la base de datos mediante la instrucción T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

> [!NOTE]
> El paquete **sqlmlutils** que se describe en este artículo se usa para agregar paquetes de R a SQL Server 2019 o versiones posteriores. Respecto a SQL Server 2017 y versiones anteriores, vea [Instalación de paquetes con herramientas de R](./install-r-packages-standard-tools.md?view=sql-server-2017&preserve-view=true).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
En este artículo se describe cómo usar las funciones del paquete [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) para instalar paquetes de R en una instancia de [Machine Learning Services en Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview). Los paquetes que se instalen podrán usarse en los scripts de R que se ejecutan en la base de datos mediante la instrucción T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
::: moniker-end

## <a name="prerequisites"></a>Prerrequisitos

- Instale [R](https://www.r-project.org) y [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) en el equipo cliente que usa para conectarse a SQL Server. Puede usar cualquier IDE de R para ejecutar scripts, pero en este artículo se da por supuesto que es RStudio.

  La versión de R en el equipo cliente debe coincidir con la del servidor, y los paquetes que instale deben ser compatibles con la versión de R que tenga.
  Para obtener información sobre la versión de R que se incluye con cada versión de SQL Server, consulte [Versiones de Python y R](../sql-server-machine-learning-services.md#versions).
  
  Para comprobar la versión de R en una instancia concreta de SQL Server, use el siguiente comando de T-SQL.

  ```sql
  EXECUTE sp_execute_external_script @language = N'R'
   , @script = N'print(R.version)'
  ```

- Instale [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) en el equipo cliente que usa para conectarse a SQL Server. Puede usar otras herramientas de consulta o administración de bases de datos, pero en este artículo se da por supuesto que emplea Azure Data Studio.

### <a name="other-considerations"></a>Otras consideraciones

- La instalación del paquete es específica de la instancia, la base de datos y el usuario de SQL especificados en la información de conexión proporcionada a **sqlmlutils**. Para usar el paquete en varias instancias o bases de datos de SQL, o bien para usuarios diferentes, deberá instalar el paquete para cada uno. La excepción es que si un miembro de `dbo` instala el paquete, este será *público* y se compartirá con todos los usuarios. Si un usuario instala una versión más reciente de un paquete público, este no se verá afectado, pero ese usuario tendrá acceso a la versión más reciente.

- El script de R que se ejecuta en SQL Server solo puede usar los paquetes instalados en la biblioteca de instancia predeterminada. SQL Server no puede cargar paquetes de bibliotecas externas, aunque la biblioteca esté en el mismo equipo. Esto incluye las bibliotecas de R instaladas con otros productos de Microsoft.

- En un entorno de SQL Server protegido, probablemente le interese evitar lo siguiente:
  - Paquetes que requieren acceso a la red
  - Paquetes que requieren acceso con privilegios elevados al sistema de archivos
  - Paquetes que se usan para el desarrollo web u otras tareas que no obtienen ningún beneficio al ejecutarse en SQL Server

## <a name="install-sqlmlutils-on-the-client-computer"></a>Instalación de sqlmlutils en el equipo cliente

Para usar **sqlmlutils**, primero debe instalarlo en el equipo cliente que usa para conectarse a SQL Server.

El paquete **sqlmlutils** depende del paquete **odbc** y **odbc** depende de otros paquetes. En los procedimientos siguientes se instalan todos estos paquetes en el orden correcto.

### <a name="install-sqlmlutils-online"></a>Instalación de sqlmlutils en línea

Si el equipo cliente tiene acceso a Internet, puede descargar e instalar **sqlmlutils** y sus paquetes dependientes en línea.

1. Descargue el archivo **sqlmlutils** más reciente (`.zip` para Windows, `.tar.gz` para Linux) de https://github.com/Microsoft/sqlmlutils/tree/master/R/dist en el equipo cliente. No expanda el archivo.

1. Abra un **símbolo del sistema**, y ejecute los comandos siguientes para instalar los paquetes **odbc** y **sqlmlutils**. Sustituya la ruta de acceso al archivo **sqlmlutils** que descargó. El paquete **odbc** se ha encontrado en línea y se ha instalado.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

### <a name="install-sqlmlutils-offline"></a>Instalación de sqlmlutils sin conexión

Si el equipo cliente no tiene conexión a Internet, tendrá que descargar los paquetes **odbc** y **sqlmlutils** con antelación mediante un equipo que sí tenga acceso a Internet. Después, puede copiar los archivos en una carpeta en el equipo cliente e instalar los paquetes sin conexión.

El paquete **odbc** tiene varios paquetes dependientes, y resulta complicado identificar todas las dependencias de un paquete. Recomendamos usar [**miniCRAN**](https://andrie.github.io/miniCRAN/) para crear una carpeta de repositorio local para el paquete que incluya todos los paquetes dependientes.
Para obtener más información, consulte [Creación de un repositorio de paquete de R local mediante miniCRAN](create-a-local-package-repository-using-minicran.md).

El paquete de **sqlmlutils** consta de un solo archivo que se puede copiar e instalar en el equipo cliente.

En un equipo con acceso a Internet:

1. Instale **miniCRAN**. Para obtener más información, consulte [Instalación de miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran).

1. En RStudio, ejecute el siguiente script de R para crear un repositorio local del paquete **odbc**. En este ejemplo se da por supuesto que el repositorio se creará en la carpeta `odbc`.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   Para el valor `Rversion`, use la versión de R instalada en SQL Server. Para comprobar la versión instalada, use el siguiente comando de T-SQL.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Descargue el archivo **sqlmlutils** más reciente (`.zip` para Windows, `.tar.gz` para Linux) de [https://github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist). No expanda el archivo.

1. Copie toda la carpeta del repositorio **odbc** y el archivo **sqlmlutils** en el equipo cliente.

En el equipo cliente que usa para conectarse a SQL Server:

1. Abra un símbolo del sistema.

1. Ejecute los comandos siguientes para instalar **odbc** y, después, **sqlmlutils**. Sustituya las rutas de acceso completas a la carpeta del repositorio **odbc** y el archivo **sqlmlutils** que ha copiado en este equipo.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

## <a name="add-an-r-package-on-sql-server"></a>Adición de un paquete de R en SQL Server

En el ejemplo siguiente, agregará el paquete de [**glue**](https://cran.r-project.org/web/packages/glue/) a SQL Server.

### <a name="add-the-package-online"></a>Adición del paquete en línea

Si el equipo cliente que usa para conectarse a SQL Server tiene acceso a Internet, puede usar **sqlmlutils** para buscar el paquete de **glue** y las dependencias a través de Internet y, después, instalar el paquete en una instancia de SQL Server de forma remota.

1. En el equipo cliente, abra RStudio y cree un nuevo archivo de **script de R**.

1. Use el siguiente script de R para instalar el paquete de **glue** mediante **sqlmlutils**. Sustituya la información de conexión de base de datos de SQL Server propia.

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server   = "server",
     database = "database",
     uid      = "username",
     pwd      = "password")

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
   ```

   > [!TIP]
   > El **ámbito** puede ser **PÚBLICO** o **PRIVADO**. El ámbito público es útil para que el administrador de bases de datos instale paquetes que todos los usuarios pueden usar. El ámbito privado hace que el paquete esté disponible solo para el usuario que lo instala. Si no especifica el ámbito, el ámbito predeterminado es **PRIVADO**.

### <a name="add-the-package-offline"></a>Adición del paquete sin conexión

Si el equipo cliente no tiene una conexión a Internet, puede utilizar **miniCRAN** para descargar el paquete de **glue** mediante un equipo que no tenga acceso a Internet. Después, copie el paquete en el equipo cliente en el que puede instalar el paquete sin conexión.
Consulte [Instalación de miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran) para obtener información sobre cómo instalar **miniCRAN**.

En un equipo con acceso a Internet:

1. Ejecute el siguiente script de R para crear un repositorio local para **glue**. En este ejemplo, la carpeta del repositorio se crea en `c:\downloads\glue`.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   Para el valor `Rversion`, use la versión de R instalada en SQL Server. Para comprobar la versión instalada, use el siguiente comando de T-SQL.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Copie toda la carpeta del repositorio de **glue** (`c:\downloads\glue`) en el equipo cliente. Por ejemplo, cópiela en la carpeta `c:\temp\packages\glue`.

En el equipo cliente:

1. Abra RStudio y cree un nuevo archivo de **script de R**.

1. Use el siguiente script de R para instalar el paquete de **glue** mediante **sqlmlutils**. Sustituya su propia información de conexión de base de datos de SQL Server (si no usa la autenticación de Windows, agregue los parámetros `uid` y `pwd`).

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server= "yourserver",
     database = "yourdatabase")
   localRepo = "c:/temp/packages/glue"

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC", repos=paste0("file:///",localRepo))
   ```

   > [!TIP]
   > El **ámbito** puede ser **PÚBLICO** o **PRIVADO**. El ámbito público es útil para que el administrador de bases de datos instale paquetes que todos los usuarios pueden usar. El ámbito privado hace que el paquete esté disponible solo para el usuario que lo instala. Si no especifica el ámbito, el ámbito predeterminado es **PRIVADO**.

## <a name="use-the-package"></a>Uso del paquete

Una vez instalado el paquete de **glue**, puede usarlo en un script de R en SQL Server con el comando T-SQL **sp_execute_external_script**.

1. Abra Azure Data Studio y conéctese a la base de datos de SQL Server.

1. Ejecute el siguiente comando:

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
       , @script = N'
   library(glue)

   name <- "Fred"
   birthday <- as.Date("2020-06-14")
   text <- glue(''My name is {name} '',
   ''and my birthday is {format(birthday, "%A, %B %d, %Y")}.'')

   print(text)
         ';
   ```

    **Resultados**

    ```text
    My name is Fred and my birthday is Sunday, June 14, 2020.
    ```

## <a name="remove-the-package"></a>Eliminación del paquete

Si desea eliminar el paquete de **glue**, ejecute el siguiente script de R. Use la misma variable de **conexión** que definió anteriormente.

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

## <a name="more-sqlmlutils-functions"></a>Más funciones de sqlmlutils

El paquete **sqlmlutils** contiene una serie de funciones para administrar paquetes de R, así como para crear, administrar y ejecutar consultas y procedimientos almacenados en SQL Server. Para obtener más información, vea el [archivo Léame de sqlmlutils para R](https://github.com/microsoft/sqlmlutils/tree/master/R).

Para obtener información sobre cualquier función de **sqlmlutils**, use la función **help** de R o **?** . . Por ejemplo:

```R
library(sqlmlutils)
help("sql_install.packages")
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre los paquetes de R instalados, consulte [Obtener información de paquetes de R](r-package-information.md).
- Para obtener ayuda para trabajar con paquetes de R, consulte [Sugerencias para usar paquetes de R](tips-for-using-r-packages.md).
- Para obtener información sobre la instalación de paquetes de Python, consulte [Instalación de paquetes de Python con PIP](install-additional-python-packages-on-sql-server.md).
- Para obtener más información sobre SQL Server Machine Learning Services, consulte [¿Qué es SQL Server Machine Learning Services (Python y R)?](../sql-server-machine-learning-services.md)