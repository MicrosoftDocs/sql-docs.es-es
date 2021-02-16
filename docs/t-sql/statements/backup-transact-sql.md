---
description: BACKUP (Transact-SQL)
title: BACKUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BACKUP_TSQL
- BACKUP
- BACKUP_DATABASE_TSQL
- BACKUP_LOG_TSQL
- BACKUP LOG
- BACKUP DATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], BACKUP statement
- backing up filegroups [SQL Server]
- backup file formats [SQL Server]
- backups [SQL Server]
- database backups [SQL Server], BACKUP statement
- multifamily media sets [SQL Server]
- mirrored media sets [SQL Server]
- backing up files [SQL Server]
- backup sets [SQL Server], BACKUP statement
- backup compression [SQL Server], BACKUP statement
- BACKUP statement
- backups [SQL Server], BACKUP statement
- backup history tables [SQL Server]
- transaction log backups [SQL Server], BACKUP LOG statement
- READ_WRITE_FILEGROUPS option
- BACKUP DATABASE statement
- concurrency [SQL Server], backups
- backing up databases [SQL Server]
- passwords [SQL Server], backups
- security [SQL Server], backups
- media families [SQL Server]
- BACKUP LOG statement
- backing up transaction logs [SQL Server]
- stripe sets [SQL Server]
- cross-platform backups
ms.assetid: 89a4658a-62f1-4289-8982-f072229720a1
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||>=aps-pdw-2016'
ms.openlocfilehash: 6025ca2bec08327f668a437b5428519cf85790dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075796"
---
# <a name="backup-transact-sql"></a>BACKUP (Transact-SQL)

Hace una copia de seguridad de una base de datos SQL.

Haga clic en una de las pestañas siguientes para obtener la sintaxis, los argumentos, los comentarios, los permisos y los ejemplos para una versión de SQL concreta con la que está trabajando.

Para obtener más información sobre las convenciones de sintaxis, vea [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](backup-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Hace copia de seguridad de una base de datos completa de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para crear una copia de seguridad de la base de datos, o uno o más archivos o grupos de archivos de la base de datos para crear una copia de seguridad de archivo (BACKUP DATABASE). Además, con el modelo de recuperación completa o con el modelo de recuperación optimizado para cargas masivas de registros, realiza la copia de seguridad del registro de transacciones de la base de datos para crear una copia de seguridad de registros (BACKUP LOG).

## <a name="syntax"></a>Sintaxis

```syntaxsql
--Backing Up a Whole Database
BACKUP DATABASE { database_name | @database_name_var }
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { DIFFERENTIAL
           | <general_WITH_options> [ ,...n ] } ]
[;]

--Backing Up Specific Files or Filegroups
BACKUP DATABASE { database_name | @database_name_var }
 <file_or_filegroup> [ ,...n ]
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]
[;]

--Creating a Partial Backup
BACKUP DATABASE { database_name | @database_name_var }
 READ_WRITE_FILEGROUPS [ , <read_only_filegroup> [ ,...n ] ]
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]
[;]

--Backing Up the Transaction Log (full and bulk-logged recovery models)
BACKUP LOG
  { database_name | @database_name_var }
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { <general_WITH_options> | \<log-specific_optionspec> } [ ,...n ] ]
[;]
  
<backup_device>::=
 {
  { logical_device_name | @logical_device_name_var }
 | {   DISK
     | TAPE
     | URL } =
     { 'physical_device_name' | @physical_device_name_var | 'NUL' }
 }

<MIRROR TO clause>::=
 MIRROR TO <backup_device> [ ,...n ]

<file_or_filegroup>::=
 {
   FILE = { logical_file_name | @logical_file_name_var }
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }
 }

<read_only_filegroup>::=
FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }

<general_WITH_options> [ ,...n ]::=
--Backup Set Options
   COPY_ONLY
 | { COMPRESSION | NO_COMPRESSION }
 | DESCRIPTION = { 'text' | @text_variable }
 | NAME = { backup_set_name | @backup_set_name_var }
 | CREDENTIAL
 | ENCRYPTION
 | FILE_SNAPSHOT
 | { EXPIREDATE = { 'date' | @date_var }
        | RETAINDAYS = { days | @days_var } }

--Media Set Options
   { NOINIT | INIT }
 | { NOSKIP | SKIP }
 | { NOFORMAT | FORMAT }
 | MEDIADESCRIPTION = { 'text' | @text_variable }
 | MEDIANAME = { media_name | @media_name_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
   BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
   { NO_CHECKSUM | CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Compatibility Options
   RESTART

--Monitoring Options
   STATS [ = percentage ]

--Tape Options
   { REWIND | NOREWIND }
 | { UNLOAD | NOUNLOAD }

--Log-specific Options
   { NORECOVERY | STANDBY = undo_file_name }
 | NO_TRUNCATE

--Encryption Options
 ENCRYPTION (ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY } , encryptor_options ) <encryptor_options> ::=
   `SERVER CERTIFICATE` = Encryptor_Name | SERVER ASYMMETRIC KEY = Encryptor_Name
```

## <a name="arguments"></a>Argumentos

DATABASE Especifica una copia de seguridad completa de la base de datos. Si se especifica una lista de archivos y grupos de archivos, solo se realiza la copia de seguridad de esos archivos o grupos de archivos. Durante una copia de seguridad completa o diferencial de una base de datos, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza la copia de seguridad de una parte suficiente del registro de transacciones para producir una base de datos coherente cuando se restaure la base de datos.

Al restaurar una copia de seguridad creada por BACKUP DATABASE (una *copia de seguridad de datos*), se restaura la copia de seguridad completa. Solo una copia de seguridad del registro se puede restaurar hasta un momento o transacción concretos dentro de la copia de seguridad.

> [!NOTE]
> Solo se puede realizar una copia de seguridad completa de la base de datos **maestra**.

REGISTRO

Especifica que solo se realizará la copia de seguridad del registro de transacciones. Se realiza la copia de seguridad del registro desde la última copia de seguridad del registro ejecutada correctamente hasta el final actual del registro. Para poder crear la primera copia de seguridad de registros, debe crear una copia de seguridad completa.

Puede restaurar una copia de seguridad del registro hasta un momento o transacción concretos dentro de la copia de seguridad especificando `WITH STOPAT`, `STOPATMARK` o `STOPBEFOREMARK` en la instrucción [RESTORE LOG](../../t-sql/statements/restore-statements-transact-sql.md).

> [!NOTE]
> Después de una copia de seguridad del registro típica, algunas entradas del registro de transacciones se quedan inactivas, a menos que se especifique `WITH NO_TRUNCATE` o `COPY_ONLY`. El registro se trunca después de que todos los registros de uno o varios archivos del registro virtual se queden inactivos. Si el registro no se trunca después de las copias de seguridad del registro rutinarias, algo podría estar retrasando el truncamiento de los registros. Para más información, vea [Factores que pueden ralentizar el truncamiento del registro](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation).

{ _database\_name_ |  **@** _database\_name\_var_ } Es la base de datos para la que se realiza la copia de seguridad del registro de transacciones, de una parte de la base de datos o de la base de datos completa. Si se proporciona como una variable ( **@** _database\_name\_var_), este nombre se puede especificar como una constante de cadena ( **@** _database\_name\_var_ **=** _database name_) o como una variable de tipo de datos de cadena de caracteres, excepto los tipos de datos **ntext** o **text**.

> [!NOTE]
> No se puede hacer una copia de seguridad de la base de datos reflejada en una asociación de creación de reflejo de la base de datos.

\<file_or_filegroup> [ **,** ...*n* ] Se utiliza solo con BACKUP DATABASE, especifica un grupo de archivos o un archivo de copia de seguridad que se va a incluir en una copia de seguridad de archivos, o especifica un grupo de archivos o un archivo de solo lectura que se va a incluir en una copia de seguridad parcial.

FILE **=** { *logical_file_name* | **@** _logical\_file\_name\_var_ } Es el nombre lógico de un archivo o una variable cuyo valor equivale al nombre lógico de un archivo que se va a incluir en la copia de seguridad.

FILEGROUP **=** { _logical\_filegroup\_name_ |  **@** _logical\_filegroup\_name\_var_ } Es el nombre lógico de un grupo de archivos o una variable cuyo valor equivale al nombre lógico de un grupo de archivos que se va a incluir en la copia de seguridad. En el modelo de recuperación simple, se permite la copia de seguridad de un grupo de archivos solo si se trata de un grupo de archivos de solo lectura.

> [!NOTE]
> Considere la posibilidad de utilizar copias de seguridad de archivos cuando el tamaño y los requisitos de rendimiento de la base de datos no permitan realizar una copia de seguridad completa de la base de datos. El dispositivo NUL se puede usar para probar el rendimiento de las copias de seguridad, pero no debe usarse en entornos de producción.

*n* Es un marcador de posición que indica que se pueden especificar varios archivos y grupos de archivos en una lista separada por comas. El número es ilimitado.

Para más información, vea [Copias de seguridad de archivos completas](../../relational-databases/backup-restore/full-file-backups-sql-server.md) y [Realizar copias de seguridad de archivos y grupos de archivos](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md).

READ_WRITE_FILEGROUPS [ **,** FILEGROUP = { _logical\_filegroup\_name_ |  **@** _logical\_filegroup\_name\_var_ } [ **,** ..._n_ ] ] Especifica una copia de seguridad parcial. Una copia de seguridad parcial incluye todos los archivos de lectura/escritura en una base de datos: el grupo de archivos principal y los grupos de archivos secundarios de lectura/escritura, así como los grupos de archivos o archivos de solo lectura especificados.

READ_WRITE_FILEGROUPS Especifica que en la copia de seguridad parcial se copiarán todos los grupos de archivos de lectura/escritura. Si la base de datos es de solo lectura, READ_WRITE_FILEGROUPS incluye tan solo el grupo de archivos principal.

> [!IMPORTANT]
> Si se enumeran de forma explícita los grupos de archivos de lectura/escritura con FILEGROUP en vez de READ_WRITE_FILEGROUPS, se crea una copia de seguridad de archivos.

FILEGROUP = { *logical_filegroup_name* |  **@** _logical\_filegroup\_name\_var_ } Es el nombre lógico de un grupo de archivos de solo lectura o una variable cuyo valor equivale al nombre lógico de un grupo de archivos de solo lectura que se va a incluir en la copia de seguridad parcial. Para obtener más información, consulte "\<file_or_filegroup>", que encontrará previamente en este tema.

*n* Es un marcador de posición que indica que se pueden especificar varios grupos de archivos de solo lectura en una lista separada por comas.

Para más información sobre las copias de seguridad parciales, vea [Copias de seguridad parciales](../../relational-databases/backup-restore/partial-backups-sql-server.md).

TO \<backup_device> [ **,** ...*n* ] Indica que el conjunto de [dispositivos de copia de seguridad](../../relational-databases/backup-restore/backup-devices-sql-server.md) correspondiente es un conjunto de medios no reflejado o el primero de los reflejos de un conjunto de medios reflejado (para los que se declaran una o más cláusulas MIRROR TO).

\<backup_device>

Especifica el dispositivo de copia de seguridad físico o lógico que se va a utilizar para la operación de copia de seguridad.

{ *logical_device_name* \| **@** _logical\_device\_name\_var_ } **Se aplica a:** SQL Server Es el nombre lógico del dispositivo de copia de seguridad en que se hace la copia de seguridad de la base de datos. El nombre lógico debe seguir las reglas definidas para los identificadores. Si se proporciona como una variable (@*logical_device_name_var*), el nombre del dispositivo de copia de seguridad se puede especificar como una constante de cadena (@_logical\_device\_name\_var_ **=** logical backup device name) o como una variable de cualquier tipo de datos de cadena de caracteres, excepto los tipos de datos **ntext** o **text**.

{ DISK \| TAPE \| URL} **=** { **"** _physical\_device\_name_ **"** \| **@** _physical\_device\_name\_var_ \| 'NUL' } **Se aplica a:** DISK, TAPE y URL se aplican a SQL Server.
Especifica un archivo de disco o un dispositivo de cinta, o bien un servicio Microsoft Azure Blob Storage. El formato de las direcciones URL solo se usa para crear copias de seguridad en el servicio de almacenamiento Microsoft Azure. Para obtener información y ejemplos, vea [Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). Para obtener un tutorial, vea [Tutorial: Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](~/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md).

> [!NOTE]
> El dispositivo de disco NUL descartará toda la información que se le envíe y solo se debe usar para realizar pruebas. No se debe usar en entornos de producción.
> [!IMPORTANT]
> A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 y hasta [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], cuando se hagan copias de seguridad en URL, solo se podrán hacer en un único dispositivo. Para hacer una copia de seguridad en varios dispositivos al realizar copias de seguridad en direcciones URL, debe usar [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y tokens de firma de acceso compartido (SAS). Para ver ejemplos sobre cómo crear una Firma de acceso compartido, vea [Copia de seguridad en URL de SQL Server](../../relational-databases/backup-restore/sql-server-backup-to-url.md) y [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell) (Simplificación de la creación de credenciales de SQL con tokens de firmas de acceso compartido [SAS] en Almacenamiento de Azure con PowerShell).

**URL se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 y versiones posteriores)

No es necesario que exista un dispositivo de disco antes de que se especifique en una instrucción BACKUP. Si el dispositivo físico existe y no se especifica la opción INIT en la instrucción BACKUP, la copia de seguridad se anexa al dispositivo.

> [!NOTE]
> El dispositivo NUL descartará todas las entradas que se envíen a este archivo, si bien la copia de seguridad seguirá marcando todas las páginas para reflejar que se ha hecho una copia de seguridad de ellas.

Para obtener más información, vea [Dispositivos de copia de seguridad](../../relational-databases/backup-restore/backup-devices-sql-server.md).

> [!NOTE]
> La opción TAPE se quitará en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.

*n* Es un marcador de posición que indica que se pueden especificar hasta 64 dispositivos de copia de seguridad en una lista separada por comas.

MIRROR TO \<backup_device> [ **,** ...*n* ] Especifica un conjunto de hasta tres dispositivos de copia de seguridad, cada uno de los cuales crea un reflejo de los dispositivos de copia de seguridad especificados en la cláusula TO. La cláusula MIRROR TO debe incluir el mismo número y tipo de dispositivos de copia de seguridad que la cláusula TO. El número máximo de cláusulas MIRROR TO es tres.

Esta opción solo está disponible en la edición Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> Para MIRROR TO = DISK, BACKUP determina automáticamente el tamaño de bloque apropiado de los dispositivos de disco en función del tamaño de sector del disco. Si al disco MIRROR TO se le aplica formato con un tamaño de sector diferente al del disco especificado como dispositivo de copia de seguridad principal, se producirá un error en el comando de copia de seguridad. Con el fin de reflejar las copias de seguridad en los dispositivos que tienen otros tamaños de sector, se debe especificar el parámetro BLOCKSIZE y se debe establecer en el tamaño de sector mayor de todos los dispositivos de destino. Para obtener más información sobre el tamaño de bloque, vea "BLOCKSIZE" más adelante en este tema.

\<backup_device> Consulte "\<backup_device>", que encontrará previamente en esta sección.

*n* Es un marcador de posición que indica que se pueden especificar hasta 64 dispositivos de copia de seguridad en una lista separada por comas. El número de dispositivos de la cláusula MIRROR TO debe ser igual al número de dispositivos de la cláusula TO.

Para más información, vea "Familias de medios en conjuntos de medios reflejados" en la sección [Comentarios](#general-remarks) más adelante en este tema.

[ *next-mirror-to* ] Es un marcador de posición que indica que una sola instrucción BACKUP puede contener hasta tres cláusulas MIRROR TO, además de una sola cláusula TO.

### <a name="with-options"></a>Opciones de WITH

Especifica las opciones que se van a utilizar con una operación de copia de seguridad.

CREDENTIAL **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 y versiones posteriores)
Solo se usa al crear una copia de seguridad en el servicio Microsoft Azure Blob Storage.

FILE_SNAPSHOT **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores)

