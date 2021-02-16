---
title: Implementación y conexión a contenedores de Docker de SQL Server
description: Explore cómo se puede implementar SQL Server en contenedores de Docker y obtenga información sobre diversas herramientas para conectarse a SQL Server desde dentro y fuera del contenedor.
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: f789dd2b80515116e7bf0a12e26e6e8fe6c45b03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273106"
---
# <a name="deploy-and-connect-to-sql-server-docker-containers"></a>Implementación y conexión a contenedores de Docker de SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se explica cómo implementar y conectarse a contenedores de Docker de SQL Server.

Para otros escenarios de implementación, consulte:

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Linux](../linux/sql-server-linux-setup.md)
- [Kubernetes: clústeres de macrodatos](../big-data-cluster/deploy-get-started.md)

> [!NOTE]
> En este artículo, nos centraremos específicamente en el uso de la imagen mssql-server-linux. No trataremos la imagen de Windows, pero puede obtener más información sobre ella en la [página mssql-server-windows de Docker Hub](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/).

> [!IMPORTANT]
> Antes de elegir la ejecución de un contenedor de SQL Server para casos de uso de producción, revise la [directiva de compatibilidad de contenedores de SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) para garantizar que se ejecuta una configuración compatible.

En este vídeo de 6 minutos se muestra una introducción a la ejecución de SQL Server en contenedores:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2019-in-Containers/player?WT.mc_id=dataexposed-c9-niner]

## <a name="pull-and-run-the-container-image"></a>Extraer y ejecutar la imagen de contenedor

Para extraer y ejecutar las imágenes de contenedor de Docker para SQL Server 2017 y SQL Server 2019, siga los requisitos previos y los pasos descritos en las siguientes guías de inicio rápido:

- [Inicio rápido: Ejecución de imágenes de contenedor de SQL Server con Docker](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) (SQL Server 2017)
- [Ejecución de imágenes de contenedor de SQL Server 2019 con Docker](quickstart-install-connect-docker.md?view=sql-server-ver15&preserve-view=true)

En este artículo de configuración se proporcionan escenarios de uso adicionales en las secciones siguientes.

## <a name="connect-and-query"></a>Conexión y consultas

Puede conectarse a SQL Server y realizar consultas en un contenedor desde fuera o desde dentro del contenedor. En las siguientes secciones se explican ambos escenarios.

### <a name="tools-outside-the-container"></a>Herramientas fuera del contenedor

Puede conectarse a la instancia de SQL Server en la máquina de Docker desde cualquier herramienta externa de Linux, Windows o Mac OS que admita conexiones de SQL. Algunas herramientas comunes son:

- [Azure Data Studio](../azure-data-studio/quickstart-sql-server.md)
- [sqlcmd](sql-server-linux-setup-tools.md)
- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) en Windows](sql-server-linux-manage-ssms.md)

En el ejemplo siguiente se usa **sqlcmd** para conectarse a una instancia de SQL Server que se ejecuta en un contenedor de Docker. La dirección IP de la cadena de conexión es la dirección IP del equipo host que ejecuta el contenedor.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

Si asignó un puerto de host distinto del valor predeterminado **1433**, agréguelo a la cadena de conexión. Por ejemplo, si especificó `-p 1400:1433` en el comando `docker run`, establezca de forma explícita el puerto 1400.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1400 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

### <a name="tools-inside-the-container"></a>Herramientas dentro del contenedor

A partir de SQL Server 2017, las [herramientas de línea de comandos de SQL Server](sql-server-linux-setup-tools.md) se incluyen en la imagen de contenedor. Si se asocia a la imagen con un símbolo del sistema interactivo, puede ejecutar las herramientas de forma local.

