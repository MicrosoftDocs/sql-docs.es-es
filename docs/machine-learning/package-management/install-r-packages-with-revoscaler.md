---
title: Uso de RevoScaleR para instalar paquetes de R
description: Aprenda a usar las funciones de RevoScaleR para instalar paquetes de R en SQL Server con Machine Learning Services o R Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 99750753f7b74070ecb3bc8217d72b91255a0962
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082616"
---
# <a name="use-revoscaler-to-install-r-packages"></a>Uso de RevoScaleR para instalar paquetes de R

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

En este artículo se describe cómo usar las funciones de [RevoScaleR](../r/ref-r-revoscaler.md) (versión 9.0.1 y posteriores) para instalar paquetes de R en SQL Server con Machine Learning Services o R Services. Las funciones RevoScaleR las pueden usar los usuarios remotos y no administradores para instalar paquetes en SQL Server sin acceso directo al servidor.

::: moniker range="=sql-server-2016"
> [!NOTE]
> Los clientes de SQL Server R Services deben realizar una [actualización de componentes](../install/upgrade-r-and-python.md) para obtener las funciones de administración de paquetes de RevoScaleR. Para obtener instrucciones sobre cómo recuperar la versión y el contenido del paquete, vea [Obtención de información de paquetes de R](../package-management/r-package-information.md).
::: moniker-end

## <a name="revoscaler-functions-for-package-management"></a>Funciones de RevoScaleR para administración de paquetes

En la tabla siguiente se describen las funciones que se usan para la instalación y administración de paquetes de R.

| Función | Descripción |
|----------|-------------|
| [rxSqlLibPaths](/machine-learning-server/r-reference/revoscaler/rxsqllibpaths) | Determina la ruta de acceso de la biblioteca de instancias en el servidor SQL Server remoto. |
| [rxFindPackage](/machine-learning-server/r-reference/revoscaler/rxfindpackage) | Obtiene la ruta de acceso de uno o varios paquetes en el servidor SQL Server remoto. |
| [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) | Llame a esta función desde un cliente R remoto para instalar paquetes en un contexto de proceso de SQL Server, ya sea desde un repositorio especificado o leyendo paquetes comprimidos guardados de forma local. Esta función comprueba si hay dependencias y se asegura de que se puedan instalar todos los paquetes relacionados en SQL Server, como la instalación de paquetes de R en el contexto de proceso local. Para usar esta opción, debe tener habilitada la administración de paquetes en el servidor y en la base de datos. Los entornos de cliente y servidor deben tener la misma versión de RevoScaleR. |
| [rxInstalledPackages](/machine-learning-server/r-reference/revoscaler/rxinstalledpackages) | Obtiene una lista de los paquetes instalados en el contexto de proceso especificado. |
| [rxSyncPackages](/machine-learning-server/r-reference/revoscaler/rxsyncpackages) | Copia información acerca de una biblioteca de paquetes entre el sistema de archivos y la base de datos para el contexto de proceso especificado. |
| [rxRemovePackages](/machine-learning-server/r-reference/revoscaler/rxremovepackages) | Quita los paquetes en el contexto de proceso especificado. También calcula las dependencias y se asegura de que se quiten los paquetes que otros paquetes de SQL Server ya no usan, con el objetivo de liberar recursos. |

## <a name="prerequisites"></a>Prerequisites

+ Administración remota habilitada en SQL Server. Para más información, vea [Habilitación de la administración de paquetes de R remota en SQL Server](r-package-how-to-enable-or-disable.md).

+ Las versiones de RevoScaleR son las mismas en los entornos de cliente y servidor. Para más información, vea [Obtener información sobre paquetes de R](../package-management/r-package-information.md).

