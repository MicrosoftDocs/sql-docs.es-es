---
title: 'Tutorial: Configuración de la replicación (T-SQL)'
description: Configure la replicación de instantáneas de SQL Server en Linux con dos instancias de SQL Server mediante Transact-SQL (T-SQL).
ms.custom: seo-dt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
titleSuffix: SQL Server on Linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 825fec3eeb0abbfacd4bb5f1311e13b6f99189e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346462"
---
# <a name="configure-replication-with-t-sql"></a>Configuración de la replicación con T-SQL

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)] 

En este tutorial se configura la replicación de instantáneas de SQL Server en Linux con dos instancias de SQL Server mediante Transact-SQL. El publicador y el distribuidor son la misma instancia, mientras que el suscriptor está en una instancia independiente.

> [!div class="checklist"]
> * Habilitación de los agentes de replicación de SQL Server en Linux
> * Crear una base de datos de ejemplo
> * Configuración de la carpeta de instantáneas para el acceso de los agentes de SQL Server
> * Configurar el distribuidor
> * Configuración del publicador
> * Configuración de la publicación y los artículos
> * Configuración del suscriptor 
> * Ejecución de los trabajos de replicación

Todas las configuraciones de replicación se pueden definir con [procedimientos almacenados de replicación](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).

## <a name="prerequisites"></a>Prerrequisitos
Para realizar este tutorial, necesita:

