---
title: Copia de seguridad en URL de SQL Server | Microsoft Docs
description: Obtenga información sobre los conceptos, requisitos y componentes necesarios para que SQL Server use la instancia de Microsoft Azure Blob Storage como destino de copia de seguridad.
ms.custom: ''
ms.date: 03/25/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 11be89e9-ff2a-4a94-ab5d-27d8edf9167d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 75f74fcfca0e039b6f11bc36fd6cf694dfb756b3
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765577"
---
# <a name="sql-server-backup-to-url"></a>Copia de seguridad en dirección URL de SQL Server

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

En este tema se presentan los conceptos, requisitos y componentes necesarios para usar el servicio Microsoft Azure Blob Storage como destino de copia de seguridad. La funcionalidad de copia de seguridad y restauración es igual o similar que la que usa DISK o TAPE, con algunas diferencias. En este tema se incluyen esas diferencias y algunos ejemplos de código.  
  
## <a name="overview"></a>Información general

Es importante entender los componentes y la interacción entre ellos para realizar una copia de seguridad o una restauración desde el servicio Microsoft Azure Blob Storage.  
  
 La creación de una cuenta de Azure Storage en su suscripción de Azure es el primer paso de este proceso. Esta cuenta de almacenamiento es una cuenta administrativa que tiene permisos administrativos completos en todos los contenedores y objetos creados con la cuenta de almacenamiento. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede usar el nombre de la cuenta de almacenamiento de Azure y su valor de clave de acceso para autenticarse y escribir y leer los blobs en el servicio Microsoft Azure Blob Storage, o bien usar un token de firma de acceso compartido generado en determinados contenedores que le concede derechos de escritura y lectura. Para más información sobre cuentas de Azure Storage, vea [Acerca de las cuentas de Azure Storage](/azure/storage/common/storage-account-create); para más información sobre las firmas de acceso compartido, vea [Firmas de acceso compartido, Parte 1: Descripción del modelo SAS](/azure/storage/common/storage-sas-overview). La credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] almacena esta información de autenticación y se emplea durante las operaciones de copia de seguridad o restauración.  
  
###  <a name="backup-to-block-blob-vs-page-blob"></a><a name="blockbloborpageblob"></a> Copia de seguridad en blobs en bloques y blobs en páginas

En el servicio Microsoft Azure Blob Storage se pueden almacenar dos tipos de blobs: en bloques y en páginas. Para SQL Server 2016 y versiones posteriores, se prefiere utilizar un blob en bloques.

si se usa la clave de almacenamiento en la credencial, se usará el blob en páginas; si se usa la firma de acceso compartido, se usará el blob en bloques.

La copia de seguridad en blobs en bloques solo está disponible en SQL Server 2016 o una versión posterior. Realice una copia de seguridad en los blobs en bloque en lugar de en los blobs en página si ejecuta SQL Server 2016 o una versión posterior.

Estos son los motivos:

