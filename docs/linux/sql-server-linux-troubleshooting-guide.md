---
title: Solución de problemas de SQL Server en Linux
description: Solucione los problemas de Microsoft SQL Server en Linux o en un contenedor de Docker. Obtenga información sobre dónde encontrar información de las características admitidas y las limitaciones conocidas.
author: VanMSFT
ms.author: vanto
ms.date: 05/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 99636ee8-2ba6-4316-88e0-121988eebcf9S
ms.openlocfilehash: 1f2df7f7cb0660cb879334a1099f6b7ce639d211
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464643"
---
# <a name="troubleshoot-sql-server-on-linux"></a>Solución de problemas de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este documento se describe cómo solucionar problemas de Microsoft SQL Server en Linux o en un contenedor de Docker. A la hora de solucionar problemas de SQL Server en Linux, no olvide revisar las características admitidas y las limitaciones conocidas en las [Notas de la versión de SQL Server en Linux](sql-server-linux-release-notes.md).

> [!TIP]
> Para obtener respuesta a las preguntas más frecuentes, vea [Preguntas más frecuentes sobre SQL Server en Linux](sql-server-linux-faq.yml).

## <a name="troubleshoot-connection-failures"></a><a id="connection"></a> Solución de errores de conexión
Si tiene dificultades para conectarse a la instancia de SQL Server de Linux, hay que comprobar varias cosas.

- Si no puede conectarse localmente mediante **localhost**, pruebe a usar la dirección IP 127.0.0.1. Es posible que **localhost** no esté asignado correctamente a esta dirección.

