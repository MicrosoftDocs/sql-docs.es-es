---
title: Creación de una copia de seguridad de base de datos completa | Microsoft Docs
description: En este artículo se muestra cómo crear una copia de seguridad completa de la base de datos en SQL Server con SQL Server Management Studio, Transact-SQL o PowerShell.
ms.custom: sqlfreshmay19
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backing up databases [SQL Server], full backups
- backing up databases [SQL Server], SQL Server Management Studio
- backups [SQL Server], creating
- database backups [SQL Server], SQL Server Management Studio
ms.assetid: 586561fc-dfbb-4842-84f8-204a9100a534
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3eb0b33f2891c8f48d63b4446d31c1918fcb7aab
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076117"
---
# <a name="create-a-full-database-backup"></a>Crear una copia de seguridad completa de base de datos

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se explica cómo crear una copia de seguridad completa de la base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o PowerShell.

Para obtener información sobre la copia de seguridad de SQL Server en el servicio Azure Blob Storage, vea [Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) y [Copia de seguridad en URL de SQL Server](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones

- La instrucción `BACKUP` no se permite en una transacción explícita o implícita.
- Las copias de seguridad que se crean en una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se pueden restaurar en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Para obtener información general, pero también especializada, sobre los conceptos y las tareas de copia de seguridad, vea [Backup Overview &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md) (Información general de copia de seguridad [SQL Server]) antes de continuar.

## <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones

- A medida que la base de datos aumenta de tamaño, las copias de seguridad completas requieren más tiempo para finalizar y más espacio de almacenamiento. Para las bases de datos grandes, considere la posibilidad de complementar las copias de seguridad completas con una serie de [copias de seguridad diferenciales](../../relational-databases/backup-restore/differential-backups-sql-server.md).
- Calcule el tamaño de una copia de seguridad completa de la base de datos mediante el procedimiento almacenado del sistema [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) .
- De forma predeterminada, cada operación de copia de seguridad correcta agrega una entrada en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y en el registro de eventos del sistema. Si realiza copias de seguridad con frecuencia, estos mensajes de aprobación se acumularán rápidamente, lo que dará lugar a enormes registros de errores. Esto puede dificultar la búsqueda de otros mensajes. En esos casos, puede suprimir estas entradas de registro de copia de seguridad con la marca de seguimiento 3226 si ninguno de los scripts depende de ellas. Para obtener más información, vea [Marcas de seguimiento &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

## <a name="security"></a><a name="Security"></a> Seguridad

**TRUSTWORTHY** se establece en OFF en una copia de seguridad de base de datos. Para obtener información sobre cómo establecer **TRUSTWORTHY** en ON, vea [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).

A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], ya no se incluyen las opciones **PASSWORD** y **MEDIAPASSWORD** para crear copias de seguridad. Todavía puede restaurar las copias de seguridad creadas con contraseñas.

## <a name="permissions"></a><a name="Permissions"></a> Permisos

De forma predeterminada, los permisos `BACKUP DATABASE` y `BACKUP LOG` se corresponden a los miembros del rol fijo de servidor **sysadmin** y de los roles fijos de base de datos **db_owner** y **db_backupoperator**.

 Los problemas de propiedad y permisos del archivo físico del dispositivo de copia de seguridad pueden interferir con una operación de copia de seguridad. El servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe poder leer y escribir en el dispositivo, lo que significa que la cuenta en la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener permisos de escritura en el dispositivo de copia de seguridad. En cambio, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), que agrega una entrada para un dispositivo de copia de seguridad en las tablas del sistema, no comprueba los permisos de acceso a los archivos. Como resultado, es posible que estos problemas con el archivo físico del dispositivo de copia de seguridad no aparezcan hasta que se tenga acceso al recurso físico, al intentar la copia de seguridad o la restauración.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio

> [!NOTE]
> Al especificar una tarea de copia de seguridad mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puede generar el script [BACKUP](../../t-sql/statements/backup-transact-sql.md) de [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondiente si hace clic en el botón **Script** y selecciona un destino de script.

1. Tras conectarse a la instancia adecuada de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], expanda el árbol de servidores en el **Explorador de objetos**.