- La firma de acceso compartido es una manera más segura de autorizar el acceso al blob en comparación con la clave de almacenamiento.
- Puede hacer copias de seguridad en varios blobs en bloques para obtener el mejor rendimiento de copia de seguridad y restauración, y admite la copia de seguridad de bases de datos más grandes.
- La opción de [blob en bloques](https://azure.microsoft.com/pricing/details/storage/blobs/) es más económica que la de [blob en páginas](https://azure.microsoft.com/pricing/details/storage/page-blobs/).
- Los clientes que necesiten realizar una copia de seguridad en blobs en página a través de un servidor proxy deberán usar backuptourl.exe.

La copia de seguridad de una base de datos grande en el almacenamiento de blobs está sujeta a las limitaciones enumeradas en el apartado sobre [diferencias, limitaciones y problemas conocidos de T-SQL de la instancia administrada](/azure/sql-database/sql-database-managed-instance-transact-sql-information#backup).

Si la base de datos es demasiado grande, puede:

- Usar la compresión de copia de seguridad
- Hacer una copia de seguridad en varios blobs en bloques

#### <a name="support-on-linux-containers-and-azure-arc-enabled-sql-managed-instance"></a>Compatibilidad con Linux, contenedores y SQL Managed Instance habilitada para Azure Arc

Si la instancia de SQL Server está hospedada en Linux, lo que incluye:

- Sistema operativo independiente
- Contenedores
- SQL Managed Instance para Azure Arc
- Cualquier otro entorno basado en Linux

El único patrón admitido de copia de seguridad en URL es utilizar blobs en bloques, con una firma de acceso compartido.

###  <a name="microsoft-azure-blob-storage-service"></a><a name="Blob"></a> Servicio Microsoft Azure Blob Storage  

**Cuenta de almacenamiento**: La cuenta de almacenamiento es el punto de partida para todos los servicios de almacenamiento. Para acceder al servicio Microsoft Azure Blob Storage, cree primero una cuenta de almacenamiento de Azure. Para obtener más información, vea [Crear una cuenta de almacenamiento](/azure/storage/common/storage-account-create).  
  
**Contenedor:** un contenedor proporciona una agrupación de un conjunto de blobs y puede almacenar un número ilimitado de blobs. Para escribir una copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servicio Microsoft Azure Blob Storage, debe haber creado al menos el contenedor raíz. Puede generar un token de firma de acceso compartido en un contenedor y conceder acceso a objetos en un único contenedor específico.  
  
**Blob:** Un archivo de cualquier tipo y tamaño. En el servicio Microsoft Azure Blob Storage se pueden almacenar dos tipos de blobs: en bloques y en páginas. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] La copia de seguridad puede usar cualquier tipo de blob en función de la sintaxis de Transact-SQL usada. Los blobs son direccionables mediante el formato de dirección URL siguiente: https://\<storage account>.blob.core.windows.net/\<container>/\<blob>. Para obtener más información sobre el servicio Microsoft Azure Blob Storage, vea [Procedimiento para usar Blob Storage desde .NET](https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/). Para obtener más información sobre los blobs en páginas y en bloques, vea [Descripción de los blobs en bloques, en anexos y en páginas](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).  
  
![Azure Blob Storage](../../relational-databases/backup-restore/media/backuptocloud-blobarchitecture.gif "Azure Blob Storage")  
  
**Instantánea de Azure:** una instantánea de un blob de Azure realizada en un momento dado. Para obtener más información, vea [Crear una instantánea de un blob](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob). La copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ahora admite copias de seguridad de instantáneas de Azure de los archivos de base de datos almacenados en el servicio Microsoft Azure Blob Storage. Para obtener más información, vea [Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
###  <a name="ssnoversion-components"></a>Componentes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de <a name="sqlserver"></a>  

**Dirección URL:** Una dirección URL especifica un identificador uniforme de recursos (URI) para un archivo de copia de seguridad único. La dirección URL se emplea para proporcionar la ubicación y el nombre del archivo de copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La dirección URL debe apuntar a un blob real, no solo a un contenedor. Si el blob no existe, se crea. Si se especifica un blob existente, se produce un error en BACKUP, a menos que se especifique la opción "WITH FORMAT" para sobrescribir el archivo de copia de seguridad existente en el blob.  
  
 Este es un valor de dirección URL de ejemplo: http[s]://ACCOUNTNAME.blob.core.windows.net/\<CONTAINER>/\<FILENAME.bak>. HTTPS no es necesario, pero es recomendable.  
  
**Credencial:** Una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un objeto que se usa para almacenar la información de autenticación necesaria para conectarse a un recurso fuera de SQL Server. Aquí, los procesos de copia de seguridad y restauración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usan credenciales para autenticarse en el servicio Microsoft Azure Blob Storage y en sus objetos de contenedor y blob. La credencial almacena el nombre de la cuenta de almacenamiento y los valores de **clave de acceso** de la cuenta de almacenamiento, o bien la dirección URL del contenedor y su token de firma de acceso compartido. Una vez creada la credencial, la sintaxis de las instrucciones BACKUP/RESTORE determina el tipo de blob y las credenciales que se requieren.  
  
Para obtener un ejemplo sobre cómo crear una firma de acceso compartido, consulte los ejemplos de [Crear una firma de acceso compartido](../../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS) más adelante en este tema; para crear una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea los ejemplos de [Creación de una credencial](../../relational-databases/backup-restore/sql-server-backup-to-url.md#credential) más adelante en este tema.  
  
Para obtener información general sobre las credenciales, vea [Credenciales](../security/authentication-access/credentials-database-engine.md).  
  
Para obtener información sobre otros ejemplos donde se usan credenciales, vea [Crear un proxy del Agente SQL Server](../../ssms/agent/create-a-sql-server-agent-proxy.md).  
  
##  <a name="security"></a><a name="security"></a> Seguridad  

A continuación se muestran los requisitos y las consideraciones de seguridad al hacer copias de seguridad o restaurar desde el servicio Microsoft Azure Blob Storage.  
  
- Al crear un contenedor para el servicio Microsoft Azure Blob Storage, se recomienda establecer el acceso en **privado**. Al configurar el acceso privado se restringe el acceso a los usuarios o las cuentas capaces de proporcionar la información necesaria para autenticarse en la cuenta de Azure.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere que en una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se almacenen bien un nombre de cuenta de Azure y una autenticación de clave de acceso, bien una firma de acceso compartido y un token de acceso. Esta información se emplea para autenticarse en la cuenta de Azure cuando se realizan operaciones de copia de seguridad o de restauración.  
  
- La cuenta de usuario que se usa para emitir comandos BACKUP o RESTORE debe tener el rol de base de datos **operador de db_backup** con permisos **Modificar cualquier credencial** .   

##  <a name="limitations"></a><a name="limitations"></a> Limitaciones  
  
- SQL Server limita a 1 TB el tamaño máximo de copia de seguridad admitido con un blob en páginas. El tamaño máximo de copia de seguridad admitido con blobs en bloques está limitado a aproximadamente 200 GB (50 000 bloques * 4 MB MAXTRANSFERSIZE). Los blobs en bloques admiten la creación de bandas para admitir tamaños de copia de seguridad considerablemente mayores.  
  
    > [!IMPORTANT]  
    >  Aunque el tamaño máximo de copia de seguridad admitido por cada blob en bloques sea de 200 GB, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede escribir en tamaños de bloque más pequeños, lo que puede provocar que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alcance el límite de 50 000 bloques antes de que se transfiera toda la copia de seguridad. Divida las copias de seguridad en secciones (aunque tengan menos de 200 GB) para evitar el límite de bloques, especialmente cuando use copias de seguridad diferenciales o descomprimidas.

- Puede emitir una copia de seguridad o restaurar instrucciones mediante TSQL, SMO, cmdlets de PowerShell o el Asistente para copia de seguridad o restauración de SQL Server Management Studio.   
  
- No se admite la creación de un nombre de dispositivo lógico. Por tanto, no se admite agregar URL como dispositivo de copia de seguridad mediante sp_dumpdevice o mediante SQL Server Management Studio.  
  
- No se admite anexar a blobs de copia de seguridad existentes. Las copias de seguridad en un blob existente solo se pueden sobrescribir mediante la opción **WITH FORMAT** . Pero cuando se usan copias de seguridad de instantánea de archivos (con el argumento **WITH FILE_SNAPSHOT** ), no se permite usar el argumento **WITH FORMAT** para evitar que queden huérfanas las instantáneas de archivos creadas con la copia de seguridad de instantánea de archivos original.  
  
- La copia de seguridad en varios blobs en una sola operación de copia de seguridad solo es compatible con blobs en bloques y utilizando un token de firma de acceso compartido (SAS) en lugar de la clave de la cuenta de almacenamiento para la credencial SQL.  
  
- No se puede especificar **BLOCKSIZE** en los blobs en páginas. 
  
- No se puede especificar **MAXTRANSFERSIZE** en los blobs en páginas. 
  
- No se pueden especificar las opciones de conjunto de copia de seguridad **RETAINDAYS** y **EXPIREDATE** .  
  
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tiene un límite máximo de 259 caracteres para un nombre de dispositivo de copia de seguridad. BACKUP TO URL consume 36 caracteres para los elementos necesarios que se usan para especificar la dirección URL: "https://.blob.core.windows.net//.bak", y deja 223 caracteres para la cuenta, el contenedor y los nombres de blob.  

- Si el servidor tiene acceso a Azure a través de un servidor proxy, debe usar la marca de seguimiento 1819 y, a continuación, establecer la configuración del proxy WinHTTP mediante alguno de los métodos siguientes:
   - La utilidad [proxycfg.exe](/windows/win32/winhttp/proxycfg-exe--a-proxy-configuration-tool) en Windows XP o Windows Server 2003 y versiones anteriores. 
   - La utilidad [netsh.exe](/windows/win32/winsock/netsh-exe) en Windows Vista y Windows Server 2008 o versiones posteriores. 

- No se admite el [almacenamiento inmutable para Azure Blob Storage](/azure/storage/blobs/storage-blob-immutable-storage). Establezca la directiva **Almacenamiento inmutable** en "false". 
  
## <a name="supported-arguments--statements"></a>Instrucciones y argumentos admitidos

###  <a name="support-for-backuprestore-statements"></a><a name="Support"></a> Compatibilidad con instrucciones BACKUP y RESTORE  
  
|Instrucción BACKUP/RESTORE|Compatible|Excepciones|Comentarios|
|-|-|-|-|
|BACKUP|Y|BLOCKSIZE y MAXTRANSFERSIZE admiten los blobs en bloques, pero no admiten los blobs en páginas. | La instrucción BACKUP en un blob en bloques requiere que se guarde una firma de acceso compartido en una credencial de SQL Server. La instrucción BACKUP en un blob en páginas requiere que se guarde la clave de cuenta de almacenamiento en una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y que se especifique el argumento WITH CREDENTIAL.|  
|RESTORE|Y||Requiere que se defina una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y que se especifique el argumento WITH CREDENTIAL si la credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se define utilizando la clave de la cuenta de almacenamiento como el secreto.|  
|RESTORE FILELISTONLY|Y||Requiere que se defina una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y que se especifique el argumento WITH CREDENTIAL si la credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se define utilizando la clave de la cuenta de almacenamiento como el secreto.|  
|RESTORE HEADERONLY|Y||Requiere que se defina una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y que se especifique el argumento WITH CREDENTIAL si la credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se define utilizando la clave de la cuenta de almacenamiento como el secreto.|  
|RESTORE LABELONLY|Y||Requiere que se defina una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y que se especifique el argumento WITH CREDENTIAL si la credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se define utilizando la clave de la cuenta de almacenamiento como el secreto.|  
|RESTORE VERIFYONLY|Y||Requiere que se defina una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y que se especifique el argumento WITH CREDENTIAL si la credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se define utilizando la clave de la cuenta de almacenamiento como el secreto.|  
|RESTORE REWINDONLY|-|||  
  
 Para conocer la sintaxis y obtener información general sobre las instrucciones de copia de seguridad, vea [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
 Para conocer la sintaxis y obtener información general sobre las instrucciones de restauración, vea [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
### <a name="support-for-backup-arguments"></a>Compatibilidad con los argumentos de copia de seguridad  

|Argumento|Compatible|Excepción|Comentarios|  
|-|-|-|-|  
|DATABASE|Y|||  
|REGISTRO|Y|||  
||  
|TO (URL)|Y|A diferencia de DISK y TAPE, URL no admite especificar o crear un nombre lógico.|Este argumento se emplea para especificar la ruta de acceso de la dirección URL del archivo de copia de seguridad.|  
|MIRROR TO|Y|||  
|**OPCIONES DE WITH:**||||  
|CREDENTIAL|Y||WITH CREDENTIAL solo se admite cuando se usa la opción BACKUP TO URL para hacer una copia de seguridad en el servicio Microsoft Azure Blob Storage, y solo cuando la credencial [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se define con la clave de la cuenta de almacenamiento como el secreto.|  
|FILE_SNAPSHOT|Y|||  
|ENCRYPTION|Y||Cuando se especifica el argumento **WITH ENCRYPTION** , la copia de seguridad de instantánea de archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se asegura de que toda la base de datos tenga cifrado TDE antes de realizar la copia de seguridad y, si es así, cifra el archivo de copia de seguridad de instantánea de archivos mediante el algoritmo especificado para TDE en la base de datos. Si no están cifrados la totalidad de los datos de la base de datos, se producirá un error en la copia de seguridad (por ejemplo, proceso de cifrado sin finalizar).|  
|DIFFERENTIAL|Y|||  
|COPY_ONLY|Y|||  
|COMPRESSION&#124;NO_COMPRESSION|Y|No compatible con la copia de seguridad de instantánea de archivos||  
|DESCRIPTION|Y|||  
|NAME|Y|||  
|EXPIREDATE &#124; RETAINDAYS|-|||  
|NOINIT &#124; INIT|-||No es posible anexar a blobs. Para sobrescribir una copia de seguridad, use el argumento **WITH FORMAT** . Pero cuando se usan copias de seguridad de instantánea de archivos (con el argumento **WITH FILE_SNAPSHOT** ), no se permite usar el argumento **WITH FORMAT** para evitar que queden huérfanas las instantáneas de archivos creadas con la copia de seguridad original.|  
|NOSKIP &#124; SKIP|-|||  
|NOFORMAT &#124; FORMAT|Y||Una copia de seguridad realizada en un blob existente producirá un error a menos que se especifique **WITH FORMAT** . El blob existente se sobrescribe si se especifica **WITH FORMAT** . Pero cuando se usan copias de seguridad de instantánea de archivos (con el argumento **WITH FILE_SNAPSHOT** ), no se permite usar el argumento FORMAT para evitar que queden huérfanas las instantáneas de archivos creadas con la copia de seguridad de instantánea de archivos original. Pero cuando se usan copias de seguridad de instantánea de archivos (con el argumento **WITH FILE_SNAPSHOT** ), no se permite usar el argumento **WITH FORMAT** para evitar que queden huérfanas las instantáneas de archivos creadas con la copia de seguridad original.|  
|MEDIADESCRIPTION|Y|||  
|MEDIANAME|Y|||  
|BLOCKSIZE|Y|No se admite para los blobs en páginas. Se admite para blob en bloques.| Se recomienda BLOCKSIZE=65536 para optimizar el uso de los 50.000 bloques permitidos en un blob en bloques. |  
|BUFFERCOUNT|Y|||  
|MAXTRANSFERSIZE|Y|No se admite para los blobs en páginas. Se admite para blob en bloques.| El valor predeterminado es el 1048576. El valor puede llegar a los 4 MB en incrementos de 65.536 bytes.</br> Se recomienda MAXTRANSFERSIZE=4194304 para optimizar el uso de los 50.000 bloques permitidos en un blob en bloques. |  
|NO_CHECKSUM &#124; CHECKSUM|Y|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|Y|||  
|STATS|Y|||  
|REWIND &#124; NOREWIND|-|||  
|UNLOAD &#124; NOUNLOAD|-|||  
|NORECOVERY &#124; STANDBY|Y|||  
|NO_TRUNCATE|Y|||  
  
 Para obtener más información sobre los argumentos de copia de seguridad, vea [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
### <a name="support-for-restore-arguments"></a>Compatibilidad con los argumentos de restauración  
  
|Argumento|Compatible|Excepciones|Comentarios|  
|-|-|-|-|  
|DATABASE|Y|||  
|REGISTRO|Y|||  
|FROM (URL)|Y||El argumento FROM URL se emplea para especificar la ruta de acceso de la dirección URL del archivo de copia de seguridad.|  
|**WITH Options:**||||  
|CREDENTIAL|Y||WITH CREDENTIAL solo se admite cuando se usa la opción RESTORE FROM URL para restaurar desde el servicio Microsoft Azure Blob Storage.|  
|PARTIAL|Y|||  
|RECOVERY &#124; NORECOVERY &#124; STANDBY|Y|||  
|LOADHISTORY|Y|||  
|MOVE|Y|||  
|REPLACE|Y|||  
|RESTART|Y|||  
|RESTRICTED_USER|Y|||  
|FILE|-|||  
|PASSWORD|Y|||  
|MEDIANAME|Y|||  
|MEDIAPASSWORD|Y|||  
|BLOCKSIZE|Y|||  
|BUFFERCOUNT|-|||  
|MAXTRANSFERSIZE|-|||  
|CHECKSUM &#124; NO_CHECKSUM|Y|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|Y|||  
|FILESTREAM|Y|No compatible con la copia de seguridad de instantánea||  
|STATS|Y|||  
|REWIND &#124; NOREWIND|-|||  
|UNLOAD &#124; NOUNLOAD|-|||  
|KEEP_REPLICATION|Y|||  
|KEEP_CDC|Y|||  
|ENABLE_BROKER &#124; ERROR_BROKER_CONVERSATIONS &#124; NEW_BROKER|Y|||  
|STOPAT &#124; STOPATMARK &#124; STOPBEFOREMARK|Y|||  
  
 Para obtener más información sobre los argumentos de restauración, vea [RESTORE &#40;argumentos, Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
##  <a name="back-up-with-ssms"></a><a name="BackupTaskSSMS"></a> Copia de seguridad con SSMS  
Puede realizar una copia de seguridad de una base de datos en una dirección URL mediante la tarea Copia de seguridad en SQL Server Management Studio con una credencial de SQL Server.  
  
> [!NOTE]  
>  Para crear una copia de seguridad de instantánea de archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o sobrescribir un conjunto de medios existente, use Transact-SQL, PowerShell o C# en lugar de la tarea Copia de seguridad en SQL Server Management Studio.  
  
 En los siguientes pasos se describen los cambios realizados en la tarea Copia de seguridad de la base de datos en SQL Server Management Studio para permitir realizar la copia de seguridad en Azure Storage:  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del Motor de base de datos de SQL Server y expándala.

2.  Expanda **Bases de datos**, haga clic con el botón derecho en la base de datos que quiera, seleccione **Tareas** y haga clic en **Copia de seguridad...**
  
3.  En la página **General** de la sección **Destino** , la opción **Dirección URL** está disponible en la lista desplegable **Copia de seguridad en:** .  La opción **Dirección URL** se usa para crear una copia de seguridad para el almacenamiento de Microsoft Azure. Haga clic en **Agregar** y se abrirá el cuadro de diálogo **Seleccionar destino de la copia de seguridad** :
    1.  **Contenedor de almacenamiento de Azure:** El nombre del contenedor de almacenamiento de Microsoft Azure para almacenar los archivos de copia de seguridad.  Seleccione un contenedor existente en la lista desplegable o especifique manualmente el contenedor. 
  
    2.  **Directiva de acceso compartido:** Especifique la firma de acceso compartido para un contenedor especificado manualmente.  Este campo no está disponible si se ha elegido un contenedor existente. 
  
    3.  **Archivo de copia de seguridad:** Nombre del archivo de copia de seguridad.
    
    4.  **Nuevo contenedor:** Se usa para registrar un contenedor existente para el que no tiene una firma de acceso compartido.  Vea [Connect to a Microsoft Azure Subscription (Conectarse a una suscripción de Microsoft Azure)](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).

> [!NOTE] 
>  **Agregar** admite varios contenedores de almacenamiento y archivos de copia de seguridad para un conjunto de medios.

Al seleccionar **Dirección URL** como destino, ciertas opciones de la página **Opciones multimedia** están deshabilitadas.  Los temas siguientes tienen más información acerca del cuadro de diálogo Copia de seguridad de la base de datos:  
  
 [Copia de seguridad de base de datos &#40;página General&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)  
  
 [Copia de seguridad de la base de datos &#40;página Opciones multimedia&#41;](../../relational-databases/backup-restore/back-up-database-media-options-page.md)  
  
 [Copia de seguridad de la base de datos &#40;página Opciones de copia de seguridad&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)  
  
 [Crear credencial - autenticarse en Azure Storage](../../relational-databases/backup-restore/create-credential-authenticate-to-azure-storage.md)  
  
##  <a name="back-up-with-maintenance-plan"></a><a name="MaintenanceWiz"></a> Copia de seguridad con el plan de mantenimiento  
 Al igual que la tarea de copia de seguridad descrita antes, el Asistente para planes de mantenimiento de SQL Server Management Studio incluye **Dirección URL** como una de las opciones de destino, así como otros objetos de compatibilidad necesarios para realizar la copia de seguridad en Azure Storage, como la credencial SQL. Para obtener más información, consulte la sección **Definir las tareas Copia de seguridad** en [Using Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure).  
  
> [!NOTE]  
>  Para crear un conjunto de copia de seguridad con bandas, una copia de seguridad de instantánea de archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o una credencial SQL con el token de acceso compartido, use Transact-SQL, PowerShell o C# en lugar de la tarea Copia de seguridad en el Asistente para planes de mantenimiento.  
  
##  <a name="restore-with-ssms"></a><a name="RestoreSSMS"></a> Restauración con SSMS 
La tarea Restaurar base de datos incluye **Dirección URL** como dispositivo desde el que se puede restaurar.  Los pasos siguientes describen el uso de la tarea Restaurar para realizar una operación de restauración desde el servicio Microsoft Azure Blob Storage: 
  
1. Haga clic con el botón derecho en **Bases de datos** y seleccione **Restaurar base de datos...** 
  
2. En la página **General** , seleccione **Dispositivo** en la sección **Origen** .
  
3. Haga clic en el botón Examinar (...) para abrir el cuadro de diálogo **Seleccionar dispositivos de copia de seguridad** . 

4. Seleccione **Dirección URL** en la lista desplegable **Tipo de medio de copia de seguridad:** .  Haga clic en **Agregar** para abrir el cuadro de diálogo **Seleccionar ubicación de archivo de copia de seguridad** .

    1. **Contenedor de almacenamiento de Azure:** nombre completo del contenedor de almacenamiento de Microsoft Azure que contiene los archivos de copia de seguridad.  Seleccione un contenedor existente en la lista desplegable o especifique manualmente el nombre completo del contenedor.
      
    2. **Firma de acceso compartido:**  se usa para especificar la firma de acceso compartido del contenedor designado.
      
    3. **Agregar:**  Se usa para registrar un contenedor existente para el que no tiene una firma de acceso compartido.  Vea [Connect to a Microsoft Azure Subscription (Conectarse a una suscripción de Microsoft Azure)](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).
      
    4. **Aceptar:**    SQL Server se conecta al almacenamiento de Microsoft Azure con la información de la credencial SQL proporcionada y abre el cuadro de diálogo **Buscar archivo de copia de seguridad en Microsoft Azure**. Los archivos de copia de seguridad que residen en el contenedor de almacenamiento se muestran en esta página. Seleccione el archivo que desee usar para restaurar y haga clic en **Aceptar**. De esta forma, vuelve al cuadro de diálogo **Seleccionar dispositivos de copia de seguridad** y, haciendo clic en **Aceptar** en este cuadro de diálogo, vuelve al cuadro de diálogo principal **Restaurar** donde podrá completar la restauración. 
  
     [Restaurar la base de datos &#40;página General&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
     [Restaurar base de datos &#40;página Archivos&#41;](../../relational-databases/backup-restore/restore-database-files-page.md)  
  
     [Restaurar base de datos &#40;página Opciones&#41;](../../relational-databases/backup-restore/restore-database-options-page.md)  
  
##  <a name="code-examples"></a><a name="Examples"></a> Ejemplos de código  

Esta sección contiene los siguientes ejemplos.  
  
- [Creación de una credencial](#credential)  
  
- [Realizar una copia de seguridad completa de la base de datos](#complete)  

- [Restaurar a un momento dado con STOPAT](#PITR)  
  
> [!NOTE]  
> Para obtener un tutorial sobre el uso de SQL Server 2016 con el servicio Microsoft Azure Blob Storage, vea [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
### <a name="create-a-shared-access-signature"></a><a name="SAS"></a> Crear una firma de acceso compartido

En el siguiente ejemplo se crean firmas de acceso compartido que pueden usarse para crear una credencial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un contenedor recién creado. El script crea una firma de acceso compartido que está asociada a una directiva de acceso almacenada. Para más información, vea [Firmas de acceso compartido, parte 1: Descripción del modelo SAS](/azure/storage/common/storage-sas-overview). El script también escribe el comando de T-SQL necesario para crear la credencial en SQL Server. 

> [!NOTE]
> Este ejemplo requiere Microsoft Azure PowerShell. Para obtener información sobre la instalación y el uso de Azure PowerShell, vea [Cómo instalar y configurar Azure PowerShell](/powershell/azure/).  
> Estos scripts se verificaron con Azure PowerShell 5.1.15063. 

**Firma de acceso compartido que está asociada a una directiva de acceso almacenada**
  
```Powershell  
# Define global variables for the script  
$prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
$subscriptionName='<your subscription name>'   # the name of subscription name you will use  
$locationName = '<a data center location>'  # the data center region you will use  
$storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
$containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
$policyName = $prefixName + 'policy' # the name of the SAS policy  

# Set a variable for the name of the resource group you will create or use  
$resourceGroupName=$prefixName + 'rg'

# adds an authenticated Azure account for use in the session
Connect-AzAccount

# set the tenant, subscription and environment for use in the rest of
Set-AzContext -SubscriptionName $subscriptionName

# create a new resource group - comment out this line to use an existing resource group  
New-AzResourceGroup -Name $resourceGroupName -Location $locationName

# Create a new ARM storage account - comment out this line to use an existing ARM storage account  
New-AzStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   

# Get the access keys for the ARM storage account  
$accountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  

# Create a new storage account context using an ARM storage account  
$storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].value 

# Creates a new container in blob storage  
$container = New-AzStorageContainer -Context $storageContext -Name $containerName  
$cbc = $container.CloudBlobContainer  

# Sets up a Stored Access Policy and a Shared Access Signature for the new container  
$policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission "rwld"
$sas = New-AzStorageContainerSASToken -Policy $policyName -Context $storageContext -Container $containerName
Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

# Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
Write-Host 'Credential T-SQL'  
$tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
$tSql | clip  
Write-Host $tSql  
```  

Después de ejecutar el script correctamente, copie el comando `CREATE CREDENTIAL` en una herramienta de consultas, conéctese a una instancia de SQL Server y ejecute el comando para crear la credencial con la firma de acceso compartido. 

###  <a name="create-a-credential"></a><a name="credential"></a> Creación de una credencial

En los ejemplos siguientes se crean credenciales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la autenticación en el servicio Microsoft Azure Blob Storage. Realice una de las acciones siguientes.
  
1. **Uso de la firma de acceso compartido**  

   Si ejecutó el script para crear la firma de acceso compartido anterior, copie el comando `CREATE CREDENTIAL` a un editor de consultas conectado a la instancia de SQL Server y ejecútelo. 

   El siguiente código T-SQL es un ejemplo que crea la credencial para usar una firma de acceso compartido. 

   ```sql  
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials   
   WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
   CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
      SECRET = '<SAS_TOKEN>';  
   ```  
  
2. **Uso de la identidad y la clave de acceso de la cuenta de almacenamiento**  
  
   ```sql 
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials   
   WHERE name = '<mycredentialname>')  
   CREATE CREDENTIAL [<mycredentialname>] WITH IDENTITY = '<mystorageaccountname>'  
   ,SECRET = '<mystorageaccountaccesskey>';  
   ```  
  
### <a name="perform-a-full-database-backup"></a><a name="complete"></a> Realizar una copia de seguridad completa de la base de datos  

En los ejemplos siguientes se realiza una copia de seguridad completa de la base de datos AdventureWorks2016 en el servicio Microsoft Azure Blob Storage. Realice una de las siguientes acciones:
  
1. **En URL con una firma de acceso compartido**  
  
   ```sql  
   BACKUP DATABASE AdventureWorks2016   
   TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak';  
   GO   
   ```  

1. **En URL con la identidad y la clave de acceso de la cuenta de almacenamiento**  
  
   ```sql
   BACKUP DATABASE AdventureWorks2016  
   TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak'   
         WITH CREDENTIAL = '<mycredentialname>'   
        ,COMPRESSION  
        ,STATS = 5;  
   GO
   ```  

###  <a name="restoring-to-a-point-in-time-using-stopat"></a><a name="PITR"></a> Restaurar a un momento dado con STOPAT  

En el ejemplo siguiente se restaura la base de datos AdventureWorks2016 al estado que tenía en un momento dado y se muestra una operación de restauración.  
  
**Desde URL con una firma de acceso compartido**  
  
   ```sql
   RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.bak'   
   WITH MOVE 'AdventureWorks2016_data' to 'C:\Program Files\Microsoft SQL Server\<myinstancename>\MSSQL\DATA\AdventureWorks2016.mdf'  
   ,MOVE 'AdventureWorks2016_log' to 'C:\Program Files\Microsoft SQL Server\<myinstancename>\MSSQL\DATA\AdventureWorks2016.ldf'  
   ,NORECOVERY  
   ,REPLACE  
   ,STATS = 5;  
   GO   
  
   RESTORE LOG AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_18_00_00.trn'   
   WITH   
   RECOVERY   
   ,STOPAT = 'May 18, 2015 5:35 PM'   
   GO  
   ```  
  
## <a name="see-also"></a>Consulte también  

- [Procedimientos recomendados y solución de problemas de Copia de seguridad en URL de SQL Server](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)
- [Realizar copias de seguridad y restaurar bases de datos del sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)
- [Tutorial: Uso del servicio Microsoft Azure Blob Storage con bases de datos de SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).