1. Use el comando `docker exec -it` para iniciar un shell de Bash interactivo dentro de su contenedor en ejecución. En el ejemplo siguiente, `e69e056c702d` es el identificador del contenedor.

    ```bash
    docker exec -it e69e056c702d "bash"
    ```

    > [!TIP]
    > No siempre tiene que especificar el id. completo del contenedor. Solo tiene que especificar suficientes caracteres para identificarlo de forma única. Por tanto, en este ejemplo, podría ser suficiente usar `e6` o `e69` en lugar del id. completo. Para averiguar el id. del contenedor, ejecute el comando `docker ps -a`.

2. Una vez dentro del contenedor, conecte localmente con sqlcmd. Sqlcmd no está en la ruta de acceso de forma predeterminada, por lo que deberá especificar la ruta de acceso completa.

    ```bash
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'
    ```

3. Cuando termine con sqlcmd, escriba `exit`.

4. Cuando termine con el símbolo del sistema interactivo, escriba `exit`. El contenedor continuará ejecutándose después de salir del shell de Bash interactivo.

## <a name="check-the-container-version"></a><a id="version"></a> Comprobación de la versión de contenedor

Si quiere conocer la versión de SQL Server en un contenedor de Docker en ejecución, ejecute el siguiente comando para mostrarla: Reemplace `<Container ID or name>` por el identificador o el nombre del contenedor de destino. Reemplace `<YourStrong!Passw0rd>` por la contraseña de SQL Server para el inicio de sesión de SA.

::: zone pivot="cs1-bash"
```bash
sudo docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd \
-S localhost -U SA -P '<YourStrong!Passw0rd>' \
-Q 'SELECT @@VERSION'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd `
-S localhost -U SA -P "<YourStrong!Passw0rd>" `
-Q "SELECT @@VERSION"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd ^
-S localhost -U SA -P "<YourStrong!Passw0rd>" ^
-Q "SELECT @@VERSION"
```
::: zone-end

También puede identificar la versión de SQL Server y el número de compilación de una imagen de contenedor de Docker de destino. El comando siguiente muestra la versión de SQL Server y la información de compilación de la imagen **mcr.microsoft.com/mssql/server:2017-latest**. Para ello, ejecuta un nuevo contenedor con una variable de entorno **PAL_PROGRAM_INFO=1**. El contenedor resultante se cierra al instante y el comando `docker rm` lo quita.

::: zone pivot="cs1-bash"
```bash
sudo docker run -e PAL_PROGRAM_INFO=1 --name sqlver \
-ti mcr.microsoft.com/mssql/server:2019-latest && \
sudo docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e PAL_PROGRAM_INFO=1 --name sqlver `
-ti mcr.microsoft.com/mssql/server:2019-latest; `
docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e PAL_PROGRAM_INFO=1 --name sqlver ^
-ti mcr.microsoft.com/mssql/server:2019-latest && ^
docker rm sqlver
```
::: zone-end

Los comandos anteriores muestran información de versión similar a la siguiente salida:

```output
sqlservr
  Version 15.0.4063.15
  Build ID 8a3bb4cca325e1d0b3071b3a193f6a1d74b440fbd95d2fb18881651a5b9ec8e8
  Build Type release
  Git Version 0335c462
  Built at Fri Aug 28 04:50:27 GMT 2020

PAL
  Build ID cc5ceea1b3d294f7d0166f99932f98c7eacfaaa81bcd7cf23c6a89f785829b63
  Build Type release
  Git Version ae9d66dff
  Built at Fri Aug 28 04:46:48 GMT 2020

Packages
  system.security                         6.2.9200.10,unset,
  system.certificates                     6.2.9200.10,unset,
  secforwarderxplat                       15.0.4063.15
  sqlservr                                15.0.4063.15
  system.common                           10.0.17134.1246.202005133
  system.netfx                            4.7.2.461814
  system                                  6.2.9200.10,unset,
  sqlagent                                15.0.4063.15
```

## <a name="run-a-specific-sql-server-container-image"></a><a id="tags"></a> Ejecución de una imagen de contenedor de SQL Server determinada

> [!NOTE]
> A partir de SQL Server 2019 CU3, se admite Ubuntu 18.04. Puede recuperar una lista de todas las etiquetas disponibles para mssql/server en <https://mcr.microsoft.com/v2/mssql/server/tags/list>.