Se usa para crear una instantánea de Azure de los archivos de base de datos cuando todos los archivos de base de datos de SQL Server se han almacenado usando el servicio Azure Blob Storage. Para más información, vea [Archivos de datos de SQL Server en Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). La copia de seguridad de instantánea de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] toma instantáneas de Azure de los archivos de base de datos (archivos de datos y de registro) en un estado coherente. Un conjunto coherente de instantáneas de Azure conforma una copia de seguridad y se registra en el archivo de copia de seguridad. La única diferencia entre `BACKUP DATABASE TO URL WITH FILE_SNAPSHOT` y `BACKUP LOG TO URL WITH FILE_SNAPSHOT` es que este último también trunca el registro de transacciones, cosa que no hace el primero. Con la copia de seguridad de instantánea de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], después de la copia de seguridad completa inicial que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere para establecer la cadena de copia de seguridad, solo se necesita una copia de seguridad del registro de transacciones para restaurar una base de datos al momento en el tiempo de la copia de seguridad del registro de transacciones. Además, solo se necesitan dos copias de seguridad del registro de transacciones para restaurar una base de datos a un momento en el tiempo dentro del intervalo entre ambas copias de seguridad del registro de transacciones.

DIFFERENTIAL

Se utiliza solo con BACKUP DATABASE. Especifica que la copia de seguridad de la base de datos o el archivo solo debe estar compuesta por las partes de la base de datos o el archivo que hayan cambiado desde la última copia de seguridad completa. Una copia de seguridad diferencial suele ocupar menos espacio que una copia de seguridad completa. Utilice esta opción para que no tenga que aplicar todas las copias de seguridad del registro individuales efectuadas desde que se realizó la última copia de seguridad completa.

> [!NOTE]
> De forma predeterminada, `BACKUP DATABASE` crea una copia de seguridad completa.

Para obtener más información, vea [Copias de seguridad diferenciales](../../relational-databases/backup-restore/differential-backups-sql-server.md).

ENCRYPTION Se utiliza para especificar el cifrado para una copia de seguridad. Puede especificar un algoritmo de cifrado para cifrar la copia de seguridad o especificar `NO_ENCRYPTION` para no hacer que la copia de seguridad se cifre. El cifrado es una práctica recomendada para ayudar a proteger los archivos de copia de seguridad. La lista de algoritmos que puede especificar son:

- `AES_128`
- `AES_192`
- `AES_256`
- `TRIPLE_DES_3KEY`
- `NO_ENCRYPTION`

Si elige cifrar, tendrá que especificar el sistema de cifrado mediante estas opciones de cifrado:

- `SERVER CERTIFICATE` = Encryptor_Name
- `SERVER ASYMMETRIC KEY` = Encryptor_Name

`SERVER CERTIFICATE` y `SERVER ASYMMETRIC KEY` son un certificado y una clave asimétrica creados en la base de datos `master`. Para obtener más información, consulte [`CREATE CERTIFICATE`](../../t-sql/statements/create-certificate-transact-sql.md) y [`CREATE ASYMMETRIC KEY`](../../t-sql/statements/create-asymmetric-key-transact-sql.md) respectivamente.

