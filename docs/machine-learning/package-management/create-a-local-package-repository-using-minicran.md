---
title: Creación de un repositorio con miniCRAN
titleSuffix: SQL machine learning
description: Obtenga información sobre cómo instalar paquetes de R sin conexión mediante el paquete miniCRAN para crear un repositorio local de paquetes y dependencias.
ms.prod: sql
ms.technology: machine-learning
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: f93a1180f25f764330db44306457e775961bc0da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350697"
---
# <a name="create-a-local-r-package-repository-using-minicran"></a>Creación de un repositorio de paquetes de R local mediante miniCRAN
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

En este artículo se describe cómo instalar paquetes de R sin conexión mediante [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) para crear un repositorio local de paquetes y dependencias. **miniCRAN** identifica y descarga paquetes y dependencias en una única carpeta que se copia en otros equipos para la instalación de paquetes de R sin conexión.

Puede especificar uno o más paquetes, y **miniCRAN** lee de forma recursiva el árbol de dependencias para estos paquetes. A continuación, descarga solo los paquetes enumerados y sus dependencias de CRAN o repositorios similares.

Una vez terminado, **miniCRAN** crea un repositorio internamente coherente que consta de los paquetes seleccionados y de todas las dependencias necesarias. Puede trasladar este repositorio local al servidor y pasar a instalar los paquetes sin conexión a Internet.

Los usuarios con experiencia en R suelen buscar la lista de paquetes dependientes en el archivo DESCRIPTION de un paquete descargado. Sin embargo, los paquetes enumerados en **Importaciones** pueden tener dependencias de segundo nivel. Por esta razón, recomendamos **miniCRAN** para ensamblar la colección completa de paquetes necesarios.

## <a name="why-create-a-local-repository"></a>Por qué crear un repositorio local

El objetivo de crear un repositorio de paquetes local es proporcionar una ubicación única que un administrador de servidor u otros usuarios de la organización pueden usar para instalar nuevos paquetes de R en un servidor, especialmente uno que no tenga acceso a Internet. Después de crear el repositorio, puede modificarlo agregando nuevos paquetes o actualizando la versión de los paquetes existentes.

Los repositorios de paquetes son útiles en estos escenarios:

- **Seguridad**: muchos usuarios de R están acostumbrados a descargar e instalar nuevos paquetes de R a discreción, desde CRAN o desde uno de sus sitios reflejados. Sin embargo, por motivos de seguridad, los servidores de producción que ejecutan [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] normalmente no tienen conexión a Internet.

- **Instalación sin conexión más sencilla**: para instalar un paquete en un servidor sin conexión, es necesario que también descargue todas las dependencias de paquete. El uso de miniCRAN hace que sea más fácil obtener todas las dependencias en el formato correcto y evitar errores de dependencia.

- **Administración de versiones mejorada**: en un entorno de varios usuarios, existen buenas razones para evitar la instalación sin restricciones de varias versiones de paquetes en el servidor. Use un repositorio local para proporcionar un conjunto coherente de paquetes para los usuarios.

## <a name="install-minicran"></a>Instalación de miniCRAN

El propio paquete de **miniCRAN** depende de otros 18 paquetes de CRAN, entre los que se encuentra el paquete de **RCurl**, que tiene una dependencia de sistema en el paquete de **curl-devel**. De forma similar, el paquete de **XML** tiene una dependencia en **libxml2-devel**. Para resolver las dependencias, recomendamos compilar el repositorio local inicialmente en una máquina con acceso a Internet completo.

Ejecute los comandos siguientes en un equipo con una base de R, herramientas de R y conexión a Internet. Se supone que este no es su equipo con SQL Server. Mediante los comandos siguientes, se instalan los paquetes de **miniCRAN** e **igraph**. En este ejemplo se comprueba si el paquete ya está instalado, pero se pueden omitir las instrucciones `if` e instalar los paquetes directamente.

```R
if(!require("miniCRAN")) install.packages("miniCRAN") 
if(!require("igraph")) install.packages("igraph") 
library("miniCRAN")
```

## <a name="set-the-cran-mirror-and-mran-snapshot"></a>Establecimiento del reflejo de CRAN y de la instantánea de informe de MRAN

Especifique un sitio reflejado que se usará para obtener paquetes. Por ejemplo, puede usar el sitio MRAN o cualquier otro sitio de su región que contenga los paquetes que necesite. Si se produce un error en una descarga, pruebe con otro sitio reflejado.

```R
CRAN_mirror <- c(CRAN = "https://cran.cnr.berkeley.edu")
```

## <a name="create-a-local-folder"></a>Creación de una carpeta local

Cree una carpeta local en la que almacenar los paquetes recopilados. Si repite esta operación con frecuencia, tal vez sea conveniente usar un nombre descriptivo, como "miniCRANZooPackages" o "miniCRANMyRPackageV2".

Especifique la carpeta como repositorio local. La sintaxis de R usa una barra diagonal para los nombres de ruta de acceso, de manera opuesta a las convenciones de Windows.

