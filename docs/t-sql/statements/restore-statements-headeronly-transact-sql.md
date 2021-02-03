---
description: 'Instrucciones RESTORE: HEADERONLY (Transact-SQL)'
title: RESTORE HEADERONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HEADERONLY
- RESTORE HEADERONLY
- RESTORE_HEADERONLY_TSQL
- HEADERONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup sets [SQL Server], header information
- headers [SQL Server]
- RESTORE HEADERONLY statement
- backup header information [SQL Server]
ms.assetid: 4b88e98c-49c4-4388-ab0e-476cc956977c
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: a270c24ee685e6e04fa999737828caf9c9559475
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182979"
---
# <a name="restore-statements---headeronly-transact-sql"></a>Instrucciones RESTORE: HEADERONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Devuelve un conjunto de resultados que contiene toda la información de encabezado de copia de seguridad de todos los conjuntos de copia de seguridad de un dispositivo de copia de seguridad determinado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
 
> [!NOTE]  
>  Para obtener las descripciones de los argumentos, vea [Argumentos de RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
RESTORE HEADERONLY   
FROM <backup_device>   
[ WITH   
 {  
--Backup Set Options  
   FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Tape Options  
 | { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }    
 } [ ,...n ]  
]  
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | { DISK | TAPE | URL } = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}  
  
