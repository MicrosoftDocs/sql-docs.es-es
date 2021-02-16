---
title: Configuración de la recopilación de datos de uso y diagnóstico para SQL Server en Linux
description: Se explica cómo se recopilan y se configuran los datos de diagnóstico y uso de los clientes de SQL Server en Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 31f391360636bb27bf346ee76614c409738347e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347318"
---
# <a name="configure-usage--diagnostic-data-collection-for-sql-server-on-linux"></a>Configuración de la recopilación de datos de uso y diagnóstico para SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

De forma predeterminada, Microsoft SQL Server recopila información sobre cómo sus clientes usan la aplicación. En concreto, SQL Server recopila información sobre la experiencia de instalación, el uso y el rendimiento. Esta información ayuda a Microsoft a mejorar el producto para satisfacer mejor las necesidades del cliente. Por ejemplo, Microsoft recopila información sobre los tipos de códigos de error que encuentran los clientes para que podamos corregir errores relacionados, mejorar nuestra documentación sobre cómo usar SQL Server y determinar si deben agregarse características al producto para ofrecer un mejor servicio a los clientes.

En este documento se proporcionan detalles sobre los tipos de información que se recopila y sobre cómo configurar Microsoft SQL Server en Linux para enviar esa información recopilada a Microsoft. SQL Server 2017 incluye una declaración de privacidad que explica qué información de los usuarios se recopila y cuál no. Para obtener más información, vea la [declaración de privacidad](../sql-server/sql-server-privacy.md).

En concreto, Microsoft no envía ninguno de los tipos de información siguientes a través de este mecanismo:

- Valores de dentro de las tablas de usuario
- Credenciales de inicio de sesión u otra información de autenticación
- Información de identificación personal

SQL Server 2017 siempre recopila y envía información sobre la experiencia de instalación del proceso de configuración para que podamos encontrar y corregir con rapidez cualquier problema de instalación que experimente el cliente. SQL Server 2017 se puede configurar para que no envíe información (en cada instancia de servidor) a Microsoft mediante **mssql-conf**. mssql-conf es un script de configuración que se instala con SQL Server 2017 para Red Hat Enterprise Linux, SUSE Linux Enterprise Server y Ubuntu.

> [!NOTE]
> Puede deshabilitar el envío de información a Microsoft solo en versiones de pago de SQL Server.

## <a name="disable-usage-and-diagnostic-data-collection"></a>Deshabilitar la recopilación de datos de uso y diagnóstico

Esta opción permite cambiar si SQL Server envía la recopilación de datos de uso y diagnóstico a Microsoft o no. De forma predeterminada, este valor está establecido en true. Para cambiar el valor, ejecute los siguientes comandos:

> [!IMPORTANT]
> No puede desactivar la recopilación de datos de uso y diagnóstico en las ediciones gratuitas de SQL Server, Express y Developer.

### <a name="on-red-hat-suse-and-ubuntu"></a>En Red Hat, SUSE y Ubuntu

1. Ejecute el script mssql-conf como raíz con el comando **set** para **telemetry.customerfeedback**. En el ejemplo siguiente se desactiva la recopilación de datos de uso y diagnóstico al especificar **false**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>En Docker
Para deshabilitar la recopilación de datos de uso y diagnóstico en Docker, debe hacer que Docker [conserve los datos](./sql-server-linux-docker-container-deployment.md). 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Agregue un archivo `mssql.conf` con las líneas `[telemetry]` y `customerfeedback = false` en el directorio host:
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. Ejecute la imagen de contenedor.

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Agregue un archivo `mssql.conf` con las líneas `[telemetry]` y `customerfeedback = false` en el directorio host:

   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'customerfeedback = false' >> <host directory>/mssql.conf
   ```

2. Ejecute la imagen de contenedor.

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="local-audit-for-sql-server-on-linux-usage-and-diagnostic-data-collection"></a>Auditoría local para recopilación de datos de uso y diagnóstico de SQL Server en Linux

Microsoft SQL Server 2017 contiene características habilitadas para Internet que pueden recopilar y enviar información sobre el equipo o dispositivo ("información estándar sobre equipos") a Microsoft. El componente Auditoría local para recopilación de datos de uso y diagnóstico de SQL Server puede escribir los datos recopilados por el servicio en una carpeta designada, lo que representa los datos (registros) que se van a enviar a Microsoft. El propósito de la Auditoría local es permitir que los clientes vean todos los datos que Microsoft recopila con esta característica, por motivos de cumplimiento, reglamentarios o por validación de privacidad.

En SQL Server en Linux, Auditoría local se puede configurar en el nivel de instancia de Motor de base de datos de SQL Server. Otros componentes y herramientas de SQL Server no tienen la capacidad Auditoría local para recopilación de datos de uso y diagnóstico.

### <a name="enable-local-audit"></a>Habilitar Auditoría local

Esta opción habilita Auditoría local y permite establecer el directorio en el que se crean los registros de Auditoría local.

1. Cree un directorio de destino para los nuevos registros de Auditoría local. En el ejemplo siguiente se crea un nuevo directorio **/tmp/audit**:

   ```bash
   sudo mkdir /tmp/audit
   ```

2. Cambie el propietario y el grupo del directorio al usuario **mssql**:

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

3. Ejecute el script mssql-conf como raíz con el comando **set** para **telemetry.userrequestedlocalauditdirectory**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

4. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```
   
### <a name="on-docker"></a>En Docker
Para habilitar Auditoría local en Docker, debe hacer que Docker [conserve los datos](./sql-server-linux-docker-container-deployment.md). 

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. El directorio de destino de los nuevos registros de Auditoría local está en el contenedor. Cree un directorio de destino para los nuevos registros de Auditoría local en el directorio host del equipo. En el ejemplo siguiente se crea un nuevo directorio **/audit**:

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. Agregue un archivo `mssql.conf` con las líneas `[telemetry]` y `userrequestedlocalauditdirectory = <host directory>/audit` en el directorio host:
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. Ejecute la imagen de contenedor.

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. El directorio de destino de los nuevos registros de Auditoría local está en el contenedor. Cree un directorio de destino para los nuevos registros de Auditoría local en el directorio host del equipo. En el ejemplo siguiente se crea un nuevo directorio **/audit**:

   ```bash
   sudo mkdir <host directory>/audit
   ```

1. Agregue un archivo `mssql.conf` con las líneas `[telemetry]` y `userrequestedlocalauditdirectory = <host directory>/audit` en el directorio host:
 
   ```bash
   echo '[telemetry]' >> <host directory>/mssql.conf
   ```

   ```bash
   echo 'userrequestedlocalauditdirectory = <host directory>/audit' >> <host directory>/mssql.conf
   ```

1. Ejecute la imagen de contenedor.

   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
   ```

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre SQL Server en Linux, vea [Introducción a SQL Server en Linux](sql-server-linux-overview.md).