1. Expanda **Bases de datos** y seleccione la base de datos de un usuario o expanda **Bases de datos del sistema** y seleccione una base de datos del sistema.

1. Haga clic con el botón derecho en la base de datos de la que quiera hacer una copia de seguridad, seleccione **Tareas** y, después, haga clic en **Copia de seguridad...**

1. En el cuadro de diálogo **Copia de seguridad de base de datos**, la base de datos seleccionada aparece en la lista desplegable (que puede cambiar a cualquier otra base de datos del servidor).

1. En la lista desplegable **Tipo de copia de seguridad**, seleccione el tipo de copia de seguridad adecuado; el valor predeterminado es **Completa**.

   > [!IMPORTANT]
   > Debe realizar al menos una copia de seguridad completa de la base de datos para poder realizar una copia de seguridad diferencial o del registro de transacciones.
   
1. En **Componente de copia de seguridad**, seleccione **Base de datos**.

1. En la sección **Destino**, revise la ubicación predeterminada del archivo de copia de seguridad (en la carpeta ../mssql/data).

   Para realizar la copia de seguridad en otro dispositivo, cambie la selección mediante la lista desplegable **Copia de seguridad en**. Para dividir el conjunto de copia de seguridad en varios archivos para aumentar la velocidad de copia de seguridad, haga clic en **Agregar** para agregar objetos o destinos de copia de seguridad adicionales.
 
   Para eliminar un destino de copia de seguridad, selecciónelo y haga clic en **Quitar**. Para ver el contenido de un destino de copia de seguridad existente, selecciónelo y haga clic en **Contenido**.

1. (Opcional) Revise las demás opciones disponibles en las páginas **Opciones multimedia** y **Opciones de copia de seguridad**.

   Para obtener más información sobre las distintas opciones de copia de seguridad, vea [página General](back-up-database-general-page.md), [página Opciones multimedia](back-up-database-media-options-page.md) y [página Opciones de copia de seguridad](back-up-database-backup-options-page.md).

1. Haga clic en **Aceptar** para iniciar la copia de seguridad.

1. Cuando la copia de seguridad se complete correctamente, haga clic en **Aceptar** para cerrar el cuadro de diálogo SQL Server Management Studio.

### <a name="additional-information"></a>Información adicional

- Después de crear una copia de seguridad completa de la base de datos, puede crear una [copia de seguridad diferencial](create-a-differential-database-backup-sql-server.md) o una [copia de seguridad del registro de transacciones](back-up-a-transaction-log-sql-server.md).

- (opcional) También puede activar la casilla **Copia de seguridad de solo copia** para crear una copia de seguridad de solo copia. Una *copia de seguridad de solo copia* es una copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] independiente de la secuencia de copias de seguridad convencionales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Copias de seguridad de solo copia &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md). Una copia de seguridad de solo copia no está disponible para el tipo de copia de seguridad **Diferencial**.

- La opción **Sobrescribir medios** está deshabilitada en la página **Opciones multimedia** si la copia de seguridad se realiza en una dirección URL.

### <a name="examples"></a>Ejemplos

Para los ejemplos siguientes, cree una base de datos de prueba con el siguiente código Transact-SQL:

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest
   (
      ID INT NOT NULL PRIMARY KEY,
      c1 VARCHAR(100) NOT NULL,
      dt1 DATETIME NOT NULL DEFAULT getdate()
   );
GO

USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO
```

#### <a name="a-full-back-up-to-disk-to-default-location"></a>A. Crear copia de seguridad completa en disco en una ubicación predeterminada

En este ejemplo, se creará una copia de seguridad de la base de datos `SQLTestDB` en disco en la ubicación de copia de seguridad predeterminada.

1. Tras conectarse a la instancia adecuada de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], expanda el árbol de servidores en el **Explorador de objetos**.

1. Expanda **Bases de datos**, haga clic con el botón derecho en `SQLTestDB`, seleccione **Tareas** y, luego, haga clic en **Copia de seguridad...**

1. Haga clic en **OK**.

1. Cuando la copia de seguridad se complete correctamente, haga clic en **Aceptar** para cerrar el cuadro de diálogo SQL Server Management Studio.

![Realizar copia de seguridad SQL](media/quickstart-backup-restore-database/backup-db-ssms.png)

#### <a name="b-full-back-up-to-disk-to-non-default-location"></a>B. Crear copia de seguridad completa en disco en una ubicación no predeterminada

En este ejemplo, se creará una copia de seguridad de la base de datos `SQLTestDB` en disco en la ubicación que elija.

1. Tras conectarse a la instancia adecuada de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], expanda el árbol de servidores en el **Explorador de objetos**.

1. Expanda **Bases de datos**, haga clic con el botón derecho en `SQLTestDB`, seleccione **Tareas** y, luego, haga clic en **Copia de seguridad...**

1. En la página **General** , en la sección **Destino** , seleccione **Disco** en la lista desplegable **Copia de seguridad en:** .

1. Seleccione **Quitar** hasta que se quiten todos los archivos de copia de seguridad existentes.

1. Seleccione **Agregar** y se abrirá el cuadro de diálogo **Seleccionar destino de la copia de seguridad**.

1. Escriba una ruta de acceso y un nombre de archivo válidos en el cuadro de texto **Nombre de archivo** y use **.bak** como extensión para simplificar la clasificación de este archivo.

1. Haga clic en **Aceptar** y nuevamente en **Aceptar** para iniciar la copia de seguridad.

1. Cuando la copia de seguridad se complete correctamente, haga clic en **Aceptar** para cerrar el cuadro de diálogo SQL Server Management Studio.

![Cambiar la ubicación de la base de datos](media/create-a-full-database-backup-sql-server/change-db-location.png)

#### <a name="c-create-an-encrypted-backup"></a>C. Crear una copia de seguridad cifrada

En este ejemplo se creará una copia de seguridad de la base de datos `SQLTestDB` con cifrado en la ubicación de copia de seguridad predeterminada.

1. Tras conectarse a la instancia adecuada de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], expanda el árbol de servidores en el **Explorador de objetos**.

1. Expanda **Bases de datos** y **Bases de datos del sistema**, haga clic con el botón derecho en `master` y haga clic en **Nueva consulta** para abrir una ventana de consulta con una conexión a la base de datos `SQLTestDB`.

1. Ejecute los comandos siguientes para crear una [**clave maestra de base de datos**](../../relational-databases/security/encryption/create-a-database-master-key.md) y un [**certificado**](../../t-sql/statements/create-certificate-transact-sql.md) en la base de datos `master`.  

   ```sql
   -- Create the master key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';  

   -- If the master key already exists, open it in the same session that you create the certificate (see next step)
   OPEN MASTER KEY DECRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe'

   -- Create the certificate encrypted by the master key
   CREATE CERTIFICATE MyCertificate
   WITH SUBJECT = 'Backup Cert', EXPIRY_DATE = '20201031';  
   ```

1. En el **Explorador de objetos**, en el nodo **Bases de datos**, haga clic con el botón derecho en `SQLTestDB`, seleccione **Tareas** y luego haga clic en **Hacer copia de seguridad…**

1. En la página **Opciones multimedia**, en la sección **Sobrescribir medios**, seleccione **Hacer copia de seguridad en un nuevo conjunto de medios y borrar todos los conjuntos de copia de seguridad existentes**.

1. En la página **Opciones de copia de seguridad** , en la sección **Cifrado** , active la casilla **Cifrar copia de seguridad** .

1. En la lista desplegable Algoritmo, seleccione **AES 256**.

1. En la lista desplegable **Certificado o clave asimétrica** , seleccione `MyCertificate`.

1. Seleccione **Aceptar**.

![Copia de seguridad cifrada](media/create-a-full-database-backup-sql-server/encrypted-backup.png)

#### <a name="d-back-up-to-the-azure-blob-storage-service"></a>D. Copia de seguridad en el servicio de almacenamiento de blobs de Azure

En el ejemplo siguiente se realiza una copia de seguridad completa de `SQLTestDB` en el servicio Azure Blob Storage. En este ejemplo se asume que ya tiene una cuenta de almacenamiento con un contenedor de blobs. En este ejemplo se crea una firma de acceso compartido de forma automática; se produce un error si el contenedor tiene una firma de acceso compartido existente.

Si no tiene un contenedor de blobs de Azure en una cuenta de almacenamiento, cree uno antes de continuar. Para más información, vea [Creación de una cuenta de almacenamiento de uso general](/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) y [Creación de un contenedor](/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container).

1. Tras conectarse a la instancia adecuada de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], expanda el árbol de servidores en el **Explorador de objetos**.

1. Expanda **Bases de datos**, haga clic con el botón derecho en `SQLTestDB`, seleccione **Tareas** y, luego, haga clic en **Copia de seguridad...**

1. En la página **General** de la sección **Destino** , seleccione **URL** en la lista desplegable **Copia de seguridad en:** .

1. Haga clic en **Agregar** y se abrirá el cuadro de diálogo **Seleccionar destino de la copia de seguridad** .

1. Si ha registrado previamente el contenedor de almacenamiento de Azure que quiere usar con SQL Server Management Studio, selecciónelo. En caso contrario, haga clic en **Nuevo contenedor** para registrar un contenedor nuevo.

1. En el cuadro de diálogo **Conectarse a una suscripción de Microsoft**, inicie sesión en la cuenta.

1. En el cuadro de texto desplegable **Seleccionar cuenta de almacenamiento**, seleccione la cuenta de almacenamiento.

1. En el cuadro de texto desplegable **Seleccionar contenedor de blobs**, seleccione el contenedor de blobs.

1. En el cuadro de calendario desplegable **Expiración de la directiva de acceso compartido**, seleccione una fecha de expiración para la directiva de acceso compartido que se va a crear en este ejemplo.

1. Haga clic en **Crear credencial** para generar una firma de acceso compartido y una credencial en SQL Server Management Studio.

1. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Conectarse a una suscripción de Microsoft**.

1. En el cuadro de texto **Archivo de copia de seguridad**, modifique el nombre del archivo de copia de seguridad (opcional).

1. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Seleccionar destino de la copia de seguridad**.

1. Haga clic en **Aceptar** para iniciar la copia de seguridad.

1. Cuando la copia de seguridad se complete correctamente, haga clic en **Aceptar** para cerrar el cuadro de diálogo SQL Server Management Studio.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL

Ejecute la instrucción `BACKUP DATABASE` para crear la copia de seguridad de base de datos completa, y especifique lo siguiente:

- El nombre de la base de datos de la que se va a realizar una copia de seguridad.
- El dispositivo de copia de seguridad en el que se escribe la copia de seguridad de base de datos completa.

La sintaxis básica de [!INCLUDE[tsql](../../includes/tsql-md.md)] para crear una copia de seguridad de base de datos completa es:

 BACKUP DATABASE *base_de_datos* TO *dispositivo_de_copia_de_seguridad* [ **,** ...*n* ] [ WITH *con_opciones* [ **,** ...*o* ] ] ;

|Opción|Descripción|
|------------|-----------------|
|*database*|Es la base de datos cuya copia de seguridad se desea hacer.|
|*backup_device* [ **,** ...*n* ]|Especifica una lista de 1 a 64 dispositivos de copia de seguridad que se pueden utilizar en la operación de copia de seguridad. Puede especificar un dispositivo físico de copia de seguridad o puede especificar un dispositivo de copia de seguridad lógico correspondiente, si ya se definió. Para especificar un dispositivo de copia de seguridad físico, use la opción DISK o TAPE:<br /><br /> { DISK &#124; TAPE } **=** _nombre\_dispositivo\_copia de seguridad\_física_<br /><br /> Para obtener más información, vea [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).|
|WITH *with_options* [ **,** ...*o* ]|De forma opcional, puede especificar una o varias opciones, *o*. Para obtener información sobre algunas de las opciones de WITH básicas, vea el paso 2.|
|||

Opcionalmente, especifique una o varias opciones de **WITH**. A continuación se describen algunas de las opciones de **WITH** básicas. Para obtener información sobre todas las opciones de **WITH**, vea [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).

Opciones de **WITH** básicas del conjunto de copia de seguridad:

- **{ COMPRESSION | NO_COMPRESSION }** : En [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] y versiones posteriores únicamente, especifica si la [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md) se realiza en esta copia de seguridad, lo que invalida la configuración predeterminada del servidor.
- **ENCRYPTION (ALGORITHM, SERVER CERTIFICATE | ASYMMETRIC KEY)** : En SQL Server 2014 o versiones posteriores únicamente, especifica el algoritmo de cifrado que se va a utilizar y el certificado o la clave asimétrica que se va a usar para proteger el cifrado.
- **DESCRIPTION** **=** { **'** _text_ **'**  |  **@** _text\_variable_ }: Especifica el texto sin formato que describe el conjunto de copia de seguridad. La cadena puede tener un máximo de 255 caracteres.
- **NAME = { *backup_set_name* |  **@** _backup\_set\_name\_var_ }** : Especifica el nombre del conjunto de copia de seguridad. Los nombres pueden tener un máximo de 128 caracteres. Si no se especifica NAME, está en blanco.

De forma predeterminada, `BACKUP`anexa la copia de seguridad a un conjunto de medios existente, y se conservan los conjuntos de copia de seguridad existentes. Para especificar esto de forma explícita, use la opción `NOINIT`. Para obtener información sobre la anexión a conjuntos de copia de seguridad existentes, vea [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

Opcionalmente, para dar formato a los medios de copia de seguridad, use la opción **FORMAT**:

 FORMAT [ **,** MEDIANAME **=** { *nombre_del_medio* |  **@** _variable\_nombre\_medio_ } ] [ **,** MEDIADESCRIPTION **=** { *texto* |  **@** _variable\_texto_ } ]

 Use la cláusula **FORMAT** cuando utilice los medios por primera vez o cuando quiera sobrescribir todos los datos existentes. De manera opcional, puede asignar a los nuevos medios un nombre y una descripción.

 > [!IMPORTANT]
 > Tenga mucho cuidado al usar la cláusula **FORMAT** de la instrucción `BACKUP`, ya que destruye cualquier copia de seguridad existente en el medio de copia de seguridad.

### <a name="examples"></a><a name="TsqlExample"></a> Ejemplos

Para los ejemplos siguientes, cree una base de datos de prueba con el siguiente código Transact-SQL:

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
   ID INT NOT NULL PRIMARY KEY,
   c1 VARCHAR(100) NOT NULL,
   dt1 DATETIME NOT NULL DEFAULT GETDATE()
)
GO

USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO
```