```R
local_repo <- "C:/miniCRANZooPackages"
```

## <a name="add-packages-to-the-local-repo"></a>Adición de paquetes al repositorio local

Después de instalar y cargar **miniCRAN**, cree una lista en que se especifiquen los paquetes adicionales que desea descargar.

**No** agregue dependencias a esta lista inicial. El paquete de **igraph** que usa **miniCRAN** genera automáticamente la lista de dependencias. Para obtener más información sobre cómo utilizar el gráfico de dependencias generado, consulte [Uso de miniCRAN para identificar las dependencias de paquete](https://cran.r-project.org/web/packages/miniCRAN/vignettes/miniCRAN-dependency-graph.html).

1. Agregue los paquetes de destino "Zoo" y "Forecast" a una variable.

    ```R
    pkgs_needed <- c("zoo", "forecast")
    ```

2. Opcionalmente, trace el gráfico de dependencias. Esto no es necesario, pero puede proporcionar información.

    ```R
    plot(makeDepGraph(pkgs_needed))
    ```

3. Cree el repositorio local. Asegúrese de cambiar la versión de R, si es necesario, a la versión instalada en la instancia de SQL Server. Si ha actualizado algún componente, es posible que la versión sea más reciente que la versión original. Para más información, vea [Obtener información sobre paquetes de R](../package-management/r-package-information.md).

    ```R
    pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);
    makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.3");
    ```

   A partir de esta información, el paquete de miniCRAN crea la estructura de carpetas necesaria para copiar los paquetes en [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] más adelante.

En este punto, debe tener una carpeta que contenga los paquetes que necesita y los paquetes adicionales necesarios. La carpeta debe contener una colección de paquetes comprimidos. No descomprima los paquetes ni cambie el nombre de los archivos.

Opcionalmente, ejecute el código siguiente para enumerar los paquetes contenidos en el repositorio local de miniCRAN.

```R
pdb <- as.data.frame(pkgAvail(local_repo, type = "win.binary", Rversion = "3.3"), stringsAsFactors = FALSE);
head(pdb);
pdb$Package;
pdb[, c("Package", "Version", "License")]
```

## <a name="add-packages-to-the-instance-library"></a>Adición de paquetes a la biblioteca de instancias

Una vez que tenga un repositorio local con los paquetes que necesita, mueva el repositorio de paquetes al equipo con SQL Server. En el procedimiento siguiente se describe cómo instalar los paquetes mediante herramientas de R.

::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> El método recomendado para instalar paquetes es usar **sqlmlutils**. Consulte [Instalación de nuevos paquetes de R con sqlmlutils](install-additional-r-packages-on-sql-server.md).
::: moniker-end

1. Copie la carpeta que contiene el repositorio de miniCRAN, en su totalidad, en el servidor donde va a instalar los paquetes. Normalmente, la carpeta tiene esta estructura: 

   `<miniCRAN root>/bin/windows/contrib/version/<all packages>`

   En este procedimiento, se supone que una carpeta está fuera de la unidad raíz.

2. Abra una herramienta de R asociada a la instancia (por ejemplo, puede usar Rgui.exe). Haga clic con el botón derecho y seleccione **Ejecutar como administrador** para permitir que la herramienta realice actualizaciones en el sistema.

   ::: moniker range="=sql-server-2016"
   - Por ejemplo, la ubicación de archivo predeterminada para RGUI es `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   - Por ejemplo, la ubicación de archivo para RGUI es `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   - Por ejemplo, la ubicación de archivo para RGUI es `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

3. Obtenga la ruta de acceso de la biblioteca de instancias y agréguela a la lista de rutas de acceso de la biblioteca.

   ::: moniker range="=sql-server-2016"
   Por ejemplo,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL13.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Por ejemplo,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   Por ejemplo,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL15.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

4. Especifique la nueva ubicación en el servidor en que ha copiado el repositorio de **miniCRAN** como `server_repo`.

    En este ejemplo, se supone que ha copiado el repositorio en una carpeta temporal del servidor.

    ```R
    inputlib <- "C:/miniCRANZooPackages"
    ```

5. Dado que está trabajando en una nueva área de trabajo de R en el servidor, también debe proporcionar la lista de paquetes que se van a instalar.

    ```R
    mypackages <- c("zoo", "forecast")
    ```

6. Instale los paquetes y proporcione la ruta de acceso a la copia local del repositorio de miniCRAN.

    ```R
    install.packages(mypackages, repos = file.path("file://", normalizePath(inputlib, winslash = "/")), lib = outputlib, type = "win.binary", dependencies = TRUE);
    ```

7. Desde la biblioteca de instancias, puede ver los paquetes instalados con un comando similar al siguiente:

    ```R
    installed.packages()
    ```

## <a name="see-also"></a>Consulte también

+ [Obtención de información de paquetes de R](../package-management/r-package-information.md)
+ [Tutoriales de R](../tutorials/r-tutorials.md)