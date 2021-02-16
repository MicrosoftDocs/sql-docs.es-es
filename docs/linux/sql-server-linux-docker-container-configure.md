---
title: Configuración y personalización de contenedores de Docker de SQL Server
description: Comprenda las distintas formas de personalizar contenedores de Docker de SQL Server y cómo puede configurarlos en función de los requisitos
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 2fa5cc3b5fe8ecc5a2b3ed87444474f1de54b022
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273076"
---
# <a name="configure-and-customize-sql-server-docker-containers"></a>Configuración y personalización de contenedores de Docker de SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se explica cómo puede configurar y personalizar contenedores de Docker de SQL Server mediante la conservación de los datos, la transferencia de archivos desde y hacia contenedores, y el cambio de la configuración predeterminada.

## <a name="create-a-customized-container"></a><a id="customcontainer"></a> Creación de un contenedor personalizado

Es posible crear su propio [Dockerfile](https://docs.docker.com/engine/reference/builder/#usage) para crear un contenedor de SQL Server personalizado. Para obtener más información, vea [una demostración que combina SQL Server y una aplicación de nodo](https://github.com/twright-msft/mssql-node-docker-demo-app). Si crea su propio Dockerfile, tenga en cuenta el proceso en primer plano, ya que este proceso controla la vida del contenedor. Si existe, se apagará el contenedor. Por ejemplo, si desea ejecutar un script e iniciar SQL Server, asegúrese de que el proceso de SQL Server sea el más adecuado. Todos los demás comandos se ejecutan en segundo plano. El comando siguiente ilustra esto dentro de un Dockerfile:

```bash
/usr/src/app/do-my-sql-commands.sh & /opt/mssql/bin/sqlservr
```

Si en el ejemplo anterior ha invertido los comandos, el contenedor se cerrará cuando se complete el script do-my-sql-commands.sh.

## <a name="persist-your-data"></a><a id="persist"></a> Conservación de los datos

Los cambios de configuración de SQL Server y los archivos de base de datos se conservan en el contenedor aunque se reinicie el contenedor con `docker stop` y `docker start`. Pero si quita el contenedor con `docker rm`, se elimina todo el contenedor, incluidos SQL Server y las bases de datos. En la siguiente sección se explica cómo usar **volúmenes de datos** para conservar los archivos de base de datos incluso si se eliminan los contenedores asociados.

> [!IMPORTANT]
> Por SQL Server, es fundamental que comprenda la persistencia de los datos en Docker. Además de la explicación de esta sección, consulte la documentación de Docker sobre [cómo administrar datos en contenedores de Docker](https://docs.docker.com/engine/tutorials/dockervolumes/).

### <a name="mount-a-host-directory-as-data-volume"></a>Montaje de un directorio host como volumen de datos

La primera opción consiste en montar un directorio en el host como un volumen de datos en el contenedor. Para ello, use el comando `docker run` con la marca `-v <host directory>:/var/opt/mssql`. Esto permite restaurar los datos entre las ejecuciones del contenedor.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Esta técnica también le permite compartir y ver los archivos en el host fuera de Docker.

> [!IMPORTANT]
> La asignación de volúmenes de host para **Docker en Windows** no admite actualmente la asignación del directorio completo de `/var/opt/mssql`. Pero puede asignar un subdirectorio, como `/var/opt/mssql/data`, a su equipo host.
>
> En este momento no se admite la asignación de volumen de host para **Docker en Mac** con la imagen de SQL Server en Linux. En su lugar, use contenedores de volúmenes de datos. Esta restricción es específica del directorio `/var/opt/mssql`. La lectura desde un directorio montado funciona correctamente. Por ejemplo, puede montar un directorio host mediante -v en Mac y restaurar una copia de seguridad desde un archivo .bak que resida en el host.

### <a name="use-data-volume-containers"></a>Uso de contenedores de volúmenes de datos

La segunda opción consiste en usar un contenedor de volúmenes de datos. Puede crear un contenedor de volúmenes de datos especificando un nombre de volumen en lugar de un directorio host con el parámetro `-v`. En el ejemplo siguiente se crea un volumen de datos compartidos denominado **sqlvolume**.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

> [!NOTE]
> Esta técnica para crear de forma implícita un volumen de datos en el comando de ejecución no funciona con versiones anteriores de Docker. En ese caso, use los pasos explícitos que se describen en la documentación de Docker, [Creación y montaje de un contenedor de volúmenes de datos](https://docs.docker.com/engine/tutorials/dockervolumes/#creating-and-mounting-a-data-volume-container).

Incluso si detiene y quita este contenedor, el volumen de datos se conserva. Puede verlo con el comando `docker volume ls`.

```command
docker volume ls
```

Si después crea otro contenedor con el mismo nombre de volumen, el nuevo contenedor usará los mismos datos de SQL Server contenidos en el volumen.

Para quitar un contenedor de volúmenes de datos, use el comando `docker volume rm`.

> [!WARNING]
> Si elimina el contenedor de volúmenes de datos, todos los datos de SQL Server del contenedor se eliminarán *de forma permanente*.

### <a name="backup-and-restore"></a>Copia de seguridad y restauración

Además de estas técnicas de contenedor, también puede usar las técnicas estándar de copia de seguridad y restauración de SQL Server. Puede usar archivos de copia de seguridad para proteger los datos o para trasladarlos a otra instancia de SQL Server. Para obtener más información, vea [Copia de seguridad y restauración de bases de datos de SQL Server en Linux](sql-server-linux-backup-and-restore-database.md).

> [!WARNING]
> Si crea copias de seguridad, asegúrese de crear o copiar los archivos de copia de seguridad fuera del contenedor. De lo contrario, si se quita el contenedor, también se eliminarán los archivos de copia de seguridad.

## <a name="copy-files-from-a-container"></a>Copia de archivos desde un contenedor

Para copiar un archivo del contenedor, use el siguiente comando:

```command
docker cp <Container ID>:<Container path> <host path>
```

Puede obtener el id. de contenedor si ejecuta el comando `docker ps -a`.

**Ejemplo**:

::: zone pivot="cs1-bash"
```bash
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog /tmp/errorlog
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

## <a name="copy-files-into-a-container"></a>Copia de archivos en un contenedor

Para copiar un archivo en el contenedor, use el siguiente comando:

```command
docker cp <Host path> <Container ID>:<Container path>
```

**Ejemplo**:

::: zone pivot="cs1-bash"
```bash
docker cp /tmp/mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

## <a name="configure-the-timezone"></a><a id="tz"></a> Configuración de la zona horaria

Para ejecutar SQL Server en un contenedor de Linux con una zona horaria específica, configure la variable de entorno `TZ`. Para buscar el valor de zona horaria correcto, ejecute el comando `tzselect` desde un símbolo del sistema de Bash de Linux:

```command
tzselect
```

Después de seleccionar la zona horaria, `tzselect` muestra una salida similar a la siguiente:

```output
The following information has been given:

        United States
        Pacific

Therefore TZ='America/Los_Angeles' will be used.
```

Puede usar esta información para establecer la misma variable de entorno en el contenedor de Linux. En el ejemplo siguiente se muestra cómo ejecutar SQL Server en un contenedor en la zona horaria de `Americas/Los_Angeles`:

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'A_PASSWORD=<YourStrong!Passw0rd>' \
-p 1433:1433 --name sql1 \
-e 'TZ=America/Los_Angeles'\
-d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 `
-e "TZ=America/Los_Angeles" `
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 ^
-e "TZ=America/Los_Angeles" ^
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
   -p 1433:1433 --name sql1 \
   -e 'TZ=America/Los_Angeles'\
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="change-the-default-file-location"></a><a id="changefilelocation"></a> Cambio de la ubicación predeterminada del archivo

Agregue la variable `MSSQL_DATA_DIR` para cambiar el directorio de datos en el comando `docker run` y, después, monte un volumen en esa ubicación a la que tenga acceso el usuario del contenedor.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Para empezar a trabajar con imágenes de contenedor de SQL Server 2017 en Docker, revise el [inicio rápido](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Para empezar a trabajar con imágenes de contenedor de SQL Server 2019 en Docker, revise el [inicio rápido](quickstart-install-connect-docker.md)

::: moniker-end

- [Implementación y conexión a contenedores de Docker de SQL Server](sql-server-linux-docker-container-deployment.md)

- [Solución de problemas de contenedores de Docker de SQL Server](sql-server-linux-docker-container-troubleshooting.md)

- [Protección de contenedores de Docker de SQL Server](sql-server-linux-docker-container-security.md)