- Dos instancias de SQL Server con la versión más reciente de SQL Server en Linux
- Una herramienta para emitir consultas de T-SQL a fin de configurar la replicación, como SQLCMD o SSMS

   Vea [Empleo de SSMS para administrar SQL Server en Linux](./sql-server-linux-manage-ssms.md).

   >[!NOTE]
   >[!INCLUDE[SQL Server 2017](../includes/sssql17-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) y versiones posteriores admiten Replicación de SQL Server para instancias de SQL Server en Linux.

## <a name="detailed-steps"></a>Pasos detallados

1. Habilite los agentes de replicación de SQL Server en Linux. En ambos equipos host, ejecute los siguientes comandos en el terminal. 

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   sudo systemctl restart mssql-server
   ```

1. Cree la base de datos y la tabla de ejemplo. En el publicador, cree una base de datos y una tabla de ejemplo para que actúen como artículos de una publicación.

   ```sql
   CREATE DATABASE Sales
   GO
   USE [SALES]
   GO 
   CREATE TABLE CUSTOMER([CustomerID] [int] NOT NULL, [SalesAmount] [decimal] NOT NULL)
   GO 
   INSERT INTO CUSTOMER (CustomerID, SalesAmount) VALUES (1,100),(2,200),(3,300)
   ```

   En la otra instancia de SQL Server, el suscriptor, cree la base de datos para recibir los artículos.

   ```sql
   CREATE DATABASE Sales
   GO
   ```

1. Cree la carpeta de instantáneas en la que van a leer o escribir los agentes de SQL Server en el distribuidor, cree la carpeta de instantáneas y conceda acceso al usuario "mssql". 

   ```bash
   sudo mkdir /var/opt/mssql/data/ReplData/
   sudo chown mssql /var/opt/mssql/data/ReplData/
   sudo chgrp mssql /var/opt/mssql/data/ReplData/
   ```

1. Configure el distribuidor. En este ejemplo, el publicador también es el distribuidor. Ejecute los siguientes comandos en el publicador para configurar también la instancia de distribución.

   ```sql
   DECLARE @distributor AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @distributor = N'<distributor instance name>'--in this example, it will be the name of the publisher
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 
   
   use master
   exec sp_adddistributor @distributor = @distributor -- this should be the hostname

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   exec sp_adddistributiondb @database = N'distribution', @log_file_size = 2, @deletebatchsize_xact = 5000, @deletebatchsize_cmd = 2000, @security_mode = 0, @login = @distributorlogin, @password = @distributorpassword
   GO

   DECLARE @snapshotdirectory AS nvarchar(500)
   SET @snapshotdirectory = N'/var/opt/mssql/data/ReplData/'

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   use [distribution] 
   if (not exists (select * from sysobjects where name = 'UIProperties' and type = 'U ')) 
          create table UIProperties(id int) 
   if (exists (select * from ::fn_listextendedproperty('SnapshotFolder', 'user', 'dbo', 'table', 'UIProperties', null, null))) 
          EXEC sp_updateextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties' 
   else 
         EXEC sp_addextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties'
   GO
   ```

1. Configure el publicador. Ejecute los siguientes comandos de TSQL en el publicador.

   ```sql
   DECLARE @publisher AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @publisher = N'<instance name>' 
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 

   -- Adding the distribution publishers
   exec sp_adddistpublisher @publisher = @publisher, 
   @distribution_db = N'distribution', 
   @security_mode = 0, 
   @login = @distributorlogin, 
   @password = @distributorpassword, 
   @working_directory = N'/var/opt/mssql/data/ReplData', 
   @trusted = N'false', 
   @thirdparty_flag = 0, 
   @publisher_type = N'MSSQLSERVER'
   GO
   ```

1. Configure el trabajo de publicación. Ejecute los siguientes comandos de TSQL en el publicador.

   ```sql
   DECLARE @replicationdb AS sysname
   DECLARE @publisherlogin AS sysname
   DECLARE @publisherpassword AS sysname
   SET @replicationdb = N'Sales'
   SET @publisherlogin = N'<Publisher login>'
   SET @publisherpassword = N'<Publisher Password>'

   use [Sales]
   exec sp_replicationdboption @dbname = N'Sales', @optname = N'publish', @value = N'true'
   
   -- Add the snapshot publication
   exec sp_addpublication 
   @publication = N'SnapshotRepl', 
   @description = N'Snapshot publication of database ''Sales'' from Publisher ''<PUBLISHER HOSTNAME>''.',
   @retention = 0, 
   @allow_push = N'true', 
   @repl_freq = N'snapshot', 
   @status = N'active', 
   @independent_agent = N'true'

   exec sp_addpublication_snapshot @publication = N'SnapshotRepl', 
   @frequency_type = 1, 
   @frequency_interval = 1, 
   @frequency_relative_interval = 1, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 8, 
   @frequency_subday_interval = 1, 
   @active_start_time_of_day = 0,
   @active_end_time_of_day = 235959, 
   @active_start_date = 0, 
   @active_end_date = 0, 
   @publisher_security_mode = 0, 
   @publisher_login = @publisherlogin, 
   @publisher_password = @publisherpassword
   ```

1. Cree artículos a partir de la tabla de ventas y ejecute los siguientes comandos de TSQL en el publicador.

   ```sql
   use [Sales]
   exec sp_addarticle 
   @publication = N'SnapshotRepl', 
   @article = N'customer', 
   @source_owner = N'dbo', 
   @source_object = N'customer', 
   @type = N'logbased', 
   @description = null, 
   @creation_script = null, 
   @pre_creation_cmd = N'drop', 
   @schema_option = 0x000000000803509D,
   @identityrangemanagementoption = N'manual', 
   @destination_table = N'customer', 
   @destination_owner = N'dbo', 
   @vertical_partition = N'false'
   ```

1. Configure la suscripción. Ejecute los siguientes comandos de TSQL en el publicador.

   ```sql
   DECLARE @subscriber AS sysname
   DECLARE @subscriber_db AS sysname
   DECLARE @subscriberLogin AS sysname
   DECLARE @subscriberPassword AS sysname
   SET @subscriber = N'<Instance Name>' -- for example, MSSQLSERVER
   SET @subscriber_db = N'Sales'
   SET @subscriberLogin = N'<Subscriber Login>'
   SET @subscriberPassword = N'<Subscriber Password>'

   use [Sales]
   exec sp_addsubscription 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @destination_db = @subscriber_db, 
   @subscription_type = N'Push', 
   @sync_type = N'automatic', 
   @article = N'all', 
   @update_mode = N'read only', 
   @subscriber_type = 0

   exec sp_addpushsubscription_agent 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @subscriber_db = @subscriber_db, 
   @subscriber_security_mode = 0, 
   @subscriber_login = @subscriberLogin,
   @subscriber_password = @subscriberPassword,
   @frequency_type = 1,
   @frequency_interval = 0, 
   @frequency_relative_interval = 0, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 0, 
   @frequency_subday_interval = 0, 
   @active_start_time_of_day = 0, 
   @active_end_time_of_day = 0, 
   @active_start_date = 0, 
   @active_end_date = 19950101
   GO
   ```

1. Ejecute los trabajos del agente de replicación. Ejecute la consulta siguiente para obtener una lista de trabajos:

   ```sql
   SELECT name, date_modified FROM msdb.dbo.sysjobs order by date_modified desc
   ```

   Ejecute el trabajo de replicación de instantáneas para generar la instantánea:

   ```sql
   USE msdb;   
   --generate snapshot of publications, for example
   EXEC dbo.sp_start_job N'PUBLISHER-PUBLICATION-SnapshotRepl-1'
   GO
   ```

   Ejecute el trabajo de replicación de instantáneas para generar la instantánea:

   ```sql
   USE msdb;
   --distribute the publication to subscriber, for example
   EXEC dbo.sp_start_job N'DISTRIBUTOR-PUBLICATION-SnapshotRepl-SUBSCRIBER'
   GO
   ```

1. Conéctese al suscriptor y consulte los datos replicados.    

   En el suscriptor, compruebe que la replicación funciona al ejecutar la siguiente consulta:

   ```sql
   SELECT * from [Sales].[dbo].[CUSTOMER]
   ```

En este tutorial se ha configurado la replicación de instantáneas de SQL Server en Linux con dos instancias de SQL Server mediante Transact-SQL.

> [!div class="checklist"]
> * Habilitación de los agentes de replicación de SQL Server en Linux
> * Crear una base de datos de ejemplo
> * Configuración de la carpeta de instantáneas para el acceso de los agentes de SQL Server
> * Configurar el distribuidor
> * Configuración del publicador
> * Configuración de la publicación y los artículos
> * Configuración del suscriptor 
> * Ejecución de los trabajos de replicación

## <a name="see-also"></a>Consulte también

Para obtener más información sobre la replicación, vea la [documentación sobre Replicación de SQL Server](../relational-databases/replication/sql-server-replication.md).

## <a name="next-steps"></a>Pasos siguientes

[Conceptos: Replicación de SQL Server en Linux](sql-server-linux-replication.md)

[Procedimientos almacenados de replicación](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)
