---
title: Copia de seguridad y restauración de bases de datos de SQL Server en Linux
description: Aprenda a realizar copias de seguridad de bases de datos de SQL Server y a restaurarlas en Linux. Obtenga también información sobre cómo realizar copias de seguridad y restaurar con SQL Server Management Studio (SSMS).
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 11/14/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: d30090fb-889f-466e-b793-5f284fccc4e6
ms.openlocfilehash: 92210c7d111f44a965383a8c2099068b2030227e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338286"
---
# <a name="backup-and-restore-sql-server-databases-on-linux"></a>Copia de seguridad y restauración de bases de datos de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Puede realizar copias de seguridad de bases de datos de SQL Server 2017 en Linux con muchas opciones diferentes. En un servidor Linux, puede usar **sqlcmd** para conectarse a SQL Server y realizar copias de seguridad. En Windows, puede conectarse a SQL Server en Linux y realizar copias de seguridad con la interfaz de usuario. La funcionalidad de copia de seguridad es la misma en todas las plataformas. Por ejemplo, puede realizar copias de seguridad de bases de datos localmente, en unidades remotas o en el [servicio de almacenamiento Microsoft Azure Blob](../relational-databases/backup-restore/sql-server-backup-to-url.md).

> [!IMPORTANT]
> SQL Server en Linux solo admite la copia de seguridad en Azure Blob Storage mediante blobs en bloques. El uso de una clave de almacenamiento para la copia de seguridad y la restauración hará que se utilice un blob en páginas, lo que no se admite. En su lugar, utilice una firma de acceso compartido. Para obtener información sobre los blobs en bloques en comparación con los blogs en páginas, consulte [Copia de seguridad en blobs en bloques y blobs en páginas](../relational-databases/backup-restore/sql-server-backup-to-url.md#blockbloborpageblob).

## <a name="backup-a-database"></a>Realizar una copia de seguridad de una base de datos

En el ejemplo siguiente, **sqlcmd** se conecta a la instancia local de SQL Server y realiza una copia de seguridad completa de una base de datos de usuario denominada `demodb`.

```bash
sqlcmd -S localhost -U SA -Q "BACKUP DATABASE [demodb] TO DISK = N'/var/opt/mssql/data/demodb.bak' WITH NOFORMAT, NOINIT, NAME = 'demodb-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
```

Al ejecutar el comando, SQL Server pide una contraseña. Después de escribir la contraseña, el shell devuelve los resultados del progreso de la copia de seguridad. Por ejemplo:

```
Password:
10 percent processed.
21 percent processed.
32 percent processed.
40 percent processed.
51 percent processed.
61 percent processed.
72 percent processed.
80 percent processed.
91 percent processed.
Processed 296 pages for database 'demodb', file 'demodb' on file 1.
100 percent processed.
Processed 2 pages for database 'demodb', file 'demodb_log' on file 1.
BACKUP DATABASE successfully processed 298 pages in 0.064 seconds (36.376 MB/sec).
```

### <a name="backup-the-transaction-log"></a>Realización de una copia de seguridad del registro de transacciones

Si la base de datos está en el modelo de recuperación completa, también puede realizar copias de seguridad del registro de transacciones para obtener opciones de restauración más pormenorizadas. En el ejemplo siguiente, **sqlcmd** se conecta a la instancia local de SQL Server y realiza una copia de seguridad del registro de transacciones.

```bash
sqlcmd -S localhost -U SA -Q "BACKUP LOG [demodb] TO DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak' WITH NOFORMAT, NOINIT, NAME = N'demodb_LogBackup', NOSKIP, NOREWIND, NOUNLOAD, STATS = 5"
```

## <a name="restore-a-database"></a>Restaurar una base de datos

En el ejemplo siguiente, **sqlcmd** se conecta a la instancia local de SQL Server y restaura la base de datos demodb. Observe que se usa la opción `NORECOVERY` para permitir restauraciones adicionales de copias de seguridad de archivos de registro. Si no tiene previsto restaurar otros archivos de registro, quite la opción `NORECOVERY`.

```bash
sqlcmd -S localhost -U SA -Q "RESTORE DATABASE [demodb] FROM DISK = N'/var/opt/mssql/data/demodb.bak' WITH FILE = 1, NOUNLOAD, REPLACE, NORECOVERY, STATS = 5"
```

> [!TIP]
> Si usa NORECOVERY accidentalmente pero no tiene copias de seguridad adicionales de archivos de registro, ejecute el comando `RESTORE DATABASE demodb` sin parámetros adicionales. Esto finaliza la restauración y deja operativa la base de datos.

### <a name="restore-the-transaction-log"></a>Restauración del registro de transacciones

El comando siguiente restaura la copia de seguridad anterior del registro de transacciones.

```bash
sqlcmd -S localhost -U SA -Q "RESTORE LOG demodb FROM DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak'"
```

## <a name="backup-and-restore-with-sql-server-management-studio-ssms"></a>Realización de copias de seguridad y restaurar con SQL Server Management Studio (SSMS)

Puede usar SSMS desde un equipo Windows para conectarse a una base de datos de Linux y realizar una copia de seguridad mediante la interfaz de usuario.

>[!NOTE] 
> Use la versión más reciente de SSMS para conectarse a SQL Server. Para descargar e instalar la versión más reciente, vea [Descarga de SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md). Para obtener más información sobre el uso de SSMS, vea [Usar SQL Server Management Studio en Windows para administrar SQL Server en Linux](sql-server-linux-manage-ssms.md).

Los pasos siguientes le guían durante la realización de una copia de seguridad con SSMS. 

1. Inicie SSMS y conéctese al servidor en SQL Server 2017 en Linux.

1. En el Explorador de objetos, haga clic con el botón derecho en la base de datos, haga clic en **Tareas** y, luego, en **Hacer copia de seguridad...**.

1. En el cuadro de diálogo **Hacer copia de seguridad de base de datos**, compruebe los parámetros y las opciones y haga clic en **Aceptar**.
 
SQL Server completa la copia de seguridad de base de datos.

### <a name="restore-with-sql-server-management-studio-ssms"></a>Restauración con SQL Server Management Studio (SSMS) 

Los siguientes pasos le guían durante la restauración de una base de datos con SSMS.

1. En SSMS, haga clic con el botón derecho en **Bases de datos** y, luego, en **Restaurar bases de datos...**. 

1. En **Origen**, haga clic en **Dispositivo:** y, luego, en los puntos suspensivos (...).

1. Busque el archivo de copia de seguridad de base de datos y haga clic en **Aceptar**. 

1. En **Plan de restauración**, compruebe el archivo de copia de seguridad y la configuración. Haga clic en **Aceptar**. 

1. SQL Server restaura la base de datos. 

## <a name="see-also"></a>Vea también

* [Creación de una copia de seguridad completa de base de datos (SQL Server)](../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)
* [Realización de una copia de seguridad de un registro de transacciones (SQL Server)](../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)
* [BACKUP (Transact-SQL)](../t-sql/statements/backup-transact-sql.md)
* [Copia de seguridad en URL de SQL Server](../relational-databases/backup-restore/sql-server-backup-to-url.md)
