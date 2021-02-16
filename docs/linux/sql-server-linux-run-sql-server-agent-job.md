---
title: Creación y ejecución de trabajos de SQL Server en Linux
description: Obtenga información sobre cómo crear un trabajo del Agente SQL Server en Linux con Transact-SQL y SQL Server Management Studio (SSMS).
author: VanMSFT
ms.author: vanto
ms.date: 02/20/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1d93d95e-9c89-4274-9b3f-fa2608ec2792
ms.openlocfilehash: bc52f0a2052d0b9abd1700e92af3a30c0e02bad2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346447"
---
# <a name="create-and-run-sql-server-agent-jobs-on-linux"></a>Creación y ejecución de trabajos del Agente SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Los trabajos de SQL Server sirven para realizar regularmente la misma secuencia de comandos en la base de datos de SQL Server. En este tutorial se muestra un ejemplo de cómo crear un trabajo del Agente SQL Server en Linux con Transact-SQL y SQL Server Management Studio (SSMS).

> [!div class="checklist"]
> * Instalar el Agente SQL Server en Linux
> * Crear un trabajo que haga una copia de seguridad de base de datos cada día
> * Programar y ejecutar el trabajo
> * Realizar los mismos pasos en SSMS (opcional)

Para ver los problemas conocidos del Agente SQL Server en Linux, consulte las [notas de la versión](sql-server-linux-release-notes.md).

## <a name="prerequisites"></a>Prerequisites

Debe disponer de lo siguiente para poder completar este tutorial:

* Un equipo Linux con lo siguiente:
  * SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md) o [Ubuntu](quickstart-install-connect-ubuntu.md)) con herramientas de línea de comandos

Lo siguiente es opcional:

* Equipo Windows con SSMS:
  * [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) para realizar los pasos opcionales de SSMS

## <a name="enable-sql-server-agent"></a>Habilitar el Agente SQL Server

Para usar el Agente SQL Server en Linux, primero debe habilitarlo en un equipo que ya tenga SQL Server instalado.

1. Haga lo siguiente para habilitar el Agente SQL Server.
  ```bash
  sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
  ```

1. Reinicie SQL Server con el siguiente comando:
  ```bash
  sudo systemctl restart mssql-server
  ```

> [!NOTE]
> A partir de SQL Server 2017 CU4, el Agente SQL Server se incluye con el paquete **mssql-server** y está deshabilitado de forma predeterminada. Para instalar el Agente antes de CU4, vea [Instalación del Agente SQL Server en Linux](sql-server-linux-setup-sql-agent.md).

## <a name="create-a-sample-database"></a>Crear una base de datos de ejemplo

Use los siguientes pasos para crear una base de datos de ejemplo denominada **SampleDB**. Esta base de datos se usará para el trabajo de copia de seguridad diario.

1. En el equipo Linux, abra una sesión de terminal de Bash.

1. Use **sqlcmd** para ejecutar un comando **CREATE DATABASE** de Transact-SQL.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'CREATE DATABASE SampleDB'
   ```

1. Para comprobar que la base de datos se ha creado, muestre una lista de las bases de datos del servidor.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -Q 'SELECT Name FROM sys.Databases'
   ```

## <a name="create-a-job-with-transact-sql"></a>Crear un trabajo con Transact-SQL

Con los siguientes pasos se crea un trabajo del Agente SQL Server en Linux con comandos Transact-SQL. El trabajo ejecuta una copia de seguridad diaria de la base de datos de ejemplo, **SampleDB**.

> [!TIP]
> Puede usar cualquier cliente de T-SQL para ejecutar estos comandos. Por ejemplo, en Linux puede usar [sqlcmd](sql-server-linux-setup-tools.md) o [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md). Desde un servidor Windows Server remoto, también puede ejecutar consultas en SQL Server Management Studio (SSMS) o usar la interfaz de usuario para la administración de trabajos (que explicaremos en la siguiente sección).

1. Use [sp_add_job](../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md) para crear un trabajo denominado `Daily SampleDB Backup`.

   ```sql
   -- Adds a new job executed by the SQLServerAgent service
   -- called 'Daily SampleDB Backup'
   USE msdb ;
   GO
   EXEC dbo.sp_add_job
      @job_name = N'Daily SampleDB Backup' ;
   GO
   ```