#### <a name="a-back-up-to-a-disk-device"></a>A. Copia de seguridad en un dispositivo de disco

En el ejemplo siguiente se realiza una copia de seguridad completa de la base de datos `SQLTestDB` en el disco y se usa `FORMAT` para crear un conjunto de medios nuevo.

```sql
USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
TO DISK = 'c:\tmp\SQLTestDB.bak'
   WITH FORMAT,
      MEDIANAME = 'SQLServerBackups',
      NAME = 'Full Backup of SQLTestDB';
GO
```

#### <a name="b-back-up-to-a-tape-device"></a>B. Copia de seguridad en un dispositivo de cinta

 En este ejemplo se realiza una copia de seguridad en cinta de la base de datos `SQLTestDB` completa y se anexa a las copias de seguridad anteriores.

```sql
USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
   TO TAPE = '\\.\Tape0'
   WITH NOINIT,
      NAME = 'Full Backup of SQLTestDB';
GO
```

#### <a name="c-back-up-to-a-logical-tape-device"></a>C. Copia de seguridad en un dispositivo de cinta lógico

En este ejemplo, se crea un dispositivo de copia de seguridad lógico para una unidad de cinta. A continuación, se realiza una copia de seguridad completa de la base de datos SQLTestDB en dicho dispositivo.