+ Tiene permiso para conectarse al servidor y a una base de datos, y para ejecutar comandos de R. Debe ser miembro de un rol de base de datos que le permite instalar paquetes en la instancia y la base de datos especificadas.

  + Los usuarios del **ámbito compartido** pueden instalarse por los usuarios que pertenecen al rol `rpkgs-shared` en una base de datos especificada. Todos los usuarios de este rol pueden desinstalar paquetes compartidos.

  + Los usuarios del **ámbito privado** pueden instalarse por cualquier usuario que pertenezca al rol `rpkgs-private` en una base de datos. Sin embargo, los usuarios solo pueden ver y desinstalar sus propios paquetes.

  + Los propietarios de bases de datos pueden trabajar con paquetes compartidos o privados.

## <a name="client-connections"></a>Conexiones de cliente

Una estación de trabajo cliente puede ser [Microsoft R Client](/machine-learning-server/r-client/install-on-windows) o [Microsoft Machine Learning Server](/machine-learning-server/install/machine-learning-server-windows-install) (los científicos de datos suelen usar la edición gratuita para desarrolladores) en la misma red.

Al llamar a las funciones de administración de paquetes desde un cliente remoto de R, debe crear primero un objeto de contexto de proceso mediante la función [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver). A partir de ese momento, para cada función de administración de paquetes que use, pase el contexto de proceso como argumento.

La identidad del usuario se especifica normalmente al establecer el contexto de proceso. Si no especifica un nombre de usuario y una contraseña al crear el contexto de proceso, se usa la identidad del usuario que ejecuta el código de R.

1. En una línea de comandos de R, defina una cadena de conexión para la instancia y la base de datos.
2. Use el constructor [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver) para definir un contexto de proceso de SQL Server, usando la cadena de conexión.

    ```R
    sqlcc <- RxInSqlServer(connectionString = myConnString, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```
3. Cree una lista de los paquetes que desea instalar y guarde la lista en una variable de cadena.

    ```R
    packageList <- c("e1071", "mice")
    ```

4. Llame a [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) y pase el contexto de proceso y la variable de cadena que contiene los nombres de paquete.

    ```R
    rxInstallPackages(pkgs = packageList, verbose = TRUE, computeContext = sqlcc)
    ```

    Si se requieren paquetes dependientes, también se instalan, suponiendo que haya una conexión a Internet disponible en el cliente.
    
    Los paquetes se instalan con las credenciales del usuario que realiza la conexión, en el ámbito predeterminado para ese usuario.

## <a name="call-package-management-functions-in-stored-procedures"></a>Llamada a funciones de administración de paquetes en procedimientos almacenados

Puede ejecutar funciones de administración de paquetes dentro de `sp_execute_external_script`. Al hacerlo, la función se ejecuta utilizando el contexto de seguridad del autor de la llamada del procedimiento almacenado.

## <a name="examples"></a>Ejemplos

En esta sección se ofrecen ejemplos de uso de estas funciones desde un cliente remoto al conectarse a una instancia o base de datos de SQL Server como contexto de proceso.

Para todos los ejemplos, debe proporcionar una cadena de conexión o un contexto de proceso, lo cual requiere una cadena de conexión. En este ejemplo se proporciona una manera de crear un contexto de proceso para SQL Server:

```R
instance_name <- "computer-name/instance-name";
database_name <- "TestDB";
sqlWait= TRUE;
sqlConsoleOutput <- TRUE;
connString <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
sqlcc <- RxInSqlServer(connectionString = connString, wait = sqlWait, consoleOutput = sqlConsoleOutput, numTasks = 4);
```

Dependiendo de dónde se encuentre el servidor y del modelo de seguridad, es posible que deba proporcionar una especificación de dominio y subred en la cadena de conexión, o bien usar un inicio de sesión de SQL. Por ejemplo:

```R
connStr <- "Driver=SQL Server;Server=myserver.financeweb.contoso.com;Database=Finance;Uid=RUser1;Pwd=RUserPassword"
```

### <a name="get-package-path-on-a-remote-sql-server-compute-context"></a>Obtención de una ruta de acceso de paquete en un contexto de proceso de SQL Server

En este ejemplo se obtiene la ruta de acceso del paquete **RevoScaleR** en el contexto del proceso, `sqlcc`.

```R
sqlPackagePaths <- rxFindPackage(package = "RevoScaleR", computeContext = sqlcc)
print(sqlPackagePaths)
```

