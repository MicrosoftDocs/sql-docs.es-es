---
description: Instrucciones RESTORE (Transact-SQL)
title: RESTORE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE DATABASE
- RESTORE_TSQL
- RESTORE_DATABASE_TSQL
- RESTORE
- RESTORE_LOG_TSQL
- RESTORE LOG
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE DATABASE, see RESTORE statement
- full backups [SQL Server]
- RECOVERY option
- database snapshots [SQL Server], reverting to
- STOPAT syntax
- differential backups, RESTORE statement
- point in time recovery [SQL Server]
- restoring [SQL Server]
- NORECOVERY option
- online restores [SQL Server], RESTORE statement
- moving databases
- RESTORE statement, syntax
- cross-platform restores
- restoring databases [SQL Server], options
- RESTORE statement
- snapshots [SQL Server database snapshots], reverting to
- reverting database snapshots
- transaction log backups [SQL Server], RESTORE statement
- RESTORE LOG, see RESTORE statement
ms.assetid: 877ecd57-3f2e-4237-890a-08f16e944ef1
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||>=aps-pdw-2016'
ms.openlocfilehash: fe799de197de0fc18bb8750b4a6f8db2e288be32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017504"
---
# <a name="restore-statements-transact-sql"></a>Instrucciones RESTORE (Transact-SQL)

Restaura copias de seguridad de bases de datos de SQL realizadas con el comando BACKUP.

Haga clic en una de las pestañas siguientes para obtener la sintaxis, los argumentos, los comentarios, los permisos y los ejemplos para una versión de SQL concreta con la que está trabajando.

Para obtener más información sobre las convenciones de sintaxis, vea [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Este comando le permite realizar los siguientes escenarios de restauración:

- Restaurar una base de datos completa a partir de una copia de seguridad completa de la base de datos (restauración completa).
- Restaurar parte de una base de datos (restauración parcial).
- Restaurar archivos o grupos de archivos en una base de datos (restauración de archivos).
- Restaurar páginas específicas en una base de datos (restauración de páginas).
- Restaurar un registro de transacciones en una base de datos (restauración del registro de transacciones).
- Revertir una base de datos al punto temporal capturado por una instantánea de base de datos.

Para más información sobre los escenarios de restauración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Información general sobre restauración y recuperación](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md). Para obtener las descripciones de los argumentos, vea [RESTORE Arguments](../../t-sql/statements/restore-statements-arguments-transact-sql.md). (Argumentos de RESTORE). Cuando restaure una base de datos desde otra instancia, considere la información de [Administrar los metadatos cuando una base de datos pasa a estar disponible en otra instancia de servidor (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).

> [!NOTE]
> Para más información sobre cómo restaurar desde el servicio Windows Azure Blob Storage, vea [Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

## <a name="syntax"></a>Sintaxis

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
    [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
   | ,  <general_WITH_options> [ ,...n ]
   | , <replication_WITH_option>
   | , <change_data_capture_WITH_option>
   | , <FILESTREAM_WITH_option>
   | , <service_broker_WITH options>
   | , \<point_in_time_WITH_options-RESTORE_DATABASE>
   } [ ,...n ]
 ]
[;]

--To perform the first step of the initial restore sequence of a piecemeal restore:
RESTORE DATABASE { database_name | @database_name_var }
   <files_or_filegroups> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
      PARTIAL, NORECOVERY
      [  , <general_WITH_options> [ ,...n ]
       | , \<point_in_time_WITH_options-RESTORE_DATABASE>
      ] [ ,...n ]
[;]  
  
--To Restore Specific Files or Filegroups:
RESTORE DATABASE { database_name | @database_name_var }
   <file_or_filegroup> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
   {
      [ RECOVERY | NORECOVERY ]
      [ , <general_WITH_options> [ ,...n ] ]
   } [ ,...n ]
[;]  
  
--To Restore Specific Pages:
RESTORE DATABASE { database_name | @database_name_var }
   PAGE = 'file:page [ ,...n ]'
 [ , <file_or_filegroups> ] [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
       NORECOVERY
      [ , <general_WITH_options> [ ,...n ] ]
[;]

--To Restore a Transaction Log:
RESTORE LOG { database_name | @database_name_var }
 [ <file_or_filegroup_or_pages> [ ,...n ] ]
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
     [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
    | , <general_WITH_options> [ ,...n ]
    | , <replication_WITH_option>
    | , \<point_in_time_WITH_options-RESTORE_LOG>
   } [ ,...n ]
 ]
[;]

--To Revert a Database to a Database Snapshot:
RESTORE DATABASE { database_name | @database_name_var }
FROM DATABASE_SNAPSHOT = database_snapshot_name

<backup_device>::=
{
   { logical_backup_device_name |
      @logical_backup_device_name_var }
 | { DISK
     | TAPE
     | URL
   } = { 'physical_backup_device_name' |
      @physical_backup_device_name_var }
}
Note: URL is the format used to specify the location and the file name for the Microsoft Azure Blob. Although Microsoft Azure storage is a service, the implementation is similar to disk and tape to allow for a consistent and seamless restore experience for all the three devices.
<files_or_filegroups>::=
{
   FILE = { logical_file_name_in_backup | @logical_file_name_in_backup_var }
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }
 | READ_WRITE_FILEGROUPS
}

<general_WITH_options> [ ,...n ]::=
--Restore Operation Options
   MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'
          [ ,...n ]
 | REPLACE
 | RESTART
 | RESTRICTED_USER | CREDENTIAL

--Backup Set Options
 | FILE = { backup_set_file_number | @backup_set_file_number }
 | PASSWORD = { password | @password_variable }

--Media Set Options
 | MEDIANAME = { media_name | @media_name_variable }
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
 | BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
 | { CHECKSUM | NO_CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Monitoring Options
 | STATS [ = percentage ]

--Tape Options.
 | { REWIND | NOREWIND }
 | { UNLOAD | NOUNLOAD }

<replication_WITH_option>::=
 | KEEP_REPLICATION

<change_data_capture_WITH_option>::=
 | KEEP_CDC

<FILESTREAM_WITH_option>::=
 | FILESTREAM ( DIRECTORY_NAME = directory_name )

<service_broker_WITH_options>::=
 | ENABLE_BROKER
 | ERROR_BROKER_CONVERSATIONS
 | NEW_BROKER

\<point_in_time_WITH_options-RESTORE_DATABASE>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
   }

\<point_in_time_WITH_options-RESTORE_LOG>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
   }