- Compruebe que es posible acceder al nombre del servidor o a la dirección IP desde el equipo cliente.

   > [!TIP]
   > Para buscar la dirección IP de la máquina Ubuntu, puede ejecutar el comando ifconfig como en el ejemplo siguiente:
   >
   >   ```bash
   >   sudo ifconfig eth0 | grep 'inet addr'
   >   ```
   > Para Red Hat, puede usar ip addr como en el ejemplo siguiente:
   >
   >   ```bash
   >   sudo ip addr show eth0 | grep "inet"
   >   ```
   > Las máquinas virtuales de Azure son una excepción a esta técnica. En este caso, [busque la dirección IP pública de la máquina virtual en Azure Portal](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#connect).

- Si procede, compruebe que ha abierto el puerto de SQL Server (valor predeterminado 1433) en el firewall.

- En el caso de las máquinas virtuales de Azure, compruebe que tiene una [regla de grupo de seguridad de red para el puerto predeterminado de SQL Server](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#remote).

- Compruebe que el nombre de usuario y la contraseña no contienen errores tipográficos ni espacios adicionales, y que el uso de mayúsculas sea correcto.

- Intente establecer explícitamente el protocolo y el número de puerto con el nombre del servidor, como en el ejemplo siguiente: **tcp:servername,1433**.

- Los problemas de conectividad de red también pueden producir tiempos de espera y errores de conexión. Después de comprobar la información de conexión y la conectividad de red, intente realizar la conexión de nuevo.

## <a name="manage-the-sql-server-service"></a>Administración del servicio SQL Server

En las secciones siguientes se muestra cómo iniciar, detener, reiniciar y comprobar el estado del servicio SQL Server.

### <a name="manage-the-mssql-server-service-in-red-hat-enterprise-linux-rhel-and-ubuntu"></a>Administración del servicio mssql-server en Red Hat Enterprise Linux (RHEL) y Ubuntu 

Compruebe el estado del servicio SQL Server con este comando:

   ```bash
   sudo systemctl status mssql-server
   ```

Puede detener, iniciar o reiniciar el servicio SQL Server según sea necesario mediante los comandos siguientes:

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

### <a name="manage-the-execution-of-the-mssql-docker-container"></a>Administración de la ejecución del contenedor de Docker mssql

Puede obtener el estado y el identificador del contenedor de Docker de SQL Server creado más recientemente mediante el comando siguiente (el identificador está en la columna **ID. DE CONTENEDOR**):

   ```bash
   sudo docker ps -l
   ```
   
Puede detener o reiniciar el servicio SQL Server según sea necesario mediante los comandos siguientes:
   
   ```bash
   sudo docker stop <container ID>
   sudo docker restart <container ID>
   ```

> [!TIP]
> Para obtener más sugerencias para la solución de problemas de Docker, vea [Solución de problemas de contenedores de Docker de SQL Server](./sql-server-linux-docker-container-troubleshooting.md).

## <a name="access-the-log-files"></a>Acceso a los archivos de registro
   
El motor de SQL Server crea registros en el archivo /var/opt/mssql/log/errorlog tanto en las instalaciones de Linux como en las de Docker. Debe estar en modo de "superusuario" para poder examinar este directorio.

El instalador crea registros aquí: /var/opt/mssql/setup-<marca de tiempo que representa la hora de la instalación>. Puede examinar los archivos de errorlog con cualquier herramienta compatible con UTF-16, como, por ejemplo, "vim" o "cat": 

   ```bash
   sudo cat errorlog
   ```

Si lo prefiere, también puede usar el comando siguiente para convertir los archivos a UTF-8 y leerlos con "more" o "less":
   
   ```bash
   sudo iconv -f UTF-16LE -t UTF-8 <errorlog> -o <output errorlog file>
   ```
## <a name="extended-events"></a>Eventos extendidos

Los eventos extendidos se pueden consultar con un comando SQL.  [Aquí](../relational-databases/extended-events/extended-events.md) encontrará más información sobre los eventos extendidos:

## <a name="crash-dumps"></a>Volcados de memoria 

Busque volcados en el directorio de registro de Linux. Compruebe en el directorio /var/opt/mssql/log si hay volcados de Linux Core (extensión .tar.gz2) o minivolcados de SQL (extensión .mdmp)

Para volcados de Core 
   ```bash
   sudo ls /var/opt/mssql/log | grep .tar.gz2 
   ```

Para volcados de SQL 
   ```bash
   sudo ls /var/opt/mssql/log | grep .mdmp 
   ```
   
## <a name="start-sql-server-in-minimal-configuration-or-in-single-user-mode"></a>Inicio de SQL Server con la configuración mínima o en modo de usuario único

### <a name="start-sql-server-in-minimal-configuration-mode"></a>Inicio de SQL Server con la configuración mínima
Esta opción resulta útil si el valor de una opción de configuración (por ejemplo, la confirmación excesiva de memoria) ha impedido el inicio del servidor.
  
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -f
   ```

### <a name="start-sql-server-in-single-user-mode"></a>Inicio de SQL Server en modo de usuario único
En determinadas circunstancias, puede que sea necesario iniciar una instancia de SQL Server en modo de usuario único mediante la opción de inicio -m. Por ejemplo, es posible que desee cambiar las opciones de configuración del servidor o recuperar una base de datos maestra dañada u otra base de datos del sistema. Por ejemplo, es posible que desee cambiar las opciones de configuración del servidor o recuperar una base de datos maestra dañada u otra base de datos del sistema.   

Inicio de SQL Server en modo de usuario único
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m
   ```

Inicio de SQL Server en modo de usuario único con SQLCMD
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m SQLCMD
   ```
  
> [!WARNING]  
>  Inicie SQL Server en Linux con el usuario "mssql" para evitar problemas de inicio futuros. Ejemplo "sudo -u mssql /opt/mssql/bin/sqlservr [OPCIONES DE INICIO]" 

Si ha iniciado accidentalmente SQL Server con otro usuario, debe devolver la propiedad de los archivos de base de datos de SQL Server al usuario "mssql" antes de iniciar SQL Server con systemd. Por ejemplo, para cambiar la propiedad de todos los archivos de base de datos en /var/opt/mssql al usuario "mssql", ejecute el siguiente comando:

   ```bash
   chown -R mssql:mssql /var/opt/mssql/
   ```

## <a name="rebuild-system-databases"></a>Regeneración de las bases de datos del sistema
Como último recurso, puede volver a generar las bases de datos maestra y modelo a las versiones predeterminadas.

> [!WARNING]
> Con estos pasos **se ELIMINARÁN todos los datos del sistema SQL Server** que haya configurado. Esto incluye la información sobre las bases de datos de usuario (pero no las propias bases de datos de usuario). También se eliminará otra información almacenada en las bases de datos del sistema, como la información de la clave maestra, los certificados cargados en la base de datos maestra, la contraseña de inicio de sesión de administrador del sistema, la información relacionada con el trabajo de msdb, la información de correo de base de datos de msdb y las opciones de sp_configure. Solo debe usarse si se entienden las implicaciones.

1. Detenga SQL Server.

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Ejecute **sqlservr** con el parámetro **force-setup**. 

   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr --force-setup
   ```
   
   > [!WARNING]
   > Vea la advertencia anterior. Además, debe ejecutarlo como usuario **mssql**, tal como se muestra aquí.

1. Después de ver el mensaje "la recuperación se ha completado", presione CTRL + C. Se cerrará SQL Server.

1. Vuelva a configurar la contraseña de administrador del sistema.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```
   
1. Inicie SQL Server y vuelva a configurar el servidor. Esto incluye restaurar o volver a adjuntar todas las bases de datos de usuario.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="improve-performance"></a>Mejorar el rendimiento

Hay muchos factores que afectan al rendimiento, como el diseño de la base de datos, el hardware y las demandas de cargas de trabajo. Si quiere mejorar el rendimiento, empiece por revisar los procedimientos recomendados que aparecen en el artículo [Procedimientos recomendados e instrucciones de configuración para SQL Server en Linux](sql-server-linux-performance-best-practices.md). A continuación, explore algunas de las herramientas disponibles para solucionar problemas de rendimiento.

- [Almacén de consultas](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Vistas de administración dinámica del sistema (DMV)](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [Panel de rendimiento en SQL Server Management Studio](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in)

## <a name="common-issues"></a>Problemas comunes

1. No es posible conectarse a la instancia remota de SQL Server.

   Consulte la sección de solución de problemas del artículo [Conectarse a SQL Server en Linux](#connection).

2. ERROR: El nombre debe tener 15 caracteres como máximo.

   Se trata de un problema conocido que se produce siempre que el nombre de la máquina que intenta instalar el paquete de Debian de SQL Server tiene más de 15 caracteres. Actualmente no hay ninguna solución alternativa que no sea cambiar el nombre de la máquina. Una manera de lograrlo es editar el archivo de nombre de host y reiniciar el equipo. En la siguiente [guía de sitio web](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/) se explica con detalle este procedimiento.

3. Restablecimiento de la contraseña de administración del sistema (SA).

   Si ha olvidado la contraseña de administrador del sistema (SA) o necesita restablecerla por algún otro motivo, siga estos pasos.

   > [!NOTE]
   > En los pasos siguientes se detiene temporalmente el servicio SQL Server.

   Inicie sesión en el terminal del host, ejecute los siguientes comandos y siga las indicaciones para restablecer la contraseña de administrador del sistema:

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf setup
   ```

4. Uso de caracteres especiales en la contraseña.

   Si usa caracteres en la contraseña de inicio de sesión de SQL Server, es posible que tenga que usar un carácter de escape (una barra diagonal inversa) al utilizarlos en un comando de Linux en el terminal. Por ejemplo, debe usar un carácter de escape en el signo de dólar ($) siempre que lo use en un script de comando/shell del terminal:

   No funciona:

   ```bash
   sudo sqlcmd -S myserver -U sa -P Test$$
   ```

   Funciona:

   ```bash
   sqlcmd -S myserver -U sa -P Test\$\$
   ```

   Recursos: [Caracteres especiales](https://tldp.org/LDP/abs/html/special-chars.html)
   [Escape](https://tldp.org/LDP/abs/html/escapingsection.html)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]