---
title: Restauración de una base de datos de SQL Server en Docker
description: En este tutorial se muestra cómo restaurar una copia de seguridad de base de datos de SQL Server en un nuevo contenedor de Docker para Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 1bb137243e5a297baae55e72afeaca356e8f4d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347353"
---
# <a name="restore-a-sql-server-database-in-a-linux-docker-container"></a>Restauración de una base de datos de SQL Server en un contenedor de Docker para Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

En este tutorial se muestra cómo mover y restaurar un archivo de copia de seguridad de SQL Server en una imagen de contenedor de Linux de SQL Server 2017 que se ejecuta en Docker.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

En este tutorial se muestra cómo mover y restaurar un archivo de copia de seguridad de SQL Server en una imagen de contenedor de Linux de SQL Server 2019 que se ejecuta en Docker.

::: moniker-end

> [!div class="checklist"]
> * Extraiga y ejecute la imagen de contenedor de Linux de SQL Server más reciente.
> * Copie el archivo de base de datos de Wide World Importers en el contenedor.
> * Restaure la base de datos en el contenedor.
> * Ejecute instrucciones de Transact-SQL para ver y modificar la base de datos.
> * Realice una copia de seguridad de la base de datos modificada.

## <a name="prerequisites"></a>Prerequisites

* Motor de Docker 1.8 o versiones posteriores en cualquier distribución de Linux admitida o Docker para Mac y Windows. Para obtener más información, consulte [Instalar Docker](https://docs.docker.com/engine/installation/).
* Un mínimo de 2 GB de espacio en disco.
* Un mínimo de 2 GB de RAM.
* [Requisitos del sistema para SQL Server en Linux](sql-server-linux-setup.md#system).

## <a name="pull-and-run-the-container-image"></a>Extraer y ejecutar la imagen de contenedor

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Abra un terminal de Bash en Linux/Mac o una sesión de PowerShell con privilegios elevados en Windows.

1. Extraiga la imagen de contenedor de SQL Server 2017 para Linux de Docker Hub.

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   > [!TIP]
   > En este tutorial se proporcionan ejemplos de comandos de Docker para el shell de Bash (Linux/Mac) y PowerShell (Windows).

1. Para ejecutar la imagen de contenedor con Docker, puede usar el siguiente comando:

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   Este comando crea un contenedor de SQL Server 2017 con la edición Developer (valor predeterminado). El puerto **1433** de SQL Server se expone en el host como puerto **1401**. El parámetro `-v sql1data:/var/opt/mssql` opcional crea un contenedor de volúmenes de datos denominado **sql1ddata**. Se usa para almacenar los datos creados por SQL Server.

   > [!IMPORTANT]
   > En este ejemplo se usa un contenedor de volúmenes de datos dentro de Docker. Si en su lugar decide asignar un directorio host, tenga en cuenta que existen limitaciones para este enfoque en Docker para Mac y Windows. Para obtener más información, consulte [Configuración de imágenes de contenedor de SQL Server en Docker](./sql-server-linux-docker-container-configure.md#persist).

1. Para ver los contenedores de Docker, use el comando `docker ps`.

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. Si la columna **ESTADO** muestra el estado **Activo**, esto indica que SQL Server se está ejecutando en el contenedor y que está escuchando en el puerto especificado en la columna **PUERTOS**. Si la columna **ESTADO** de su contenedor de SQL Server muestra **Cerrado**, consulte la [sección Solución de problemas de la guía de configuración](./sql-server-linux-docker-container-troubleshooting.md).

  ```bash
  $ sudo docker ps -a

  CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
  941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
  ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Abra un terminal de Bash en Linux/Mac o una sesión de PowerShell con privilegios elevados en Windows.

1. Extraiga la imagen de contenedor de SQL Server 2019 para Linux de Docker Hub.

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   > [!TIP]
   > En este tutorial se proporcionan ejemplos de comandos de Docker para el shell de Bash (Linux/Mac) y PowerShell (Windows).

1. Para ejecutar la imagen de contenedor con Docker, puede usar el siguiente comando:

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   Este comando crea un contenedor de SQL Server 2019 con la edición Developer (valor predeterminado). El puerto **1433** de SQL Server se expone en el host como puerto **1401**. El parámetro `-v sql1data:/var/opt/mssql` opcional crea un contenedor de volúmenes de datos denominado **sql1ddata**. Se usa para almacenar los datos creados por SQL Server.

1. Para ver los contenedores de Docker, use el comando `docker ps`.

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. Si la columna **ESTADO** muestra el estado **Activo**, esto indica que SQL Server se está ejecutando en el contenedor y que está escuchando en el puerto especificado en la columna **PUERTOS**. Si la columna **ESTADO** de su contenedor de SQL Server muestra **Cerrado**, consulte la [sección Solución de problemas de la guía de configuración](./sql-server-linux-docker-container-troubleshooting.md).

   ```bash
   $ sudo docker ps -a

   CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
   941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
   ```

::: moniker-end

## <a name="change-the-sa-password"></a>Cambiar la contraseña de SA

[!INCLUDE [Change docker password](../includes/sql-server-linux-change-docker-password.md)]

## <a name="copy-a-backup-file-into-the-container"></a>Copia de un archivo de copia de seguridad en el contenedor

En este tutorial se usa la [base de datos de ejemplo Wide World Importers](../samples/wide-world-importers-what-is.md). Siga estos pasos para descargar y copiar el archivo de copia de seguridad de base de datos de Wide World Importers en el contenedor de SQL Server.

1. En primer lugar, use **docker exec** para crear una carpeta de copia de seguridad. El siguiente comando crea un directorio **/var/opt/mssql/backup** dentro del contenedor de SQL Server.

   ```bash
   sudo docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

   ```PowerShell
   docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

1. Luego, descargue el archivo [WideWorldImporters-Full.bak](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0) en el equipo host. Los siguientes comandos llevan al directorio home/user y descargan el archivo de copia de seguridad como **wwi.bak**.

   ```bash
   cd ~
   curl -L -o wwi.bak 'https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak'
   ```

   ```PowerShell
   curl -OutFile "wwi.bak" "https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak"
   ```

1. Use **docker cp** para copiar el archivo de copia de seguridad en el contenedor, en el directorio **/var/opt/mssql/backup**.

   ```bash
   sudo docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

   ```PowerShell
   docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

## <a name="restore-the-database"></a>Restaurar la base de datos

El archivo de copia de seguridad ahora se encuentra dentro del contenedor. Antes de restaurar la copia de seguridad, es importante conocer los nombres de archivo lógicos y los tipos de archivo que hay dentro de la copia de seguridad. Los siguientes comandos de Transact-SQL examinan la copia de seguridad y realizan la restauración con **sqlcmd** en el contenedor.

> [!TIP]
> En este tutorial se usa **sqlcmd** dentro del contenedor, ya que este incluye esta herramienta preinstalada. Pero también puede ejecutar instrucciones de Transact-SQL con otras herramientas de cliente fuera del contenedor, como [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) o [SQL Server Management Studio](sql-server-linux-manage-ssms.md). Para conectarse, use el puerto de host que se ha asignado al puerto 1433 en el contenedor. En este ejemplo es **localhost,1401** en el equipo host y **Host_IP_Address,1401** de forma remota.

1. Ejecute **sqlcmd** dentro del contenedor para enumerar los nombres de archivo lógicos y las rutas de acceso que hay dentro de la copia de seguridad. Esto se hace con la instrucción de Transact-SQL **RESTORE FILELISTONLY**.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost \
      -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE FILELISTONLY FROM DISK = "/var/opt/mssql/backup/wwi.bak"' \
      | tr -s ' ' | cut -d ' ' -f 1-2
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost `
      -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/wwi.bak'"
   ```

   Debería ver un resultado similar al siguiente:

   ```
   LogicalName   PhysicalName
   ------------------------------------------
   WWI_Primary   D:\Data\WideWorldImporters.mdf
   WWI_UserData   D:\Data\WideWorldImporters_UserData.ndf
   WWI_Log   E:\Log\WideWorldImporters.ldf
   WWI_InMemory_Data_1   D:\Data\WideWorldImporters_InMemory_Data_1
   ```

1. Llame al comando **RESTORE DATABASE** para restaurar la base de datos dentro del contenedor. Especifique nuevas rutas de acceso para cada uno de los archivos del paso anterior.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE DATABASE WideWorldImporters FROM DISK = "/var/opt/mssql/backup/wwi.bak" WITH MOVE "WWI_Primary" TO "/var/opt/mssql/data/WideWorldImporters.mdf", MOVE "WWI_UserData" TO "/var/opt/mssql/data/WideWorldImporters_userdata.ndf", MOVE "WWI_Log" TO "/var/opt/mssql/data/WideWorldImporters.ldf", MOVE "WWI_InMemory_Data_1" TO "/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1"'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE DATABASE WideWorldImporters FROM DISK = '/var/opt/mssql/backup/wwi.bak' WITH MOVE 'WWI_Primary' TO '/var/opt/mssql/data/WideWorldImporters.mdf', MOVE 'WWI_UserData' TO '/var/opt/mssql/data/WideWorldImporters_userdata.ndf', MOVE 'WWI_Log' TO '/var/opt/mssql/data/WideWorldImporters.ldf', MOVE 'WWI_InMemory_Data_1' TO '/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1'"
   ```

   Debería ver un resultado similar al siguiente:

   ```
   Processed 1464 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   Processed 33 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   Processed 3862 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Converting database 'WideWorldImporters' from version 852 to the current version 869.
   Database 'WideWorldImporters' running the upgrade step from version 852 to version 853.
   Database 'WideWorldImporters' running the upgrade step from version 853 to version 854.
   Database 'WideWorldImporters' running the upgrade step from version 854 to version 855.
   Database 'WideWorldImporters' running the upgrade step from version 855 to version 856.
   Database 'WideWorldImporters' running the upgrade step from version 856 to version 857.
   Database 'WideWorldImporters' running the upgrade step from version 857 to version 858.
   Database 'WideWorldImporters' running the upgrade step from version 858 to version 859.
   Database 'WideWorldImporters' running the upgrade step from version 859 to version 860.
   Database 'WideWorldImporters' running the upgrade step from version 860 to version 861.
   Database 'WideWorldImporters' running the upgrade step from version 861 to version 862.
   Database 'WideWorldImporters' running the upgrade step from version 862 to version 863.
   Database 'WideWorldImporters' running the upgrade step from version 863 to version 864.
   Database 'WideWorldImporters' running the upgrade step from version 864 to version 865.
   Database 'WideWorldImporters' running the upgrade step from version 865 to version 866.
   Database 'WideWorldImporters' running the upgrade step from version 866 to version 867.
   Database 'WideWorldImporters' running the upgrade step from version 867 to version 868.
   Database 'WideWorldImporters' running the upgrade step from version 868 to version 869.
   RESTORE DATABASE successfully processed 58455 pages in 18.069 seconds (25.273 MB/sec).
   ```

## <a name="verify-the-restored-database"></a>Comprobar la base de datos restaurada

Ejecute la consulta siguiente para mostrar una lista de nombres de bases de datos del contenedor:

```bash
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
   -Q 'SELECT Name FROM sys.Databases'
```

```PowerShell
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
   -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
   -Q "SELECT Name FROM sys.Databases"
```

Debería ver **WideWorldImporters** en la lista de bases de datos.

## <a name="make-a-change"></a>Realización de cambios

Con los pasos siguientes se realiza un cambio en la base de datos.

1. Ejecute una consulta para ver los diez elementos principales de la tabla **Warehouse.StockItems**.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID"
   ```

   Debería ver una lista de identificadores y nombres de elemento:

   ```
   StockItemID StockItemName
   ----------- -----------------
             1 USB missile launcher (Green)
             2 USB rocket launcher (Gray)
             3 Office cube periscope (Black)
             4 USB food flash drive - sushi roll
             5 USB food flash drive - hamburger
             6 USB food flash drive - hot dog
             7 USB food flash drive - pizza slice
             8 USB food flash drive - dim sum 10 drive variety pack
             9 USB food flash drive - banana
            10 USB food flash drive - chocolate bar
   ```

1. Actualice la descripción del primer elemento con la siguiente instrucción **UPDATE**:

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName="USB missile launcher (Dark Green)" WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName='USB missile launcher (Dark Green)' WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   Debería ver un resultado similar al texto siguiente:

   ```
   (1 rows affected)
   StockItemID StockItemName
   ----------- ------------------------------------
             1 USB missile launcher (Dark Green)
   ```

## <a name="create-a-new-backup"></a>Crear una nueva copia de seguridad

Después de haber restaurado la base de datos en un contenedor, puede que también quiera crear periódicamente copias de seguridad de base de datos dentro del contenedor en ejecución. Los pasos siguen un patrón similar a los pasos anteriores, pero en orden inverso.

1. Use el comando de Transact-SQL **BACKUP DATABASE** para crear una copia de seguridad de base de datos en el contenedor. En este tutorial se crea un nuevo archivo de copia de seguridad **wwi_2.bak** en el directorio **/var/opt/mssql/backup** creado previamente.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   Debería ver un resultado similar al siguiente:

   ```
   10 percent processed.
   20 percent processed.
   30 percent processed.
   40 percent processed.
   50 percent processed.
   60 percent processed.
   70 percent processed.
   Processed 1200 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   80 percent processed.
   Processed 3865 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Processed 938 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   100 percent processed.
   BACKUP DATABASE successfully processed 59099 pages in 25.056 seconds (18.427 MB/sec).
   ```

1. Luego, copie el archivo de copia de seguridad del contenedor en el equipo host.

   ```bash
   cd ~
   sudo docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls -l wwi*
   ```

   ```PowerShell
   cd ~
   docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls wwi*
   ```

## <a name="use-the-persisted-data"></a>Uso de los datos almacenados

Además de realizar copias de seguridad de las bases de datos para proteger los datos, puede usar contenedores de volúmenes de datos. Al principio de este tutorial se ha creado el contenedor **sql1** con el parámetro `-v sql1data:/var/opt/mssql`. El contenedor de volúmenes de datos **sql1data** almacena los datos de **/var/opt/mssql** incluso después de quitar el contenedor. En los pasos siguientes se quita por completo el contenedor **sql1** y, luego, se crea un nuevo contenedor, **sql2**, con los datos almacenados.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Detenga el contenedor **sql1**.

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. Quite el contenedor. Esto no elimina el contenedor de volúmenes de datos **sql1data** creado previamente ni los datos almacenados en él.

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. Cree un nuevo contenedor, **sql2**, y vuelva a usar el contenedor de volúmenes de datos **sql1data**.

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

1. La base de datos Wide World Importers está ahora en el nuevo contenedor. Ejecute una consulta para comprobar el cambio anterior que ha realizado.

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > La contraseña de administrador del sistema no es la que ha especificado para el contenedor **sql2**, `MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`. Todos los datos de SQL Server se han restaurado desde **sql1**, incluida la contraseña modificada anteriormente en el tutorial. En efecto, algunas opciones como esta se omiten debido a la restauración de los datos de /var/opt/mssql. Por esta razón, la contraseña es `<YourNewStrong!Passw0rd>`, como se muestra aquí.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Detenga el contenedor **sql1**.

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. Quite el contenedor. Esto no elimina el contenedor de volúmenes de datos **sql1data** creado previamente ni los datos almacenados en él.

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. Cree un nuevo contenedor, **sql2**, y vuelva a usar el contenedor de volúmenes de datos **sql1data**.

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
       --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
       --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

1. La base de datos Wide World Importers está ahora en el nuevo contenedor. Ejecute una consulta para comprobar el cambio anterior que ha realizado.

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > La contraseña de administrador del sistema no es la que ha especificado para el contenedor **sql2**, `MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`. Todos los datos de SQL Server se han restaurado desde **sql1**, incluida la contraseña modificada anteriormente en el tutorial. En efecto, algunas opciones como esta se omiten debido a la restauración de los datos de /var/opt/mssql. Por esta razón, la contraseña es `<YourNewStrong!Passw0rd>`, como se muestra aquí.

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

En este tutorial ha aprendido a realizar una copia de seguridad de una base de datos en Windows y a moverla a un servidor de Linux con SQL Server 2017. Ha aprendido a:

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

En este tutorial ha aprendido a realizar una copia de seguridad de una base de datos en Windows y a moverla a un servidor de Linux con SQL Server 2019. Ha aprendido a:

::: moniker-end

> [!div class="checklist"]
> * Crear imágenes de contenedor de Linux de SQL Server.
> * Copiar copias de seguridad de base de datos de SQL Server en un contenedor.
> * Ejecutar instrucciones de Transact-SQL dentro del contenedor con **sqlcmd**.
> * Crear y extraer archivos de copia de seguridad desde un contenedor.
> * Usar contenedores de volúmenes de datos en Docker para almacenar datos de SQL Server.

Después, revise otros escenarios de configuración y solución de problemas de Docker:

> [!div class="nextstepaction"]
>[Guía de configuración de SQL Server 2017 en Docker](./sql-server-linux-docker-container-deployment.md)