```

## <a name="arguments"></a>Argumentos

Para obtener las descripciones de los argumentos, vea [RESTORE Arguments](../../t-sql/statements/restore-statements-arguments-transact-sql.md) (Argumentos de RESTORE).

## <a name="about-restore-scenarios"></a>Escenarios de restauración

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite una serie de escenarios de restauración:

- restauración de la base de datos completa

  Restaura la base de datos completa, empezando por una copia de seguridad completa de la base de datos, que puede ir seguida de una restauración de una copia de seguridad diferencial de la base de datos (y copias de seguridad de registros). Para obtener más información, vea [Restauraciones de base de datos completas - modelo de recuperación simple](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md) o [Restauraciones de base de datos completas - modelo de recuperación completa](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).

- Restauración de archivos

  Restaura un archivo o un grupo de archivos en una base de datos de varios grupos de archivos. Tenga en cuenta que con el modelo de recuperación simple, el archivo debe pertenecer a un grupo de archivos de solo lectura. Después de una restauración de archivos completa, se puede restaurar una copia de seguridad de archivos diferencial. Para más información, vea [Restauraciones de archivos - modelo de recuperación completa](../../relational-databases/backup-restore/file-restores-full-recovery-model.md) y [Restauraciones de archivos - modelo de recuperación simple](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md).

- Restauración de página

  Restaura páginas individuales. La restauración de página está disponible solo con el modelo de recuperación completa y el modelo de recuperación optimizado para cargas masivas de registros. Para obtener más información, vea [Restaurar páginas - SQL Server](../../relational-databases/backup-restore/restore-pages-sql-server.md).

- Restauración por etapas

  Restaura la base de datos por etapas, empezando por el grupo de archivos principal y uno o más grupos de archivos secundarios. Una restauración por etapas empieza por RESTORE DATABASE mediante la opción PARTIAL y la especificación de uno o más grupos de archivos secundarios que se van a restaurar. Para obtener más información, vea [Restauraciones por etapas - SQL Server](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).

- Solo recuperación

  Recupera los datos coherentes con la base de datos y solo necesita estar disponible. Para obtener más información, vea [Recuperar una base de datos sin restaurar los datos](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).

- Restauración del registro de transacciones.

  Con el modelo de recuperación completa o el modelo de recuperación optimizado para cargas masivas de registros, es necesaria la restauración de copias de seguridad de registros para alcanzar el punto de recuperación deseado. Para más información sobre cómo restaurar copias de seguridad del registro, vea [Aplicar copias de seguridad de registros de transacción - SQL Server](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).

- Preparar una base de datos de disponibilidad para un grupo de disponibilidad AlwaysOn

  Para obtener más información, vea [Preparar manualmente una base de datos secundaria para un grupo de disponibilidad - SQL Server](../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).

- Preparar una base de datos reflejada para la creación de reflejo de la base de datos

  Para obtener más información, vea [Preparar una base de datos reflejada para la creación de reflejo - SQL Server](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).

- Restauración en línea

  > [!NOTE]
  > La restauración en línea solo es posible en la edición Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Cuando se admite la restauración en línea, si la base de datos está en línea, las restauraciones de archivos y de página se convierten automáticamente en restauraciones en línea y en restauraciones de grupos de archivos secundarios tras la fase inicial de una restauración por etapas.

  > [!NOTE]
  > Las restauraciones en línea pueden implicar [transacciones diferidas](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).

Para tener más información, consulte [Restauración en línea](../../relational-databases/backup-restore/online-restore-sql-server.md).

## <a name="additional-considerations-about-restore-options"></a>Consideraciones adicionales sobre las opciones de RESTORE

### <a name="discontinued-restore-keywords"></a>Palabras clave de RESTORE no incluidas

Las siguientes palabras clave no se incluyeron en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]:

|Palabra clave no incluida|Se reemplaza por…|Ejemplo de palabra clave de reemplazo|
|--------------------------|------------------|------------------------------------|
|LOAD|RESTORE|`RESTORE DATABASE`|
|TRANSACTION|REGISTRO|`RESTORE LOG`|
|DBO_ONLY|RESTRICTED_USER|`RESTORE DATABASE ... WITH RESTRICTED_USER`|

### <a name="restore-log"></a>RESTORE LOG

RESTORE LOG puede incluir una lista de archivos que permita la creación de archivos durante la puesta al día. Esta opción se utiliza cuando la copia de seguridad de registros contiene entradas de registro escritas al agregar un archivo a la base de datos.

> [!NOTE]
> En el caso de una base de datos que utilice el modelo de recuperación completa o el modelo de recuperación optimizado para cargas masivas de registros, en la mayoría de los casos se debe realizar una copia de seguridad del final del registro antes de restaurar la base de datos. Restaurar una base de datos sin hacer primero una copia del final del registro produce un error, a menos que la instrucción RESTORE DATABASE contenga una cláusula WITH REPLACE o WITH STOPAT, que deben especificar un tiempo o una transacción producidos después de finalizar la copia de seguridad de los datos. Para obtener más información sobre las copias del final del registro, vea [Copias del final del registro](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

### <a name="comparison-of-recovery-and-norecovery"></a>Comparación de RECOVERY y NORECOVERY
La reversión se controla con la instrucción RESTORE mediante las opciones [ RECOVERY | NORECOVERY ]:

- NORECOVERY especifica que la reversión no se produce. Esto permite la puesta al día para continuar con la siguiente instrucción de la secuencia.

  En este caso, la secuencia de restauración puede restaurar otras copias de seguridad y ponerlas al día.

- RECOVERY (predeterminado) indica que se debe realizar la reversión una vez completada la puesta al día para la copia de seguridad actual.

  La recuperación de la base de datos requiere a su vez que la restauración del conjunto de datos completo (*conjunto de puestas al día*) sea coherente con la base de datos. Si el conjunto de puestas al día no se ha puesto al día lo suficiente como para ser coherente con la base de datos y se especifica RECOVERY, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error. Para más información sobre el proceso de recuperación, vea [Información general sobre restauración y recuperación (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

## <a name="compatibility-support"></a>Soporte de compatibilidad
Las copias de seguridad de las bases de datos **maestra**, de **modelos** y **msdb** creadas usando una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se pueden restaurar con [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> No se puede restaurar una copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anterior a la versión en la que se haya creado la copia de seguridad.

Cada versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza una ruta de acceso predeterminada distinta de la de las versiones anteriores. Por tanto, para restaurar una base de datos creada en la ubicación predeterminada para las copias de seguridad de versiones anteriores, es preciso usar la opción MOVE. Para más información sobre la nueva ruta de acceso predeterminada, vea [Ubicaciones de archivos para las instancias predeterminadas y con nombre de SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md).

Después de restaurar una base de datos de una versión anterior en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], la base de datos se actualiza automáticamente. Normalmente, la base de datos está disponible inmediatamente. Pero si la base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] tiene índices de texto completo, el proceso de actualización los importa, los restablece o los vuelve a generar, en función del valor de la propiedad del servidor **upgrade_option**. Si la opción de actualización se establece en importar (**upgrade_option** = 2) o en volver a generar (**upgrade_option** = 0), los índices de texto completo no estarán disponibles durante la actualización. Dependiendo de la cantidad de datos que se indicen, la importación puede requerir varias horas y volver a generar puede requerir hasta diez veces más. Observe también que cuando la opción de actualización se establece en importar, se vuelven a generar los índices de texto completo asociados si no se dispone de un catálogo de texto completo. Para cambiar el valor de la propiedad de servidor **upgrade_option** , use [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).

La primera vez que se adjunta una base de datos o se restaura en una instancia nueva de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], aún no se ha almacenado una copia de la clave maestra de la base de datos (cifrada por la clave maestra de servicio) en el servidor. Debe usar la instrucción **OPEN MASTER KEY** para descifrar la clave maestra de la base de datos (DMK). Una vez que se ha descifrado la clave maestra de la base de datos, tiene la posibilidad de habilitar el descifrado automático en el futuro usando la instrucción **ALTER MASTER KEY REGENERATE** para proporcionar al servidor una copia de la clave maestra de la base de datos cifrada con la clave maestra de servicio (SMK). Cuando una base de datos se haya actualizado desde una versión anterior, se debe volver a generar la DMK para usar el algoritmo AES más reciente. Para obtener más información sobre cómo volver a generar la DMK, vea [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). El tiempo necesario para volver a generar la DMK con el fin de actualizarse a AES depende del número de objetos protegidos por la DMK. Solo es necesario volver a generar la DMK una vez y no tiene ningún efecto sobre las nuevas generaciones futuras como parte de una estrategia de rotación de claves.

## <a name="general-remarks"></a>Notas generales
Durante una restauración sin conexión, si la base de datos especificada se está usando, RESTORE obliga a los usuarios a desconectarse tras un breve retraso. En el caso de una restauración en línea de un grupo de archivos no principal, la base de datos puede seguir usándose excepto si el grupo de archivos que se está restaurando se ha puesto fuera de conexión. Los datos de la base de datos especificada se reemplazan por los datos restaurados.

Se pueden realizar operaciones de restauración entre plataformas, incluso entre diferentes tipos de procesador, siempre que el sistema operativo admita la intercalación de la base de datos.

RESTORE se puede reiniciar después de un error. Además, puede indicar a RESTORE que continúe a pesar de los errores para que restaure la mayor cantidad de datos posible (vea la opción `CONTINUE_AFTER_ERROR`).

RESTORE no se permite en una transacción explícita o implícita.

Para restaurar una base de datos **maestra** dañada se usa un procedimiento especial. Para obtener más información, vea [Realizar copias de seguridad y restaurar bases de datos del sistema](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).

Al restaurar una base de datos se borra la memoria caché del plan de la base de datos que se está restaurando. Al borrar la memoria caché de planes, se provoca una nueva compilación de todos los planes de ejecución posteriores y puede ocasionar una disminución repentina y temporal del rendimiento de las consultas. 

Para restaurar una base de datos de disponibilidad, restaure primero la base de datos a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, a continuación, agréguela al grupo de disponibilidad

## <a name="interoperability"></a>Interoperabilidad

### <a name="database-settings-and-restoring"></a>Configuración y restauración de bases de datos

Durante una restauración, la mayoría de las opciones de la base de datos que se pueden configurar con ALTER DATABASE se restablecen a los valores vigentes en el momento en que finaliza la copia de seguridad.

Sin embargo, la opción WITH RESTRICTED_USER invalida este comportamiento para configurar la opción de acceso del usuario. Esta configuración siempre se establece tras una instrucción RESTORE que contiene la opción WITH RESTRICTED_USER.

### <a name="restoring-an-encrypted-database"></a>Restaurar una base de datos cifrada

Para restaurar una base de datos cifrada, debe tener acceso al certificado o la clave asimétrica que se usó para cifrarla. La base de datos no se puede restaurar sin el certificado o la clave asimétrica. Como resultado, se debe conservar el certificado que se usa para cifrar la clave de cifrado de base de datos mientras se necesite la copia de seguridad. Para obtener más información, consulte [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

### <a name="restoring-a-database-enabled-for-vardecimal-storage"></a>Restaurar una base de datos habilitada para el formato de almacenamiento vardecimal

Las copias de seguridad y restauración se realizan correctamente con el formato de almacenamiento **vardecimal**. Para más información sobre el formato de almacenamiento **vardecimal**, vea [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).

### <a name="restore-full-text-data"></a>Restaurar datos de texto completo

Los datos de texto completo se restauran junto con otros datos de la base de datos durante una restauración completa. Con el uso de la sintaxis `RESTORE DATABASE database_name FROM backup_device` normal, los archivos de texto completo se restauran como parte de la restauración de archivos de la base de datos.

La instrucción RESTORE también se puede utilizar para realizar restauraciones en ubicaciones alternativas, restauraciones diferenciales, restauraciones de archivos y grupos de archivos, y restauraciones de archivos y grupos de archivos diferenciales de datos de texto completo. Además, la instrucción RESTORE puede restaurar solo los archivos de texto completo, al igual que con datos de la base de datos.

> [!NOTE]
> Los catálogos de texto completo importados de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] se tratan todavía como archivos de base de datos. Para estos, el procedimiento de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] para realizar la copia de seguridad de los catálogos de texto completo se sigue pudiendo aplicar, excepto en que ya no es necesario pausar y reanudar la operación de copia de seguridad. Para más información, vea [Realizar copias de seguridad de los catálogos de texto completo y restaurarlos](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md).

### [!INCLUDE [ssbigdataclusters-ss-nover](../../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE [big-data-clusters-master-instance-ha-endpoint-requirement](../../includes/big-data-clusters-master-instance-ha-endpoint-requirement.md)]

## <a name="metadata"></a>Metadatos

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye las tablas del historial de copias de seguridad y restauración que realizan el seguimiento de estas actividades para cada instancia del servidor. Cuando se realiza una restauración, se modifican también las tablas del historial de copias de seguridad. Para más información sobre estas tablas, vea [Historial de copias de seguridad e información de encabezados](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md).

## <a name="replace-option-impact"></a><a name="REPLACEoption"></a> Impacto de la opción REPLACE
REPLACE no debe usarse a menudo y solo después de haberlo pensado detenidamente. La opción Restore suele impedir que se sobrescriba accidentalmente una base de datos con otra base de datos. Si la base de datos especificada en una instrucción RESTORE ya existe en el servidor actual y el GUID de la familia de base de datos especificado difiere del GUID de la familia de base de datos registrado en el conjunto de copia de seguridad, no se restaura la base de datos. Ésta es una importante medida preventiva.

La opción REPLACE omite varias comprobaciones de seguridad importantes que suele realizar la opción Restore. Las comprobaciones que se omiten son:

- Restauración sobre una base de datos existente con una copia de seguridad tomada de otra base de datos.

  Con la opción REPLACE, la operación de restauración permite sobrescribir una base de datos existente con cualquier base de datos del conjunto de copia de seguridad, incluso si el nombre de base de datos especificado difiere del nombre de base de datos grabado en el conjunto de copia de seguridad. Esto puede dar lugar a que se sobrescriba accidentalmente una base de datos con una base de datos diferente.
  
- Restauración de una base de datos con el modelo de recuperación completa o con el modelo de recuperación optimizado para cargas masivas de registros donde no se ha realizado una copia del final del registro y no se utiliza la opción STOPAT.

  Con la opción REPLACE, puede perder trabajo comprometido, porque no se ha realizado una copia de seguridad del registro escrito más recientemente.

- Sobrescritura de archivos existentes.

  Por ejemplo, un error puede permitir la sobrescritura de archivos del tipo equivocado, como archivos .xls, o archivos que está utilizando otra base de datos que no está actualmente en línea. La pérdida arbitraria de datos es posible si se sobrescriben archivos existentes, aunque la base de datos restaurada esté completa.

## <a name="redoing-a-restore"></a>Rehacer una restauración
Aunque no es posible deshacer los efectos de una restauración, puede cancelar los efectos de la copia de datos y realizar una puesta al día si comienza de nuevo con los archivos de uno en uno. Para comenzar de nuevo, restaure el archivo que desee y vuelva a realizar la puesta al día. Por ejemplo, si ha restaurado demasiadas copias de seguridad de registros por error y ha superado el punto de detención deseado, debe reiniciar la secuencia.

Una secuencia de restauración se puede anular y reiniciar mediante la restauración de todo el contenido de los archivos afectados.

## <a name="reverting-a-database-to-a-database-snapshot"></a>Revertir una base de datos a una instantánea de base de datos
Una *operación de reversión de base de datos* (especificada con la opción DATABASE_SNAPSHOT) hace retroceder en el tiempo una base de datos de origen completa al revertirla a una instantánea de base de datos, es decir, al sobrescribir la base de datos de origen con datos del momento en que se creó la instantánea especificada. En un momento dado solo puede existir la instantánea a la que se va a revertir la base de datos. A continuación, la operación de reversión vuelve a generar el registro (por lo tanto, no puede poner al día una base de datos revertida en el punto del error del usuario).

La pérdida de datos se limita a las actualizaciones de la base de datos desde la creación de la instantánea. Los metadatos de una base de datos revertida son iguales a los metadatos en el momento de la creación de la instantánea. No obstante, si se revierte a una instantánea, se quitan todos los catálogos de texto completo.

La reversión a partir de una instantánea de base de datos no se utiliza para la recuperación de medios. A diferencia de un conjunto de copia de seguridad normal, la instantánea de base de datos es una copia incompleta de los archivos de la base de datos. Si la base de datos o la instantánea de base de datos están dañadas, es probable que no se pueda realizar la reversión a partir de una instantánea. Además, aunque sea posible, no es probable que la reversión corrija el problema si se produjeran daños.

### <a name="restrictions-on-reverting"></a>Restricciones de la reversión
La reversión no se admite en las siguientes condiciones:

- La base de datos de origen contiene todos los grupos de archivos de solo lectura o comprimidos.
- Algunos archivos sin conexión estaban en línea en el momento de crear la instantánea.
- Actualmente existen varias instantáneas de la base de datos.

Para más información, vea [Revertir una base de datos a una instantánea de base de datos](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

## <a name="security"></a>Seguridad
La operación de copia de seguridad puede especificar opcionalmente contraseñas para un conjunto de medios, para un conjunto de copia de seguridad o para ambos. Si se ha definido una contraseña en un conjunto de medios o un conjunto de copia de seguridad, debe especificar la contraseña o contraseñas correctas en la instrucción RESTORE. Estas contraseñas impiden operaciones de restauración y anexiones no autorizadas de los conjuntos de copia de seguridad en medios que utilizan herramientas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No obstante, los medios protegidos con contraseña se pueden sobrescribir mediante la opción FORMAT de la instrucción BACKUP.

> [!IMPORTANT]
> El nivel de protección que proporciona esta contraseña es bajo. El objetivo es impedir una restauración incorrecta con las herramientas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ya sea por parte de usuarios autorizados o no autorizados. No impide la lectura de los datos de las copias de seguridad por otros medios o el reemplazo de la contraseña. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]El procedimiento recomendado para proteger las copias de seguridad consiste en almacenar las cintas de copia de seguridad en una ubicación segura o en hacer una copia de seguridad en archivos de disco protegidos con las listas de control de acceso (ACL) adecuadas. Las ACL se deben establecer en el directorio raíz en el que se crean las copias de seguridad.

> [!NOTE]
> Para más información específica sobre las operaciones de copia de seguridad y restauración de SQL Server con Microsoft Azure Blob Storage, vea [Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

### <a name="permissions"></a>Permisos
Si la base de datos que se va a restaurar no existe, el usuario debe tener permisos `CREATE DATABASE` para poder ejecutar RESTORE. Si la base de datos existe, los permisos RESTORE tienen como valor predeterminado los miembros de los roles fijos de servidor `sysadmin` y `dbcreator`, y el propietario (`dbo`) de la base de datos (para la opción `FROM DATABASE_SNAPSHOT`, la base de datos siempre existe).

Los permisos RESTORE se conceden a los roles en los que la información acerca de la pertenencia está siempre disponible para el servidor. Debido a que la pertenencia a un rol fijo de base de datos solo se puede comprobar cuando la base de datos es accesible y no está dañada, lo que no siempre ocurre cuando se ejecuta RESTORE, los miembros del rol fijo de base de datos `db_owner` no tienen permisos RESTORE.

## <a name="examples"></a><a name="examples"></a> Ejemplos

En todos los ejemplos se supone que se ha realizado una copia de seguridad completa de la base de datos.

Entre los ejemplos de RESTORE se incluyen los siguientes:

- A. [Restaurar una base de datos completa](#restoring_full_db)
- B. [Restaurar copias de seguridad de bases de datos completas y diferenciales](#restoring_full_n_differential_db_backups)
- C. [Restaurar una base de datos con la sintaxis de RESTART](#restoring_db_using_RESTART)
- D. [Restaurar una base de datos y mover archivos](#restoring_db_n_move_files)
- E. [Copiar una base de datos con BACKUP y RESTORE](#copying_db_using_bnr)
- F. [Restaurar a un momento dado con STOPAT](#restoring_to_pit_using_STOPAT)
- G. [Restaurar el registro de transacciones hasta una marca](#restoring_transaction_log_to_mark)
- H. [Restaurar con la sintaxis de TAPE](#restoring_using_TAPE)
- I. [Restaurar con la sintaxis de FILE y FILEGROUP](#restoring_using_FILE_n_FG)
- J. [Revertir desde una instantánea de base de datos](#reverting_from_db_snapshot)
- K. [Restaurar desde el servicio Microsoft Azure Blob Storage](#Azure_Blob)

> [!NOTE]
> Para obtener más ejemplos, vea los temas sobre cómo restaurar que aparecen en [Información general sobre restauración y recuperación](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).

### <a name="a-restoring-a-full-database"></a><a name="restoring_full_db"></a> A. Restaurar una base de datos completa

En el siguiente ejemplo se restaura una copia de seguridad completa de la base de datos desde un dispositivo lógico de copia de seguridad de `AdventureWorksBackups`. Para obtener un ejemplo de creación de este dispositivo, vea [Dispositivos de copia de seguridad](../../relational-databases/backup-restore/backup-devices-sql-server.md).

```sql
RESTORE DATABASE AdventureWorks2012
  FROM AdventureWorks2012Backups;