```sql
-- Create a logical backup device,
-- SQLTestDB_Bak_Tape, for tape device \\.\tape0.
USE master;
GO
EXEC sp_addumpdevice 'tape', 'SQLTestDB_Bak_Tape', '\\.\tape0'; USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
   TO SQLTestDB_Bak_Tape
   WITH FORMAT,
      MEDIANAME = 'SQLTestDB_Bak_Tape',
      MEDIADESCRIPTION = '\\.\tape0',
      NAME = 'Full Backup of SQLTestDB';
GO
```

## <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell

Use el cmdlet **Backup-SqlDatabase** . Para indicar de forma explícita que se trata una copia de seguridad completa de la base de datos, especifique el parámetro **-BackupAction** con su valor predeterminado, **Database**. Este parámetro es opcional para las copias de seguridad de base de datos completas.

> [!NOTE]
> Estos ejemplos requieren el módulo SqlServer. Para determinar si está instalado, ejecute `Get-Module -Name SqlServer`. Para instalar este módulo, ejecute `Install-Module -Name SqlServer` en una sesión de administrador de PowerShell.
>
> Para más información, consulte [SQL Server PowerShell Provider](../../powershell/sql-server-powershell-provider.md).

> [!IMPORTANT]
> Si abre una ventana de PowerShell desde SQL Server Management Studio para conectarse a una instalación de SQL Server, puede omitir la parte de la credencial de este ejemplo, ya que se usa automáticamente la credencial en SSMS para establecer la conexión entre PowerShell y la instancia de SQL Server.

