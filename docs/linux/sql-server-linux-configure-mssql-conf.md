---
title: Configuración de las opciones de SQL Server en Linux
description: En este artículo se describe cómo usar la herramienta mssql-conf para configurar las opciones de SQL Server en Linux.
author: VanMSFT
ms.author: vanto
ms.date: 08/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 06798dff-65c7-43e0-9ab3-ffb23374b322
ms.openlocfilehash: 72d5179d64cda06a95e531d8cbc847b33ce9b92f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237250"
---
# <a name="configure-sql-server-on-linux-with-the-mssql-conf-tool"></a>Configuración de SQL Server en Linux con la herramienta mssql-conf

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

**mssql-conf** es un script de configuración que se instala con SQL Server 2017 para Red Hat Enterprise Linux, SUSE Linux Enterprise Server y Ubuntu. Modifica el [**archivo mssql.conf**](#mssql-conf-format) donde se almacenan los valores de configuración. Puede usar la utilidad **mssql-conf** para establecer los parámetros siguientes:

|Parámetro|Descripción|
|---|---|
| [Agent](#agent) | Habilite el Agente SQL Server. |
| [Intercalación](#collation) | Establezca una nueva intercalación para SQL Server en Linux. |
| [Comentarios del cliente](#customerfeedback) | Elija si SQL Server envía comentarios a Microsoft o no. |
| [Perfil de Correo electrónico de base de datos](#dbmail) | Establezca el perfil de correo electrónico de base de datos predeterminado para SQL Server en Linux. |
| [Directorio de datos predeterminado](#datadir) | Cambie el directorio predeterminado de los nuevos archivos de datos de la base de datos de SQL Server (.mdf). |
| [Directorio de registro predeterminado](#datadir) | Cambie el directorio predeterminado de los nuevos archivos de registro de la base de datos de SQL Server (.ldf). |
| [Directorio de base de datos maestra predeterminado](#masterdatabasedir) | Cambie el directorio predeterminado de la base de datos maestra y los archivos de registro.|
| [Nombre de archivo de base de datos maestra predeterminado](#masterdatabasename) | Cambie el nombre de los archivos de base de datos maestra. |
| [Directorio de volcado predeterminado](#dumpdir) | Cambie el directorio predeterminado de los nuevos volcados de memoria y otros archivos de solución de problemas. |
| [Directorio de registro de errores predeterminado](#errorlogdir) | Cambie el directorio predeterminado de los nuevos archivos de registro de errores, seguimiento del generador de perfiles predeterminado, XE de sesión de mantenimiento del sistema y XE de sesión de Hekaton de SQL Server. |
| [Directorio de copia de seguridad predeterminado](#backupdir) | Cambie el directorio predeterminado de los nuevos archivos de copia de seguridad. |
| [Tipo de volcado](#coredump) | Elija el tipo de archivo de volcado de memoria que se va a recopilar. |
| [Alta disponibilidad](#hadr) | Habilite los grupos de disponibilidad. |
| [Directorio de auditoría local](#localaudit) | Establezca un directorio para agregar los archivos de la auditoría local. |
| [Configuración regional](#lcid) | Establezca la configuración regional de SQL Server que se va a usar. |
| [Límite de memoria](#memorylimit) | Establezca el límite de memoria de SQL Server. |
| [Configuración de red](#network) | Configuración de red adicional para SQL Server. |
| [Microsoft DTC (Coordinador de transacciones distribuidas)](#msdtc) | Configure y solucione problemas de MSDTC en Linux. |
| [Puerto TCP](#tcpport) | Cambie el puerto en el que SQL Server escucha las conexiones. |
| [TLS](#tls) | Configure la seguridad de nivel de transporte. |
| [Marcas de seguimiento](#traceflags) | Establezca las marcas de seguimiento que va a usar el servicio. |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

**mssql-conf** es un script de configuración que se instala con [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] para Red Hat Enterprise Linux, SUSE Linux Enterprise Server y Ubuntu. Puede usar esta utilidad para establecer los siguientes parámetros:

|Parámetro|Descripción|
|---|---|
| [Agent](#agent) | Habilitar el Agente SQL Server |
| [Intercalación](#collation) | Establezca una nueva intercalación para SQL Server en Linux. |
| [Comentarios del cliente](#customerfeedback) | Elija si SQL Server envía comentarios a Microsoft o no. |
| [Perfil de Correo electrónico de base de datos](#dbmail) | Establezca el perfil de correo electrónico de base de datos predeterminado para SQL Server en Linux. |
| [Directorio de datos predeterminado](#datadir) | Cambie el directorio predeterminado de los nuevos archivos de datos de la base de datos de SQL Server (.mdf). |
| [Directorio de registro predeterminado](#datadir) | Cambie el directorio predeterminado de los nuevos archivos de registro de la base de datos de SQL Server (.ldf). |
| [Directorio de archivos de base de datos maestra predeterminado](#masterdatabasedir) | Cambie el directorio predeterminado de los archivos de base de datos maestra en la instalación existente de SQL.|
| [Nombre de archivo de base de datos maestra predeterminado](#masterdatabasename) | Cambie el nombre de los archivos de base de datos maestra. |
| [Directorio de volcado predeterminado](#dumpdir) | Cambie el directorio predeterminado de los nuevos volcados de memoria y otros archivos de solución de problemas. |
| [Directorio de registro de errores predeterminado](#errorlogdir) | Cambie el directorio predeterminado de los nuevos archivos de registro de errores, seguimiento del generador de perfiles predeterminado, XE de sesión de mantenimiento del sistema y XE de sesión de Hekaton de SQL Server. |
| [Directorio de copia de seguridad predeterminado](#backupdir) | Cambie el directorio predeterminado de los nuevos archivos de copia de seguridad. |
| [Tipo de volcado](#coredump) | Elija el tipo de archivo de volcado de memoria que se va a recopilar. |
| [Alta disponibilidad](#hadr) | Habilite los grupos de disponibilidad. |
| [Directorio de auditoría local](#localaudit) | Establezca un directorio para agregar los archivos de la auditoría local. |
| [Configuración regional](#lcid) | Establezca la configuración regional de SQL Server que se va a usar. |
| [Límite de memoria](#memorylimit) | Establezca el límite de memoria de SQL Server. |
| [Microsoft DTC (Coordinador de transacciones distribuidas)](#msdtc) | Configure y solucione problemas de MSDTC en Linux. |
| [CLUF de MLServices](#mlservices-eula) | Acepte los CLUF de R y Python de los paquetes de mlservices. Solo se aplica a SQL Server 2019.|
| [Configuración de red](#network) | Configuración de red adicional para SQL Server. |
| [outboundnetworkaccess](#mlservices-outbound-access) |Habilite el acceso de red saliente para las extensiones de R, Python y Java de [mlservices](sql-server-linux-setup-machine-learning.md).|
| [Puerto TCP](#tcpport) | Cambie el puerto en el que SQL Server escucha las conexiones. |
| [TLS](#tls) | Configure la seguridad de nivel de transporte. |
| [Marcas de seguimiento](#traceflags) | Establezca las marcas de seguimiento que va a usar el servicio. |

::: moniker-end

> [!TIP]
> Algunos de estos valores también se pueden configurar con variables de entorno. Para obtener más información, consulte [Configuración de opciones de SQL Server con variables de entorno](sql-server-linux-configure-environment-variables.md).

## <a name="usage-tips"></a>Consejos de uso

* Para los grupos de disponibilidad Always On y los clústeres de discos compartidos, haga siempre los mismos cambios de configuración en cada nodo.

* En el caso del escenario de clúster de disco compartido, no intente reiniciar el servicio **mssql-server** para aplicar los cambios. SQL Server se ejecuta como una aplicación. En su lugar, desconecte el recurso y vuelva a conectarlo.

* En estos ejemplos se ejecuta mssql-conf al especificar la ruta de acceso completa: **/opt/mssql/bin/mssql-conf**. Si opta por ir a esa ruta de acceso en su lugar, ejecute mssql-conf en el contexto del directorio actual: **./mssql-conf**.

## <a name="enable-sql-server-agent"></a><a id="agent"></a> Habilitación del Agente SQL Server

La configuración **sqlagent.enabled** habilita el [Agente SQL Server](sql-server-linux-run-sql-server-agent-job.md). De forma predeterminada, el Agente SQL Server está deshabilitado. Si **sqlagent.enabled** no está en el archivo de configuración mssql.conf, SQL Server asume de forma interna que el Agente SQL Server está deshabilitado.

Para cambiar esta configuración, siga estos pasos:

1. Habilite el Agente SQL Server:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   ```

2. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="set-the-default-database-mail-profile-for-sql-server-on-linux"></a><a id="dbmail"></a> Establecimiento del perfil de correo electrónico de base de datos predeterminado para SQL Server en Linux

**sqlpagent.databasemailprofile** le permite establecer el perfil de correo electrónico de base de datos predeterminado de las alertas por correo electrónico.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.databasemailprofile <profile_name>
```

### <a name="sql-agent-error-logs"></a><a id="agenterrorlog"></a> Registros de errores del Agente SQL

La configuración **sqlpagent.errorlogfile** y **sqlpagent.errorlogginglevel** permite establecer el nivel de registro y la ruta del archivo de registro del Agente SQL, respectivamente. 

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorfile <path>
```

Los niveles de registro del Agente SQL son valores de máscara de bits con las siguientes equivalencias:

- 1 = errores
- 2 = advertencias
- 4 = información

Si desea capturar todos los niveles, utilice `7` como valor.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorlogginglevel <level>
```

## <a name="change-the-sql-server-collation"></a><a id="collation"></a> Cambio de la intercalación de SQL Server

La opción **set-collation** cambia el valor de intercalación por cualquiera de las intercalaciones admitidas.

1. En primer lugar, [realice una copia de seguridad de todas las bases de datos de usuario](sql-server-linux-backup-and-restore-database.md) del servidor.

1. Después, use el procedimiento almacenado [sp_detach_db](../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md) para desasociar las bases de datos de usuario.

1. Ejecute la opción **set-collation** y siga las indicaciones:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-collation
   ```

1. La utilidad mssql-conf intentará cambiar al valor de intercalación especificado y reiniciar el servicio. Si hay algún error, revierte la intercalación al valor anterior.

1. Restaure las copias de seguridad de las bases de datos de usuario.

Para obtener una lista de las intercalaciones admitidas, ejecute la función [sys.fn_helpcollations](../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md): `SELECT Name from sys.fn_helpcollations()`.

## <a name="configure-customer-feedback"></a><a id="customerfeedback"></a> Configuración de comentarios del cliente

La configuración **telemetry.customerfeedback** cambia si SQL Server envía comentarios a Microsoft o no. De forma predeterminada, este valor se establece en **true** en todas las ediciones. Para cambiar el valor, ejecute los siguientes comandos:

> [!IMPORTANT]
> No puede desactivar los comentarios del cliente en las ediciones gratuitas de SQL Server, Express y Developer.

1. Ejecute el script mssql-conf como raíz con el comando **set** para **telemetry.customerfeedback**. En el siguiente ejemplo se especifica **false** para desactivar los comentarios del cliente.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

Para obtener más información, vea [Comentarios del cliente para SQL Server en Linux](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md) y la [Declaración de privacidad de SQL Server](../sql-server/sql-server-privacy.md).

## <a name="change-the-default-data-or-log-directory-location"></a><a id="datadir"></a> Cambio de la ubicación predeterminada del directorio de registro o los datos

Las configuraciones **filelocation.defaultdatadir** y **filelocation.defaultlogdir** cambian la ubicación en la que se crean los archivos de registro y de base de datos. De forma predeterminada, esta ubicación es /var/opt/mssql/data. Para cambiar estas configuraciones, siga estos pasos:

1. Cree el directorio de destino de los nuevos archivos de registro y datos de las bases de datos. En el ejemplo siguiente se crea un directorio **/tmp/data**:

   ```bash
   sudo mkdir /tmp/data
   ```

1. Cambie el propietario y el grupo del directorio al usuario **mssql**:

   ```bash
   sudo chown mssql /tmp/data
   sudo chgrp mssql /tmp/data
   ```

1. Use mssql-conf para cambiar el directorio de datos predeterminado con el comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdatadir /tmp/data
   ```

1. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

1. Ahora todos los archivos de base de datos de las nuevas bases de datos creadas se almacenarán en esta nueva ubicación. Si quiere cambiar la ubicación de los archivos de registro (.ldf) de las nuevas bases de datos, puede usar el siguiente comando "set":

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultlogdir /tmp/log
   ```

1. Este comando también supone que hay un directorio /tmp/log y que está en el usuario y grupo **mssql**.


## <a name="change-the-default-master-database-file-directory-location"></a><a id="masterdatabasedir"></a> Cambio de la ubicación del directorio de archivos de base de datos maestra predeterminado

Las configuraciones **filelocation.masterdatafile** y **filelocation.masterlogfile** cambian la ubicación en la que el motor de SQL Server busca los archivos de base de datos maestra. De forma predeterminada, esta ubicación es /var/opt/mssql/data.

Para cambiar estas configuraciones, siga estos pasos:

1. Cree el directorio de destino de los nuevos archivos de registro de errores. En el ejemplo siguiente se crea un directorio **/tmp/masterdatabasedir**:

   ```bash
   sudo mkdir /tmp/masterdatabasedir
   ```

1. Cambie el propietario y el grupo del directorio al usuario **mssql**:

   ```bash
   sudo chown mssql /tmp/masterdatabasedir
   sudo chgrp mssql /tmp/masterdatabasedir
   ```

1. Use mssql-conf para cambiar el directorio de base de datos maestra predeterminado de los archivos de registro y datos maestros con el comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /tmp/masterdatabasedir/master.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterlogfile /tmp/masterdatabasedir/mastlog.ldf
   ```

   > [!NOTE]
   > Además de mover los archivos de registro y datos maestros, también mueve la ubicación predeterminada de todas las demás bases de datos del sistema.

1. Detenga el servicio SQL Server:

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Mueva los archivos master.mdf y masterlog.ldf:

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /tmp/masterdatabasedir/master.mdf 
   sudo mv /var/opt/mssql/data/mastlog.ldf /tmp/masterdatabasedir/mastlog.ldf
   ```

1. Inicie el servicio SQL Server:

   ```bash
   sudo systemctl start mssql-server
   ```

   > [!NOTE]
   > Si SQL Server no encuentra los archivos master.mdf y mastlog.ldf en el directorio especificado, se creará automáticamente una copia con plantilla de las bases de datos del sistema en el directorio especificado y SQL Server se iniciará correctamente. En cambio, los metadatos como las bases de datos de usuario, los inicios de sesión del servidor, los certificados del servidor, las claves de cifrado, los trabajos del Agente SQL o la contraseña de inicio de sesión de SA antigua no se actualizarán en la nueva base de datos maestra. Tendrá que detener SQL Server y mover los antiguos archivos master.mdf y mastlog.ldf a la nueva ubicación especificada e iniciar SQL Server para seguir usando los metadatos existentes.
 
## <a name="change-the-name-of-master-database-files"></a><a id="masterdatabasename"></a> Cambio del nombre de los archivos de base de datos maestra

Las configuraciones **filelocation.masterdatafile** y **filelocation.masterlogfile** cambian la ubicación en la que el motor de SQL Server busca los archivos de base de datos maestra. También puede usarlas para cambiar el nombre de los archivos de registro y de base de datos maestra. 

Para cambiar estas configuraciones, siga estos pasos:

1. Detenga el servicio SQL Server:

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Use mssql-conf para cambiar los nombres esperados de la base de datos maestra de los archivos de registro y datos maestros con el comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /var/opt/mssql/data/masternew.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.mastlogfile /var/opt/mssql/data/mastlognew.ldf
   ```

   > [!IMPORTANT]
   > Solo puede cambiar el nombre de los archivos de registro y de base de datos maestra después de que SQL Server se haya iniciado correctamente. Antes de la ejecución inicial, SQL Server espera que los archivos se denominen master.mdf y mastlog.ldf.

1. Cambio del nombre de los archivos de registro y datos de la base de datos maestra 

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /var/opt/mssql/data/masternew.mdf
   sudo mv /var/opt/mssql/data/mastlog.ldf /var/opt/mssql/data/mastlognew.ldf
   ```

1. Inicie el servicio SQL Server:

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="change-the-default-dump-directory-location"></a><a id="dumpdir"></a> Cambio de la ubicación predeterminada del directorio de volcado

La configuración **filelocation.defaultdumpdir** cambia la ubicación predeterminada en la que se generan los volcados de memoria y SQL cuando se produce un bloqueo. De forma predeterminada, estos archivos se generan en /var/opt/mssql/log.

Para configurar esta nueva ubicación, use los comandos siguientes:

1. Cree el directorio de destino de los nuevos archivos de volcado. En el ejemplo siguiente se crea un directorio **/tmp/dump**:

   ```bash
   sudo mkdir /tmp/dump
   ```

1. Cambie el propietario y el grupo del directorio al usuario **mssql**:

   ```bash
   sudo chown mssql /tmp/dump
   sudo chgrp mssql /tmp/dump
   ```

1. Use mssql-conf para cambiar el directorio de datos predeterminado con el comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdumpdir /tmp/dump
   ```

1. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="change-the-default-error-log-file-directory-location"></a><a id="errorlogdir"></a> Cambio de la ubicación predeterminada del directorio de los archivos de registro de errores

La configuración **filelocation.errorlogfile** cambia la ubicación en la que se crean los archivos de registro de errores, seguimiento del generador de perfiles predeterminado, XE de sesión de mantenimiento del sistema y XE de sesión de Hekaton. De forma predeterminada, esta ubicación es /var/opt/mssql/log. El directorio en el que se establece el archivo de registro de errores de SQL se convierte en el directorio de registro predeterminado para otros registros.

Para cambiar esta configuración:

1. Cree el directorio de destino de los nuevos archivos de registro de errores. En el ejemplo siguiente se crea un directorio **/tmp/logs**:

   ```bash
   sudo mkdir /tmp/logs
   ```

1. Cambie el propietario y el grupo del directorio al usuario **mssql**:

   ```bash
   sudo chown mssql /tmp/logs
   sudo chgrp mssql /tmp/logs
   ```

1. Use mssql-conf para cambiar el nombre de archivo predeterminado del registro de errores con el comando **set**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.errorlogfile /tmp/logs/errorlog
   ```

1. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

La configuración `errorlog.numerrorlogs` permitirá especificar el número de registros de errores mantenidos antes de recorrer el registro.

## <a name="change-the-default-backup-directory-location"></a><a id="backupdir"></a> Cambio de la ubicación predeterminada del directorio de copia de seguridad

La configuración **filelocation.defaultbackupdir** cambia la ubicación predeterminada en la que se generan los archivos de copia de seguridad. De forma predeterminada, estos archivos se generan en /var/opt/mssql/data.

Para configurar esta nueva ubicación, use los comandos siguientes:

1. Cree el directorio de destino de los nuevos archivos de copia de seguridad. En el ejemplo siguiente se crea un directorio **/tmp/backup**:

   ```bash
   sudo mkdir /tmp/backup
   ```

1. Cambie el propietario y el grupo del directorio al usuario **mssql**:

   ```bash
   sudo chown mssql /tmp/backup
   sudo chgrp mssql /tmp/backup
   ```

1. Use mssql-conf para cambiar el directorio predeterminado de copia de seguridad con el comando “set”:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultbackupdir /tmp/backup
   ```

1. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="specify-core-dump-settings"></a><a id="coredump"></a> Especificación de la configuración de volcado principal

Si se produce una excepción en uno de los procesos de SQL Server, este crea un volcado de memoria.

Hay dos opciones para controlar el tipo de los volcados de memoria que SQL Server recopila: **coredump.coredumptype** y **coredump.captureminiandfull**. Estas se relacionan con las dos fases de la captura de volcado principal. 

La primera captura de fase se controla mediante la configuración **coredump.coredumptype**, que determina el tipo de archivo de volcado generado durante una excepción. La segunda fase se habilita con la configuración **coredump.captureminiandfull**. Si **coredump.captureminiandfull** se establece en true, se genera el archivo de volcado especificado por **coredump.coredumptype** y también se genera un segundo minivolcado. Si se establece **coredump.captureminiandfull** en false, se deshabilita el segundo intento de captura.

1. Decida si quiere capturar los minivolcados y los volcados completos con la configuración **coredump.captureminiandfull**.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.captureminiandfull <true or false>
    ```

    Valor predeterminado: **false**.

1. Especifique el tipo de archivo de volcado de la configuración **coredump.coredumptype**.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.coredumptype <dump_type>
    ```

    Valor predeterminado: **miniplus**.

    En la siguiente tabla se muestran los posibles valores de **coredump.coredumptype**.

    | Tipo | Descripción |
    |-----|-----|
    | **mini** | Mini es el tipo de archivo de volcado más pequeño. Usa la información del sistema Linux para determinar los subprocesos y los módulos del proceso. El volcado solo contiene los módulos y las pilas de subprocesos del entorno del host. No contiene referencias de memoria indirectas ni globales. |
    | **miniplus** | MiniPlus es similar a mini, pero incluye memoria adicional. Comprende los aspectos internos de SQLPAL y el entorno de host, y agrega las siguientes regiones de memoria al volcado:</br></br> - Varias globales</br> - Toda la memoria superior a 64 TB</br> - Todas las regiones con nombre que están en **/proc/$pid/maps**</br> - Memoria indirecta de los subprocesos y las pilas</br> - Información del subproceso</br> - Valores de Teb y Peb asociados</br> - Información del módulo</br> - Árbol de VAD y VMM |
    | **filtered** | Filtered usa un diseño basado en la resta en el que se incluye toda la memoria del proceso a menos que se excluya específicamente. El diseño comprende los aspectos internos de SQLPAL y el entorno de host, y excluye determinadas regiones del volcado.
    | **full** | Full es un volcado de proceso completo que incluye todas las regiones que están en **/proc/$pid/maps**. No se controla mediante la configuración **coredump.captureminiandfull**. |

## <a name="high-availability"></a><a id="hadr"></a> Alta disponibilidad

La opción **hadr.hadrenabled** habilita los grupos de disponibilidad en la instancia de SQL Server. El siguiente comando habilita los grupos de disponibilidad al establecer **hadr.hadrenabled** en 1. Debe reiniciar SQL Server para que la configuración surta efecto.

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled  1
sudo systemctl restart mssql-server
```

Para obtener información sobre cómo se usa esto con los grupos de disponibilidad, vea los siguientes dos temas.

- [Configuración del grupo de disponibilidad Always On para SQL Server en Linux](sql-server-linux-availability-group-configure-ha.md)
- [Configurar el grupo de disponibilidad de escalado de lectura para SQL Server en Linux](sql-server-linux-availability-group-configure-rs.md)


## <a name="set-local-audit-directory"></a><a id="localaudit"></a> Establecimiento del directorio de auditoría local

La configuración **telemetry.userrequestedlocalauditdirectory** habilita la auditoría local y le permite establecer el directorio en el que se crean los registros de auditoría local.

1. Cree un directorio de destino para los nuevos registros de Auditoría local. En el ejemplo siguiente se crea un nuevo directorio **/tmp/audit**:

   ```bash
   sudo mkdir /tmp/audit
   ```

1. Cambie el propietario y el grupo del directorio al usuario **mssql**:

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

1. Ejecute el script mssql-conf como raíz con el comando **set** para **telemetry.userrequestedlocalauditdirectory**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

1. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

Para obtener más información, vea [Comentarios del cliente para SQL Server en Linux](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md).

## <a name="change-the-sql-server-locale"></a><a id="lcid"></a> Cambio de la configuración regional de SQL Server

La configuración **language.lcid** cambia la configuración regional de SQL Server por cualquier identificador de lenguaje (LCID) admitido. 

1. En el ejemplo siguiente se cambia la configuración regional a Francés (1036):

   ```bash
   sudo /opt/mssql/bin/mssql-conf set language.lcid 1036
   ```

1. Reinicie el servicio SQL Server para aplicar los cambios:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="set-the-memory-limit"></a><a id="memorylimit"></a> Establecimiento del límite de memoria

La configuración **memory.memorylimitmb** controla la cantidad de memoria física (en MB) disponible para SQL Server. El valor predeterminado es el 80 % de la memoria física.

1. Ejecute el script mssql-conf como raíz con el comando **set** para **memory.memorylimitmb**. En el ejemplo siguiente se cambia la memoria disponible para SQL Server a 3,25 GB (3328 MB).

   ```bash
   sudo /opt/mssql/bin/mssql-conf set memory.memorylimitmb 3328
   ```

1. Reinicie el servicio SQL Server para aplicar los cambios:

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="additional-memory-settings"></a>Configuración de memoria adicional

Las opciones siguientes están disponibles para la configuración de memoria.

|Opción |Descripción |
|--- |--- |
| memory.disablememorypressure | Deshabilitación de la presión de memoria de SQL Server. Los valores pueden ser `true` o `false`. |
| memory.memory_optimized | Habilitación o deshabilitación de características optimizadas para memoria de SQL Server: optimización de archivos de memoria persistente, protección de la memoria. Los valores pueden ser `true` o `false`. |

## <a name="configure-msdtc"></a><a id="msdtc"></a> Configuración de MSDTC

Las configuraciones **network.rpcport** y **distributedtransaction.servertcpport** se usan para configurar Microsoft DTC (Coordinador de transacciones distribuidas). Para cambiar estas configuraciones, ejecute los siguientes comandos:

1. Ejecute el script mssql-conf como raíz con el comando **set** para "network.rpcport":

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.rpcport <rcp_port>
   ```

2. Después, establezca la configuración "distributedtransaction.servertcpport":

   ```bash
   sudo /opt/mssql/bin/mssql-conf set distributedtransaction.servertcpport <servertcpport_port>
   ```

Además de establecer estos valores, también debe configurar el enrutamiento y actualizar el firewall para el puerto 135. Para obtener más información sobre cómo hacerlo, consulte [Procedimiento para configurar MSDTC en Linux](sql-server-linux-configure-msdtc.md).

Hay algunas otras configuraciones de mssql-conf que puede usar para supervisar y solucionar problemas de MSDTC. En la siguiente tabla se describen brevemente estas configuraciones. Para obtener más información sobre su uso, consulte los detalles en el artículo de soporte técnico de Windows [Cómo habilitar el seguimiento de diagnóstico para MS DTC](https://support.microsoft.com/help/926099/how-to-enable-diagnostic-tracing-for-ms-dtc-on-a-windows-based-compute).

| Parámetro de mssql-conf | Descripción |
|---|---|
| distributedtransaction.allowonlysecurerpccalls | Configure solo llamadas RPC seguras para transacciones distribuidas. |
| distributedtransaction.fallbacktounsecurerpcifnecessary | Configure solo llamadas RPC de seguridad para transacciones distribuidas. |
| distributedtransaction.maxlogsize | Tamaño del archivo de registro de transacciones de DTC en MB. El valor predeterminado es 64 MB. |
| distributedtransaction.memorybuffersize | Tamaño de búfer circular en el que se almacenan los seguimientos. Este tamaño está en MB y el valor predeterminado es 10 MB. |
| distributedtransaction.servertcpport | Puerto de servidor RPC de MSDTC. |
| distributedtransaction.trace_cm | Seguimientos en el administrador de conexiones. |
| distributedtransaction.trace_contact | Se hace un seguimiento del grupo de contactos y los contactos. |
| distributedtransaction.trace_gateway | Se hace un seguimiento del origen de la puerta de enlace. |
| distributedtransaction.trace_log | Seguimiento del registro. |
| distributedtransaction.trace_misc | Seguimientos que no se pueden clasificar en las otras categorías. |
| distributedtransaction.trace_proxy | Seguimientos que se generan en el proxy de MSDTC. |
| distributedtransaction.trace_svc | Se hace un seguimiento del inicio del archivo .exe y el servicio. |
| distributedtransaction.trace_trace | Infraestructura de seguimiento. |
| distributedtransaction.trace_util | Rutinas de la utilidad de seguimiento a las que se llama desde varias ubicaciones. |
| distributedtransaction.trace_xa | Origen de seguimiento del administrador de transacciones XA (XATM). |
| distributedtransaction.tracefilepath | Carpeta en la que deben almacenarse los archivos de seguimiento. |
| distributedtransaction.turnoffrpcsecurity | Habilite o deshabilite la seguridad RPC de las transacciones distribuidas. |

::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="accept-mlservices-eulas"></a><a id="mlservices-eula"></a> Aceptación de los CLUF de MLServices

Al agregar [paquetes de R o Python de aprendizaje automático](sql-server-linux-setup-machine-learning.md) al motor de base de datos, tiene que aceptar los términos de licencia de las distribuciones de código abierto de R y Python. En la tabla siguiente se enumeran todos los comandos disponibles o las opciones relacionadas con los CLUF de mlservices. Se usa el mismo parámetro de CLUF para R y Python, en función de lo que haya instalado.

```bash
# For all packages: database engine and mlservices
# Setup prompts for mlservices EULAs, which you need to accept
sudo /opt/mssql/bin/mssql-conf setup

# Add R or Python to an existing installation
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml

# Alternative valid syntax
# Adds the EULA section to the INI and sets acceptulam to yes
sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y

# Rescind EULA acceptance and removes the setting
sudo /opt/mssql/bin/mssql-conf unset EULA accepteulaml
```

También puede agregar la aceptación del CLUF directamente al [archivo mssql.conf](#mssql-conf-format):

```ini
[EULA]
accepteula = Y
accepteulaml = Y
```
:::moniker-end
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="enable-outbound-network-access"></a><a id="mlservices-outbound-access"></a> Habilitación del acceso de red saliente

El acceso de red saliente para las extensiones de R, Python y Java en la característica [SQL Server Machine Learning Services](sql-server-linux-setup-machine-learning.md) está deshabilitado de forma predeterminada. Para habilitar las solicitudes salientes, establezca la propiedad booleana "outboundnetworkaccess" con mssql-conf.

Después de establecer la propiedad, reinicie el servicio SQL Server Launchpad para leer los valores actualizados del archivo INI. En un mensaje de reinicio se le recuerda si se ha modificado un valor relacionado con la extensibilidad.

```bash
# Adds the extensibility section and property.
# Sets "outboundnetworkaccess" to true.
# This setting is required if you want to access data or operations off the server.
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1

# Turns off network access but preserves the setting
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 0

# Removes the setting and rescinds network access
sudo /opt/mssql/bin/mssql-conf unset extensibility.outboundnetworkaccess
```

También puede agregar “outboundnetworkaccess” directamente al [archivo mssql.conf](#mssql-conf-format):

```ini
[extensibility]
outboundnetworkaccess = 1
```
:::moniker-end

## <a name="change-the-tcp-port"></a><a id="tcpport"></a> Cambio del puerto TCP

La configuración **network.tcpport** cambia el puerto TCP en el que SQL Server escucha las conexiones. De forma predeterminada, este puerto se establece en el 1433. Para cambiar el puerto, ejecute los siguientes comandos:

1. Ejecute el script mssql-conf como raíz con el comando "set" para "network.tcpport":

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.tcpport <new_tcp_port>
   ```

2. Reinicie el servicio SQL Server:

   ```bash
   sudo systemctl restart mssql-server
   ```

3. Al conectarse ahora a SQL Server, debe especificar el puerto personalizado con una coma (,) después del nombre de host o la dirección IP. Por ejemplo, para conectarse con SQLCMD, usaría el siguiente comando:

   ```bash
   sqlcmd -S localhost,<new_tcp_port> -U test -P test
   ```

## <a name="specify-tls-settings"></a><a id="tls"></a> Especificación de la configuración de TLS

Las opciones siguientes configuran TLS para una instancia de SQL Server que se ejecuta en Linux.

|Opción |Descripción |
|--- |--- |
|**network.forceencryption** |Si es 1, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] obliga a que se cifren todas las conexiones. De forma predeterminada, esta opción es 0. |
|**network.tlscert** |La ruta de acceso absoluta al archivo de certificado que usa [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para TLS. Ejemplo:   `/etc/ssl/certs/mssql.pem`. La cuenta de mssql debe poder acceder al archivo de certificado. Microsoft recomienda restringir el acceso al archivo mediante `chown mssql:mssql <file>; chmod 400 <file>`. |
|**network.tlskey** |Ruta de acceso absoluta al archivo de clave privada que usa [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para TLS. Ejemplo:  `/etc/ssl/private/mssql.key`. La cuenta de mssql debe poder acceder al archivo de certificado. Microsoft recomienda restringir el acceso al archivo mediante `chown mssql:mssql <file>; chmod 400 <file>`. |
|**network.tlsprotocols** |Lista separada por comas de los protocolos TLS que admite SQL Server. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] siempre intenta negociar el protocolo más seguro permitido. Si un cliente no admite ningún protocolo permitido, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] rechaza el intento de conexión.  Por motivos de compatibilidad, todos los protocolos admitidos se permiten de forma predeterminada (1.2, 1.1 y 1.0).  Si sus clientes admiten TLS 1.2, Microsoft recomienda permitir solo TLS 1.2. |
|**network.tlsciphers** |Especifica qué cifrados permite [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para TLS. Esta cadena debe tener el formato que se indica en la [lista de cifrado de OpenSSL](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html). En general, no debería tener que cambiar esta opción. <br /> De forma predeterminada, se permiten los siguientes cifrados: <br /> `ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA` |
| **network.kerberoskeytabfile** |Ruta de acceso al archivo keytab de Kerberos. |

Para obtener un ejemplo del uso de la configuración de TLS, consulte [Cifrar conexiones a SQL Server en Linux](sql-server-linux-encrypted-connections.md).

## <a name="network-settings"></a><a id="network"></a> Configuración de red

Consulte [Tutorial: Uso de la autenticación de Active Directory con SQL Server en Linux](sql-server-linux-active-directory-authentication.md) para obtener información completa sobre el uso de la autenticación de AD con SQL Server en Linux.

Las siguientes opciones son configuraciones de red adicionales que se pueden configurar mediante `mssql-conf`.

|Opción |Descripción |
|--- |--- |
| network.disablesssd | Deshabilitación de la consulta SSSD para obtener información de cuenta de AD y establecer las llamadas LDAP como método predeterminado. Los valores pueden ser `true` o `false`. |
| network.enablekdcfromkrb5conf | Habilitación de la búsqueda de información de KDC desde krb5.conf. Los valores pueden ser `true` o `false`. |
| network.forcesecureldap | Forzamiento del uso de LDAPS para contactar con el controlador de dominio. Los valores pueden ser `true` o `false`. |
| network.ipaddress | Dirección IP para las conexiones entrantes. |
| network.kerberoscredupdatefrequency | Tiempo en segundos entre las comprobaciones de las credenciales de Kerberos que deben actualizarse. El valor es un entero.|
| network.privilegedadaccount | Usuario con privilegios de AD que se usará para la autenticación de AD. El valor es `<username>`. Para más información, consulte el [Tutorial: Usar la autenticación de Active Directory con SQL Server en Linux](sql-server-linux-active-directory-authentication.md#spn)|
| uncmapping | Asigna la ruta de acceso UNC a una ruta de acceso local. Por ejemplo, `sudo /opt/mssql/bin/mssql-conf set uncmapping //servername/sharename /tmp/folder`. |

## <a name="enabledisable-traceflags"></a><a id="traceflags"></a> Habilitación o deshabilitación de las marcas de seguimiento

La opción **traceflag** habilita o deshabilita las marcas de seguimiento del inicio del servicio SQL Server. Para habilitar o deshabilitar una marca de seguimiento, use los siguientes comandos:

1. Habilite una marca de seguimiento con el siguiente comando. Por ejemplo, para la marca de seguimiento 1234:

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 on
   ```

1. Puede habilitar varias marcas de seguimiento si las especifica por separado:

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 2345 3456 on
   ```

1. De forma similar, puede deshabilitar una o varias marcas de seguimiento habilitadas si las especifica y agrega el parámetro **off**:

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 2345 3456 off
   ```

1. Reinicie el servicio SQL Server para aplicar los cambios:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="remove-a-setting"></a>Eliminación de una configuración

Para anular cualquier configuración que haya hecho con `mssql-conf set`, llame a **mssql-conf** con la opción `unset` y el nombre de la configuración. De esta forma, se borra la configuración y se devuelve a su valor predeterminado.

1. En el siguiente ejemplo se borra la opción **network.tcpport**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf unset network.tcpport
   ```

1. Reinicie el servicio SQL Server.

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="view-current-settings"></a>Visualización de la configuración actual

Para ver todas las opciones configuradas, ejecute el siguiente comando para generar el contenido del archivo **mssql.conf**:

```bash
sudo cat /var/opt/mssql/mssql.conf
```

Cualquier configuración que no se muestre en este archivo usa sus valores predeterminados. En la sección siguiente se proporciona un archivo **mssql.conf** de ejemplo.


## <a name="mssqlconf-format"></a><a id="mssql-conf-format"></a> Formato de mssql.conf

En el siguiente archivo **/var/opt/mssql/mssql.conf** se proporciona un ejemplo de cada configuración. Puede usar este formato para realizar cambios manualmente en el archivo **mssql.conf** según sea necesario. Si cambia el archivo manualmente, debe reiniciar SQL Server antes de que se apliquen los cambios. Para usar el archivo **mssql.conf** con Docker, debe hacer que Docker [conserve sus datos](./sql-server-linux-docker-container-deployment.md). En primer lugar, agregue un archivo **mssql.conf** completo al directorio de host y después ejecute el contenedor. Hay un ejemplo de este procedimiento en [Comentarios del cliente](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

```ini
[EULA]
accepteula = Y

[coredump]
captureminiandfull = true
coredumptype = full

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

```ini
[EULA]
accepteula = Y
accepteulaml = Y

[coredump]
captureminiandfull = true
coredumptype = full

[distributedtransaction]
servertcpport = 51999

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
rpcport = 13500
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

Para usar variables de entorno en su lugar a fin de realizar algunos de estos cambios de configuración, consulte [Configuración de SQL Server con variables de entorno](sql-server-linux-configure-environment-variables.md).

Para ver otras herramientas y escenarios de administración, consulte [Administrar SQL Server en Linux](sql-server-linux-management-overview.md).