```

> [!NOTE]
> En el caso de una base de datos que utilice el modelo de recuperación completa o el modelo de recuperación optimizado para cargas masivas de registros, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere en la mayoría de los casos que realice una copia de seguridad de registros después del error antes de restaurar la base de datos. Para obtener más información, vea [Copias del final del registro](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="b-restoring-full-and-differential-database-backups"></a><a name="restoring_full_n_differential_db_backups"></a> B. Restaurar copias de seguridad de bases de datos completas y diferenciales

En el siguiente ejemplo se restaura una copia de seguridad completa de la base de datos seguida de una copia de seguridad diferencial desde un dispositivo de copia de seguridad de `Z:\SQLServerBackups\AdventureWorks2012.bak`, que contiene las dos copias de seguridad. La copia de seguridad de base de datos completa que se va a restaurar es el sexto conjunto de copia de seguridad del dispositivo (`FILE = 6`), y la copia de seguridad de base de datos diferencial es el noveno conjunto de copia de seguridad en el dispositivo (`FILE = 9`). En cuanto se recupere la copia de seguridad diferencial, se recuperará la base de datos.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 6
      NORECOVERY;
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 9
      RECOVERY;
```

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="c-restoring-a-database-using-restart-syntax"></a><a name="restoring_db_using_RESTART"></a> C. Restaurar una base de datos con la sintaxis de RESTART