Hay escenarios en los que es posible que no quiera usar la imagen de contenedor de SQL Server más reciente. Para ejecutar una imagen de contenedor de SQL Server determinada, siga estos pasos:

1. Identifique la **etiqueta** de Docker para la versión que quiere usar. Para ver las etiquetas disponibles, vea [la página mssql-server-linux de Docker Hub](https://hub.docker.com/_/microsoft-mssql-server).

2. Extraiga la imagen de contenedor de SQL Server con la etiqueta. Por ejemplo, para extraer la imagen 2019-CU7-ubuntu-18.04, reemplace `<image_tag>` en el comando siguiente por `2019-CU7-ubuntu-18.04`.

   ```bash
   docker pull mcr.microsoft.com/mssql/server:<image_tag>
   ```

3. Para ejecutar un nuevo contenedor con esa imagen, especifique el nombre de la etiqueta en el comando `docker run`. En el siguiente comando, reemplace `<image_tag>` por la versión que quiere ejecutar.

   ::: zone pivot="cs1-bash"
   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

Estos pasos también se pueden usar para degradar un contenedor existente. Por ejemplo, puede que quiera revertir o degradar un contenedor en ejecución para solucionar problemas o realizar pruebas. Para degradar un contenedor en ejecución, debe usar una técnica de persistencia para la carpeta de datos. Siga los mismos pasos descritos en la [sección de actualización](#upgrade), pero especifique el nombre de etiqueta de la versión anterior al ejecutar el nuevo contenedor.

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="run-rhel-based-container-images"></a><a id="rhel"></a> Ejecución de imágenes de contenedor basadas en RHEL

La documentación sobre las imágenes de contenedor de SQL Server en Linux apunta a contenedores basados en Ubuntu. A partir de SQL Server 2019, puede usar contenedores basados en Red Hat Enterprise Linux (RHEL). Un ejemplo de la imagen para RHEL será similar a **mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8**.

Por ejemplo, el comando siguiente extrae la actualización acumulativa 1 para el contenedor de SQL Server 2019 que usa RHEL 8:

::: zone pivot="cs1-bash"
```bash
sudo docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: moniker-end

## <a name="run-production-container-images"></a><a id="production"></a> Ejecución de imágenes de contenedor de producción

En el [inicio rápido](quickstart-install-connect-docker.md) de la sección anterior se ejecuta la edición para desarrolladores gratuita de SQL Server de Docker Hub. La mayor parte de la información sigue siendo aplicable si quiere ejecutar imágenes de contenedor de producción, como las ediciones Enterprise, Standard o Web. Pero hay algunas diferencias que se describen aquí.

- Solo puede usar SQL Server en un entorno de producción si tiene una licencia válida. Puede obtener una licencia de producción gratuita de SQL Server Express [aquí](https://go.microsoft.com/fwlink/?linkid=857693). Las licencias de SQL Server Standard y Enterprise Edition están disponibles a través de [licencias por volumen de Microsoft](https://www.microsoft.com/licensing/default.aspx).

- La imagen de contenedor para desarrolladores también se puede configurar para que ejecute las ediciones de producción. Siga estos pasos para ejecutar las ediciones de producción:

Revise los requisitos y ejecute los procedimientos de la [guía de inicio rápido](quickstart-install-connect-docker.md). Debe especificar la edición de producción con la variable de entorno **MSSQL_PID**. En el ejemplo siguiente se muestra cómo ejecutar la imagen de contenedor de SQL Server 2017 más reciente para la Enterprise Edition:

::: zone pivot="cs1-bash"
```bash
docker run --name sqlenterprise \
-e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
-e 'MSSQL_PID=Enterprise' -p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-e "MSSQL_PID=Enterprise" -p 1433:1433 `
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" ^
-e "MSSQL_PID=Enterprise" -p 1433:1433 ^
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

> [!IMPORTANT]
> Al pasar el valor **Y** a la variable de entorno **ACCEPT_EULA** y un valor de edición a **MSSQL_PID**, está expresando que tiene una licencia válida y existente para la edición y la versión de SQL Server que va a utilizar. También acepta que el uso del software de SQL Server que se ejecuta en una imagen de contenedor de Docker se regirá por los términos de la licencia de SQL Server.

> [!NOTE]
> Para obtener una lista completa de los posibles valores de **MSSQL_PID**, vea [Configuración de opciones de SQL Server con variables de entorno en Linux](sql-server-linux-configure-environment-variables.md).

## <a name="run-multiple-sql-server-containers"></a><a id="multiple"></a>Ejecución de varios contenedores de SQL Server

Docker proporciona una manera de ejecutar varios contenedores de SQL Server en el mismo equipo host. Use este enfoque para escenarios en los que se requieran varias instancias de SQL Server en el mismo host. Cada contenedor debe exponerse en un puerto diferente.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

En el ejemplo siguiente se crean dos contenedores de SQL Server 2017 y se asignan a los puertos **1401** y **1402** en el equipo host.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

En el ejemplo siguiente se crean dos contenedores de SQL Server 2019 y se asignan a los puertos **1401** y **1402** en el equipo host.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Ahora hay dos instancias de SQL Server que se ejecutan en contenedores independientes. Los clientes pueden conectarse a cada instancia de SQL Server mediante la dirección IP del host de Docker y el número de puerto del contenedor.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
sqlcmd -S 10.3.2.4,1402 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

## <a name="upgrade-sql-server-in-containers"></a><a id="upgrade"></a> Actualización de SQL Server en contenedores

Para actualizar la imagen de contenedor con Docker, primero identifique la etiqueta de la versión de la actualización. Extraiga esta versión del registro con el comando `docker pull`:

```command
docker pull mcr.microsoft.com/mssql/server:<image_tag>
```

Esto actualiza la imagen de SQL Server para los nuevos contenedores que cree, pero no actualiza SQL Server en ningún contenedor en ejecución. Para ello, debe crear un nuevo contenedor con la imagen de contenedor de SQL Server más reciente y migrar los datos a ese nuevo contenedor.

1. Asegúrese de que usa una de las [técnicas de persistencia de datos](sql-server-linux-docker-container-configure.md#persist) para el contenedor de SQL Server existente. Esto le permite iniciar un nuevo contenedor con los mismos datos.

1. Detenga el contenedor de SQL Server con el comando `docker stop`.

1. Cree un nuevo contenedor de SQL Server con `docker run` y especifique un directorio de host asignado o un contenedor de volúmenes de datos. Asegúrese de usar la etiqueta específica para la actualización de SQL Server. El nuevo contenedor ahora usa una nueva versión de SQL Server con los datos de SQL Server existentes.

   > [!IMPORTANT]
   > La actualización solo se admite entre RC1, RC2 y GA en este momento.

1. Compruebe las bases de datos y los datos del nuevo contenedor.

1. Opcionalmente, quite el contenedor anterior con `docker rm`.

## <a name="next-steps"></a>Pasos siguientes

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Para empezar a trabajar con imágenes de contenedor de SQL Server 2017 en Docker, revise el [inicio rápido](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Para empezar a trabajar con imágenes de contenedor de SQL Server 2019 en Docker, revise el [inicio rápido](quickstart-install-connect-docker.md)

::: moniker-end

- [Referencia de la configuración y personalización adicionales para contenedores de Docker](sql-server-linux-docker-container-configure.md)

- Vea el [repositorio mssql-docker de GitHub](https://github.com/Microsoft/mssql-docker) para obtener recursos, comentarios y problemas conocidos.

- [Solución de problemas de contenedores de Docker de SQL Server](sql-server-linux-docker-container-troubleshooting.md)

- [Explore la Alta disponibilidad para contenedores de SQL Server](sql-server-linux-container-ha-overview.md)

- [Protección de contenedores de Docker de SQL Server](sql-server-linux-docker-container-security.md)