1. Llame a [sp_add_jobstep](../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md) para crear un paso de trabajo que cree una copia de seguridad de la base de datos `SampleDB`.

   ```sql
   -- Adds a step (operation) to the job
   EXEC sp_add_jobstep
      @job_name = N'Daily SampleDB Backup',
      @step_name = N'Backup database',
      @subsystem = N'TSQL',
      @command = N'BACKUP DATABASE SampleDB TO DISK = \
         N''/var/opt/mssql/data/SampleDB.bak'' WITH NOFORMAT, NOINIT, \
         NAME = ''SampleDB-full'', SKIP, NOREWIND, NOUNLOAD, STATS = 10',
      @retry_attempts = 5,
      @retry_interval = 5 ;
   GO
   ```

1. Luego, cree una programación diaria para el trabajo con [sp_add_schedule](../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md).

   ```sql
   -- Creates a schedule called 'Daily'
   EXEC dbo.sp_add_schedule
      @schedule_name = N'Daily SampleDB',
      @freq_type = 4,
      @freq_interval = 1,
      @active_start_time = 233000 ;
   USE msdb ;
   GO
   ```

1. Adjunte la programación del trabajo al trabajo con [sp_attach_schedule](../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md).

   ```sql
   -- Sets the 'Daily' schedule to the 'Daily SampleDB Backup' Job
   EXEC sp_attach_schedule
      @job_name = N'Daily SampleDB Backup',
      @schedule_name = N'Daily SampleDB';
   GO
   ```

1. Use [sp_add_jobserver](../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md) para asignar el trabajo a un servidor de destino. En este ejemplo, el destino es el servidor local.

   ```sql
   EXEC dbo.sp_add_jobserver
      @job_name = N'Daily SampleDB Backup',
      @server_name = N'(LOCAL)';
   GO
   ```
1. Inicie el trabajo con [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md).

   ```sql
   EXEC dbo.sp_start_job N' Daily SampleDB Backup' ;
   GO
   ```

## <a name="create-a-job-with-ssms"></a>Crear un trabajo con SSMS

También puede crear y administrar trabajos de forma remota mediante SQL Server Management Studio (SSMS) en Windows.

1. Inicie SSMS en Windows y conéctese a la instancia de SQL Server de Linux. Para más información, vea [Administrar SQL Server en Linux con SSMS](sql-server-linux-manage-ssms.md).

1. Confirme que ha creado una base de datos de ejemplo denominada **SampleDB**.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-0.png" alt="Create a SampleDB database" style="width: 550px;"/>

1. Compruebe que el Agente SQL se ha [instalado](sql-server-linux-setup-sql-agent.md) y configurado correctamente. Busque el signo más junto al Agente SQL Server en el Explorador de objetos. Si el Agente SQL Server no está habilitado, pruebe a reiniciar el servicio **mssql-server** en Linux.

   ![Comprobar que el Agente SQL Server se ha instalado](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-1.png)

1. Cree un trabajo.

   ![Crear un trabajo](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-2.png)

1. Asigne un nombre al trabajo y cree el paso de trabajo.

   ![Crear un paso de trabajo](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-3.png)

1. Especifique qué subsistema desea usar y qué debe hacer el paso de trabajo.

   ![Subsistema del trabajo](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-4.png)

   ![Acción del paso de trabajo](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-5.png)

1. Cree una programación.

   ![Captura de pantalla del cuadro de diálogo Nuevo trabajo con la opción Programaciones y el botón Nueva resaltados.](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-6.png)

   ![Captura de pantalla del cuadro de diálogo Nuevo trabajo con la opción Aceptar resaltada.](./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-8.png)

1. Inicie el trabajo.

   <img src="./media/sql-server-linux-run-sql-server-agent-job/ssms-agent-9.png" alt="Start the SQL Server Agent job" style="width: 550px;"/>

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Instalar el Agente SQL Server en Linux
> * Usar Transact-SQL y procedimientos almacenados del sistema para crear trabajos
> * Crear un trabajo que realice copias de seguridad de base de datos diarias
> * Usar la interfaz de usuario de SSMS para crear y administrar trabajos

Tras esto, explore otras funcionalidades para crear y administrar trabajos:

> [!div class="nextstepaction"]
>[Documentación del Agente SQL Server](../ssms/agent/sql-server-agent.md)