En el ejemplo siguiente se usa la opción `RESTART` para reiniciar una operación `RESTORE` interrumpida por un error de alimentación del servidor.

```sql
-- This database RESTORE halted prematurely due to power failure.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups;
-- Here is the RESTORE RESTART operation.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups WITH RESTART;
```

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="d-restoring-a-database-and-move-files"></a><a name="restoring_db_n_move_files"></a> D. Restaurar una base de datos y mover archivos

En el ejemplo siguiente se restaura una base de datos completa y el registro de transacciones, y se mueve la base de datos restaurada al directorio `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data`.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH NORECOVERY,
      MOVE 'AdventureWorks2012_Data' TO
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.mdf',
      MOVE 'AdventureWorks2012_Log'
TO 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.ldf';
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH RECOVERY;
```

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="e-copying-a-database-using-backup-and-restore"></a><a name="copying_db_using_bnr"></a> E. Copiar una base de datos con BACKUP y RESTORE

En el ejemplo siguiente se usan las instrucciones `BACKUP` y `RESTORE` para realizar una copia de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. La instrucción `MOVE` hace que se restauren los datos y el archivo de registro en las ubicaciones especificadas. La instrucción `RESTORE FILELISTONLY` se usa para determinar el número y los nombres de los archivos de la base de datos que se están restaurando. La nueva copia de la base de datos se denomina `TestDB`. Para obtener más información, vea [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).

```sql
BACKUP DATABASE AdventureWorks2012
    TO AdventureWorksBackups ;