```  
> [!NOTE] 
> URL es el formato que se usa para especificar la ubicación y el nombre del archivo para Microsoft Azure Blob Storage y se admite a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2. Aunque Microsoft Azure Storage es un servicio, la implementación es similar al disco y la cinta para permitir una experiencia de restauración coherente y sin problemas para los tres dispositivos.

## <a name="arguments"></a>Argumentos  
 Para obtener las descripciones de los argumentos RESTORE HEADERONLY, vea [RESTORE Arguments &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md) (Argumentos de RESTORE [Transact-SQL]).  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Por cada copia de seguridad que hay en un dispositivo determinado, el servidor envía una fila de información de encabezado con las siguientes columnas:  
  
> [!NOTE]
>  RESTORE HEADERONLY consulta todos los conjuntos de copia de seguridad en los medios. Por tanto, puede llevar algún tiempo generar este conjunto de resultados si se utilizan unidades de cinta de alta capacidad. Para tener una visión rápida de los medios sin obtener información sobre cada conjunto de copia de seguridad, use RESTORE LABELONLY o especifique FILE **=** _backup_set_file_number_.  
> 
> [!NOTE]
>  Debido a la naturaleza del formato de cinta de [!INCLUDE[msCoName](../../includes/msconame-md.md)], es posible que los conjuntos de copia de seguridad de otros programas de software ocupen espacio en los mismos medios que los conjuntos de copia de seguridad de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El conjunto de resultados que devuelve RESTORE HEADERONLY contiene una fila por cada uno de estos otros conjuntos de copia de seguridad.  
  
|Nombre de la columna|Tipo de datos|Descripción de los conjuntos de copia de seguridad de SQL Server|  
|-----------------|---------------|--------------------------------------------|  
|**BackupName**|**nvarchar(128)**|Nombre del conjunto de copia de seguridad.|  
|**BackupDescription**|**nvarchar(255)**|Descripción del conjunto de copia de seguridad.|  
|**BackupType**|**smallint**|Tipo de copia de seguridad:<br /><br /> **1** = Base de datos<br /><br /> **2** = Registro de transacciones<br /><br /> **4** = Archivo<br /><br /> **5** = Base de datos diferencial<br /><br /> **6** = Archivo diferencial<br /><br /> **7** = Parcial<br /><br /> **8** = Parcial diferencial|  
|**ExpirationDate**|**datetime**|Fecha de expiración del conjunto de copia de seguridad.|  
|**Compressed**|**BIT(1)**|Si el conjunto de copia de seguridad se comprime con el sistema de compresión por software:<br /><br /> **0** = No<br /><br /> **1** = Sí|  
|**Posición**|**smallint**|Posición del conjunto de copia de seguridad en el volumen (para utilizarlo con la opción FILE =).|  
|**DeviceType**|**tinyint**|Número correspondiente al dispositivo utilizado para la operación de copia de seguridad.<br /><br /> Disco:<br /><br /> **2** = Lógico<br /><br /> **102** = Físico<br /><br /> Cinta:<br /><br /> **5** = Lógico<br /><br /> **105** = Físico<br /><br /> Dispositivo virtual:<br /><br /> **7** = Lógico<br /><br /> **107** = Físico<br /><br /> URL<br /><br /> **9** = Lógico<br /><br /> **109** = Físico<br /><br />  Los nombres de dispositivos lógicos y los números de dispositivo se encuentran en **sys.backup_devices**; para más información, vea [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md).|  
|**UserName**|**nvarchar(128)**|Nombre del usuario que ha ejecutado la operación de copia de seguridad.|  
|**ServerName**|**nvarchar(128)**|Nombre del servidor que escribió el conjunto de copia de seguridad.|  
|**DatabaseName**|**nvarchar(128)**|Nombre de la base de datos de la que se realizó la copia de seguridad.|  
|**DatabaseVersion**|**int**|Versión de la base de datos de la que se creó la copia de seguridad.|  
|**DatabaseCreationDate**|**datetime**|Fecha y hora en que se creó la base de datos.|  
|**BackupSize**|**numeric(20,0)**|Tamaño de la copia de seguridad, en bytes.|  
|**FirstLSN**|**numeric(25,0)**|Número de secuencia de registro de la primera entrada del registro del conjunto de copia de seguridad.|  
|**LastLSN**|**numeric(25,0)**|Número de secuencia de registro de la siguiente entrada del registro después del conjunto de copia de seguridad.|  
|**CheckpointLSN**|**numeric(25,0)**|Número de flujo de registro del punto de comprobación más reciente en el momento en que se creó la copia de seguridad.|  
|**DatabaseBackupLSN**|**numeric(25,0)**|Número de secuencia de registro de la copia de seguridad completa más reciente de la base de datos.<br /><br /> **DatabaseBackupLSN** es el “inicio del punto de control” que se desencadena cuando se inicia la copia de seguridad. Este LSN coincide con **FirstLSN** si la copia de seguridad se realiza cuando la base de datos está inactiva y no está configurada la replicación.|  
|**BackupStartDate**|**datetime**|Fecha y hora en que comenzó la operación de copia de seguridad.|  
|**BackupFinishDate**|**datetime**|Fecha y hora en que terminó la operación de copia de seguridad.|  
|**SortOrder**|**smallint**|Criterio de ordenación del servidor. Esta columna solo es válida para copias de seguridad de bases de datos. Se proporciona para mantener la compatibilidad con versiones anteriores.|  
|**CodePage**|**smallint**|Página de códigos del servidor o juego de caracteres utilizado por el servidor.|  
|**UnicodeLocaleId**|**int**|Opción de configuración de Id. de configuración regional Unicode del servidor utilizada para ordenar datos de caracteres Unicode. Se proporciona para mantener la compatibilidad con versiones anteriores.|  
|**UnicodeComparisonStyle**|**int**|Opción de configuración para el estilo de comparación Unicode del servidor, que proporciona control adicional sobre el orden de los datos Unicode. Se proporciona para mantener la compatibilidad con versiones anteriores.|  
|**CompatibilityLevel**|**tinyint**|Configuración del nivel de compatibilidad de la base de datos de la que se creó la copia de seguridad.|  
|**SoftwareVendorId**|**int**|Número de identificación del proveedor de software. En SQL Server, este número es **4608** (o su equivalente hexadecimal **0x1200**).|  
|**SoftwareVersionMajor**|**int**|Número de versión principal del servidor donde se creó el conjunto de copia de seguridad.|  
|**SoftwareVersionMinor**|**int**|Número de versión secundario del servidor donde se creó el conjunto de copia de seguridad.|  
|**SoftwareVersionBuild**|**int**|Número de compilación del servidor donde se creó el conjunto de copia de seguridad.|  
|**MachineName**|**nvarchar(128)**|Nombre del equipo donde se realizó la operación de copia de seguridad.|  
|**Marcas**|**int**|Significados de los bits de marcas individuales si se establece en **1**:<br /><br /> **1** = La copia de seguridad de registros contiene registros de operaciones masivas.<br /><br /> **2** = Copia de seguridad de instantánea.<br /><br /> **4** = La base de datos era de solo lectura en el momento de la copia de seguridad.<br /><br /> **8** = La base de datos estaba en modo de usuario único en el momento de la copia de seguridad.<br /><br /> **16** = La copia de seguridad contiene sumas de comprobación de copia de seguridad.<br /><br /> **32** = La base de datos estaba dañada cuando se realizó la copia de seguridad, pero se solicitó que continuase a pesar de los errores.<br /><br /> **64** = Copia del final del registro.<br /><br /> **128** = Copia del final del registro con metadatos incompletos.<br /><br /> **256** = Copia del final del registro con NORECOVERY.<br /><br /> **Importante:** Se recomienda que, en lugar de **Flags**, use las columnas booleanas individuales que se enumeran a continuación, desde **HasBulkLoggedData** hasta **IsCopyOnly**.|  
|**BindingID**|**uniqueidentifier**|Id. de enlace de la base de datos. Corresponde a **sys.database_recovery_status database_guid**. Cuando se restaura una base de datos, se asigna un valor nuevo. Vea también **FamilyGUID** (abajo).|  
|**RecoveryForkID**|**uniqueidentifier**|Id. de la bifurcación de recuperación final. Esta columna corresponde a **last_recovery_fork_guid** en la tabla [backupset](../../relational-databases/system-tables/backupset-transact-sql.md).<br /><br /> En las copias de seguridad de datos, **RecoveryForkID** es igual que **FirstRecoveryForkID**.|  
|**Intercalación**|**nvarchar(128)**|Intercalación que utiliza la base de datos.|  
|**FamilyGUID**|**uniqueidentifier**|Id. de la base de datos original cuando se creó. Este valor permanece invariable cuando se restaura la base de datos.|  
|**HasBulkLoggedData**|**bit**|**1** = Copia del registro que contiene registros de operaciones masivas.|  
|**IsSnapshot**|**bit**|**1** = Copia de seguridad de instantánea.|  
|**IsReadOnly**|**bit**|**1** = La base de datos era de solo lectura en el momento de la copia de seguridad.|  
|**IsSingleUser**|**bit**|**1** = La base de datos era de un solo usuario en el momento de la copia de seguridad.|  
|**HasBackupChecksums**|**bit**|**1** = La copia de seguridad contiene sumas de comprobación de copia de seguridad.|  
|**IsDamaged**|**bit**|**1** = La base de datos estaba dañada cuando se realizó la copia de seguridad, pero se solicitó que continuase a pesar de los errores.|  
|**BeginsLogChain**|**bit**|**1** = Es el primer elemento de una cadena continua de copias de seguridad de registros. Una cadena de registro empieza por la primera copia de seguridad de registros realizada después de crear la base de datos o cuando se cambia del modelo de recuperación simple al completo o al modelo de recuperación optimizado para cargas masivas de registros.|  
|**HasIncompleteMetaData**|**bit**|**1** = Copia del final del registro con metadatos incompletos.<br /><br /> Para más información sobre las copias del final del registro con metadatos incompletos, vea [Copias del final del registro &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).|  
|**IsForceOffline**|**bit**|**1** = Copia de seguridad realizada con NORECOVERY; el proceso de copia de seguridad dejó la base de datos sin conexión.|  
|**IsCopyOnly**|**bit**|**1** = Copia de seguridad de solo copia.<br /><br /> Una copia de seguridad de solo copia no afecta a los procedimientos de copias de seguridad y restauración generales de la base de datos. Para obtener más información, vea [Copias de seguridad de solo copia &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).|  
|**FirstRecoveryForkID**|**uniqueidentifier**|Id. de la bifurcación de recuperación inicial. Esta columna corresponde a **first_recovery_fork_guid** en la tabla [backupset](../../relational-databases/system-tables/backupset-transact-sql.md).<br /><br /> En las copias de seguridad de datos, **FirstRecoveryForkID** es igual que **RecoveryForkID**.|  
|**ForkPointLSN**|**numeric(25,0)** NULL|Si **FirstRecoveryForkID** no es igual que **RecoveryForkID**, este es el número de secuencia de registro del punto de bifurcación. De lo contrario, este valor es NULL.|  
|**RecoveryModel**|**nvarchar(60)**|Modelo de recuperación de la base de datos; uno de los siguientes valores:<br /><br /> FULL<br /><br /> BULK-LOGGED<br /><br /> SIMPLE|  
|**DifferentialBaseLSN**|**numeric(25,0)** NULL|Para una copia de seguridad diferencial con una única copia de seguridad base, el valor es igual al **FirstLSN** de la base diferencial; los cambios con LSN superiores o iguales a **DifferentialBaseLSN** se incluyen en la copia diferencial.<br /><br /> Para una copia de seguridad diferencial con varias copias de seguridad base, el valor es NULL y el LSN de la copia de seguridad base debe determinarse en el nivel de archivo. Para obtener más información, vea [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).<br /><br /> Para los tipos de copia de seguridad no diferenciales, el valor es siempre NULL.<br /><br /> Para obtener más información, vea [Copias de seguridad diferenciales &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).|  
|**DifferentialBaseGUID**|**uniqueidentifier**|Para una copia de seguridad diferencial con una única copia de seguridad base, el valor es el identificador único de la base diferencial.<br /><br /> Para las copias de seguridad diferenciales con varias copias de seguridad base, el valor es NULL y la base diferencial debe determinarse por archivo.<br /><br /> Para los tipos de copia de seguridad no diferenciales, el valor es NULL.|  
|**BackupTypeDescription**|**nvarchar(60)**|Tipo de copia de seguridad como cadena, uno de los siguientes valores:<br /><br /> DATABASE<br /><br /> TRANSACTION LOG<br /><br /> FILE OR FILEGROUP<br /><br /> DATABASE DIFFERENTIAL<br /><br /> FILE DIFFERENTIAL PARTIAL<br /><br /> PARTIAL DIFFERENTIAL|  
|**BackupSetGUID**|**uniqueidentifier** NULL|Número de identificación único del conjunto de copia de seguridad mediante el cual se identifica en los medios.|  
|**CompressedBackupSize**|**bigint**|Recuento de bytes del conjunto de copia de seguridad. Para las copias de seguridad sin comprimir, este valor es igual que **BackupSize**.<br /><br /> Para calcular la razón de compresión, use **CompressedBackupSize** y **BackupSize**.<br /><br /> Durante una actualización de **msdb**, se establece este valor para que coincida con el valor de la columna **BackupSize**.|  
|**containment**|**tinyint** not NULL|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Indica el estado de contención de la base de datos.<br /><br /> 0 = el estado de contención de la base de datos es OFF<br /><br /> 1 = la base de datos está en estado de contención parcial|  
|**KeyAlgorithm**|**nvarchar(32)**|**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actualización acumulativa 1 de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a la versión actual.<br /><br /> Algoritmo de cifrado usado para cifrar la copia de seguridad. NO_Encryption indica que no se cifró la copia de seguridad. Cuando no se puede determinar el valor correcto, el valor debe ser NULL.|  
|**EncryptorThumbprint**|**varbinary(20)**|**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actualización acumulativa 1 de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a la versión actual.<br /><br /> La huella digital del sistema de cifrado que se puede utilizar para encontrar el certificado o la clave asimétrica en la base de datos. Si la copia de seguridad no se cifró, este valor es NULL.|  
|**EncryptorType**|**nvarchar(32)**|**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actualización acumulativa 1 de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a la versión actual.<br /><br /> Tipo de sistema de cifrado usado: certificado o clave asimétrica. Si la copia de seguridad no se cifró, este valor es NULL.|  
  
> [!NOTE]  
>  Si se definen contraseñas para los conjuntos de copia de seguridad, RESTORE HEADERONLY solo muestra información completa para el conjunto de copia de seguridad cuya contraseña coincida con la opción PASSWORD especificada en el comando. RESTORE HEADERONLY también muestra información completa para los conjuntos de copia de seguridad no protegidos. La columna **BackupName** de los otros conjuntos de copia de seguridad protegidos con contraseña que hay en los medios se establece en **_Password Protected_** (Protegido con contraseña) y todas las demás columnas son NULL.  
  
## <a name="general-remarks"></a>Notas generales  
 Un cliente puede utilizar RESTORE HEADERONLY para obtener toda la información de encabezado de todas las copias de seguridad de un dispositivo determinado. Para cada copia de seguridad del dispositivo de copia de seguridad, el servidor envía la información del encabezado como una fila.  
  
## <a name="security"></a>Seguridad  
 La operación de copia de seguridad puede especificar opcionalmente contraseñas para un conjunto de medios, para un conjunto de copia de seguridad o para ambos. Si se ha definido una contraseña en un conjunto de medios o un conjunto de copia de seguridad, debe especificar la contraseña o contraseñas correctas en la instrucción RESTORE. Estas contraseñas impiden operaciones de restauración y anexiones no autorizadas de los conjuntos de copia de seguridad en medios que utilizan herramientas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No obstante, la contraseña no impide que se sobrescriba el medio con la opción FORMAT de la instrucción BACKUP.  
  
> [!IMPORTANT]  
>  El nivel de protección que proporciona esta contraseña es bajo. El objetivo es impedir una restauración incorrecta con las herramientas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ya sea por parte de usuarios autorizados o no autorizados. No impide la lectura de los datos de las copias de seguridad por otros medios o el reemplazo de la contraseña. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]El procedimiento recomendado para proteger las copias de seguridad consiste en almacenar las cintas de copia de seguridad en una ubicación segura o en hacer una copia de seguridad en archivos de disco protegidos con las listas de control de acceso (ACL) adecuadas. Las ACL se deben establecer en el directorio raíz en el que se crean las copias de seguridad.  
  
### <a name="permissions"></a>Permisos  
 Para obtener información sobre un conjunto de copia de seguridad o un dispositivo de copia de seguridad, es necesario el permiso CREATE DATABASE. Para obtener más información, vea [GRANT &#40;permisos de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve la información del encabezado del archivo de disco `C:\AdventureWorks-FullBackup.bak`.  
  
```sql 
RESTORE HEADERONLY   
FROM DISK = N'C:\AdventureWorks-FullBackup.bak';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Historial de copias de seguridad e información de encabezados &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)   
 [Habilitar o deshabilitar sumas de comprobación de copia de seguridad durante copia de seguridad o restauración &#40;SQL Server&#41;](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)   
 [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [Modelos de recuperación &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
  
  
