---
title: Migración de una base de datos SQL Server de Windows a Linux
description: En este tutorial se muestra cómo realizar una copia de seguridad de base de datos SQL Server en Windows y restaurarla en una máquina Linux que ejecute SQL Server.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/16/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 9ac64d1a-9fe5-446e-93c3-d17b8f55a28f
ms.openlocfilehash: 2ae765c8bc1f274ab9b9f62978a85b5f09f6ca58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349080"
---
# <a name="migrate-a-sql-server-database-from-windows-to-linux-using-backup-and-restore"></a>Migración de una base de datos SQL Server de Windows a Linux mediante Copia de seguridad y restauración

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

La característica Copia de seguridad y restauración de SQL Server es el método recomendado para migrar una base de datos de SQL Server en Windows a SQL Server en Linux. Este tutorial le guiará por los pasos necesarios para mover una base de datos a Linux con técnicas de copia de seguridad y restauración.

> [!div class="checklist"]
> * Creación de un archivo de copia de seguridad en Windows con SSMS
> * Instalación de un shell de Bash en Windows
> * Traslado del archivo de copia de seguridad a Linux desde el shell de Bash
> * Restauración del archivo de copia de seguridad en Linux con Transact-SQL
> * Ejecución de una consulta para comprobar la migración

También puede crear un Grupo de disponibilidad Always On de SQL Server para migrar una base de datos de SQL Server de Windows a Linux. Consulte [sql-server-linux-availability-group-cross-platform](sql-server-linux-availability-group-cross-platform.md).

## <a name="prerequisites"></a>Prerequisites

Debe disponer de lo siguiente para poder completar este tutorial:

* Un equipo Windows con lo siguiente:
  * [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) instalado.
  * [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) instalado.
  * Una base de datos de destino para migrar.

* Un equipo Linux con lo siguiente instalado:
  * SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md) o [Ubuntu](quickstart-install-connect-ubuntu.md)) con herramientas de línea de comandos

## <a name="create-a-backup-on-windows"></a>Creación de una copia de seguridad en Windows

Hay varias maneras de crear un archivo de copia de seguridad de una base de datos en Windows. Para los pasos siguientes, se usa SQL Server Management Studio (SSMS).

1. Inicie **SQL Server Management Studio** en el equipo Windows.

1. En el cuadro de diálogo de conexión, escriba **localhost**.

1. En el Explorador de objetos, expanda **Bases de datos**.

1. Haga clic con el botón derecho en la base de datos de destino, seleccione **Tareas** y, luego, haga clic en **Hacer copia de seguridad…**

   ![Uso de SSMS para crear un archivo de copia de seguridad](./media/sql-server-linux-migrate-restore-database/ssms-create-backup.png)

1. En el cuadro de diálogo **Copia de seguridad de la base de datos**, compruebe que **Tipo de copia de seguridad** es **Completo** y que **Copia de seguridad en** es **Disco**. Tome nota del nombre y la ubicación del archivo. Por ejemplo, una base de datos denominada **YourDB** en SQL Server 2016 tiene una ruta de acceso de copia de seguridad predeterminada de `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak`.

1. Haga clic en **Aceptar** para realizar una copia de seguridad de la base de datos.

> [!NOTE]
> Otra opción consiste en ejecutar una consulta de Transact-SQL para crear el archivo de copia de seguridad. El siguiente comando de Transact-SQL realiza las mismas acciones que los pasos anteriores para una base de datos denominada **YourDB**:
>
> ```sql
> BACKUP DATABASE [YourDB] TO  DISK =
> N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak'
> WITH NOFORMAT, NOINIT, NAME = N'YourDB-Full Database Backup',
> SKIP, NOREWIND, NOUNLOAD, STATS = 10
> GO
> ```

## <a name="install-a-bash-shell-on-windows"></a>Instalación de un shell de Bash en Windows