RESTORE FILELISTONLY
    FROM AdventureWorksBackups ;

RESTORE DATABASE TestDB
    FROM AdventureWorksBackups
    WITH MOVE 'AdventureWorks2012_Data' TO 'C:\MySQLServer\testdb.mdf',
    MOVE 'AdventureWorks2012_Log' TO 'C:\MySQLServer\testdb.ldf';
GO
```

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="f-restoring-to-a-point-in-time-using-stopat"></a><a name="restoring_to_pit_using_STOPAT"></a> F. Restaurar a un momento dado con STOPAT

En el ejemplo siguiente se restaura una base de datos al estado en que se encontraba a las `12:00 AM` del `April 15, 2020` y se muestra una operación de restauración que implica varias copias de seguridad de registros. En el dispositivo de copia de seguridad, `AdventureWorksBackups`, la copia de seguridad de base de datos completa que se va a restaurar es el tercer conjunto de copia de seguridad en el dispositivo (`FILE = 3`), la primera copia de seguridad de registros es el cuarto conjunto de copia de seguridad (`FILE = 4`) y la segunda copia de seguridad de registros es el quinto conjunto de copia de seguridad (`FILE = 5`).

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=3, NORECOVERY;
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
RESTORE DATABASE AdventureWorks2012 WITH RECOVERY;

```

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="g-restoring-the-transaction-log-to-a-mark"></a><a name="restoring_transaction_log_to_mark"></a> G. Restaurar el registro de transacciones hasta una marca

En el ejemplo siguiente se restaura el registro de transacciones hasta la marca de la transacción marcada denominada `ListPriceUpdate`.

```sql
USE AdventureWorks2012
GO
BEGIN TRANSACTION ListPriceUpdate
    WITH MARK 'UPDATE Product list prices';
GO

UPDATE Production.Product
    SET ListPrice = ListPrice * 1.10
    WHERE ProductNumber LIKE 'BK-%';
GO

COMMIT TRANSACTION ListPriceUpdate;
GO

-- Time passes. Regular database
-- and log backups are taken.
-- An error occurs in the database.
USE master;
GO

RESTORE DATABASE AdventureWorks2012
FROM AdventureWorksBackups
WITH FILE = 3, NORECOVERY;
GO

RESTORE LOG AdventureWorks2012
  FROM AdventureWorksBackups
    WITH FILE = 4,
    RECOVERY,
    STOPATMARK = ListPriceUpdate;
```

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="h-restoring-using-tape-syntax"></a><a name="restoring_using_TAPE"></a> H. Restaurar con la sintaxis de TAPE

En el siguiente ejemplo se restaura una copia de seguridad completa de la base de datos desde un dispositivo de copia de seguridad `TAPE`.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM TAPE = '\\.\tape0';
```

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="i-restoring-using-file-and-filegroup-syntax"></a><a name="restoring_using_FILE_n_FG"></a> I. Restaurar con la sintaxis de FILE y FILEGROUP

En el siguiente ejemplo se restaura una base de datos denominada `MyDatabase` que tiene dos archivos, un grupo de archivos secundario y un registro de transacciones. La base de datos usa el modelo de recuperación completa.

La copia de seguridad de la base de datos es el noveno conjunto de copia de seguridad del conjunto de medios en un dispositivo lógico de copia de seguridad denominado `MyDatabaseBackups`. A continuación se restauran mediante `10` tres copias de seguridad de registros que están en los tres conjuntos de copia de seguridad siguientes (`11`, `12` y `MyDatabaseBackups`) en el dispositivo `WITH NORECOVERY`. Tras restaurar la última copia de seguridad de registros se restaura la base de datos.

> [!NOTE]
> La recuperación se realiza como un paso independiente para reducir las posibilidades de recuperar antes de que se hayan restaurado todas las copias de seguridad de registros. Para más información sobre el proceso de recuperación, vea [Información general sobre restauración y recuperación (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

Tenga en cuenta que en `RESTORE DATABASE` hay dos tipos de opciones `FILE`. Las opciones `FILE` que preceden al nombre del dispositivo de copia de seguridad especifican los nombres de archivos lógicos de los archivos de base de datos que se van a restaurar desde el conjunto de copia de seguridad; por ejemplo, `FILE = 'MyDatabase_data_1'`. Este conjunto de copia de seguridad no es la primera copia de seguridad de la base de datos en el conjunto de medios; por ello, su posición en el conjunto de medios se indica mediante la opción `FILE` de la cláusula `WITH`, `FILE=9`.

```sql
RESTORE DATABASE MyDatabase
    FILE = 'MyDatabase_data_1',
    FILE = 'MyDatabase_data_2',
    FILEGROUP = 'new_customers'
    FROM MyDatabaseBackups
    WITH
      FILE = 9,
      NORECOVERY;
GO  
-- Restore the log backups
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 10,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 11,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 12,
      NORECOVERY;
