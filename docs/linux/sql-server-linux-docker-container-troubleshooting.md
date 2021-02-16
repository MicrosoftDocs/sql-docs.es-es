---
title: Solución de problemas de contenedores de Docker de SQL Server
description: Explore las distintas técnicas de solución de problemas que puede usar para resolver errores comunes que se ven al usar contenedores de Docker de Linux con imágenes de SQL Server
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: d01a4b20b12dfdf47e813fa51d5862e5ce8f0cb6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273066"
---
# <a name="troubleshooting-sql-server-docker-containers"></a>Solución de problemas de contenedores de Docker de SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se describen los errores comunes que se pueden producir al implementar y usar contenedores de Docker de SQL Server y se proporcionan técnicas de solución de problemas para ayudar a resolverlos.

## <a name="docker-command-errors"></a>Errores de comandos de Docker

Si obtiene errores para los comandos `docker`, asegúrese de que el servicio Docker se está ejecutando e intente ejecutar con permisos elevados.

Por ejemplo, en Linux, podría recibir el siguiente error al ejecutar los comandos `docker`:

```output
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

Si recibe este error en Linux, intente ejecutar los mismos comandos precedidos de `sudo`. Si se produce un error, compruebe que el servicio Docker se está ejecutando e inícielo si es necesario.

```bash
sudo systemctl status docker
sudo systemctl start docker
```

En Windows, compruebe que está iniciando PowerShell o el símbolo del sistema como administrador.

## <a name="sql-server-container-startup-errors"></a>Errores de inicio del contenedor de SQL Server

Si no se puede ejecutar el contenedor de SQL Server, pruebe lo siguiente:

- Si recibe un error como `failed to create endpoint CONTAINER_NAME on network bridge. Error starting proxy: listen tcp 0.0.0.0:1433 bind: address already in use.`, está intentando asignar el puerto del contenedor 1433 a un puerto que ya está en uso. Esto puede ocurrir si está ejecutando SQL Server localmente en el equipo host. También puede ocurrir si inicia dos contenedores de SQL Server e intenta asignarlos al mismo puerto de host. Si esto ocurre, use el parámetro `-p` para asignar el puerto del contenedor 1433 a otro puerto de host. Por ejemplo: 

    <!--SQL Server 2017 on Linux -->
    ::: moniker range="= sql-server-linux-2017 || = sql-server-2017"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end
    
    <!--SQL Server 2019 on Linux-->
    ::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end

- Si recibe un error, como `Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.30tdout=1&tail=all: dial unix /var/run/docker.sock: connect: permission denied` al intentar iniciar un contenedor, agregue el usuario al grupo de Docker en Ubuntu. Después, cierre la sesión y vuelva a iniciarla, ya que este cambio afectará a las nuevas sesiones. 

   ```bash
    usermod -aG docker $USER
   ```

- Compruebe si hay algún mensaje de error del contenedor.

   ```bash
   docker logs e69e056c702d
   ```

- Asegúrese de que cumple los requisitos mínimos de memoria y disco especificados en la sección de [requisitos previos](quickstart-install-connect-docker.md#requirements) del artículo de inicio rápido.

- Si usa software de administración de contenedores, asegúrese de que admite procesos de contenedor que se ejecutan como raíz. El proceso sqlservr en el contenedor se ejecuta como raíz.

- Si el contenedor de Docker de SQL Server sale inmediatamente después de iniciarse, compruebe los registros de Docker. Si usa PowerShell en Windows con el comando `docker run`, use comillas dobles en lugar de comillas simples. Con PowerShell Core, use comillas simples.

- Revise la [Configuración y registros de errores de SQL Server](#errorlogs).

## <a name="enable-dump-captures"></a>Habilitar capturas de volcado

Si se produce un error en el proceso de SQL Server dentro del contenedor, debe crear un contenedor nuevo con **SYS_PTRACE** habilitado. Esto agrega la capacidad de Linux para realizar el seguimiento de un proceso, que es necesario para crear un archivo de volcado de memoria en una excepción. El departamento de soporte técnico puede utilizar el archivo de volcado para ayudar a solucionar el problema. El siguiente comando de ejecución de Docker habilita esta funcionalidad.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="sql-server-connection-failures"></a>Errores de conexión de SQL Server

Si no puede conectarse a la instancia de SQL Server que se ejecuta en el contenedor, pruebe lo siguiente:

- Asegúrese de que el contenedor de SQL Server se está ejecutando examinando la columna **STATUS** de la salida de `docker ps -a`. Si no es así, use `docker start <Container ID>` para iniciarlo.

- Si ha asignado a un puerto de host no predeterminado (no 1433), asegúrese de que está especificando el puerto en la cadena de conexión. Puede ver la asignación de puertos en la columna **PORTS** de la salida de `docker ps -a`. Por ejemplo, el siguiente comando conecta sqlcmd a un contenedor que escucha en el puerto 1401:

    ::: zone pivot="cs1-bash"
    ```bash
    sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
    ```
    ::: zone-end

    ::: zone pivot="cs1-powershell"
    ```PowerShell
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

    ::: zone pivot="cs1-cmd"
    ```cmd
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

- Si usa `docker run` con un volumen de datos asignado o un contenedor de volúmenes de datos existentes, SQL Server ignora el valor de `SA_PASSWORD`. En su lugar, se usa la contraseña de usuario SA preconfigurada de los datos de SQL Server del volumen de datos o el contenedor de volúmenes de datos. Compruebe que está utilizando la contraseña de SA asociada a los datos a los que está adjuntando.

- Revise la [Configuración y registros de errores de SQL Server](#errorlogs).

## <a name="sql-server-availability-groups"></a>Grupos de disponibilidad de SQL Server

Si usa Docker con grupos de disponibilidad de SQL Server, hay dos requisitos adicionales.

- Asignar el puerto que se usa para la comunicación de réplica (valor predeterminado 5022). Por ejemplo, especifique `-p 5022:5022` como parte del comando `docker run`.

- Establecer explícitamente el nombre de host del contenedor con el parámetro `-h YOURHOSTNAME` del comando `docker run`. Este nombre de host se usa al configurar el grupo de disponibilidad. Si no lo especifica con `-h`, el valor predeterminado es el **id. de contenedor**.

## <a name="sql-server-setup-and-error-logs"></a><a id="errorlogs"></a> Configuración y registros de errores de SQL Server

Puede consultar la configuración y los registros de errores de SQL Server en **/var/opt/mssql/log**. Si el contenedor no se está ejecutando, primero inicie el contenedor. Después, use un símbolo del sistema interactivo para inspeccionar los registros. Puede obtener el id. de contenedor si ejecuta el comando `docker ps`.

```bash
docker start <ContainerID>
docker exec -it <ContainerID> "bash"
```

En la sesión de Bash dentro del contenedor, ejecute los siguientes comandos:

```bash
cd /var/opt/mssql/log
cat setup*.log
cat errorlog
```

> [!TIP]
> Si montó un directorio host en **/var/opt/mssql** al crear el contenedor, puede buscar en el subdirectorio de **registro** de la ruta de acceso asignada en el host en su lugar.

## <a name="execute-commands-in-a-container"></a>Ejecución de comandos en un contenedor

Si tiene un contenedor en ejecución, puede ejecutar comandos dentro del contenedor desde un terminal del host.

Para obtener el identificador de contenedor, ejecute:

```bash
docker ps -a
```

Para iniciar un terminal de Bash en el contenedor, ejecute:

```bash
docker exec -it <Container ID> /bin/bash
```

Ahora puede ejecutar comandos como si los estuviera ejecutando en el terminal dentro del contenedor. Cuando termine, escriba `exit`. Esto cierra la sesión de comandos interactiva, pero el contenedor continúa ejecutándose.

## <a name="next-steps"></a>Pasos siguientes

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Para empezar a trabajar con imágenes de contenedor de SQL Server 2017 en Docker, revise la [guía de inicio rápido](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Para empezar a trabajar con imágenes de contenedor de SQL Server 2019 en Docker, revise el [inicio rápido](quickstart-install-connect-docker.md).

::: moniker-end

- [Implementación y conexión a contenedores de Docker de SQL Server](sql-server-linux-docker-container-deployment.md)

- [Referencia de la configuración y personalización adicionales para contenedores de Docker](sql-server-linux-docker-container-configure.md)

- [Protección de contenedores de Docker de SQL Server](sql-server-linux-docker-container-security.md)
