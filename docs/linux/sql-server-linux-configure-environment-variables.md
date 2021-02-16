---
title: Configuración de variables de entorno para SQL Server en Linux
description: En este artículo se describe cómo usar variables de entorno para configurar opciones específicas de SQL Server 2017 en Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: fb9455970c58daa65f8068792a4edc8ae7e20f51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343174"
---
# <a name="configure-sql-server-settings-with-environment-variables-on-linux"></a>Configuración de opciones de SQL Server con variables de entorno en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Puede usar diversas variables de entorno para configurar SQL Server 2017 en Linux. Estas variables se usan en dos escenarios:

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Puede usar diversas variables de entorno para configurar SQL Server 2019 en Linux. Estas variables se usan en dos escenarios:

::: moniker-end

- Para configurar la instalación inicial con el comando `mssql-conf setup`.
- Para configurar un nuevo [contenedor de SQL Server en Docker](quickstart-install-connect-docker.md).

> [!TIP]
> Si necesita configurar SQL Server después de estos escenarios de instalación, consulte [Configurar SQL Server en Linux con la herramienta mssql-conf](sql-server-linux-configure-mssql-conf.md).

## <a name="environment-variables"></a>Variables de entorno

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| Variable de entorno | Descripción |
|-----|-----|
| **ACCEPT_EULA** | Establezca la variable **ACCEPT_EULA** en cualquier valor para confirmar que acepta el [Contrato de licencia de usuario final](https://go.microsoft.com/fwlink/?LinkId=746388). Configuración requerida para la imagen de SQL Server. |
| **MSSQL_SA_PASSWORD** | Configure la contraseña de usuario SA. |
| **MSSQL_PID** | Establezca la edición de SQL Server o la clave de producto. Los valores posibles son: </br></br>**Evaluación**</br>**Developer**</br>**Express**</br>**Web**</br>**Estándar**</br>**Enterprise**</br>**Clave de producto**</br></br>Si especifica una clave de producto, debe tener el formato #####-#####-#####-#####-#####, donde "#" es un número o una letra.|
| **MSSQL_LCID** | Establece el identificador de idioma que se usará para SQL Server. Por ejemplo, 1036 es francés. |
| **MSSQL_COLLATION** | Establece la intercalación predeterminada para SQL Server. Esto reemplaza la asignación predeterminada de identificador de idioma (LCID) por la intercalación. |
| **MSSQL_MEMORY_LIMIT_MB** | Establece la cantidad máxima de memoria (en MB) que puede usar SQL Server. De forma predeterminada, es el 80 % de la memoria física total. |
| **MSSQL_TCP_PORT** | Configure el puerto TCP en el que escucha SQL Server (valor predeterminado: 1433). |
| **MSSQL_IP_ADDRESS** | Establezca la dirección IP. Actualmente, la dirección IP debe ser de estilo IPv4 (0.0.0.0). |
| **MSSQL_BACKUP_DIR** | Establezca la ubicación del directorio de copia de seguridad predeterminado. |
| **MSSQL_DATA_DIR** | Cambie el directorio en el que se crean los archivos de datos (.mdf) de la base de datos de SQL Server. |
| **MSSQL_LOG_DIR** | Cambie el directorio en el que se crean los archivos de registro (.ldf) de la base de datos de SQL Server. |
| **MSSQL_DUMP_DIR** | Cambie el directorio en el que SQL Server depositará de forma predeterminada los volcados de memoria y otros archivos de solución de problemas. |
| **MSSQL_ENABLE_HADR** | Habilite el grupo de disponibilidad. Por ejemplo, "1" significa que está habilitado y "0", deshabilitado. |
| **MSSQL_AGENT_ENABLED** | Habilite el Agente SQL Server. Por ejemplo, "true" significa que está habilitado y "false", deshabilitado. De forma predeterminada, el agente está deshabilitado.  |
| **MSSQL_MASTER_DATA_FILE** | Establece la ubicación del archivo de datos de la base de datos maestra. Debe denominarse **master.mdf** hasta la primera ejecución de SQL Server. |
| **MSSQL_MASTER_LOG_FILE** | Establece la ubicación del archivo de registro de la base de datos maestra. Debe denominarse **mastlog.ldf** hasta la primera ejecución de SQL Server. |
| **MSSQL_ERROR_LOG_FILE** | Establece la ubicación de los archivos del registro de errores. |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

| Variable de entorno | Descripción |
|-----|-----|
| **ACCEPT_EULA** | Establezca la variable **ACCEPT_EULA** en cualquier valor para confirmar que acepta el [Contrato de licencia de usuario final](https://go.microsoft.com/fwlink/?LinkId=746388). Configuración requerida para la imagen de SQL Server. |
| **MSSQL_SA_PASSWORD** | Configure la contraseña de usuario SA. |
| **MSSQL_PID** | Establezca la edición de SQL Server o la clave de producto. Los valores posibles son: </br></br>**Evaluación**</br>**Developer**</br>**Express**</br>**Web**</br>**Estándar**</br>**Enterprise**</br>**Clave de producto**</br></br>Si especifica una clave de producto, debe tener el formato #####-#####-#####-#####-#####, donde "#" es un número o una letra.|
| **MSSQL_LCID** | Establece el identificador de idioma que se usará para SQL Server. Por ejemplo, 1036 es francés. |
| **MSSQL_COLLATION** | Establece la intercalación predeterminada para SQL Server. Esto reemplaza la asignación predeterminada de identificador de idioma (LCID) por la intercalación. |
| **MSSQL_MEMORY_LIMIT_MB** | Establece la cantidad máxima de memoria (en MB) que puede usar SQL Server. De forma predeterminada, es el 80 % de la memoria física total. |
| **MSSQL_TCP_PORT** | Configure el puerto TCP en el que escucha SQL Server (valor predeterminado: 1433). |
| **MSSQL_IP_ADDRESS** | Establezca la dirección IP. Actualmente, la dirección IP debe ser de estilo IPv4 (0.0.0.0). |
| **MSSQL_BACKUP_DIR** | Establezca la ubicación del directorio de copia de seguridad predeterminado. |
| **MSSQL_DATA_DIR** | Cambie el directorio en el que se crean los archivos de datos (.mdf) de la base de datos de SQL Server. |
| **MSSQL_LOG_DIR** | Cambie el directorio en el que se crean los archivos de registro (.ldf) de la base de datos de SQL Server. |
| **MSSQL_DUMP_DIR** | Cambie el directorio en el que SQL Server depositará de forma predeterminada los volcados de memoria y otros archivos de solución de problemas. |
| **MSSQL_ENABLE_HADR** | Habilite el grupo de disponibilidad. Por ejemplo, "1" significa que está habilitado y "0", deshabilitado. |
| **MSSQL_AGENT_ENABLED** | Habilite el Agente SQL Server. Por ejemplo, "true" significa que está habilitado y "false", deshabilitado. De forma predeterminada, el agente está deshabilitado.  |
| **MSSQL_MASTER_DATA_FILE** | Establece la ubicación del archivo de datos de la base de datos maestra. Debe denominarse **master.mdf** hasta la primera ejecución de SQL Server. |
| **MSSQL_MASTER_LOG_FILE** | Establece la ubicación del archivo de registro de la base de datos maestra. Debe denominarse **mastlog.ldf** hasta la primera ejecución de SQL Server. |
| **MSSQL_ERROR_LOG_FILE** | Establece la ubicación de los archivos del registro de errores. |

::: moniker-end

## <a name="use-with-initial-setup"></a>Uso con la configuración inicial

En este ejemplo se ejecuta `mssql-conf setup` con variables de entorno configuradas. Se especifican las siguientes variables de entorno:

- **ACCEPT_EULA** acepta el contrato de licencia para el usuario final.
- **MSSQL_PID** especifica la edición para desarrolladores con licencia gratuita de SQL Server para su uso en entornos que no son de producción.
- **MSSQL_SA_PASSWORD** establece una contraseña segura.
- **MSSQL_TCP_PORT** establece en 1234 el puerto TCP en el que escucha SQL Server.

```bash
sudo ACCEPT_EULA='Y' MSSQL_PID='Developer' MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' MSSQL_TCP_PORT=1234 /opt/mssql/bin/mssql-conf setup
```

## <a name="use-with-docker"></a>Uso con Docker

Este comando de Docker de ejemplo usa las siguientes variables de entorno para crear un contenedor de SQL Server:

- **ACCEPT_EULA** acepta el contrato de licencia para el usuario final.
- **MSSQL_PID** especifica la edición para desarrolladores con licencia gratuita de SQL Server para su uso en entornos que no son de producción.
- **MSSQL_SA_PASSWORD** establece una contraseña segura.
- **MSSQL_TCP_PORT** establece en 1234 el puerto TCP en el que escucha SQL Server. Esto significa que, en lugar de asignar el puerto 1433 (predeterminado) a un puerto de host, el puerto TCP personalizado se debe asignar con el comando `-p 1234:1234` en este ejemplo.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Si ejecuta Docker en Linux/macOS, use la siguiente sintaxis con comillas simples:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

Si ejecuta Docker en Windows, use la siguiente sintaxis con comillas dobles:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

> [!NOTE]
> El proceso para ejecutar las ediciones de producción en contenedores es ligeramente diferente. Para obtener más información, vea [Run production container image](./sql-server-linux-docker-container-deployment.md#production) (Ejecutar imágenes de contenedor de producción).

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Si ejecuta Docker en Linux/macOS, use la siguiente sintaxis con comillas simples:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

Si ejecuta Docker en Windows, use la siguiente sintaxis con comillas dobles:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

Para ver otras opciones de SQL Server que no se muestran aquí, consulte [Configurar SQL Server en Linux con la herramienta mssql-conf](sql-server-linux-configure-mssql-conf.md).

Para obtener más información sobre cómo instalar y ejecutar SQL Server en Linux, consulte la [Guía de instalación para SQL Server en Linux](sql-server-linux-setup.md).