GO
--Recover the database
RESTORE DATABASE MyDatabase WITH RECOVERY;
GO
```

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="j-reverting-from-a-database-snapshot"></a><a name="reverting_from_db_snapshot"></a> J. Revertir desde una instantánea de base de datos

En este ejemplo se revierte una base de datos a una instantánea de base datos. En el ejemplo se supone que solo existe una instantánea en la base de datos. Para obtener un ejemplo de creación de esta instantánea de base de datos, vea [Crear una instantánea de base de datos](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md).

> [!NOTE]
> Si se revierte a una instantánea, se quitan todos los catálogos de texto completo.

```sql
USE master;
RESTORE DATABASE AdventureWorks2012 FROM DATABASE_SNAPSHOT = 'AdventureWorks_dbss1800';
GO
```

Para más información, vea [Revertir una base de datos a una instantánea de base de datos](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

[&#91;Inicio del ejemplo&#93;](#examples)

### <a name="k-restoring-from-the-microsoft-azure-blob-storage-service"></a><a name="Azure_Blob"></a> K. Restaurar desde el servicio Microsoft Azure Blob Storage

En los tres ejemplos siguientes se usa el servicio Microsoft Azure Blob Storage. El nombre de la cuenta de almacenamiento es `mystorageaccount`. El contenedor de los archivos de datos se denomina `myfirstcontainer`. El contenedor de los archivos de copia de seguridad se denomina `mysecondcontainer`. Se ha creado una directiva de acceso almacenada con derechos de lectura, escritura, eliminación y lista para cada contenedor. Se han creado credenciales de SQL Server con Firmas de acceso compartido asociadas a las directivas de acceso almacenadas. Para más información específica sobre las operaciones de copia de seguridad y restauración de SQL Server con Microsoft Azure Blob Storage, vea [Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

**K1. Restaurar una copia de seguridad de base de datos completa desde el servicio Microsoft Azure Storage**    
Una copia de seguridad de base de datos completa de `Sales`, situada en `mysecondcontainer`, se restaurará en `myfirstcontainer`. `Sales` no existe actualmente en el servidor.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

**K2. Restaurar una copia de seguridad de base de datos completa desde el servicio Microsoft Azure Storage en el almacenamiento local** Una copia de seguridad de base de datos completa, ubicada en `mysecondcontainer`, de `Sales`, se restaurará en el almacenamiento local. `Sales` no existe actualmente en el servidor.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'H:\DATA\Sales_Data1.mdf',
  MOVE 'Sales_log' to 'O:\LOG\Sales_log.ldf',
  STATS = 10;
```

**K3. Restaurar una copia de seguridad de base de datos completa desde el almacenamiento local al servicio Microsoft Azure Storage**

```sql
RESTORE DATABASE Sales
  FROM DISK = 'E:\BAK\Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

[&#91;Inicio del ejemplo&#93;](#examples)

## <a name="more-information"></a>Más información

[Información general sobre restauración y recuperación (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)     
[Realizar copias de seguridad y restaurar bases de datos de SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)    
[Realizar copias de seguridad y restaurar bases de datos del sistema (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)      
[Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)     
[Realizar copias de seguridad de los catálogos de texto completo y restaurarlos](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)      
[Hacer copias de seguridad y restaurar bases de datos replicadas](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)      
[BACKUP](../../t-sql/statements/restore-statements-transact-sql.md)      
[Conjuntos de medios, familias de medios y conjuntos de copias de seguridad](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)      
[RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)     
[RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)     
[RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)     
[RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)     
[Historial de copias de seguridad e información de encabezados](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)       

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-2017&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

Este comando permite restaurar una base de datos completa a partir de una copia de seguridad completa de la base de datos (restauración completa) desde una cuenta de Azure Blob Storage.

Para ver otros comandos RESTORE compatibles, vea:

- [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)
- [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)
- [RESTORE LABELONLY ONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)
- [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)

> [!IMPORTANT]
> Para restaurar desde copias de seguridad automáticas de Azure SQL Managed Instance, vea [Recuperación de una base de datos de Azure SQL Database o Instancia administrada de Azure SQL mediante copias de seguridad automatizadas](/azure/sql-database/sql-database-recovery-using-backups).

## <a name="syntax"></a>Sintaxis

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 FROM URL = { 'physical_device_name' | @physical_device_name_var } [ ,...n ]
[;]

```

## <a name="arguments"></a>Argumentos

DATABASE

Especifica la base de datos de destino.

FROM URL

Especifica uno o varios dispositivos de copia de seguridad colocados en las direcciones URL que se usarán para la operación de restauración. El formato de las direcciones URL solo se usa para restaurar copias de seguridad del servicio de almacenamiento Microsoft Azure.

> [!IMPORTANT]
> Para restaurar desde varios dispositivos cuando se restaure desde una dirección URL, debe usar tokens de Firma de acceso compartido (SAS). Para ver ejemplos sobre cómo crear una Firma de acceso compartido, vea [Copia de seguridad en URL de SQL Server](../../relational-databases/backup-restore/sql-server-backup-to-url.md) y [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell) (Simplificación de la creación de credenciales de SQL con tokens de firmas de acceso compartido [SAS] en Almacenamiento de Azure con PowerShell).

*n* Es un marcador de posición que indica que se pueden especificar hasta 64 dispositivos de copia de seguridad en una lista separada por comas.

## <a name="general-remarks"></a>Notas generales

Como requisito previo, debe crear una credencial con un nombre que coincida con la dirección URL de la cuenta de almacenamiento de blobs y la Firma de acceso compartido como secreto. El comando RESTORE buscará las credenciales mediante la dirección URL del almacenamiento de blobs para encontrar la información necesaria para leer el dispositivo de copia de seguridad.

La operación RESTORE es asincrónica; la restauración continúa incluso si se interrumpe la conexión del cliente. Si la conexión se interrumpe, puede comprobar la vista [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) del estado de una operación de restauración (así como para las bases de datos CREATE y DROP).

Las siguientes opciones de base de datos se establecen/invalidan y no se pueden cambiar más adelante:

- NEW_BROKER (si el agente no está habilitado en el archivo .bak)
- ENABLE_BROKER (si el agente no está habilitado en el archivo .bak)
- AUTO_CLOSE=OFF (si una base de datos del archivo .bak tiene AUTO_CLOSE=ON)
- RECOVERY FULL (si una base de datos del archivo .bak tiene el modo de recuperación SIMPLE o BULK_LOGGED)
- Se agrega un grupo de archivos optimizado para memoria, al que se asigna el nombre XTP si no estaba en el archivo .bak de origen. Se cambia el nombre de todos los grupos de archivos optimizados para memoria existentes a XTP
- Las opciones SINGLE_USER y RESTRICTED_USER se convierten en MULTI_USER

## <a name="limitations---sql-managed-instance"></a>Limitaciones de SQL Managed Instance

Se aplican las siguientes limitaciones:

- Los archivos .BAK que contienen varios conjuntos de copia de seguridad no se pueden restaurar.
- Los archivos .BAK que contienen varios archivos de registro no se pueden restaurar.
- Se producirá un error en la restauración si el archivo .bak contiene datos FILESTREAM.
- Las copias de seguridad que contienen bases de datos que tienen objetos en memoria activos no se pueden restaurar en un nivel de rendimiento de uso general.
- Las copias de seguridad que contienen bases de datos en modo de solo lectura no se pueden restaurar en estos momentos. Esta limitación se quitará próximamente.

