---
title: 'Inicio rápido: Copia de seguridad y restauración de una base de datos'
titleSuffix: SQL Server
description: En este artículo, obtendrá información sobre cómo crear una nueva base de datos, realizar una copia de seguridad de la base de datos y restaurar la copia de seguridad en SQL Server.
author: cawrites
ms.author: chadam
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: backup-restore
ms.prod_service: backup-restore
ms.assetid: ''
ms.openlocfilehash: 66b4277674c1144a716e5153a9fa062f988c75c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340213"
---
# <a name="quickstart-backup-and-restore-a-sql-server-database-on-premises"></a>Inicio rápido: Copias de seguridad y restauración de bases de datos de SQL Server en el entorno local
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este inicio rápido, creará una base de datos, realizará una simple copia de seguridad de ella y luego la restaurará. 

Para un procedimiento más detallado, vea [Crear una copia de seguridad completa de base de datos (SQL Server)](create-a-full-database-backup-sql-server.md) y [Restore a backup using SSMS](restore-a-database-backup-using-ssms.md) (Restaurar una copia de seguridad con SSMS).

## <a name="prerequisites"></a>Requisitos previos
Para completar este inicio rápido, necesitará lo siguiente: 

- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)
- [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)

## <a name="create-a-test-database"></a>Creación de una base de datos de prueba 

1. Inicie [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) y conéctese a la instancia de SQL Server.
1. Abra una ventana de **nueva consulta**. 
1. Ejecute el siguiente código Transact-SQL (T-SQL) para crear la base de datos de prueba. Actualice el nodo **Bases de datos** en el **Explorador de objetos** para ver la nueva base de datos. 

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
    dt1 DATETIME NOT NULL DEFAULT getdate()
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
 
## <a name="take-a-backup"></a>Realizar una copia de seguridad
Para realizar una copia de seguridad de la base de datos, haga lo siguiente: 

1. Inicie [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) y conéctese a la instancia de SQL Server.
1. Expanda el nodo **Bases de datos** del **Explorador de objetos**.  
1. Haga clic con el botón derecho en la base de datos, mantenga el puntero sobre **Tareas** y seleccione **Hacer copia de seguridad…** 
1. En **Destino**, confirme que la ruta de acceso de la copia de seguridad es correcta. Si necesita cambiarla, seleccione **Quitar** para quitar la ruta de acceso existente y luego, **Agregar** para escribir una nueva ruta de acceso. Puede usar el botón de puntos suspensivos para desplazarse a un archivo específico. 
1. Seleccione **Aceptar** para realizar una copia de seguridad de la base de datos. 

![Realizar copia de seguridad SQL](media/quickstart-backup-restore-database/backup-db-ssms.png)

También puede ejecutar el siguiente comando de Transact-SQL para realizar copias de seguridad de la base de datos: 

```sql
BACKUP DATABASE [SQLTestDB] 
TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' 
WITH NOFORMAT, NOINIT,  
NAME = N'SQLTestDB-Full Database Backup', SKIP, NOREWIND, NOUNLOAD,  STATS = 10
GO
```


## <a name="restore-a-backup"></a>Restaurar una copia de seguridad
Para restaurar la base de datos, haga lo siguiente: 

1. Inicie [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) y conéctese a la instancia de SQL Server.
1. Haga clic con el botón derecho en la carpeta **Bases de datos** en **Explorador de objetos** y seleccione **Restaurar base de datos…**

    ![Restaurar una base de datos](media/quickstart-backup-restore-database/restore-db-ssms1.png)

1. Seleccione **Dispositivo:** y luego, el botón de puntos suspensivos (…) para buscar el archivo de copia de seguridad. 
1. Seleccione **Agregar** y navegue al lugar donde se encuentra su archivo `.bak`. Seleccione el archivo `.bak` y luego, **Aceptar**. 
1. Seleccione **Aceptar** para cerrar el cuadro de diálogo **Seleccionar dispositivos de copia de seguridad**. 
1. Seleccione **Aceptar** para restaurar la copia de seguridad de la base de datos. 

    ![Restaurar la base de datos](media/quickstart-backup-restore-database/restore-db-ssms2.png)

También puede ejecutar el siguiente script de Transact-SQL para restaurar la base de datos:

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] 
FROM DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

### <a name="clean-up-resources"></a>Limpieza de recursos
Ejecute el siguiente comando de Transact-SQL para quitar la base de datos que creó, además de su historial de copia de seguridad en la base de datos MSDB:

```sql
EXEC msdb.dbo.sp_delete_database_backuphistory @database_name = N'SQLTestDB'
GO

USE [master]
DROP DATABASE [SQLTestDB]
GO
```

## <a name="see-more"></a>Ver más
[Realizar copias de seguridad y restaurar bases de datos de SQL Server](back-up-and-restore-of-sql-server-databases.md)
[Copia de seguridad en URL de SQL Server](sql-server-backup-to-url.md)
[Crear una copia de seguridad completa de base de datos (SQL Server)](create-a-full-database-backup-sql-server.md)
[Restore a database backup](restore-a-database-backup-using-ssms.md) (Restaurar una copia de seguridad de base de datos)
