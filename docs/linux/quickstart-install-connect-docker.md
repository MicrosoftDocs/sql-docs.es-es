---
title: 'Docker: Instalación de contenedores para SQL Server en Linux'
description: En este inicio rápido se muestra cómo usar Docker para ejecutar las imágenes de contenedor de SQL Server 2017 y 2019. A continuación, deberá crear una base de datos y realizar consultas en esta con sqlcmd.
ms.custom: seo-lt-2019, contperf-fy21q1
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.date: 09/07/2020
ms.topic: quickstart
ms.prod: sql
ms.technology: linux
ms.prod_service: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
monikerRange: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 9393064403fbd41255b6be0712813185e745d6c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344445"
---
# <a name="quickstart-run-sql-server-container-images-with-docker"></a>Inicio rápido: Ejecución de imágenes de contenedor de SQL Server con Docker
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

> [!NOTE]
> Los ejemplos que se muestran a continuación usan docker.exe, pero la mayoría de estos comandos también funcionan con Podman. Proporciona la CLI similar al motor de contenedor de Docker. Puede leer más sobre Podman [aquí](http://docs.podman.io/en/latest).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

En este inicio rápido, deberá usar Docker para extraer y ejecutar la imagen de contenedor de SQL Server 2017, [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server). A continuación, tendrá que conectar con **sqlcmd** para crear la primera base de datos y ejecutar consultas.

> [!TIP]
> Si quiere crear contenedores de SQL Server 2019, vea la [versión de SQL Server 2019 de este artículo](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true).

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> A partir de SQL Server 2019 CU3, se admite Ubuntu 18.04.

En este inicio rápido, usará Docker para extraer y ejecutar la imagen de contenedor de SQL Server 2019, [mssql-server](https://hub.docker.com/r/microsoft/mssql-server). A continuación, tendrá que conectar con **sqlcmd** para crear la primera base de datos y ejecutar consultas.

> [!TIP]
> En este inicio rápido se crean contenedores de SQL Server 2019. Si prefiere crear contenedores de SQL Server 2017, vea la [versión de SQL Server 2017 de este artículo](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true).

::: moniker-end

Esta imagen se compone de SQL Server, que se ejecuta en un sistema Linux basado en Ubuntu 18.04. Se puede usar junto al motor de Docker 1.8 o versiones posteriores en Linux, o bien en Docker en Mac y Windows. Este inicio rápido se centra específicamente en el uso de la imagen de SQL Server en **Linux**. No trataremos la imagen de Windows, pero puede obtener más información sobre esta en la [página mssql-server-windows-developer de Docker Hub](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/).

## <a name="prerequisites"></a><a id="requirements"></a> Requisitos previos

- Motor de Docker 1.8 o versiones posteriores en cualquier distribución de Linux admitida o Docker para Mac y Windows. Para obtener más información, consulte [Instalar Docker](https://docs.docker.com/engine/installation/).
- Controlador de almacenamiento **overlay2** de Docker. Este es el valor predeterminado para la mayoría de los usuarios. Si no usa este proveedor de almacenamiento y necesita cambiar, vea las instrucciones y advertencias de la [documentación de Docker para configurar overlay2](https://docs.docker.com/storage/storagedriver/overlayfs-driver/#configure-docker-with-the-overlay-or-overlay2-storage-driver).
- Un mínimo de 2 GB de espacio en disco.
- Un mínimo de 2 GB de RAM.
- [Requisitos del sistema para SQL Server en Linux](sql-server-linux-setup.md#system).

<!--The following H2 is versioned for 2017 and 2019. Much of the content is duplicated, so
any changes to one section should be duplicated in the other-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="pull-and-run-the-2017-container-image"></a><a id="pullandrun2017"></a> Extracción y ejecución de la imagen de contenedor 2017

Antes de iniciar los pasos siguientes, asegúrese de que ha seleccionado el shell preferido (Bash, PowerShell o cmd) en la parte superior de este artículo.

1. Extraiga la imagen de contenedor de Linux de SQL Server 2017 desde Microsoft Container Registry.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!TIP]
   > Si quiere crear contenedores de SQL Server 2019, vea la [versión de SQL Server 2019 de este artículo](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true#pullandrun2019).

   En el comando anterior, se extrae la última imagen de contenedor de SQL Server 2017. Si quiere extraer una imagen específica, agregue dos puntos y el nombre de etiqueta (por ejemplo, `mcr.microsoft.com/mssql/server:2017-GA-ubuntu`). Para ver todas las imágenes disponibles, vea la [página de Docker Hub de mssql-server](https://hub.docker.com/r/microsoft/mssql-server).

   ::: zone pivot="cs1-bash"
   En los comandos de Bash de este artículo se usa `sudo`. En macOS, es posible que no se necesite `sudo`. En Linux, si no quiere usar `sudo` para ejecutar Docker, puede configurar un grupo de **Docker** y agregarle usuarios. Para obtener más información, vea [Pasos posteriores a la instalación para Linux](https://docs.docker.com/install/linux/linux-postinstall/).

   ::: zone-end

2. Para ejecutar la imagen de contenedor con Docker, puede usar el siguiente comando desde un shell de Bash (Linux y macOS), o bien una línea de comandos de PowerShell con privilegios elevados.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d \
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > Si usa PowerShell Core, reemplace las comillas dobles por comillas simples.
   
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!NOTE]
   > La contraseña debe seguir la directiva de contraseñas predeterminada de SQL Server. En caso contrario, el contenedor no podrá instalar SQL Server y dejará de funcionar. De forma predeterminada, la contraseña debe tener al menos 8 caracteres y contener caracteres de tres de los cuatro conjuntos siguientes: Letras mayúsculas, letras minúsculas, dígitos de Base 10 y símbolos. Puede examinar el registro de errores ejecutando el comando [docker logs](https://docs.docker.com/engine/reference/commandline/logs/).
   >
   > De forma predeterminada, se creará un contenedor con la edición para desarrolladores de SQL Server 2017. El proceso para ejecutar las ediciones de producción en contenedores es ligeramente diferente. Para obtener más información, vea [Run production container image](./sql-server-linux-docker-container-deployment.md#production) (Ejecutar imágenes de contenedor de producción).

   En la tabla siguiente, se proporciona una descripción de los parámetros del ejemplo de `docker run` anterior:

   | Parámetro | Descripción |
   |-----|-----|
   | **-e "ACCEPT_EULA=Y"** |  Establezca la variable **ACCEPT_EULA** en cualquier valor para confirmar que acepta el [Contrato de licencia de usuario final](https://go.microsoft.com/fwlink/?linkid=857698). Configuración requerida para la imagen de SQL Server. |
   | **-e "SA_PASSWORD=\<YourStrong@Passw0rd\>"** | Especifique una contraseña segura propia con al menos 8 caracteres y que cumpla los [requisitos de contraseña de SQL Server](../relational-databases/security/password-policy.md). Configuración requerida para la imagen de SQL Server. |
   | **-p 1433:1433** | Asigne un puerto TCP en el entorno de host (el primer valor) a un puerto TCP en el contenedor (el segundo valor). En este ejemplo, SQL Server escucha en TCP 1433 en el contenedor y se expone al puerto 1433 del host. |
   | **--name sql1** | Especifique un nombre personalizado para el contenedor en lugar de uno generado aleatoriamente. Si ejecuta más de un contenedor, no podrá usar el mismo nombre. |
   | **-h sql1** | Cuando se usa para establecer explícitamente el nombre de host del contenedor, si no se especifica, el valor predeterminado es el identificador del contenedor, que es un GUID del sistema generado aleatoriamente. |
   | **-d** | Ejecute el contenedor en segundo plano (demonio). |
   | **mcr.microsoft.com/mssql/server:2017-latest** | La imagen de contenedor de SQL Server 2017 para Linux. |

3. Para ver los contenedores de Docker, use el comando `docker ps`.


   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   Debería ver un resultado similar al de la captura de pantalla siguiente:

   ![Resultado del comando docker ps](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. Si la columna **ESTADO** muestra el estado **Activo**, esto indica que SQL Server se está ejecutando en el contenedor y que está escuchando en el puerto especificado en la columna **PUERTOS**. Si la columna **ESTADO** de su contenedor de SQL Server muestra **Cerrado**, consulte la [sección Solución de problemas de la guía de configuración](./sql-server-linux-docker-container-troubleshooting.md).

El parámetro `-h` (nombre de host), tal y como se indicó anteriormente, cambia el nombre interno del contenedor a un valor personalizado. Este es el nombre que verá que se devuelve en la siguiente consulta de Transact-SQL:

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

Establecer `-h` y `--name` en el mismo valor es una buena manera de identificar fácilmente el contenedor de destino.

5. Como paso final, cambie la contraseña de administrador del sistema porque `SA_PASSWORD` se ve en la salida de `ps -eax` y se almacena en la variable de entorno con el mismo nombre. Vea los pasos siguientes.

::: moniker-end
<!--End of 2017 "Pull and run" section-->

<!--This is the 2019 version of the "Pull and run" section-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="pull-and-run-the-2019-container-image"></a><a id="pullandrun2019"></a> Extracción y ejecución de la imagen de contenedor 2019

Antes de iniciar los pasos siguientes, asegúrese de que ha seleccionado el shell preferido (Bash, PowerShell o cmd) en la parte superior de este artículo.

1. Extraiga la imagen de contenedor de Linux de SQL Server 2019 desde Microsoft Container Registry.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > Si usa PowerShell Core, reemplace las comillas dobles por comillas simples.
   
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!TIP]
   > En este inicio rápido se usa la imagen de Docker de SQL Server 2019. Si quiere ejecutar la imagen de SQL Server 2017, vea la [versión de SQL Server 2017 de este artículo](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true#pullandrun2017).

   El comando anterior extrae la imagen de contenedor de SQL Server 2019 basada en Ubuntu. Para usar en su lugar imágenes de contenedor basadas en RedHat, vea [Ejecución de imágenes de contenedor basadas en RHEL](./sql-server-linux-docker-container-deployment.md#rhel). Para ver todas las imágenes disponibles, vea [la página mssql-server-linux de Docker Hub](https://hub.docker.com/_/microsoft-mssql-server).

   ::: zone pivot="cs1-bash"
   En los comandos de Bash de este artículo se usa `sudo`. En macOS, es posible que no se necesite `sudo`. En Linux, si no quiere usar `sudo` para ejecutar Docker, puede configurar un grupo de **Docker** y agregarle usuarios. Para obtener más información, vea [Pasos posteriores a la instalación para Linux](https://docs.docker.com/install/linux/linux-postinstall/).
   ::: zone-end

2. Para ejecutar la imagen de contenedor con Docker, puede usar el siguiente comando desde un shell de Bash (Linux y macOS), o bien una línea de comandos de PowerShell con privilegios elevados.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!NOTE]
   > La contraseña debe seguir la directiva de contraseñas predeterminada de SQL Server. En caso contrario, el contenedor no podrá instalar SQL Server y dejará de funcionar. De forma predeterminada, la contraseña debe tener al menos 8 caracteres y contener caracteres de tres de los cuatro conjuntos siguientes: Letras mayúsculas, letras minúsculas, dígitos de Base 10 y símbolos. Puede examinar el registro de errores ejecutando el comando [docker logs](https://docs.docker.com/engine/reference/commandline/logs/).
   >
   > De forma predeterminada, se creará un contenedor con la edición para desarrolladores de SQL Server 2019.

   En la tabla siguiente, se proporciona una descripción de los parámetros del ejemplo de `docker run` anterior:

   | Parámetro | Descripción |
   |-----|-----|
   | **-e "ACCEPT_EULA=Y"** |  Establezca la variable **ACCEPT_EULA** en cualquier valor para confirmar que acepta el [Contrato de licencia de usuario final](https://go.microsoft.com/fwlink/?LinkId=746388). Configuración requerida para la imagen de SQL Server. |
   | **-e "SA_PASSWORD=\<YourStrong@Passw0rd\>"** | Especifique una contraseña segura propia con al menos 8 caracteres y que cumpla los [requisitos de contraseña de SQL Server](../relational-databases/security/password-policy.md). Configuración requerida para la imagen de SQL Server. |
   | **-p 1433:1433** | Asigne un puerto TCP en el entorno de host (el primer valor) a un puerto TCP en el contenedor (el segundo valor). En este ejemplo, SQL Server escucha en TCP 1433 en el contenedor y se expone al puerto 1433 del host. |
   | **--name sql1** | Especifique un nombre personalizado para el contenedor en lugar de uno generado aleatoriamente. Si ejecuta más de un contenedor, no podrá usar el mismo nombre. |
   | **-h sql1** | Cuando se usa para establecer explícitamente el nombre de host del contenedor, si no se especifica, el valor predeterminado es el identificador del contenedor, que es un GUID del sistema generado aleatoriamente. |
   | **mcr.microsoft.com/mssql/server:2019-latest** | Imagen de contenedor de Ubuntu Linux de SQL Server 2019. |

3. Para ver los contenedores de Docker, use el comando `docker ps`.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   Debería ver un resultado similar al de la captura de pantalla siguiente:

   ![Resultado del comando docker ps](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. Si la columna **ESTADO** muestra el estado **Activo**, esto indica que SQL Server se está ejecutando en el contenedor y que está escuchando en el puerto especificado en la columna **PUERTOS**. Si en la columna **ESTADO** del contenedor de SQL Server se muestra **Cerrado**, vea [Solución de problemas de contenedores de Docker de SQL Server](sql-server-linux-docker-container-troubleshooting.md).

El parámetro `-h` (nombre de host), tal y como se indicó anteriormente, cambia el nombre interno del contenedor a un valor personalizado. Esto cambia el nombre interno del contenedor a un valor personalizado. Este es el nombre que verá que se devuelve en la siguiente consulta de Transact-SQL:

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

Establecer `-h` y `--name` en el mismo valor es una buena manera de identificar fácilmente el contenedor de destino.


5. Como paso final, cambie la contraseña de administrador del sistema porque `SA_PASSWORD` se ve en la salida de `ps -eax` y se almacena en la variable de entorno con el mismo nombre. Vea los pasos siguientes.


::: moniker-end
<!--End of 2019 "Pull and run" section-->



## <a name="change-the-sa-password"></a><a id="sapassword"></a> Cambio de la contraseña de SA

<!-- This section was pasted in from includes/sql-server-linux-change-docker-password.md, to better support zone pivots. 2019/02/11 -->

La cuenta **SA** es un administrador del sistema en la instancia de SQL Server que se crea durante la instalación. Después de crear el contenedor de SQL Server, la variable de entorno `SA_PASSWORD` especificada se reconoce mediante la ejecución de `echo $SA_PASSWORD` en el contenedor. Por motivos de seguridad, cambie la contraseña de administrador del sistema.

1. Elija una contraseña segura que se usará para el usuario SA.

1. Use `docker exec` para ejecutar **sqlcmd** a fin de cambiar la contraseña mediante Transact-SQL. En el ejemplo siguiente, reemplace la contraseña anterior, `<YourStrong!Passw0rd>`, y la nueva contraseña, `<YourNewStrong!Passw0rd>`, por sus propios valores de contraseña.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P "<YourStrong@Passw0rd>" \
      -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong@Passw0rd>"'
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong@Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong!Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

## <a name="connect-to-sql-server"></a>Conectar a SQL Server

En los pasos siguientes se usa la herramienta de línea de comandos [**sqlcmd**](../tools/sqlcmd-utility.md) de SQL Server, dentro del contenedor para conectarse a SQL Server.

1. Use el comando `docker exec -it` para iniciar un shell de Bash interactivo dentro de su contenedor en ejecución. En el ejemplo siguiente, el parámetro `--name` especifica el nombre de `sql1` al crear el contenedor.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

2. Una vez dentro del contenedor, conecte localmente con sqlcmd. Sqlcmd no está en la ruta de acceso de forma predeterminada, por lo que deberá especificar la ruta de acceso completa.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourNewStrong@Passw0rd>"
   ```

   > [!TIP]
   > Puede omitir la contraseña en la línea de comandos para que se le solicite escribirla.

3. Si se realiza correctamente, debe ver un símbolo de sistema de **sqlcmd**: `1>`.

## <a name="create-and-query-data"></a>Creación y consulta de datos

En las secciones siguientes se le guía por el uso de **sqlcmd** y Transact-SQL para crear una base de datos, agregar datos y ejecutar una consulta.

### <a name="create-a-new-database"></a>Creación de una base de datos

En los pasos siguientes se crea una base de datos denominada `TestDB`.

1. En el símbolo del sistema de **sqlcmd**, pegue el comando Transact-SQL siguiente para crear una base de datos de prueba:

   ```sql
   CREATE DATABASE TestDB
   ```

2. En la línea siguiente, escriba una consulta para devolver el nombre de todas las bases de datos del servidor:

   ```sql
   SELECT Name from sys.Databases
   ```

3. Los dos comandos anteriores no se ejecutaron de inmediato. Escriba `GO` en una línea nueva para ejecutar los comandos anteriores:

   ```sql
   GO
   ```

### <a name="insert-data"></a>Insertar datos

Luego cree una tabla, `Inventory`, e inserte dos filas nuevas.

1. En el símbolo del sistema de **sqlcmd**, cambie el contexto a la nueva base de datos `TestDB`:

   ```sql
   USE TestDB
   ```

2. Cree una tabla llamada `Inventory`:

   ```sql
   CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
   ```

3. Inserte datos en la nueva tabla:

   ```sql
   INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
   ```

4. Escriba `GO` para ejecutar los comandos anteriores:

   ```sql
   GO
   ```

### <a name="select-data"></a>Selección de datos

Ahora ejecute una consulta para devolver datos desde la tabla `Inventory`.

1. En el símbolo del sistema **sqlcmd**, escriba una consulta que devuelva filas desde la tabla `Inventory` donde la cantidad sea mayor que 152:

   ```sql
   SELECT * FROM Inventory WHERE quantity > 152;
   ```

2. Ejecute el comando:

   ```sql
   GO
   ```

### <a name="exit-the-sqlcmd-command-prompt"></a>Salida del símbolo del sistema de sqlcmd

1. Para finalizar la sesión de **sqlcmd**, escriba `QUIT`:

   ```sql
   QUIT
   ```

2. Para salir de la línea de comandos interactiva del contenedor, escriba `exit`. El contenedor continuará ejecutándose después de salir del shell de Bash interactivo.

## <a name="connect-from-outside-the-container"></a><a id="connectexternal"></a> Conectarse desde fuera del contenedor

También puede conectarse a la instancia de SQL Server en la máquina de Docker desde cualquier herramienta externa de Linux, Windows o Mac OS que admita conexiones de SQL.

En los pasos siguientes, se usa **sqlcmd** fuera de su contenedor para conectarse a la instancia de SQL Server que se ejecuta en el contenedor. En estos pasos, se presupone que ya dispone de las herramientas de línea de comandos de SQL Server instaladas fuera de su contenedor. Se aplican los mismos principios que al usar otras herramientas, pero el proceso de conexión es único para cada herramienta.

1. Busque la dirección IP de la máquina host de su contenedor. En Linux, use **ifconfig** o **ip addr**. En Windows, use **ipconfig**.

1. En este ejemplo, instale la herramienta **sqlcmd** en el equipo cliente. Para obtener más información, vea [Instalación de sqlcmd en Windows](../tools/sqlcmd-utility.md) o [Instalación de sqlcmd en Linux](sql-server-linux-setup-tools.md).

1. Ejecute sqlcmd especificando la dirección IP y el puerto asignado al puerto 1433 en el contenedor. En este ejemplo se trata del mismo puerto, 1433, que en el equipo host. Si ha especificado otro puerto asignado en el equipo host, se usa aquí. También tendrá que abrir el puerto de entrada adecuado en el firewall para permitir la conexión.

   ::: zone pivot="cs1-bash"
   ```bash
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

1. Ejecute los comandos Transact-SQL. Cuando termine, escriba `QUIT`.

Estas son otras herramientas de uso común para conectarse a SQL Server:

- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) en Windows](sql-server-linux-manage-ssms.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
- [mssql-cli (versión preliminar)](https://github.com/dbcli/mssql-cli/blob/master/doc/usage_guide.md)
- [PowerShell Core](sql-server-linux-manage-powershell-core.md)

## <a name="remove-your-container"></a>Quitar el contenedor

Si quiere quitar el contenedor de SQL Server que se usa en este tutorial, ejecute los comandos siguientes:

::: zone pivot="cs1-bash"
```bash
sudo docker stop sql1
sudo docker rm sql1
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker stop sql1
docker rm sql1
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker stop sql1
docker rm sql1
```
::: zone-end

> [!WARNING]
> Al detener y quitar un contenedor, se eliminarán permanentemente los datos de SQL Server que contenga. Si tiene que conservar sus datos, [cree un archivo de copia de seguridad y cópielo en una ubicación externa al contenedor](tutorial-restore-backup-in-sql-server-container.md), o bien use una [técnica de persistencia de datos de contenedor](sql-server-linux-docker-container-configure.md#persist).

## <a name="docker-demo"></a>Demostración de Docker

Tras haber intentado usar la imagen de contenedor de SQL Server en Docker, puede que quiera saber cómo usar Docker para mejorar el desarrollo y las pruebas. En el vídeo siguiente se muestra cómo se puede usar Docker en un escenario de implementación e integración continuas.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T152/player]

## <a name="next-steps"></a>Pasos siguientes

Para ver un tutorial sobre cómo restaurar archivos de copia de seguridad de bases de datos en un contenedor, consulte [Restore a SQL Server database in a Linux Docker container](tutorial-restore-backup-in-sql-server-container.md) (Restaurar una base de datos de SQL Server en un contenedor de Docker en Linux). Explore otros escenarios, como la ejecución de [varios contenedores](sql-server-linux-docker-container-deployment.md#multiple), la [persistencia de datos](sql-server-linux-docker-container-configure.md#persist) y la [solución de problemas](sql-server-linux-docker-container-troubleshooting.md).

También puede consultar el [repositorio de Github mssql-docker](https://github.com/Microsoft/mssql-docker) para ver una serie de recursos, comentarios y problemas conocidos.