> [!WARNING]
> Cuando se usa cifrado junto con el argumento `FILE_SNAPSHOT`, el propio archivo de metadatos se cifra con el algoritmo de cifrado especificado, y el sistema comprueba si el [Cifrado de datos transparente (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) se ha completado en relación con la base de datos. No se aplica más cifrado a los datos. La copia de seguridad no se realiza si la base de datos no está cifrada o si el cifrado no se completa antes de que se emita la instrucción BACKUP.

**Opciones de conjunto de copia de seguridad**

Estas opciones funcionan en el conjunto de copia de seguridad que se crea con esta operación de copia de seguridad.

> [!NOTE]
> Para especificar un conjunto de copia de seguridad en una operación de restauración, use la opción `FILE = <backup_set_file_number>`. Para más información sobre cómo especificar un conjunto de copia de seguridad, vea "Especificar un conjunto de copia de seguridad" en [Argumentos de RESTORE](../../t-sql/statements/restore-statements-arguments-transact-sql.md).

COPY_ONLY especifica que la copia de seguridad es una *copia de seguridad de solo copia*, lo que no afecta a la secuencia normal de copias de seguridad. Se crea una copia de seguridad de solo copia independientemente de las copias de seguridad convencionales programadas regularmente. Una copia de seguridad de solo copia no afecta a los procedimientos de copias de seguridad y restauración generales de la base de datos.

Las copias de seguridad de solo copia deben utilizarse en situaciones en las que se realiza una copia de seguridad con un fin específico, por ejemplo al hacer la copia de seguridad del registro antes de una restauración de archivos en línea. Normalmente, una copia de seguridad de solo copia se usa una vez y se elimina.

- Cuando se usa con `BACKUP DATABASE`, la opción `COPY_ONLY` crea una copia de seguridad completa que no se puede usar como una base diferencial. El mapa de bits diferencial no se actualiza y las copias de seguridad diferenciales se comportan como si no existiera la copia de seguridad de solo copia. Las copias de seguridad diferenciales posteriores usarán la copia de seguridad completa convencional más reciente como base.

    > [!IMPORTANT]
    > Si `DIFFERENTIAL` y `COPY_ONLY` se usan juntas, `COPY_ONLY` se omite y se crea una copia de seguridad diferencial.

- Cuando se usa con `BACKUP LOG`, la opción `COPY_ONLY` crea una *copia de seguridad del registro de solo copia* , lo que no trunca el registro de transacciones. La copia de seguridad del registro de solo copia no afecta a la cadena de registros y otras copias de seguridad del registro se comportan como si no existiera la copia de seguridad de solo copia.

Para obtener más información, vea [Copias de seguridad de solo copia](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).

{ COMPRESSION | NO_COMPRESSION } En [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] y versiones posteriores únicamente, especifica si la [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md) se realiza en esta copia de seguridad, lo que invalida la configuración predeterminada del nivel servidor.

Durante la instalación, el comportamiento predeterminado es que no se realice la compresión de copia de seguridad. Este valor predeterminado se puede cambiar estableciendo la opción [valor predeterminado de compresión de copia de seguridad](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) de la configuración del servidor. Para más información sobre cómo ver el valor actual de esta opción, vea [Ver o cambiar las propiedades del servidor](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).

Para más información sobre cómo usar la compresión de copia de seguridad con bases de datos con [Cifrado de datos transparente (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) habilitado, vea la sección [Comentarios](#general-remarks).

COMPRESSION Habilita de forma explícita la compresión de copia de seguridad.

NO_COMPRESSION Deshabilita de forma explícita la compresión de copia de seguridad.

DESCRIPTION **=** { **'** _text_ **'**  |  **@** _text\_variable_ } Especifica el texto de forma libre que describe el conjunto de copia de seguridad. La cadena puede tener un máximo de 255 caracteres.

NAME **=** { *backup_set_name* |  **@** _backup\_set\_var_ } Especifica el nombre del conjunto de copia de seguridad. Los nombres pueden tener un máximo de 128 caracteres. Si no se especifica NAME, está en blanco.

{ EXPIREDATE **="** _date_ **"** | RETAINDAYS **=** _days_ } Especifica cuándo se puede sobrescribir el conjunto de copia de seguridad para esta copia de seguridad. Si se usan las dos opciones, RETAINDAYS tiene precedencia sobre EXPIREDATE.

Si no se especifica ninguna opción, la fecha de expiración se determina con el valor de configuración **mediaretention**. Para obtener más información, vea [Opciones de configuración de servidor](../../database-engine/configure-windows/server-configuration-options-sql-server.md).

> [!IMPORTANT]
> Estas opciones solo impiden que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sobrescriba un archivo. Las cintas se pueden borrar utilizando otros métodos, y los archivos de disco se pueden eliminar usando el sistema operativo. Para obtener más información acerca de la comprobación de la expiración, vea SKIP y FORMAT en este tema.

EXPIREDATE **=** { **'** _date_ **'**  |  **@** _date\_var_ } Especifica cuándo expira el conjunto de copia de seguridad y se puede sobrescribir. Si se proporciona como una variable (@_date\_var_), esta fecha debe seguir el formato **datetime** configurado para el sistema y se debe especificar de uno de los siguientes modos:

- Una constante de cadena (@_date\_var_ **=** date)
- Variable de un tipo de datos de cadena de caracteres (excepto los tipos de datos **ntext** o **text**)
- Un **smalldatetime**
- Una variable **datetime**

Por ejemplo:

- `'Dec 31, 2020 11:59 PM'`
- `'1/1/2021'`

Para más información sobre cómo especificar valores **datetime**, vea [Tipos de fecha y hora](../../t-sql/data-types/date-and-time-types.md).

> [!NOTE]
> Para omitir la fecha de caducidad, use la opción `SKIP`.

RETAINDAYS **=** { *days* |  **@** _days\_var_ } Especifica el número de días que deben transcurrir antes de que se pueda sobrescribir este conjunto de medios de copia de seguridad. Si se proporciona como una variable ( **@** _days\_var_), se debe especificar como un entero.

**Opciones de conjuntos de medios**

Estas opciones funcionan para todo el conjunto de medios.

{ **NOINIT** | INIT } Controla si la operación de copia de seguridad anexa o sobrescribe los conjuntos de copias de seguridad existentes en el medio. El valor predeterminado es anexar al conjunto de copias de seguridad más reciente en el medio (NOINIT).

> [!NOTE]
> Para más información sobre las interacciones entre { **NOINIT** | INIT } y { **NOSKIP** | SKIP }, vea [Comentarios](#general-remarks) más adelante en este tema.

NOINIT Indica que el conjunto de copia de seguridad se anexa al conjunto de medios especificado, conservando así los conjuntos de copia de seguridad existentes. Si se ha definido una contraseña para el conjunto de medios, debe proporcionarla. NOINIT es el valor predeterminado.

Para obtener más información, vea [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

INIT Especifica que se deben sobrescribir todos los conjuntos de copia de seguridad, pero conserva el encabezado de medios. Si se especifica INIT, se sobrescriben todos los conjuntos de copia de seguridad existentes en el dispositivo, si las condiciones lo permiten. De forma predeterminada, BACKUP comprueba las siguientes condiciones y no sobrescribe los medios de copia de seguridad en caso de existir alguna de las condiciones siguientes:

- Aún no ha expirado ningún conjunto de copia de seguridad. Para más información, vea las opciones `EXPIREDATE` y `RETAINDAYS`.
- El nombre del conjunto de copia de seguridad proporcionado en la instrucción BACKUP, si se especificó, no coincide con el nombre de los medios de copia de seguridad. Para obtener más información, vea la opción NAME, descrita anteriormente de esta sección.

Para invalidar estas comprobaciones, use la opción `SKIP`.

Para obtener más información, vea [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

{ **NOSKIP** | SKIP } Controla si una operación de copia de seguridad comprueba la fecha y la hora de expiración de los conjuntos de copia de seguridad en el medio antes de sobrescribirlos.

> [!NOTE]
> Para más información sobre las interacciones entre { **NOINIT** | INIT } y { **NOSKIP** | SKIP }, vea "Comentarios" más adelante en este tema.

NOSKIP Indica a la instrucción BACKUP que compruebe la fecha de expiración de todos los conjuntos de copia de seguridad de los medios antes de permitir que se sobrescriban. Este es el comportamiento predeterminado.

SKIP Deshabilita la comprobación de la expiración y el nombre del conjunto de copia de seguridad que suele realizar la instrucción BACKUP para impedir que se sobrescriban los conjuntos de copia de seguridad. Para obtener más información acerca de las interacciones entre { INIT | NOINIT } y { NOSKIP | SKIP }, vea "Comentarios", más adelante en este tema.
Para ver las fechas de expiración de los conjuntos de copias de seguridad, consulte la columna **expiration_date** de la tabla del historial de [backupset](../../relational-databases/system-tables/backupset-transact-sql.md).

{ **NOFORMAT** | FORMAT } Especifica si debe escribirse el encabezado de medios en los volúmenes usados en esta operación de copia de seguridad, con lo que se sobrescribirán los conjuntos de copias de seguridad y el encabezado de medios existentes.

NOFORMAT Especifica que la operación de copia de seguridad conservará los conjuntos de copias de seguridad y el encabezado de medios existentes en los volúmenes de medios usados en esta operación de copia de seguridad. Este es el comportamiento predeterminado.

FORMAT Especifica que se debe crear un conjunto de medios nuevo. FORMAT hace que la operación de copia de seguridad escriba un nuevo encabezado de medios en todos los volúmenes de medios usados en la operación de copia de seguridad. El contenido existente del volumen no será válido porque se sobrescribirán los conjuntos de copias de seguridad y el encabezado de medios existentes.

> [!IMPORTANT]
> Use `FORMAT` con cuidado. Al dar formato a cualquier volumen de un conjunto de medios, todo el conjunto de medios se convierte en inutilizable. Por ejemplo, si inicializa una cinta que pertenece a un conjunto de medios distribuido, queda inutilizable todo el conjunto de medios.

La especificación de FORMAT implica `SKIP` y no es necesario especificar `SKIP` de forma explícita.

MEDIADESCRIPTION **=** { *text* | **@** _text\_variable_ } Especifica la descripción de texto de forma libre, con un máximo de 255 caracteres, del conjunto de medios.

MEDIANAME **=** { *media_name* |  **@** _media\_name\_variable_ } Especifica el nombre del medio para el conjunto completo de medios de copia de seguridad. El nombre del medio no puede tener más de 128 caracteres y, si se especifica `MEDIANAME`, debe coincidir con el nombre de medio especificado que ya existe en los volúmenes de copia de seguridad. Si no se especifica o se especifica la opción SKIP, no se realiza la comprobación del nombre del medio.

BLOCKSIZE **=** { *blocksize* |  **@** _blocksize\_variable_ } Especifica el tamaño de bloque físico, en bytes. Los tamaños admitidos son 512, 1024, 2048, 4096, 8192, 16384, 32768 y 65536 (64 KB) bytes. El valor predeterminado es 65536 para dispositivos de cinta y 512 para otros dispositivos. Normalmente, esta opción no es necesaria, ya que BACKUP selecciona automáticamente un tamaño de bloque apropiado para el dispositivo. La especificación explícita de un tamaño de bloque invalida la selección automática del tamaño de bloque.

Si va a realizar una copia de seguridad en CD-ROM que pretende utilizar para copiar y restaurar, especifique BLOCKSIZE=2048.

> [!NOTE]
> Normalmente, esta opción solo afecta al rendimiento al escribir en dispositivos de cinta.

**Opciones de transferencia de datos**

BUFFERCOUNT **=** { *buffercount* |  **@** _buffercount\_variable_ } Especifica el número total de búferes de E/S que se va a usar en la operación de copia de seguridad. Puede especificar cualquier entero positivo; no obstante, un número de búferes demasiado grande podría provocar errores de "memoria insuficiente" a causa de un espacio de direcciones virtuales inadecuado en el proceso Sqlservr.exe.

El espacio total usado por los búferes está determinado por: `BUFFERCOUNT * MAXTRANSFERSIZE`.

> [!NOTE]
> Para obtener información importante sobre cómo usar la opción `BUFFERCOUNT`, vea el blog [Incorrect BufferCount data transfer option can lead to OOM condition](/archive/blogs/sqlserverfaq/incorrect-buffercount-data-transfer-option-can-lead-to-oom-condition) (Una opción de transferencia de datos BufferCount incorrecta puede agotar la memoria).

MAXTRANSFERSIZE **=** { *maxtransfersize* | _**@** maxtransfersize\_variable_ } Especifica la unidad de transferencia mayor (en bytes) que se debe usar entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el medio de copia de seguridad. Los valores posibles son múltiplos de 65536 bytes (64 KB), hasta un máximo de 4194304 bytes (4 MB).

> [!NOTE]
> Al crear copias de seguridad con el Servicio del objeto de escritura de SQL, si la base de datos tiene configurado [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md) o incluye [grupos de archivos con optimización para memoria](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md), el valor de `MAXTRANSFERSIZE` en el momento de una restauración debe ser mayor o igual que el valor de `MAXTRANSFERSIZE` que se usó cuando se creó la copia de seguridad.

> [!NOTE]
> En el caso de las bases de datos con [Cifrado de datos transparente (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) habilitado que tienen un único archivo de datos, el valor predeterminado de `MAXTRANSFERSIZE` es 65536 (64 KB). En las bases de datos que no tienen cifrado TDE, el valor predeterminado de `MAXTRANSFERSIZE` es 1048576 (1 MB) cuando se usa la copia de seguridad en DISK y 65536 (64 KB) al usar VDI o TAPE.
> Para más información sobre cómo usar la compresión de copia de seguridad con bases de datos con Cifrado de datos transparente (TDE), vea la sección [Comentarios](#general-remarks).

**Operaciones de administración de errores**

Estas opciones permiten determinar si se habilitarán las sumas de comprobación de copia de seguridad para la operación de copia de seguridad y si ésta se detiene al encontrar un error.

{ **NO_CHECKSUM** | CHECKSUM } Controla si las sumas de comprobación de copia de seguridad están habilitadas.

NO_CHECKSUM Deshabilita de forma explícita la generación de sumas de comprobación de copia de seguridad (y la validación de sumas de comprobación de página). Este es el comportamiento predeterminado.

CHECKSUM Especifica que la operación de copia de seguridad comprueba en cada página si hay suma de comprobación y página rasgada (si está habilitada y disponible) y generará una suma de comprobación para toda la copia de seguridad.

El uso de sumas de comprobación de copia de seguridad puede afectar a la carga de trabajo y al rendimiento de la copia de seguridad.

Para más información, vea [Errores posibles de medios durante copia de seguridad y restauración](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

{ **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR } Controla si una operación de copia de seguridad se detiene o continúa después de encontrar un error en la suma de comprobación de página.

STOP_ON_ERROR Indica a BACKUP que dé error si no se comprueba una suma de comprobación de página. Este es el comportamiento predeterminado.

CONTINUE_AFTER_ERROR Indica a BACKUP que continúe a pesar de la detección de errores como sumas de comprobación no válidas o páginas rasgadas.

Si no puede hacer la copia de seguridad del final del registro con la opción NO_TRUNCATE cuando la base de datos está dañada, puede intentar realizar una [copia de seguridad de registros después del registro](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) especificando CONTINUE_AFTER_ERROR en vez de NO_TRUNCATE.

Para más información, vea [Errores posibles de medios durante copia de seguridad y restauración](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

**Opciones de compatibilidad**

RESTART A partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], no tiene ningún efecto. La versión acepta esta opción por compatibilidad con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

**Opciones de supervisión**

STATS [ **=** _percentage_ ] Muestra un mensaje cada vez que se completa otro *percentage*; se usa para indicar el progreso. Si *percentage* se omite, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muestra un mensaje después de completar cada 10 por ciento.

La opción STATS informa del porcentaje completado desde el umbral para informar del próximo intervalo. Éste es aproximadamente el porcentaje especificado; por ejemplo, con STATS=10, si la cantidad completada equivale al 40 por ciento, la opción puede mostrar el 43 por ciento. En el caso de los conjuntos de copia de seguridad de gran tamaño, esto no representa ningún problema porque el porcentaje completado se mueve muy lentamente entre las llamadas de E/S.

**Opciones de cinta**

Estas opciones solo se utilizan para dispositivos de cinta. Se omitirán si se utiliza otro tipo de dispositivo.

{ **REWIND** | NOREWIND } REWIND

Especifica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libera y rebobina la cinta. REWIND es la opción predeterminada.

NOREWIND

Especifica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantendrá la cinta abierta tras la operación de copia de seguridad. Puede utilizar esta opción como ayuda para mejorar el rendimiento al realizar varias operaciones de copia de seguridad en una cinta.

NOREWIND implica NOUNLOAD, y estas opciones son incompatibles en una sola instrucción BACKUP.

> [!NOTE]
> Si se usa `NOREWIND`, la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserva la propiedad de la unidad de cinta hasta que una instrucción BACKUP o RESTORE que se ejecuta en el mismo proceso use la opción `REWIND` o `UNLOAD`, o bien se cierra la instancia del servidor. Mantener abierta la cinta evita que otros procesos obtengan acceso a la misma. Para más información sobre cómo mostrar una lista de cintas abiertas y cómo cerrar una cinta abierta, vea [Dispositivos de copia de seguridad](../../relational-databases/backup-restore/backup-devices-sql-server.md).

{ **UNLOAD** | NOUNLOAD }

> [!NOTE]
> `UNLOAD` y `NOUNLOAD` son opciones de configuración de sesión que persisten mientras dure la sesión o hasta que se restablezca especificando la alternativa.

UNLOAD

Especifica que la cinta se rebobina y descarga automáticamente al terminar la copia de seguridad. UNLOAD es el valor predeterminado cuando se inicia una sesión.

NOUNLOAD

Especifica que, tras la operación de BACKUP, la cinta permanece cargada en la unidad de cinta.

> [!NOTE]
> En una copia de seguridad de un dispositivo de cinta, la opción `BLOCKSIZE` afecta al rendimiento de la operación de copia de seguridad. Normalmente, esta opción solo afecta al rendimiento al escribir en dispositivos de cinta.

**Opciones específicas del registro**

Estas opciones solo se usan con `BACKUP LOG`.

> [!NOTE]
> Si no desea hacer copias de seguridad del registro, use el modelo de recuperación simple. Para más información, consulte [Modelos de recuperación](../../relational-databases/backup-restore/recovery-models-sql-server.md).

{ NORECOVERY | STANDBY **=** _undo_file_name_ }

NORECOVERY

Realiza una copia de seguridad del registro después del error y deja la base de datos en el estado RESTORING. NORECOVERY resulta útil cuando, en caso de error, se conmuta a una base de datos secundaria y al guardar los registros después del error antes de una operación RESTORE.

Para hacer una copia de seguridad del registro óptima que omita el truncamiento de los registros y, después, establecer la base de datos en el estado RESTORING de forma atómica, use las opciones `NO_TRUNCATE` y `NORECOVERY` conjuntamente.

STANDBY **=** _standby_file_name_

Realiza una copia de seguridad del final del registro y deja la base de datos en modo de solo lectura y en el estado STANDBY. La cláusula STANDBY escribe datos en espera (realiza la reversión, pero con la posibilidad de recuperaciones posteriores). El uso de la opción STANDBY es equivalente a BACKUP LOG WITH NORECOVERY seguido de RESTORE WITH STANDBY.

El uso del modo de espera requiere un archivo en espera especificado mediante *standby_file_name*, cuya ubicación se almacena en el registro de la base de datos. Si el archivo especificado ya existe, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] lo sobrescribe; si no existe, [!INCLUDE[ssDE](../../includes/ssde-md.md)] lo crea. El archivo en espera pasa a formar parte de la base de datos.

Este archivo contiene los cambios revertidos, que se deben invertir si las operaciones RESTORE LOG se van a aplicar posteriormente. Debe haber suficiente espacio en disco para permitir el crecimiento del archivo en espera de manera que pueda contener todas las páginas distintas de la base de datos que se modificaron al revertir las transacciones sin confirmar.

NO_TRUNCATE

Especifica que el registro no se va a truncar y provoca que el [!INCLUDE[ssDE](../../includes/ssde-md.md)] intente hacer la copia de seguridad con independencia del estado de la base de datos. Por consiguiente, una copia de seguridad realizada con `NO_TRUNCATE` puede tener metadatos incompletos. Esta opción permite realizar copias de seguridad del registro cuando la base de datos está dañada.

La opción NO_TRUNCATE de BACKUP LOG es equivalente a la especificación de COPY_ONLY y CONTINUE_AFTER_ERROR.

Sin la opción `NO_TRUNCATE`, la base de datos debe estar en el estado ONLINE. Si la base de datos está en el estado SUSPENDED, podría poder crear una copia de seguridad especificando `NO_TRUNCATE`. Pero si la base de datos se encuentra en el estado OFFLINE o EMERGENCY, no se admite BACKUP, ni siquiera con `NO_TRUNCATE`. Para más información sobre los estados de las bases de datos, vea el [Estados de base de datos](../../relational-databases/databases/database-states.md).

## <a name="about-working-with-sql-server-backups"></a>Cómo trabajar con copias de seguridad de SQL Server

En esta sección se presentan los siguiente conceptos esenciales de la copia de seguridad:

[Tipos de copia de seguridad](#Backup_Types)
[Truncamiento del registro de transacciones](#Tlog_Truncation)
[Dar formato a los medios de copia de seguridad](#Formatting_Media)
[Trabajar con dispositivos de copia de seguridad y conjuntos de medios](#Backup_Devices_and_Media_Sets)
[Restaurar copias de seguridad SQL Server](#Restoring_Backups)

> [!NOTE]
> Para ver una introducción a las copias de seguridad en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Información general de copia de seguridad](../../relational-databases/backup-restore/backup-overview-sql-server.md).

### <a name="backup-types"></a><a name="Backup_Types"></a> Tipos de copia de seguridad

Los tipos de copia de seguridad admitidos dependen del modelo de recuperación de la base de datos y son los siguientes:

- Todos los modelos de recuperación admiten copias de seguridad de datos completas y diferenciales.

    |Ámbito de la copia de seguridad|Tipos de copia de seguridad|
    |---------------------|------------------|
    |Base de datos completa|Las [copias de seguridad de bases de datos](../../relational-databases/backup-restore/full-database-backups-sql-server.md) abarcan toda la base de datos.<br /><br /> Opcionalmente, cada copia de seguridad de base de datos puede servir como la base de una serie de una o más [copias de seguridad de base de datos diferenciales](../../relational-databases/backup-restore/differential-backups-sql-server.md).|
    |Base de datos parcial|Las [copias de seguridad parciales](../../relational-databases/backup-restore/partial-backups-sql-server.md) abarcan grupos de archivos de lectura o escritura y, posiblemente, uno o varios grupos de archivos o archivos de solo lectura.<br /><br /> Opcionalmente, cada copia de seguridad parcial puede servir como la base de una serie de una o más [copias de seguridad parciales diferenciales](../../relational-databases/backup-restore/differential-backups-sql-server.md).|
    |Archivo o grupo de archivos|Las [copias de seguridad de archivos](../../relational-databases/backup-restore/full-file-backups-sql-server.md) abarcan uno o varios archivos o grupos de archivos, y solo son relevantes para las bases de datos que contengan varios grupos de archivos. En el modelo de recuperación simple, las copias de seguridad de archivos se limitan básicamente a los archivos secundarios de solo lectura.<br /> Opcionalmente, cada copia de seguridad de archivos puede servir como la base de una serie de una o más [copias de seguridad de archivos diferenciales](../../relational-databases/backup-restore/differential-backups-sql-server.md).|

- En el modelo de recuperación completa o el modelo de recuperación optimizado para cargas masivas de registros, las copias de seguridad convencionales también incluyen *copias de seguridad de registros de transacciones* secuenciales (o *copias de seguridad de registros*), las que sean necesarias. Cada copia de seguridad del registro cubre la parte del registro de transacciones que estaba activa al crear la copia de seguridad e incluye todos los registros que no se copiaron en una copia de seguridad del registro anterior.

    Para reducir lo máximo posible el riesgo de perder trabajo, lo que supondría una sobrecarga de trabajo administrativo, debería programar copias de seguridad del registro frecuentes. La programación de copias de seguridad diferenciales entre copias de seguridad completas puede reducir el tiempo de restauración al disminuir el número de copias de seguridad del registro que se deben restaurar después de restaurar los datos.

     Recomendamos que coloque las copias de seguridad del registro en un volumen que no sea el de las copias de seguridad de la base de datos.

    > [!NOTE]
    > Para poder crear la primera copia de seguridad de registros, debe crear una copia de seguridad completa.

- La *copia de seguridad de solo copia* es una copia de seguridad completa o de registros especial independiente de la secuencia normal de las copias de seguridad convencionales. Para crear una copia de seguridad de solo copia, especifique la opción COPY_ONLY en la instrucción BACKUP. Para obtener más información, vea [Copias de seguridad de solo copia](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).

### <a name="transaction-log-truncation"></a><a name="Tlog_Truncation"></a> Truncamiento del registro de transacciones

Para evitar llenar el registro de transacciones de una base de datos, las copias de seguridad rutinarias son esenciales. Normalmente, el truncamiento se produce automáticamente bajo el modelo de recuperación simple cuando se realiza una copia de seguridad de la base de datos y bajo el modelo de recuperación completa cuando se realiza una copia de seguridad del registro de transacciones. Sin embargo, en ocasiones se puede retrasar el proceso de truncamiento. Para obtener información sobre los factores que pueden retrasar el truncamiento del registro, vea [Registro de transacciones](../../relational-databases/logs/the-transaction-log-sql-server.md).

> [!NOTE]
> Las opciones `BACKUP LOG WITH NO_LOG` y `WITH TRUNCATE_ONLY` se han descontinuado. Si usa el modelo de recuperación completa o el optimizado para cargas masivas de registros y debe quitar la cadena de copia de seguridad de registros de una base de datos, cambie al modelo de recuperación simple. Para obtener más información, vea [Ver o cambiar el modelo de recuperación de una base de datos](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).

### <a name="formatting-backup-media"></a><a name="Formatting_Media"></a> Dar formato a los medios de copia de seguridad

Con una instrucción BACKUP se dan formato a los medios de copia de seguridad si y solo si se cumple alguna de las siguientes condiciones:

- Se especifica la opción `FORMAT`.
- El medio está vacío.
- En la operación se está escribiendo una cinta de continuación.

### <a name="working-with-backup-devices-and-media-sets"></a><a name="Backup_Devices_and_Media_Sets"></a> Trabajar con dispositivos de copia de seguridad y conjuntos de medios

#### <a name="backup-devices-in-a-striped-media-set-a-stripe-set"></a>Dispositivos de copia de seguridad en un conjunto de medios seccionado (conjunto seccionado)
Un *conjunto seccionado* es un conjunto de archivos de disco en el que los datos se dividen en bloques y se distribuyen en un orden fijo. El número de dispositivos de copia de seguridad usados en un conjunto de franjas debe ser siempre el mismo (a menos que el medio se reinicialice con `FORMAT`).

En el siguiente ejemplo se escribe una copia de seguridad de la base de datos [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] en un nuevo conjunto de medios distribuido que utiliza tres archivos de disco.

```sql
BACKUP DATABASE AdventureWorks2012
TO DISK='X:\SQLServerBackups\AdventureWorks1.bak',
DISK='Y:\SQLServerBackups\AdventureWorks2.bak',
DISK='Z:\SQLServerBackups\AdventureWorks3.bak'
WITH FORMAT,
  MEDIANAME = 'AdventureWorksStripedSet0',
  MEDIADESCRIPTION = 'Striped media set for AdventureWorks2012 database';
GO
```

Después de definir un dispositivo de copia de seguridad como parte de un conjunto de franjas, no se puede utilizar para realizar copias de seguridad en un único dispositivo a menos que se especifique FORMAT. De forma similar, un dispositivo de copia de seguridad que contenga copias de seguridad sin franjas no se puede utilizar en un conjunto de franjas a menos que se especifique FORMAT. Para dividir un conjunto de copia de seguridad distribuido, utilice FORMAT.

Si no se especifica MEDIANAME o MEDIADESCRIPTION al escribir el encabezado de medios, el campo de encabezado de medios que corresponde al elemento en blanco está vacío.

#### <a name="working-with-a-mirrored-media-set"></a>Trabajar con un conjunto de medios reflejado

Normalmente, las copias de seguridad no se reflejan, y las instrucciones BACKUP simplemente incluyen una cláusula TO. No obstante, puede haber hasta cuatro reflejos en total por cada conjunto de medios. En un conjunto de medios reflejado, la operación copia de seguridad escribe en varios grupos de dispositivos de copia de seguridad. Cada grupo de dispositivos de copia de seguridad contiene un único reflejo en el conjunto de medios reflejado. Cada reflejo debe usar la misma cantidad y tipo de dispositivos de copia de seguridad físicos, y todos deben tener las mismas propiedades.

Para hacer una copia de seguridad de un conjunto de medios reflejado, deben estar presentes todos los reflejos. Para realizar una copia de seguridad en un conjunto de medios reflejado, especifique la cláusula `TO` para indicar el primer reflejo y la cláusula `MIRROR TO` para cada reflejo adicional.

En el caso de un conjunto de medios reflejado, cada cláusula `MIRROR TO` debe incluir el mismo número y tipo de dispositivos que la cláusula TO. En el siguiente ejemplo se escribe en un conjunto de medios reflejado que contiene dos reflejos y usa tres dispositivos por reflejo:

```sql
BACKUP DATABASE AdventureWorks2012
TO DISK='X:\SQLServerBackups\AdventureWorks1a.bak',
  DISK='Y:\SQLServerBackups\AdventureWorks2a.bak',
  DISK='Z:\SQLServerBackups\AdventureWorks3a.bak'
MIRROR TO DISK='X:\SQLServerBackups\AdventureWorks1b.bak',
  DISK='Y:\SQLServerBackups\AdventureWorks2b.bak',
  DISK='Z:\SQLServerBackups\AdventureWorks3b.bak';
GO
```

> [!IMPORTANT]
> Este ejemplo se ha creado de modo que pueda probarlo en su sistema local. En la práctica, realizar una copia de seguridad en varios dispositivos de la misma unidad afectaría el rendimiento y eliminaría la redundancia para la que se diseñaron los conjuntos de medios reflejados.

##### <a name="media-families-in-mirrored-media-sets"></a>Familias de medios en conjuntos de medios reflejados

Cada dispositivo de copia de seguridad especificado en la cláusula `TO` de una instrucción BACKUP corresponde a una familia de medios. Por ejemplo, si la cláusula `TO` incluye tres dispositivos, BACKUP escribe los datos en tres familias de medios. En un conjunto de medios reflejado, cada reflejo debe contener una copia de cada familia de medios. Esto se debe a que el número de dispositivos debe ser idéntico en cada reflejo.

Si se incluyen varios dispositivos para cada reflejo, el orden determina qué familia de medios se escribe en cada dispositivo. Por ejemplo, en cada lista de dispositivos, el segundo dispositivo corresponde a la segunda familia de medios. En la tabla siguiente se muestra la correspondencia entre los dispositivos y las familias de medios para los dispositivos del ejemplo anterior.

|Reflejo|Familia de medios 1|Familia de medios 2|Familia de medios 3|
|---------|---------|---------|---------|
|0|`Z:\AdventureWorks1a.bak`|`Z:\AdventureWorks2a.bak`|`Z:\AdventureWorks3a.bak`|
|1|`Z:\AdventureWorks1b.bak`|`Z:\AdventureWorks2b.bak`|`Z:\AdventureWorks3b.bak`|

 Siempre debe realizarse la copia de seguridad de una familia de medios en el mismo dispositivo dentro de un reflejo específico. Por tanto, cada vez que use un conjunto de medios existente, enumere los dispositivos de cada reflejo en el mismo orden que se especificaron al crear el conjunto de medios.

Para obtener más información sobre los conjuntos de medios reflejados, vea [Conjuntos de medios de copia de seguridad reflejados](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md). Para más información sobre los conjuntos de medios y las familias de medios en general, vea [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

### <a name="restoring-sql-server-backups"></a><a name="Restoring_Backups"></a> Restaurar copias de seguridad de SQL Server

Para restaurar una base de datos y, opcionalmente, recuperarla para ponerla en línea, o para restaurar un archivo o un grupo de archivos, use la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) o las tareas **Restore** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Para más información, vea [Información general sobre restauración y recuperación](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).

## <a name="additional-considerations-about-backup-options"></a><a name="Additional_Considerations"></a> Otras consideraciones sobre las opciones de BACKUP

### <a name="interaction-of-skip-noskip-init-and-noinit"></a><a name="Interactions_SKIP_etc"></a> Interacción de SKIP, NOSKIP, INIT y NOINIT

En esta tabla se describen las interacciones entre las opciones { **NOINIT** | INIT } y { **NOSKIP** | SKIP }.

> [!NOTE]
> Si el medio de cinta está vacío o el archivo de copia de seguridad en disco no existe, todas estas interacciones escriben un encabezado de medios y continúan. Si el medio no está vacío y no contiene ningún encabezado de medios válido, estas operaciones proporcionan un comentario que indica que no se trata de un medio MTF válido y terminan la operación de copia de seguridad.

|Opción de omitir|NOINIT|INIT|
|------|------------|----------|
|NOSKIP|Si el volumen contiene un encabezado de medios válido, se comprueba que el nombre del medio coincida con la opción `MEDIANAME`, si se ha proporcionado. Si coincide, se anexa el conjunto de copia de seguridad y se mantienen todos los conjuntos de copia de seguridad existentes.<br /> Si el volumen no contiene un encabezado de medios válido, se produce un error.|Si el volumen contiene un encabezado de medios válido, se realizan las siguientes comprobaciones:<br /><ul><li>Si se especificó `MEDIANAME`, se comprueba que el nombre del medio proporcionado coincida con el nombre del encabezado de medios.<sup>1</sup></li><li>Se comprueba que no haya conjuntos de copia de seguridad sin expirar en el medio. Si los hay, se finaliza la copia de seguridad.</li></ul><br />Si las comprobaciones son correctas, se sobrescriben los conjuntos de copia de seguridad de los medios y solo se mantiene el encabezado de medios.<br /> Si el volumen no contiene ningún encabezado de medios válido, se genera uno usando las opciones `MEDIANAME` y `MEDIADESCRIPTION` especificadas, si se especificó alguna.|
|SKIP|Si el volumen contiene un encabezado de medios válido, se anexa el conjunto de copia de seguridad, conservándose todos los conjuntos de copia de seguridad existentes.|Si el volumen contiene un encabezado de medios válido <sup>2</sup>, los conjuntos de copia de seguridad de los medios se sobrescriben, conservándose solo el encabezado de medios.<br /> Si el medio está vacío, se genera un encabezado de medios usando las opciones `MEDIANAME` y `MEDIADESCRIPTION`, si se especificó alguna.|

<sup>1</sup> El usuario debe pertenecer a los roles fijos de servidor o de base de datos apropiados para realizar una operación de copia de seguridad.

<sup>2</sup> La validez incluye el número de versión de MTF y otra información acerca del encabezado. Si la versión especificada no se admite o se trata de un valor no esperado, se produce un error.

## <a name="compatibility"></a>Compatibilidad

> [!CAUTION]
> Las copias de seguridad que se crean en una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se pueden restaurar en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

BACKUP admite la opción `RESTART` para proporcionar compatibilidad con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pero RESTART no tiene ningún efecto.

## <a name="general-remarks"></a>Observaciones generales

Se pueden anexar copias de seguridad de la base de datos o de registros a cualquier dispositivo de disco o cinta, lo que permite mantener la base de datos y sus registros de transacciones en la misma ubicación física.

La instrucción BACKUP no se permite en una transacción explícita o implícita.

Se pueden realizar operaciones de copia de seguridad entre plataformas, incluso entre diferentes tipos de procesador, siempre que el sistema operativo admita la intercalación de la base de datos.

Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], el valor `MAXTRANSFERSIZE` **mayor que 65536 (64 KB)** habilita un algoritmo de compresión optimizada para bases de datos con [cifrado de datos transparente (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) que, en primer lugar, descifra una página, la comprime y, luego, la vuelve a cifrar. Si `MAXTRANSFERSIZE` no se especifica o si se usa `MAXTRANSFERSIZE = 65536` (64 KB), la compresión de copia de seguridad en bases de datos con cifrado TDE comprime directamente las páginas cifradas, con lo cual existe la posibilidad de no lograr una buena razón de compresión. Para más información, vea [Backup Compression for TDE-enabled Databases](/archive/blogs/sqlcat/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases) (Compresión de copia de seguridad en bases de datos con TDE habilitado).

Desde [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU5, ya no es necesario establecer `MAXTRANSFERSIZE` para habilitar este algoritmo de compresión optimizada con TDE. Si se especifica el comando de copia de seguridad `WITH COMPRESSION` o la configuración de servidor  *predeterminada de compresión de copia de seguridad* se establece en 1, `MAXTRANSFERSIZE` aumentará automáticamente a 128K para habilitar el algoritmo optimizado. Si se especifica `MAXTRANSFERSIZE` en el comando de copia de seguridad con un valor > 64K, se respetará el valor proporcionado. En otras palabras, SQL Server nunca disminuirá automáticamente el valor, solo lo aumentará. Si necesita hacer una copia de seguridad de una base de datos con cifrado TDE con `MAXTRANSFERSIZE = 65536`, debe especificar `WITH NO_COMPRESSION` o asegurarse de que la configuración de servidor *predeterminada de compresión de copia de seguridad* esté establecida en 0.

> [!NOTE]
> Hay algunos casos en los que el `MAXTRANSFERSIZE` predeterminado es mayor que 64 KB:
>
> - Cuando la base de datos tiene varios archivos de datos creados, usa `MAXTRANSFERSIZE` > 64 KB.
> - Al realizar una copia de seguridad en una dirección URL, el `MAXTRANSFERSIZE = 1048576` predeterminado (1 MB)
>
> Incluso si se aplica alguna de estas condiciones, debe establecer explícitamente `MAXTRANSFERSIZE` mayor que 64 KB en el comando de copia de seguridad para poder obtener el algoritmo de compresión de copia de seguridad optimizado, a menos que esté utilizando [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU5 o una versión posterior.

De forma predeterminada, cada operación de copia de seguridad correcta agrega una entrada en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y en el registro de eventos del sistema. Si hace una copia de seguridad del registro de transacciones con frecuencia, estos mensajes que indican la corrección de la operación pueden acumularse rápidamente, con lo que se crean registros de errores muy grandes que pueden dificultar la búsqueda de otros mensajes. En esos casos, puede suprimir estas entradas de registro utilizando la marca de seguimiento 3226 si ninguno de los scripts depende de esas entradas. Para obtener más información, vea [Marcas de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

## <a name="interoperability"></a>Interoperabilidad

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza un proceso de copia de seguridad en línea para permitir que se realice la copia de seguridad de una base de datos mientras se está utilizando. Durante la copia de seguridad, se pueden realizar la mayoría de las operaciones (por ejemplo, las instrucciones INSERT, UPDATE o DELETE están permitidas durante la operación de copia de seguridad).

Las operaciones que no se pueden ejecutar durante la copia de seguridad de la base de datos o los registros de transacciones son:

- Operaciones de administración de archivos, como la instrucción `ALTER DATABASE` con las opciones `ADD FILE` o `REMOVE FILE`.

- Operaciones de reducción de la base de datos o de reducción de un archivo. Esto incluye las operaciones de reducción automática.

Si una operación de copia de seguridad se solapa con una operación de administración de archivos o de reducción, surge un conflicto. Con independencia de la operación en conflicto que empieza en primer lugar, la segunda operación espera a que se agote el tiempo de espera del bloqueo establecido por la primera operación (el tiempo de espera se controla mediante un valor de tiempo de espera de sesión). Si el bloqueo se libera durante el tiempo de espera, la segunda operación continúa. Si se agota el tiempo de espera del bloqueo, la segunda operación no se realiza correctamente.

## <a name="metadata"></a>Metadatos

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye las siguientes tablas del historial de copias de seguridad que realizan un seguimiento de la actividad de copia de seguridad:

- [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)
- [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)
- [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)
- [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)
- [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)

Al realizar una restauración, si el conjunto de copia de seguridad aún no se ha registrado en la base de datos **msdb**, las tablas del historial de copias de seguridad se podrían modificar.

## <a name="security"></a>Seguridad

A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], las opciones `PASSWORD` y `MEDIAPASSWORD` se suspenden para crear copias de seguridad. Todavía es posible restaurar copias de seguridad creadas con contraseñas.

### <a name="permissions"></a>Permisos

De forma predeterminada, los permisos BACKUP DATABASE y BACKUP LOG corresponden a los miembros del rol fijo de servidor **sysadmin** y de los roles fijos de base de datos **db_owner** y **db_backupoperator** .

Los problemas de propiedad y permisos del archivo físico del dispositivo de copia de seguridad pueden interferir con una operación de copia de seguridad. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe poder leer y escribir en el dispositivo y la cuenta en la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener permisos de escritura. En cambio, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), que agrega una entrada para un dispositivo de copia de seguridad en las tablas del sistema, no comprueba los permisos de acceso a los archivos. Es posible que estos problemas con el archivo físico del dispositivo de copia de seguridad no aparezcan hasta que se tenga acceso al recurso físico, al intentar la copia de seguridad o la restauración.

## <a name="examples"></a><a name="examples"></a> Ejemplos

En esta sección se incluyen los ejemplos siguientes:

- A. [Realizar una copia de seguridad completa de la base de datos](#backing_up_db)
- B. [Realizar una copia de seguridad de la base de datos y el registro](#backing_up_db_and_log)
- C. [Crear una copia de seguridad de archivos completa de los grupos de archivos secundarios](#full_file_backup)
- D. [Crear una copia de seguridad de archivos diferencial de los grupos de archivos secundarios](#differential_file_backup)
- E. [Crear y realizar una copia de seguridad en un conjunto de medios reflejado de una sola familia](#create_single_family_mirrored_media_set)
- F. [Crear y realizar una copia de seguridad en un conjunto de medios reflejado de varias familias](#create_multifamily_mirrored_media_set)
- G. [Realizar una copia de seguridad en un conjunto de medios reflejado existente](#existing_mirrored_media_set)
- H. [Crear una copia de seguridad comprimida en un nuevo conjunto de medios](#creating_compressed_backup_new_media_set)
- I. [Realizar una copia de seguridad del servicio Microsoft Azure Blob Storage](#url)
- J. [Hacer un seguimiento del progreso de la instrucción de copia de seguridad](#backup_progress)

> [!NOTE]
> Los temas de procedimientos de copia de seguridad contienen más ejemplos. Para obtener más información, vea [Información general de copia de seguridad](../../relational-databases/backup-restore/backup-overview-sql-server.md).

### <a name="a-backing-up-a-complete-database"></a><a name="backing_up_db"></a> A. Realizar una copia de seguridad completa de la base de datos

En el siguiente ejemplo se hace una copia de seguridad de la base de datos [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] en un archivo de disco.

```sql
BACKUP DATABASE AdventureWorks2012
 TO DISK = 'Z:\SQLServerBackups\AdvWorksData.bak'
    WITH FORMAT;
GO
```

### <a name="b-backing-up-the-database-and-log"></a><a name="backing_up_db_and_log"></a> B. Realizar una copia de seguridad de la base de datos y el registro

En el ejemplo siguiente se realiza la copia de seguridad de la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], que usa de forma predeterminada un modelo de recuperación simple. Para admitir las copias de seguridad del registro, la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] se ha modificado para usar el modelo de recuperación completa.

Después, en el ejemplo se usa [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md) para crear un [dispositivo de copia de seguridad](../../relational-databases/backup-restore/backup-devices-sql-server.md) lógico para realizar la copia de seguridad de datos, `AdvWorksData`, y se crea un dispositivo de copia de seguridad lógico para copiar el registro, `AdvWorksLog`.

A continuación, en el ejemplo se crea una copia de seguridad de base de datos completa en `AdvWorksData` y, tras un periodo de actividad de actualización, se copia el registro en `AdvWorksLog`.

```sql
-- To permit log backups, before the full database backup, modify the database
-- to use the full recovery model.
USE master;
GO
ALTER DATABASE AdventureWorks2012
    SET RECOVERY FULL;
GO
-- Create AdvWorksData and AdvWorksLog logical backup devices.
USE master
GO
EXEC sp_addumpdevice 'disk', 'AdvWorksData',
'Z:\SQLServerBackups\AdvWorksData.bak';
GO
EXEC sp_addumpdevice 'disk', 'AdvWorksLog',
'X:\SQLServerBackups\AdvWorksLog.bak';
GO

-- Back up the full AdventureWorks2012 database.
BACKUP DATABASE AdventureWorks2012 TO AdvWorksData;
GO
-- Back up the AdventureWorks2012 log.
BACKUP LOG AdventureWorks2012
    TO AdvWorksLog;
GO
```

> [!NOTE]
> En el caso de una base de datos de producción, haga una copia de seguridad del registro periódicamente. Las copias de seguridad del registro se deben realizar con una frecuencia suficiente para ofrecer la protección necesaria frente a la pérdida de datos.

### <a name="c-creating-a-full-file-backup-of-the-secondary-filegroups"></a><a name="full_file_backup"></a> C. Crear una copia de seguridad de archivos completa de los grupos de archivos secundarios

En el ejemplo siguiente se crea una copia de seguridad de archivos completa de cada archivo en los dos grupos de archivos secundarios.

```sql
--Back up the files in SalesGroup1:
BACKUP DATABASE Sales
    FILEGROUP = 'SalesGroup1',
    FILEGROUP = 'SalesGroup2'
    TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck';
GO
```

### <a name="d-creating-a-differential-file-backup-of-the-secondary-filegroups"></a><a name="differential_file_backup"></a> D. Crear una copia de seguridad de archivos diferencial de los grupos de archivos secundarios

En el ejemplo siguiente se crea una copia de seguridad de archivos diferencial de cada archivo en los dos grupos de archivos secundarios.

```sql
--Back up the files in SalesGroup1:
BACKUP DATABASE Sales
    FILEGROUP = 'SalesGroup1',
    FILEGROUP = 'SalesGroup2'
    TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck'
    WITH
      DIFFERENTIAL;
GO
```

### <a name="e-creating-and-backing-up-to-a-single-family-mirrored-media-set"></a><a name="create_single_family_mirrored_media_set"></a> E. Crear y realizar una copia de seguridad en un conjunto de medios reflejado de una sola familia

En el siguiente ejemplo se crea un conjunto de medios reflejado que contiene una sola familia de medios y cuatro reflejos, y se realiza una copia de seguridad de la base de datos [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] en ellos.

```sql
BACKUP DATABASE AdventureWorks2012
TO TAPE = '\\.\tape0'
MIRROR TO TAPE = '\\.\tape1'
MIRROR TO TAPE = '\\.\tape2'
MIRROR TO TAPE = '\\.\tape3'
WITH
    FORMAT,
    MEDIANAME = 'AdventureWorksSet0';
```

### <a name="f-creating-and-backing-up-to-a-multifamily-mirrored-media-set"></a><a name="create_multifamily_mirrored_media_set"></a> F. Crear y realizar una copia de seguridad en un conjunto de medios reflejado de varias familias

En el siguiente ejemplo se crea un conjunto de medios reflejado en el que cada reflejo consta de dos familias de medios. A continuación, se realiza una copia de seguridad de la base de datos [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] en ambos reflejos.

```sql
BACKUP DATABASE AdventureWorks2012
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'
WITH
    FORMAT,
    MEDIANAME = 'AdventureWorksSet1';
```

### <a name="g-backing-up-to-an-existing-mirrored-media-set"></a><a name="existing_mirrored_media_set"></a> G. Realizar una copia de seguridad en un conjunto de medios reflejado existente

En el siguiente ejemplo se anexa un conjunto de copia de seguridad al conjunto de medios creado en el ejemplo anterior.

```sql
BACKUP LOG AdventureWorks2012
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'
WITH
    NOINIT,
    MEDIANAME = 'AdventureWorksSet1';
```

> [!NOTE]
> NOINIT, que es el valor predeterminado, se muestra aquí para mayor claridad.

### <a name="h-creating-a-compressed-backup-in-a-new-media-set"></a><a name="creating_compressed_backup_new_media_set"></a> H. Crear una copia de seguridad comprimida en un nuevo conjunto de medios

En el ejemplo siguiente se da formato a los medios, creando un nuevo conjunto de medios, y se realiza una copia de seguridad completa comprimida de la base de datos [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)].

```sql
BACKUP DATABASE AdventureWorks2012 TO DISK='Z:\SQLServerBackups\AdvWorksData.bak'
WITH
    FORMAT,
    COMPRESSION;
```

### <a name="i-backing-up-to-the-microsoft-azure-blob-storage-service"></a><a name="url"></a> I. Realizar una copia de seguridad del servicio de almacenamiento de blobs de Microsoft Azure

En los tres ejemplos siguientes se hace una copia de seguridad completa de la base de datos `Sales` en el servicio Microsoft Azure Blob Storage. El nombre de la cuenta de almacenamiento es `mystorageaccount`. El contenedor se denomina `myfirstcontainer`. Se ha creado una directiva de acceso almacenada con derechos de lectura, escritura, eliminación y lista. La credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`, se creó con una Firma de acceso compartido asociada a la directiva de acceso almacenada. Para obtener información sobre la copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servicio Microsoft Azure Blob Storage, vea [Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) y [Copia de seguridad en URL de SQL Server](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

```sql
BACKUP DATABASE Sales
TO URL = 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_20160726.bak'
WITH STATS = 5;
```

### <a name="j-track-the-progress-of-backup-statement"></a><a name="backup_progress"></a> J. Hacer un seguimiento del progreso de la instrucción de copia de seguridad

La consulta siguiente devuelve información sobre las instrucciones de copia de seguridad actualmente en ejecución:
```sql
SELECT query = a.text, start_time, percent_complete,
    eta = dateadd(second,estimated_completion_time/1000, getdate())
FROM sys.dm_exec_requests r
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
WHERE r.command LIKE 'BACKUP%'
```

## <a name="see-also"></a>Consulte también

- [Dispositivos de copia de seguridad](../../relational-databases/backup-restore/backup-devices-sql-server.md)
- [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)
- [Copias del final del registro](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)
- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)
- [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)
- [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)
- [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)
- [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)
- [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)
- [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_helpfile](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)
- [sp_helpfilegroup](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)
- [Opciones de configuración del servidor](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Restauración por etapas de bases de datos con tablas con optimización para memoria](../../relational-databases/in-memory-oltp/piecemeal-restore-of-databases-with-memory-optimized-tables.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](backup-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

Realiza una copia de seguridad de una base de datos SQL colocada u hospedada en Azure SQL Managed Instance. SQL [Managed Instance](/azure/sql-database/sql-database-managed-instance) realiza copias de seguridad automáticas y permite a los usuarios crear copias de seguridad `COPY_ONLY` de bases de datos completas. No se admiten las copias de seguridad de instantáneas de archivos, de registros ni diferenciales.

## <a name="syntax"></a>Sintaxis

```syntaxsql
BACKUP DATABASE { database_name | @database_name_var }
  TO URL = { 'physical_device_name' | @physical_device_name_var }[ ,...n ]
  WITH COPY_ONLY [, { <general_WITH_options> } ]
[;]

<general_WITH_options> [ ,...n ]::=

--Media Set Options
   MEDIADESCRIPTION = { 'text' | @text_variable }
 | MEDIANAME = { media_name | @media_name_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
   BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
   { NO_CHECKSUM | CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Compatibility Options
   RESTART

--Monitoring Options
   STATS [ = percentage ]

--Encryption Options
 ENCRYPTION (ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY } , encryptor_options ) <encryptor_options> ::=
   SERVER CERTIFICATE = Encryptor_Name | SERVER ASYMMETRIC KEY = Encryptor_Name
```

## <a name="arguments"></a>Argumentos

DATABASE Especifica una copia de seguridad completa de la base de datos. Durante una copia de seguridad de una base de datos, Azure SQL Managed Instance realiza la copia de seguridad de una parte suficiente del registro de transacciones para producir una base de datos coherente cuando se restaure la base de datos.

> [!IMPORTANT]
> Una copia de seguridad de base de datos creada en una instancia administrada solo se puede restaurar en otra instancia de Azure SQL Managed Instance. No se puede restaurar en una instancia local de SQL Server (de la misma forma que no se puede restaurar una copia de seguridad de una base de datos de SQL Server 2016 en una instancia de SQL Server 2012).

Al restaurar una copia de seguridad creada por BACKUP DATABASE (una *copia de seguridad de datos*), se restaura la copia de seguridad completa. Para restaurar a partir de copias de seguridad automáticas de SQL Managed Instance, vea [Restauración de una base de datos en una instancia administrada](/azure/sql-database/sql-database-managed-instance-get-started-restore).

{ *database_name* |  **@** _database\_name\_var_ } Es la base de datos para la que se realiza la copia de seguridad de la base de datos completa. Si se proporciona como una variable ( **@** _database\_name\_var_), este nombre se puede especificar como una constante de cadena ( **@** _database\_name\_var_ **=** _database name_) o como una variable de tipo de datos de cadena de caracteres, excepto los tipos de datos **ntext** o **text**.

Para más información, vea [Copias de seguridad de archivos completas](../../relational-databases/backup-restore/full-file-backups-sql-server.md) y [Realizar copias de seguridad de archivos y grupos de archivos](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md).

TO URL

Especifica la dirección URL que se usará para la operación de copia de seguridad. El formato de las direcciones URL solo se usa para crear copias de seguridad en el servicio de almacenamiento Microsoft Azure.

> [!IMPORTANT]
> Para hacer una copia de seguridad en varios dispositivos cuando se hagan copias de seguridad en URL, debe usar tokens de firma de acceso compartido (SAS). Para ver ejemplos sobre cómo crear una Firma de acceso compartido, vea [Copia de seguridad en URL de SQL Server](../../relational-databases/backup-restore/sql-server-backup-to-url.md) y [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell) (Simplificación de la creación de credenciales de SQL con tokens de firmas de acceso compartido [SAS] en Almacenamiento de Azure con PowerShell).

*n* Es un marcador de posición que indica que se pueden especificar hasta 64 dispositivos de copia de seguridad en una lista separada por comas.

### <a name="with-optionsspecifies-options-to-be-used-with-a-backup-operation"></a>WITH Options Especifica las opciones que se van a utilizar con una operación de copia de seguridad.

CREDENTIAL Solo se usa al crear una copia de seguridad en el servicio Microsoft Azure Blob Storage.

ENCRYPTION Se utiliza para especificar el cifrado para una copia de seguridad. Puede especificar un algoritmo de cifrado para cifrar la copia de seguridad o especificar `NO_ENCRYPTION` para no hacer que la copia de seguridad se cifre. El cifrado es una práctica recomendada para ayudar a proteger los archivos de copia de seguridad. La lista de algoritmos que puede especificar son:

- `AES_128`
- `AES_192`
- `AES_256`
- `TRIPLE_DES_3KEY`
- `NO_ENCRYPTION`

Si elige cifrar, tendrá que especificar el sistema cifrado mediante las opciones de cifrado:

- `SERVER CERTIFICATE = <Encryptor_Name>`
- `SERVER ASYMMETRIC KEY = <Encryptor_Name>`

**Opciones de conjunto de copia de seguridad**

COPY_ONLY especifica que la copia de seguridad es una *copia de seguridad de solo copia*, lo que no afecta a la secuencia normal de copias de seguridad. Se crea una copia de seguridad de solo copia independientemente de las copias de seguridad automáticas de Azure SQL Database. Para obtener más información, vea [Copias de seguridad de solo copia](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).

{ COMPRESSION | NO_COMPRESSION } Especifica si la [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md) se realiza en esta copia de seguridad, lo que invalida la configuración predeterminada del servidor.

El comportamiento predeterminado es que no se realice la compresión de copia de seguridad. Este valor predeterminado se puede cambiar estableciendo la opción [valor predeterminado de compresión de copia de seguridad](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) de la configuración del servidor. Para más información sobre cómo ver el valor actual de esta opción, vea [Ver o cambiar las propiedades del servidor](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).

COMPRESSION Habilita de forma explícita la compresión de copia de seguridad.

NO_COMPRESSION Deshabilita de forma explícita la compresión de copia de seguridad.

DESCRIPTION **=** { **'** _text_ **'**  |  **@** _text\_variable_ } Especifica el texto de forma libre que describe el conjunto de copia de seguridad. La cadena puede tener un máximo de 255 caracteres.

NAME **=** { *backup_set_name* |  **@** _backup\_set\_var_ } Especifica el nombre del conjunto de copia de seguridad. Los nombres pueden tener un máximo de 128 caracteres. Si no se especifica NAME, está en blanco.

MEDIADESCRIPTION **=** { *text* | **@** _text\_variable_ } Especifica la descripción de texto de forma libre, con un máximo de 255 caracteres, del conjunto de medios.

MEDIANAME **=** { *media_name* |  **@** _media\_name\_variable_ } Especifica el nombre del medio para el conjunto completo de medios de copia de seguridad. El nombre del medio no puede tener más de 128 caracteres y, si se especifica `MEDIANAME`, debe coincidir con el nombre de medio especificado que ya existe en los volúmenes de copia de seguridad. Si no se especifica o se especifica la opción SKIP, no se realiza la comprobación del nombre del medio.

BLOCKSIZE **=** { *blocksize* |  **@** _blocksize\_variable_ } Especifica el tamaño de bloque físico, en bytes. Los tamaños admitidos son 512, 1024, 2048, 4096, 8192, 16384, 32768 y 65536 (64 KB) bytes. El valor predeterminado es 65536 para dispositivos de cinta y 512 para otros dispositivos. Normalmente, esta opción no es necesaria, ya que BACKUP selecciona automáticamente un tamaño de bloque apropiado para el dispositivo. La especificación explícita de un tamaño de bloque invalida la selección automática del tamaño de bloque.

**Opciones de transferencia de datos**

BUFFERCOUNT **=** { *buffercount* |  **@** _buffercount\_variable_ } Especifica el número total de búferes de E/S que se va a usar en la operación de copia de seguridad. Puede especificar cualquier entero positivo; no obstante, un número de búferes demasiado grande podría provocar errores de "memoria insuficiente" a causa de un espacio de direcciones virtuales inadecuado en el proceso Sqlservr.exe.

El espacio total usado por los búferes está determinado por: `BUFFERCOUNT * MAXTRANSFERSIZE`.

> [!NOTE]
> Para obtener información importante sobre cómo usar la opción `BUFFERCOUNT`, vea el blog [Incorrect BufferCount data transfer option can lead to OOM condition](/archive/blogs/sqlserverfaq/incorrect-buffercount-data-transfer-option-can-lead-to-oom-condition) (Una opción de transferencia de datos BufferCount incorrecta puede agotar la memoria).

MAXTRANSFERSIZE **=** { *maxtransfersize* | _**@** maxtransfersize\_variable_ } Especifica la unidad de transferencia mayor (en bytes) que se debe usar entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el medio de copia de seguridad. Los valores posibles son múltiplos de 65536 bytes (64 KB), hasta un máximo de 4194304 bytes (4 MB).

> [!NOTE]
> En el caso de las bases de datos con [Cifrado de datos transparente (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) habilitado que tienen un único archivo de datos, el valor predeterminado de `MAXTRANSFERSIZE` es 65536 (64 KB). En las bases de datos que no tienen cifrado TDE, el valor predeterminado de `MAXTRANSFERSIZE` es 1048576 (1 MB) cuando se usa la copia de seguridad en DISK y 65536 (64 KB) al usar VDI o TAPE.

**Operaciones de administración de errores**

Estas opciones permiten determinar si se habilitarán las sumas de comprobación de copia de seguridad para la operación de copia de seguridad y si ésta se detiene al encontrar un error.

{ **NO_CHECKSUM** | CHECKSUM } Controla si las sumas de comprobación de copia de seguridad están habilitadas.

NO_CHECKSUM Deshabilita de forma explícita la generación de sumas de comprobación de copia de seguridad (y la validación de sumas de comprobación de página). Este es el comportamiento predeterminado.

CHECKSUM Especifica que la operación de copia de seguridad comprueba en cada página si hay suma de comprobación y página rasgada (si está habilitada y disponible) y generará una suma de comprobación para toda la copia de seguridad.

El uso de sumas de comprobación de copia de seguridad puede afectar a la carga de trabajo y al rendimiento de la copia de seguridad.

Para más información, vea [Errores posibles de medios durante copia de seguridad y restauración](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

{ **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR } Controla si una operación de copia de seguridad se detiene o continúa después de encontrar un error en la suma de comprobación de página.

STOP_ON_ERROR Indica a BACKUP que dé error si no se comprueba una suma de comprobación de página. Este es el comportamiento predeterminado.

CONTINUE_AFTER_ERROR Indica a BACKUP que continúe a pesar de la detección de errores como sumas de comprobación no válidas o páginas rasgadas.

Si no puede hacer la copia de seguridad del final del registro con la opción NO_TRUNCATE cuando la base de datos está dañada, puede intentar realizar una [copia de seguridad de registros después del registro](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) especificando CONTINUE_AFTER_ERROR en vez de NO_TRUNCATE.

Para más información, vea [Errores posibles de medios durante copia de seguridad y restauración](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

**Opciones de compatibilidad**

RESTART no tiene ningún efecto. La versión acepta esta opción por compatibilidad con versiones anteriores de SQL Server.

**Opciones de supervisión**

STATS [ **=** _percentage_ ] Muestra un mensaje cada vez que se completa otro *percentage*; se usa para indicar el progreso. Si *percentage* se omite, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muestra un mensaje después de completar cada 10 por ciento.

La opción STATS informa del porcentaje completado desde el umbral para informar del próximo intervalo. Éste es aproximadamente el porcentaje especificado; por ejemplo, con STATS=10, si la cantidad completada equivale al 40 por ciento, la opción puede mostrar el 43 por ciento. En el caso de los conjuntos de copia de seguridad de gran tamaño, esto no representa ningún problema porque el porcentaje completado se mueve muy lentamente entre las llamadas de E/S.

## <a name="limitations-for-sql-managed-instance"></a>Limitaciones de SQL Managed Instance

El tamaño máximo de la franja de copia de seguridad es de 195 GB (tamaño máximo de blob). Aumente el número de franjas en el comando de copia de seguridad para reducir el tamaño de las franjas y permanecer dentro de este límite.

## <a name="security"></a>Seguridad

### <a name="permissions"></a>Permisos

De forma predeterminada, los permisos BACKUP DATABASE corresponden a los miembros del rol fijo de servidor **sysadmin** y de los roles fijos de base de datos **db_owner** y **db_backupoperator**.

Los problemas de propiedad y permisos de la dirección URL pueden interferir con una operación de copia de seguridad. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe poder leer y escribir en el dispositivo y la cuenta en la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener permisos de escritura.

## <a name="examples"></a><a name="examples"></a> Ejemplos

En los tres ejemplos siguientes se hace una copia de seguridad COPY_ONLY de `Sales` en el servicio Microsoft Azure Blob Storage. El nombre de la cuenta de almacenamiento es `mystorageaccount`. El contenedor se denomina `myfirstcontainer`. Se ha creado una directiva de acceso almacenada con derechos de lectura, escritura, eliminación y lista. La credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`, se creó con una Firma de acceso compartido asociada a la directiva de acceso almacenada. Para obtener información sobre la copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servicio Microsoft Azure Blob Storage, vea [Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) y [Copia de seguridad en URL de SQL Server](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

```sql
BACKUP DATABASE Sales
TO URL = 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_20160726.bak'
WITH STATS = 5, COPY_ONLY;
```

## <a name="see-also"></a>Consulte también

[Restaurar base de datos](restore-statements-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](backup-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](backup-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Sistema de la plataforma de análisis

Crea una copia de seguridad de una base de datos de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] y la almacena fuera del dispositivo en una ubicación de red especificada por el usuario. Use esta instrucción con [RESTORE DATABASE - Analytics Platform System](../../t-sql/statements/restore-statements-transact-sql.md) para la recuperación ante desastres, o bien para copiar una base de datos de un dispositivo a otro.

**Antes de comenzar**, vea "Adquirir y configurar un servidor de copia de seguridad" en la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

En [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] hay dos tipos de copias de seguridad. Una *copia de seguridad completa de base de datos* es una copia de seguridad de una base de datos de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] completa. Una *copia de seguridad diferencial* solo incluye los cambios realizados desde la última copia de seguridad completa de base de datos. Una copia de seguridad de una base de datos de usuario incluye usuarios de base de datos y roles de base de datos. Una copia de seguridad de la base de datos maestra incluye los inicios de sesión.

Para más información sobre las copias de seguridad de bases de datos de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], vea "Copia de seguridad y restauración" en la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

## <a name="syntax"></a>Sintaxis

```syntaxsql
--Create a full backup of a user database or the master database.
BACKUP DATABASE database_name
    TO DISK = '\\UNC_path\backup_directory'
    [ WITH [ ( ]<with_options> [ ,...n ][ ) ] ]
[;]

--Create a differential backup of a user database.
BACKUP DATABASE database_name
    TO DISK = '\\UNC_path\backup_directory'
    WITH [ ( ] DIFFERENTIAL
    [ , <with_options> [ ,...n ] [ ) ]
[;]

<with_options> ::=
    DESCRIPTION = 'text'
    | NAME = 'backup_name'
```

## <a name="arguments"></a>Argumentos

*database_name* Nombre de la base de datos en la que se va a crear una copia de seguridad. La base de datos puede ser la base de datos maestra o una base de datos de usuario.

TO DISK = '\\\\*UNC_path*\\*backup_directory*' La ruta de acceso de red y el directorio en el que [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] escribirá los archivos de copia de seguridad. Por ejemplo, "\\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup".

- La ruta de acceso al nombre del directorio de copia de seguridad ya debe existir y se debe especificar como una ruta de acceso de convención de nomenclatura universal (UNC) completa.
- El directorio de copia de seguridad, *directorio_de_copia_de_seguridad*, no debe existir antes de ejecutar el comando de copia de seguridad. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] creará el directorio de copia de seguridad.
- La ruta de acceso al directorio de copia de seguridad no puede ser una ruta de acceso local y no puede ser una ubicación en ninguno de los nodos del dispositivo de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
- La longitud máxima de la ruta de acceso UNC y el nombre del directorio de copia de seguridad es de 200 caracteres.
- El servidor o host se debe especificar como una dirección IP. No se puede especificar como el nombre de host o servidor.

DESCRIPTION = **'** _text_ **'** Especifica una descripción textual de la copia de seguridad. La longitud máxima del texto es 255 caracteres.

La descripción se almacena en los metadatos y se mostrará cuando se restaure el encabezado de copia de seguridad con RESTORE HEADERONLY.

NAME = **'** _backup \_name_ **'** Especifica el nombre de la copia de seguridad. El nombre de la copia de seguridad puede ser distinto del nombre de la base de datos.

- Los nombres pueden tener un máximo de 128 caracteres.
- No puede incluir una ruta de acceso.
- Debe comenzar con una letra o un carácter numérico, o bien un carácter de subrayado (_). Los caracteres especiales permitidos son el carácter de subrayado (\_), guión (-) o espacio ( ). Los nombres de copia de seguridad no pueden terminar con un carácter de espacio.
- Se producirá un error en la instrucción si *backup_name* ya existe en la ubicación especificada.

Este nombre se almacena en los metadatos y se mostrará cuando se restaure el encabezado de copia de seguridad con RESTORE HEADERONLY.

DIFFERENTIAL Especifica que se realice una copia de seguridad diferencial de una base de datos de usuario. Si se omite, el valor predeterminado es una copia de seguridad completa de base de datos. No es necesario que el nombre de la copia de seguridad diferencial coincida con el nombre de la copia de seguridad completa. Para realizar el seguimiento de la copia diferencial y su copia de seguridad completa correspondiente, considere la posibilidad de usar el mismo nombre con "completa" o "diferencial" anexado.

Por ejemplo:

`BACKUP DATABASE Customer TO DISK = '\\xxx.xxx.xxx.xxx\backups\CustomerFull';`

`BACKUP DATABASE Customer TO DISK = '\\xxx.xxx.xxx.xxx\backups\CustomerDiff' WITH DIFFERENTIAL;`

## <a name="permissions"></a>Permisos

Requiere el permiso **BACKUP DATABASE** o la pertenencia al rol fijo de base de datos **db_backupoperator**. Un usuario normal que se haya agregado al rol fijo de base de datos **db_backupoperator** no puede hacer una copia de seguridad de la base de datos maestra. Solo **sa**, el administrador del tejido o los miembros del rol fijo de servidor **sysadmin** pueden hacer una copia de seguridad de la base de datos maestra.

Requiere una cuenta de Windows que tenga permiso para obtener acceso, crear y escribir en el directorio de copia de seguridad. También se debe almacenar el nombre de la cuenta de Windows y la contraseña en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Para agregar estas credenciales de red a [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], use el procedimiento almacenado [sp_pdw_add_network_credentials - [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).

Para obtener más información sobre cómo administrar las credenciales en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], vea la sección [Seguridad](#Security).

## <a name="error-handling"></a>Tratamiento de errores

Errores de BACKUP DATABASE en las siguientes circunstancias:

- Los permisos de usuario no son suficientes para realizar una copia de seguridad.
- [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] no tiene los permisos correctos a la ubicación de red donde se almacenará la copia de seguridad.
- La base de datos no existe.
- El directorio de destino ya existe en el recurso compartido de red.
- El recurso compartido de red de destino no está disponible.
- El recurso compartido de red de destino no tiene suficiente espacio para la copia de seguridad. El comando BACKUP DATABASE no confirma que existe suficiente espacio en disco antes de iniciar la copia de seguridad, lo que permite generar un error de espacio en disco insuficiente mientras se ejecuta BACKUP DATABASE. Cuando se produce el error de espacio en disco insuficiente, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] revierte el comando BACKUP DATABASE. Para reducir el tamaño de la base de datos, ejecute [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md).
- Se intenta iniciar una copia de seguridad dentro de una transacción.

::: moniker-end
::: moniker range=">=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017"
## <a name="general-remarks"></a>Notas generales

Antes de realizar una copia de seguridad de la base de datos, use [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) para reducir el tamaño de la base de datos.

Una copia de seguridad de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] se almacena como un conjunto de varios archivos en el mismo directorio.

Una copia de seguridad diferencial normalmente tarda menos tiempo que una copia de seguridad completa y se puede realizar con más frecuencia. Cuando varias copias de seguridad diferenciales se basan en la misma copia de seguridad completa, cada copia diferencial incluye todos los cambios de la copia de seguridad diferencial anterior.

Si se cancela un comando BACKUP, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] quitará el directorio de destino y todos los archivos creados para la copia de seguridad. Si [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] pierde conectividad de red para el recurso compartido, la reversión no se puede completar.

Las copias de seguridad completas y diferenciales se almacenan en directorios independientes. No se aplican las convenciones de nomenclatura para especificar que una copia de seguridad completa y una copia de seguridad diferencial forman un conjunto. Puede realizar el seguimiento de esto a través de sus propias convenciones de nomenclatura. Como alternativa, puede controlar esto mediante la opción WITH DESCRIPTION para agregar una descripción y, después, mediante la instrucción RESTORE HEADERONLY para recuperar la descripción.

::: moniker-end
::: moniker range=">=aps-pdw-2016"
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

No se puede realizar una copia de seguridad diferencial de la base de datos maestra. Solo se admiten las copias de seguridad completas de la base de datos maestra.

Los archivos de copia de seguridad se almacenan en un formato adecuado únicamente para restaurar la copia de seguridad en un dispositivo de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] mediante la instrucción [RESTORE DATABASE - Analytics Platform System](../../t-sql/statements/restore-statements-transact-sql.md).

No se puede usar la copia de seguridad con la instrucción BACKUP DATABASE para transferir datos o información del usuario a bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de SMP. Para esa funcionalidad, se puede usar la característica de copia de la tabla remota. Para más información, vea "Copia de la tabla remota" en la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

En [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] se usa la tecnología de copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para realizar copias de seguridad y restaurar bases de datos. Las opciones de copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] están preconfiguradas para usar la compresión de copia de seguridad. No se pueden establecer opciones de copia de seguridad como la compresión, suma de comprobación, tamaño de bloque y recuento de búferes.

Solo se puede ejecutar una copia de seguridad o restauración de base de datos en el dispositivo en un momento dado. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] pondrá en cola los comandos de copia de seguridad o restauración hasta que se haya completado el comando actual de copia de seguridad o restauración.

El dispositivo de destino para restaurar la copia de seguridad debe tener al menos tantos nodos de ejecución como el dispositivo de origen. El destino puede tener más nodos de ejecución que el dispositivo de origen, pero no menos.

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] no realiza el seguimiento de la ubicación y los nombres de las copias de seguridad, ya que las copias de seguridad se almacenan fuera del dispositivo.

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] realiza el seguimiento del éxito o el fracaso de las copias de seguridad de base de datos.

Solo se permite una copia de seguridad diferencial si la última copia de seguridad completa se realizó correctamente. Por ejemplo, suponga que el lunes crea una copia de seguridad completa de la base de datos de ventas y la copia de seguridad finaliza correctamente. Después, el martes crea una copia de seguridad completa de la base de datos de ventas y se produce un error. Después de este error, no puede crear una copia de seguridad diferencial basada en la copia de seguridad completa del lunes. Primero debe crear una copia de seguridad completa correcta antes de crear una copia de seguridad diferencial.

## <a name="metadata"></a>Metadatos

Estas vistas de administración dinámicas contienen información sobre todas operaciones de copia de seguridad, restauración y carga. La información se conserva entre reinicios del sistema.

- [sys.pdw_loader_backup_runs](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-runs-transact-sql.md)
- [sys.pdw_loader_backup_run_details](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-run-details-transact-sql.md)
- [sys.pdw_loader_run_stages](../../relational-databases/system-catalog-views/sys-pdw-loader-run-stages-transact-sql.md)

## <a name="performance"></a>Rendimiento

Para realizar una copia de seguridad, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] primero crea una copia de seguridad de los metadatos y, después, realiza una copia de seguridad paralela de los datos de base de datos almacenados en los nodos de ejecución. Los datos se copian directamente desde cada nodo de ejecución al directorio de copia de seguridad. Para obtener el mejor rendimiento para mover los datos de los nodos de ejecución al directorio de copia de seguridad, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] controla el número de nodos de ejecución en los que se copian datos al mismo tiempo.

## <a name="locking"></a>Bloqueo

Toma un bloqueo ExclusiveUpdate en el objeto de base de datos.

## <a name="security"></a><a name="Security"></a> Seguridad

Las copias de seguridad de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] no se almacenan en el dispositivo. Por tanto, el equipo de TI es responsable de administrar todos los aspectos de la seguridad de las copias de seguridad. Por ejemplo, esto incluye la administración de la seguridad de los datos de copia de seguridad, la del servidor que se usa para almacenar las copias de seguridad y la de la infraestructura de red que conecta el servidor de copia de seguridad con el dispositivo de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

**Administrar las credenciales de red**

El acceso de red al directorio de copia de seguridad se basa en la seguridad del uso compartido de archivos del sistema operativo estándar. Antes de realizar una copia de seguridad, debe crear o designar una cuenta de Windows que se usó para la autenticación de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] en el directorio de copia de seguridad. Esta cuenta de Windows debe tener permiso para obtener acceso, crear y escribir en el directorio de copia de seguridad.

> [!IMPORTANT]
> Para reducir los riesgos de seguridad con los datos, se recomienda designar una cuenta de Windows exclusivamente para realizar las operaciones de copia de seguridad y restauración. Permita que esta cuenta tenga permisos para la ubicación de la copia de seguridad y ningún otro lugar más.

Debe almacenar el nombre de usuario y la contraseña en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] mediante la ejecución del procedimiento almacenado [sp_pdw_add_network_credentials - [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md). [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] usa el Administrador de credenciales de Windows para almacenar y cifrar los nombres de usuario y las contraseñas en el nodo de control y los nodos de ejecución. No se realiza una copia de seguridad de las credenciales con el comando BACKUP DATABASE.

Para quitar las credenciales de red de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], vea [sp_pdw_remove_network_credentials - [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md).

Para enumerar todas las credenciales de red almacenadas en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], use la vista de administración dinámica [sys.dm_pdw_network_credentials](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).

## <a name="examples"></a>Ejemplos

### <a name="a-add-network-credentials-for-the-backup-location"></a>A. Agregar credenciales de red para la ubicación de copia de seguridad

Para crear una copia de seguridad, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] debe tener permiso de lectura/escritura en el directorio de copia de seguridad. En el ejemplo siguiente se muestra cómo agregar las credenciales de un usuario. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] almacenará estas credenciales y las usará para las operaciones de copias de seguridad y restauración.

> [!IMPORTANT]
> Por motivos de seguridad, se recomienda crear una cuenta de dominio exclusivamente para realizar copias de seguridad.

```sql
EXEC sp_pdw_add_network_credentials 'xxx.xxx.xxx.xxx', 'domain1\backupuser', '*****';
```

### <a name="b-remove-network-credentials-for-the-backup-location"></a>B. Quitar credenciales de red para la ubicación de copia de seguridad

En el ejemplo siguiente se muestra cómo quitar las credenciales de un usuario de dominio de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

```sql
EXEC sp_pdw_remove_network_credentials 'xxx.xxx.xxx.xxx';
```

### <a name="c-create-a-full-backup-of-a-user-database"></a>C. Creación de una copia de seguridad completa de una base de datos de usuario

En el ejemplo siguiente se crea una copia de seguridad completa de la base de datos de usuario Invoices. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] creará el directorio Invoices2013 y guardará los archivos de copia de seguridad en el directorio \\\10.192.63.147\backups\yearly\Invoices2013Full.

```sql
BACKUP DATABASE Invoices TO DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';
```

### <a name="d-create-a-differential-backup-of-a-user-database"></a>D. Crear una copia de seguridad diferencial de una base de datos de usuario

En el ejemplo siguiente se crea una copia de seguridad diferencial, que incluye todos los cambios realizados desde la última copia de seguridad completa de la base de datos Invoices. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] creará el directorio \\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff en el que almacenará los archivos. La descripción "Invoices 2013 differential backup" (Copia de seguridad diferencial de Invoices 2013) se almacenará con la información de encabezado de la copia de seguridad.

La copia de seguridad diferencial solo se ejecutará correctamente si la última copia de seguridad completa de Invoices se completó correctamente.

```sql
BACKUP DATABASE Invoices TO DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'
    WITH DIFFERENTIAL,
    DESCRIPTION = 'Invoices 2013 differential backup';
```

### <a name="e-create-a-full-backup-of-the-master-database"></a>E. Crear una copia de seguridad completa de la base de datos maestra

En el ejemplo siguiente se crea una copia de seguridad completa de la base de datos maestra y se almacena en el directorio "\\\10.192.63.147\backups\2013\daily\20130722\master".

```sql
BACKUP DATABASE master TO DISK = '\\xxx.xxx.xxx.xxx\backups\2013\daily\20130722\master';
```

### <a name="f-create-a-backup-of-appliance-login-information"></a>F. Crear una copia de seguridad de la información de inicio de sesión del dispositivo

En la base de datos maestra se almacena la información de inicio de sesión del dispositivo. Para realizar una copia de seguridad de la información de inicio de sesión del dispositivo es necesario realizar una copia de seguridad de la base de datos maestra.

En el ejemplo siguiente se crea una copia de seguridad completa de la base de datos maestra.

```sql
BACKUP DATABASE master TO DISK = '\\xxx.xxx.xxx.xxx\backups\2013\daily\20130722\master'
WITH (
    DESCRIPTION = 'Master Backup 20130722',
    NAME = 'login-backup'
)
;
```

## <a name="see-also"></a>Consulte también

[RESTORE DATABASE - Parallel Data Warehouse](../../t-sql/statements/restore-statements-transact-sql.md)

::: moniker-end