Para más información, consulte [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance).

## <a name="restoring-an-encrypted-database"></a>Restaurar una base de datos cifrada
Para restaurar una base de datos cifrada, debe tener acceso al certificado o la clave asimétrica que se usó para cifrarla. La base de datos no se puede restaurar sin el certificado o la clave asimétrica. Como resultado, se debe conservar el certificado que se usa para cifrar la clave de cifrado de base de datos mientras se necesite la copia de seguridad. Para obtener más información, consulte [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

## <a name="permissions"></a>Permisos
El usuario debe tener permisos `CREATE DATABASE` para poder ejecutar RESTORE.

```sql
CREATE LOGIN mylogin WITH PASSWORD = 'Very Strong Pwd123!';
GRANT CREATE ANY DATABASE TO [mylogin];
```

Los permisos RESTORE se conceden a los roles en los que la información acerca de la pertenencia está siempre disponible para el servidor. Debido a que la pertenencia a un rol fijo de base de datos solo se puede comprobar cuando la base de datos es accesible y no está dañada, lo que no siempre ocurre cuando se ejecuta RESTORE, los miembros del rol fijo de base de datos `db_owner` no tienen permisos RESTORE.

## <a name="examples"></a><a name="examples"></a> Ejemplos

En los ejemplos siguientes se restaura una copia de seguridad de base de datos de solo copia desde una dirección URL, incluida la creación de una credencial.

### <a name="a-restore-database-from-four-backup-devices"></a><a name="restore-mi-database"></a> A. Restauración de una base de datos a partir de cuatro dispositivos de copia de seguridad

```sql

-- Create credential
CREATE CREDENTIAL [https://mybackups.blob.core.windows.net/wide-world-importers]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
      SECRET = 'sv=2017-11-09&ss=bq&srt=sco&sp=rl&se=2022-06-19T22:41:07Z&st=2018-06-01T14:41:07Z&spr=https&sig=s7wddcf0w%3D';
GO
-- Restore database
RESTORE DATABASE WideWorldImportersStandard
FROM URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/00-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/01-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/02-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/03-WideWorldImporters-Standard.bak'
```

Si la base de datos ya existe, se muestra el error siguiente:

```
Msg 1801, Level 16, State 1, Line 9
Database 'WideWorldImportersStandard' already exists. Choose a different database name.
```

### <a name="b-restore-database-specified-via-variable"></a><a name="restore-mi-database-variables"></a> B. Restauración de la base de datos especificada mediante una variable

```sql
DECLARE @db_name sysname = 'WideWorldImportersStandard';
DECLARE @url nvarchar(400) = N'https://mybackups.blob.core.windows.net/wide-world-importers/WideWorldImporters-Standard.bak';

RESTORE DATABASE @db_name
FROM URL = @url
```

### <a name="c-track-progress-of-restore-statement"></a><a name="restore-mi-database-progress"></a> C. Seguimiento del progreso de la instrucción de restauración

```sql
SELECT query = a.text, start_time, percent_complete,
    eta = dateadd(second,estimated_completion_time/1000, getdate())
FROM sys.dm_exec_requests r
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
WHERE r.command = 'RESTORE DATABASE'
```

> [!Note]
> Esta vista probablemente mostrará dos solicitudes de restauración. Una es la instrucción RESTORE original enviada por el cliente y la otra es la instrucción RESTORE en segundo plano que se ejecuta incluso si se produce un error en la conexión de cliente.

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Sistema de la plataforma de análisis

Restaura una base de datos de usuario de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] desde una copia de seguridad de base de datos a un dispositivo de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. La base de datos se restaura desde una copia de seguridad que se creó anteriormente con el comando [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] [BACKUP DATABASE - Analytics Platform System](../../t-sql/statements/backup-transact-sql.md). Use las operaciones de copia de seguridad y de restauración para crear un plan de recuperación ante desastres o para mover bases de datos de un dispositivo a otro.

> [!NOTE]
> Restaurar la base de datos maestra conlleva restaurar la información de inicio de sesión del dispositivo. Para restaurar la base de datos maestra, use la página [Restaurar la base de datos maestra](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md) de la herramienta **Configuration Manager**. Un administrador con acceso al nodo de control puede realizar esta operación. Para más información sobre las copias de seguridad de bases de datos de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], vea "Copia de seguridad y restauración" en la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

## <a name="syntax"></a>Sintaxis

```syntaxsql

-- Restore the master database
-- Use the Configuration Manager tool.

Restore a full user database backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\full_backup_directory'
[;]

--Restore a full user database backup and then a differential backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\differential_backup_directory'
    WITH [ ( ] BASE = '\\UNC_path\full_backup_directory' [ ) ]
[;]

--Restore header information for a full or differential user database backup.
RESTORE HEADERONLY
    FROM DISK = '\\UNC_path\backup_directory'
[;]
```

## <a name="arguments"></a>Argumentos

RESTORE DATABASE *database_name* Especifica que una base de datos de usuario se debe restaurar a una base de datos denominada *database_name*. La base de datos restaurada puede tener un nombre diferente de la base de datos de origen de la que se ha hecho la copia de seguridad. *database_name* no puede existir como una base de datos en el dispositivo de destino. Para más información sobre los nombres de base de datos permitidos, vea "Normas de nomenclatura de objetos" en la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Cuando se restaura una base de datos de usuario, se restaura una copia de seguridad completa de la base de datos y, después, se restaura (opcionalmente) una copia de seguridad diferencial en el dispositivo. La restauración de una base de datos de usuario conlleva restaurar los usuarios de base de datos y los roles de base de datos.

FROM DISK = '\\\\*UNC_path*\\*backup_directory*' Ruta de acceso de red y directorio desde los que [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] restaurará los archivos de copia de seguridad. Por ejemplo, FROM DISK = '\\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup'.

*backup_directory* Especifica el nombre de un directorio que contiene la copia de seguridad completa o diferencial. Por ejemplo, puede realizar una operación RESTORE HEADERONLY en una copia de seguridad completa o diferencial.

*full_backup_directory* Especifica el nombre de un directorio que contiene la copia de seguridad completa.

*differential_backup_directory* Especifica el nombre de un directorio que contiene la copia de seguridad diferencial.

- La ruta de acceso y el directorio de copia de seguridad ya deben existir y se debe especificar como una ruta de acceso de convención de nomenclatura universal (UNC) completa.
- La ruta de acceso al directorio de copia de seguridad no puede ser una ruta de acceso local y no puede ser una ubicación en ninguno de los nodos del dispositivo de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
- La longitud máxima de la ruta de acceso UNC y el nombre del directorio de copia de seguridad es de 200 caracteres.
- El servidor o host se debe especificar como una dirección IP.

RESTORE HEADERONLY Especifica que se devuelve únicamente la información de encabezado de una copia de seguridad de base de datos de usuario. Entre otros campos, el encabezado incluye el nombre y la descripción de texto de la copia de seguridad. El nombre de copia de seguridad no tiene que ser el mismo que el nombre del directorio donde se almacenan los archivos de copia de seguridad.