Para restaurar la base de datos, primero debe transferir el archivo de copia de seguridad del equipo Windows al equipo Linux de destino. En este tutorial, moveremos el archivo a Linux desde un shell de Bash (ventana de terminal) que se ejecuta en Windows.

1. Instale un shell de Bash en un equipo Windows que admita los comandos **scp** (copia segura) y **ssh** (inicio de sesión remoto). Dos ejemplos incluyen:

   * El [subsistema de Windows para Linux](/windows/wsl/about) (Windows 10)
   * El shell de Bash de Git ([https://git-scm.com/downloads](https://git-scm.com/downloads))

1. Abra una sesión de Bash en Windows.

## <a name="copy-the-backup-file-to-linux"></a><a id="scp"></a> Copia del archivo de copia de seguridad en Linux

1. En la sesión de Bash, vaya al directorio que contiene el archivo de copia de seguridad. Por ejemplo:

   ```bash
   cd 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\'
   ```

1. Use el comando **scp** para transferir el archivo al equipo Linux de destino. En el ejemplo siguiente se transfiere **YourDB.bak** al directorio particular de *user1* en el servidor Linux con una dirección IP de *192.0.2.9*:

   ```bash
   scp YourDB.bak user1@192.0.2.9:./
   ```
   ![Comando scp](./media/sql-server-linux-migrate-restore-database/scp-command.png)

> [!TIP]
> Hay alternativas al uso de scp para la transferencia de archivos. Una consiste en usar [Samba](https://help.ubuntu.com/community/Samba) para configurar un recurso compartido de red de SMB entre Windows y Linux. Para ver un tutorial sobre Ubuntu, consulte [Cómo crear un recurso compartido de red a través de Samba](https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20%28Command-line%20interface/Linux%20Terminal%29%20-%20Uncomplicated,%20Simple%20and%20Brief%20Way!). Una vez establecido, puede acceder a él como un recurso compartido de archivos de red desde Windows, como **\\\\machinenameorip\\share**.

## <a name="move-the-backup-file-before-restoring"></a>Traslado del archivo de copia de seguridad antes de la restauración

En este momento, el archivo de copia de seguridad se encuentra en el servidor Linux en el directorio particular del usuario. Antes de restaurar la base de datos a SQL Server, debe colocar la copia de seguridad en un subdirectorio de **/var/opt/mssql**.

1. En la misma sesión de Bash de Windows, conéctese de forma remota al equipo Linux de destino con **ssh**. En el ejemplo siguiente, se realiza la conexión al equipo Linux **192.0.2.9** como usuario **user1**.

   ```bash
   ssh user1@192.0.2.9
   ```

   Ahora está ejecutando comandos en el servidor Linux remoto.

1. Acceda al modo de superusuario.

   ```bash
   sudo su
   ```

1. Cree un directorio de copia de seguridad. El parámetro -p no hace nada si el directorio ya existe.

   ```bash
   mkdir -p /var/opt/mssql/backup
   ```

1. Mueva el archivo de copia de seguridad a ese directorio. En el ejemplo siguiente, el archivo de copia de seguridad reside en el directorio particular de *user1*. Cambie el comando para que coincida con la ubicación y el nombre del archivo de copia de seguridad.

   ```bash
   mv /home/user1/YourDB.bak /var/opt/mssql/backup/
   ```

1. Salga del modo de superusuario.

   ```bash
   exit
   ```

## <a name="restore-your-database-on-linux"></a>Restauración de la base de datos en Linux

Para restaurar la copia de seguridad de base de datos, puede usar el comando **RESTORE DATABASE** de Transact-SQL (TQL).

> [!NOTE]
> En los pasos siguientes se usa la herramienta **sqlcmd**. Si no ha instalado las Herramientas de SQL Server, consulte [Instalación de las herramientas de línea de comandos de SQL Server en Linux](sql-server-linux-setup-tools.md).

1. En el mismo terminal, inicie **sqlcmd**. En el ejemplo siguiente se conecta a la instancia de SQL Server local con el usuario **SA**. Escriba la contraseña cuando se le solicite o especifíquela mediante la adición del parámetro **-P**.

   ```bash
   sqlcmd -S localhost -U SA
   ```

1. En el símbolo del sistema `>1`, escriba el siguiente comando **RESTORE DATABASE**. Para ello, presione ENTRAR después de cada línea, ya que no puede copiar y pegar a la vez este comando de varias líneas al completo. Reemplace todas las apariciones de `YourDB` por el nombre de la base de datos.

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```

   Debería obtener un mensaje que indica que la base de datos se ha restaurado correctamente.

   `RESTORE DATABASE` puede devolver un error como el del ejemplo siguiente:

   ```bash
   File 'YourDB_Product' cannot be restored to 'Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf'. Use WITH MOVE to identify a valid location for the file.
   Msg 5133, Level 16, State 1, Server servername, Line 1
   Directory lookup for the file "Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf" failed with the operating system error 2(The system cannot find the file specified.).
   ```
   
   En este caso, la base de datos contiene archivos secundarios. Si estos archivos no se especifican en la cláusula `MOVE` de `RESTORE DATABASE`, el procedimiento de restauración intentará crearlos en la misma ruta de acceso que el servidor original. 

   Puede enumerar todos los archivos incluidos en la copia de seguridad:
   ```sql
   RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   GO
   ```
   Debería obtener una lista como la que aparece a continuación (solo se muestran las dos primeras columnas):
   ```sql
   LogicalName         PhysicalName                                                                 ..............
   ----------------------------------------------------------------------------------------------------------------------
   YourDB              Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB.mdf          ..............
   YourDB_Product      Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf  ..............
   YourDB_Customer     Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Customer.ndf ..............
   YourDB_log          Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Log.ldf      ..............
   ```
   
   Puede usar esta lista para crear cláusulas `MOVE` para los archivos adicionales. En este ejemplo, `RESTORE DATABASE` es:

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Product' TO '/var/opt/mssql/data/YourDB_Product.ndf',
   MOVE 'YourDB_Customer' TO '/var/opt/mssql/data/YourDB_Customer.ndf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```


1. Compruebe la restauración mediante la enumeración de todas las bases de datos del servidor. La base de datos restaurada debería aparecer en la lista.

   ```sql
   SELECT Name FROM sys.Databases
   GO
   ```

1. Ejecute otras consultas en la base de datos migrada. El comando siguiente cambia el contexto a la base de datos **YourDB** y selecciona las filas de una de sus tablas.

   ```sql
   USE YourDB
   SELECT * FROM YourTable
   GO
   ```

1. Cuando haya terminado de usar **sqlcmd**, escriba `exit`.

1. Cuando haya terminado de trabajar en la sesión remota de **ssh**, vuelva a escribir `exit`.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a realizar una copia de seguridad de una base de datos en Windows y a moverla a un servidor de Linux con SQL Server. Ha aprendido a:
> [!div class="checklist"]
> * Uso de SSMS y Transact-SQL para crear un archivo de copia de seguridad en Windows
> * Instalación de un shell de Bash en Windows
> * Uso de **scp** para mover archivos de copia de seguridad de Windows a Linux
> * Uso de **ssh** para conectarse de forma remota al equipo Linux
> * Reubicación del archivo de copia de seguridad a fin de prepararlo para la restauración
> * Uso de **sqlcmd** para ejecutar comandos Transact-SQL
> * Restauración de la copia de seguridad de base de datos con el comando **RESTORE DATABASE** 
> * Ejecución de la consulta para comprobar la migración

Explore ahora otros escenarios de migración para SQL Server en Linux. 

> [!div class="nextstepaction"]
>[Migración de bases de datos a SQL Server en Linux](sql-server-linux-migrate-overview.md)