### <a name="examples"></a>Ejemplos

#### <a name="a-full-backup-local"></a>A. Copia de seguridad completa (local)

En el ejemplo siguiente se crea una copia de seguridad completa de la base de datos `<myDatabase>` en la ubicación de copia de seguridad predeterminada de la instancia de servidor `Computer\Instance`. Opcionalmente, en este ejemplo se especifica **-BackupAction Database**.

Para obtener la sintaxis completa y otros ejemplos, vea [Backup-SqlDatabase](/powershell/module/sqlserver/backup-sqldatabase).

```powershell
$credential = Get-Credential

Backup-SqlDatabase -ServerInstance Computer[\Instance] -Database <myDatabase> -BackupAction Database -Credential $credential
```

#### <a name="b-full-backup-to-azure"></a>B. Copia de seguridad completa en Azure

En el ejemplo siguiente se crea una copia de seguridad completa de la base de datos `<myDatabase>` en la instancia de `<myServer>` en el servicio Azure Blob Storage. Se ha creado una directiva de acceso almacenada con derechos de lectura, escritura y lista. La credencial de SQL Server, `https://<myStorageAccount>.blob.core.windows.net/<myContainer>`, se creó con una firma de acceso compartido asociada a la directiva de acceso almacenada. El comando de PowerShell usa el parámetro **BackupFile** para especificar la ubicación (dirección URL) y el nombre del archivo de copia de seguridad.

```powershell
$credential = Get-Credential
$container = 'https://<myStorageAccount>blob.core.windows.net/<myContainer>'
$fileName = '<myDatabase>.bak'
$server = '<myServer>'
$database = '<myDatabase>'
$backupFile = $container + '/' + $fileName

Backup-SqlDatabase -ServerInstance $server -Database $database -BackupFile $backupFile -Credential $credential
```

## <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks

- [Realizar una copia de seguridad de una base de datos (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)
- [Crear una copia de seguridad diferencial de una base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)
- [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)
- [Restaurar una copia de seguridad de base de datos en el modelo de recuperación simple &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)
- [Restaurar una base de datos según el punto de error en el modelo de recuperación completa &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)
- [Restaurar una base de datos a una nueva ubicación &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)
- [Usar el Asistente para planes de mantenimiento](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)

## <a name="see-also"></a>Consulte también

- [Solución de problemas de SQL Server de backup y restore de las operaciones ](https://support.microsoft.com/kb/224071)
- [Información general de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)
- [Copias de seguridad de registros de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)
- [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)
- [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)
- [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)
- [Copia de seguridad de base de datos &#40;página General&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)
- [Copia de seguridad de la base de datos &#40;página Opciones de copia de seguridad&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)
- [Copias de seguridad diferenciales (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md)
- [Copias de seguridad completas de bases de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md)