**Resultados**

"C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library/RevoScaleR"

> [!TIP]
> Si ha habilitado la opción para ver la salida de la consola SQL, puede obtener mensajes de estado de la función que precede a la instrucción `print`. Una vez que haya terminado de probar el código, establezca `consoleOutput` en FALSE en el constructor de contexto de proceso para eliminar los mensajes.

### <a name="get-locations-for-multiple-packages"></a>Obtener la ubicación de varios paquetes

En el ejemplo siguiente se obtienen las rutas de acceso de los paquetes **RevoScaleR** y **lattice** en el contexto de proceso, `sqlcc`. Para obtener información sobre varios paquetes, pase un vector de cadena que contenga el nombre de los paquetes.

```R
packagePaths <- rxFindPackage(package = c("RevoScaleR", "lattice"), computeContext = sqlcc)
print(packagePaths)
```

### <a name="get-package-versions-on-a-remote-compute-context"></a>Obtención de versiones de paquete en un contexto de proceso remoto

Ejecute este comando en una consola de R para obtener el número de compilación y los números de versión de los paquetes instalados en el contexto de proceso, *sqlServer*.

```R
sqlPackages <- rxInstalledPackages(fields = c("Package", "Version", "Built"), computeContext = sqlServer)
```

### <a name="install-a-package-on-sql-server"></a>Instalar un paquete en SQL Server

En este ejemplo se instala el paquete **forecast** y sus dependencias en el contexto de proceso.

  ```R
  pkgs <- c("forecast")
  rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="remove-a-package-from-sql-server"></a>Quitar un paquete de SQL Server

En este ejemplo se quita el paquete **forecast** y sus dependencias del contexto de proceso.

  ```R
  pkgs <- c("forecast")
  rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="synchronize-packages-between-database-and-file-system"></a>Sincronización de paquetes entre la base de datos y el sistema de archivos

En el ejemplo siguiente se comprueba la base de datos **TestDB** y se determina si todos los paquetes están instalados en el sistema de archivos. Si faltan algunos paquetes, se instalan en el sistema de archivos.

```R
# Instantiate the compute context
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )

# Synchronize the packages in the file system for all scopes and users
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

La sincronización de paquetes funciona en cada base de datos y para cada usuario. Para obtener más información, consulte [Sincronización de paquete de R para SQL Server](package-install-uninstall-and-sync.md).

### <a name="use-a-stored-procedure-to-list-packages-in-sql-server"></a>Uso de un procedimiento almacenado para enumerar paquetes en SQL Server

Ejecute este comando desde Management Studio u otra herramienta que admita T-SQL para obtener una lista de los paquetes instalados en la instancia actual, utilizando `rxInstalledPackages` en un procedimiento almacenado.

```sql
EXEC sp_execute_external_script 
  @language=N'R', 
  @script=N'
    myPackages <- rxInstalledPackages();
    OutputDataSet <- as.data.frame(myPackages);
    '
```

La función `rxSqlLibPaths` se puede utilizar para determinar la biblioteca activa utilizada por SQL Server Machine Learning Services. Este script solo puede devolver la ruta de acceso de la biblioteca del servidor actual. 

```sql
declare @instance_name nvarchar(100) = @@SERVERNAME, @database_name nvarchar(128) = db_name();
exec sp_execute_external_script 
  @language = N'R',
  @script = N'
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    .libPaths(rxSqlLibPaths(connStr));
    print(.libPaths());
  ', 
  @input_data_1 = N'', 
  @params = N'@instance_name nvarchar(100), @database_name nvarchar(128)',
  @instance_name = @instance_name, 
  @database_name = @database_name;
```

## <a name="see-also"></a>Consulte también

+ [Habilitación de la administración de paquetes de R remota](r-package-how-to-enable-or-disable.md)
+ [Sincronización de paquetes de R](package-install-uninstall-and-sync.md)
+ [Sugerencias para usar paquetes de R](tips-for-using-r-packages.md)
+ [Obtención de información de paquetes de R](../package-management/r-package-information.md)