El formato de los resultados de RESTORE HEADERONLY se toma de los resultados de RESTORE HEADERONLY de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El resultado tiene más de 50 columnas, que [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] no usa en su totalidad. Para obtener una descripción de las columnas de los resultados de RESTORE HEADERONLY de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md).

## <a name="permissions"></a>Permisos
Requiere el permiso `CREATE ANY DATABASE`.

Requiere una cuenta de Windows que tenga permiso de acceso y lectura en el directorio de copia de seguridad. También se debe almacenar el nombre de la cuenta de Windows y la contraseña en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

- Para confirmar que las credenciales ya están ahí, use [sys.dm_pdw_network_credentials](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).
- Para agregar o actualizar las credenciales, use [sp_pdw_add_network_credentials: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).
- Para eliminar las credenciales de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], use [sp_pdw_remove_network_credentials: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md).

## <a name="error-handling"></a>Tratamiento de errores

El comando RESTORE DATABASE genera errores en las siguientes circunstancias:

- El nombre de la base de datos que se va a restaurar ya existe en el dispositivo de destino. Para evitarlo, elija un nombre de base de datos único o quite la base de datos existente antes de proceder a realizar la restauración.
- Hay un conjunto de archivos de copia de seguridad no válido en el directorio de copia de seguridad.
- Los permisos de inicio de sesión no son suficientes para restaurar la base de datos.
- [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] no tiene los permisos correctos en la ubicación de red donde van a estar los archivos de copia de seguridad.
- La ubicación de red del directorio de copia de seguridad no existe o no está disponible.
- No hay suficiente espacio en disco en los nodos de ejecución o de control. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] no confirma que hay suficiente espacio en disco en el dispositivo antes de iniciar la restauración. Por lo tanto, es posible que surja un error de espacio en disco agotado mientras se ejecuta la instrucción RESTORE DATABASE. Cuando se produce un error de espacio en disco insuficiente, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] revierte la restauración.
- El dispositivo de destino en el que se está restaurando la base de datos tiene menos nodos de ejecución que el dispositivo de origen desde el que se hizo la copia de seguridad de la base de datos.
- La restauración de base de datos se intenta realizar desde dentro de una transacción.

## <a name="general-remarks"></a>Notas generales

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] realiza un seguimiento para saber qué restauraciones de base de datos se realizan correctamente. Antes de restaurar una copia de seguridad diferencial de la base de datos, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] comprueba si la restauración de base de datos completa finalizó correctamente.

Después de una restauración, la base de datos de usuario tendrá nivel 120 de compatibilidad de base de datos. Esto sucede con todas las bases de datos, independientemente de su nivel de compatibilidad original.

## <a name="restoring-to-an-appliance-with-a-larger-number-of-compute-nodes"></a>Restauración a un dispositivo con más nodos de ejecución

Ejecute [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) después de restaurar una base de datos desde un dispositivo más pequeño a uno más grande, ya que la redistribución aumentará el registro de transacciones.

Restaurar una copia de seguridad en un dispositivo con un gran número de nodos de ejecución aumenta el tamaño de base de datos asignado en proporción al número de nodos de ejecución.

Por ejemplo, al restaurar una base de datos de 60 GB desde un dispositivo con 2 nodos (30 GB por nodo) a un equipo con 6 nodos, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crea una base de datos de 180 GB (6 nodos con 30 GB cada uno) en el dispositivo de 6 nodos. En principio, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] restaura la base de datos a 2 nodos para que coincida con la configuración de origen y, seguidamente, redistribuye los datos a los 6 nodos.

Después de la redistribución, cada nodo de ejecución contendrá menos datos reales y más espacio libre que cada nodo de ejecución en el dispositivo de origen (más pequeño). Use el espacio extra para agregar más datos a la base de datos. Si el tamaño de la base de datos restaurada es superior al necesario, puede usar [ALTER DATABASE - PDW](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true) para reducir el tamaño de los archivos de la base de datos.

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

En estas limitaciones y restricciones, el dispositivo de origen es el dispositivo desde el que se creó la copia de seguridad de la base de datos y el dispositivo de destino, el dispositivo en el que se va a restaurar esa base de datos.

- Restaurar una base de datos no vuelve a generar automáticamente las estadísticas.
- En el dispositivo solo se puede ejecutar una instrucción RESTORE DATABASE o BACKUP DATABASE en un momento dado. Si se envían varias instrucciones BACKUP y RESTORE al mismo tiempo, el dispositivo las pone en cola y las procesa una a una.
- Solo se puede restaurar una copia de seguridad de base de datos en un dispositivo de destino de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] que tenga el mismo número o más nodos de ejecución que el dispositivo de origen. El dispositivo de destino no puede tener menos nodos de ejecución que el de origen.
- Una copia de seguridad creada en un dispositivo con hardware de PDW de SQL Server 2012 no se puede restaurar en un dispositivo que tenga hardware de SQL Server 2008 R2. Esto es así incluso si el dispositivo se adquirió originalmente con el hardware de PDW de SQL Server 2008 R2 y ahora ejecuta el hardware de PDW de SQL Server 2012.

## <a name="locking"></a>Bloqueo
Toma un bloqueo exclusivo en el objeto DATABASE.

## <a name="examples"></a>Ejemplos

### <a name="a-simple-restore-examples"></a>A. Ejemplos sencillos de RESTORE

En el siguiente ejemplo se restaura una copia de seguridad completa en la base de datos `SalesInvoices2013`. Los archivos de copia de seguridad se almacenan en el directorio `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. La base de datos SalesInvoices2013 no debe existir ya en el dispositivo de destino o, de lo contrario, este comando producirá un error.

```sql
RESTORE DATABASE SalesInvoices2013
FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';
```

### <a name="b-restore-a-full-and-differential-backup"></a>B. Restaurar una copia de seguridad completa y diferencial

En el siguiente ejemplo se restaura una copia de seguridad completa y, luego, una diferencial en la base de datos SalesInvoices2013.

La copia de seguridad completa de la base de datos se restaura a partir de la copia de seguridad que se almacena en el directorio `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. Si la restauración se completa correctamente, la copia de seguridad diferencial se restaura en la base de datos SalesInvoices2013. La copia de seguridad diferencial se almacena en el directorio `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff`.

```syntaxsql
RESTORE DATABASE SalesInvoices2013
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'
    WITH BASE = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]

```

### <a name="c-restoring-the-backup-header"></a>C. Restaurar el encabezado de copia de seguridad

En este ejemplo se restaura la información de encabezado para la copia de seguridad de base de datos `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. El comando da como resultado una fila de información relativa a la copia de seguridad de Invoices2013Full.

```syntaxsql
RESTORE HEADERONLY
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]
```

Esta información de encabezado puede servir para comprobar el contenido de una copia de seguridad o para asegurarse de que el dispositivo de restauración de destino es compatible con el dispositivo de copia de seguridad de origen antes de intentar restaurar la copia de seguridad.

## <a name="see-also"></a>Consulte también
[BACKUP DATABASE - Analytics Platform System](../../t-sql/statements/backup-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)     

::: moniker-end