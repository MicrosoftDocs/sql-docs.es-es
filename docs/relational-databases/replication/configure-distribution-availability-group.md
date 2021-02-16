---
title: Configuración de la base de datos de distribución en un grupo de disponibilidad
description: Configure la base de datos de distribución para la replicación de SQL Server con un grupo de disponibilidad AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 01/16/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], distribution
- distribution configuration [SQL Server replication]
- remote Distributors [SQL Server replication]
- transactional replication, configuring distribution
- distribution databases [SQL Server replication], sizing
- Distributors [SQL Server replication], configuring
- distribution databases [SQL Server replication], about distribution databases
- distribution databases [SQL Server replication]
- merge replication [SQL Server replication], configuring distribution
ms.assetid: 94d52169-384e-4885-84eb-2304e967d9f7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 36e2f12793e08211e285c907f26d99e0fcc9262d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348006"
---
# <a name="set-up-replication-distribution-database-in-always-on-availability-group"></a>Configurar la base de datos de distribución de replicación en un grupo de disponibilidad AlwaysOn
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este artículo se explica cómo configurar una base de datos de distribución de replicación de SQL Server en un grupo de disponibilidad (AG) AlwaysOn.

SQL Server 2017 CU6 y SQL Server 2016 SP2-CU3 incorporan compatibilidad con la base de datos de distribución de replicación en un AG mediante los siguientes mecanismos:

- El AG de la base de datos de distribución debe tener un agente de escucha. Cuando el publicador agrega el distribuidor, usa el nombre del agente de escucha como nombre del distribuidor.
- Los trabajos de replicación se crean con el nombre del agente de escucha como nombre del distribuidor. Los trabajos de la instantánea de replicación, el lector de registros y el agente de distribución (suscripción de inserción) creados en el servidor de distribución se crean en todas las réplicas secundarias del grupo de disponibilidad para la base de datos de distribución.
 >[!NOTE]
 >Los trabajos del agente de distribución para las suscripciones de extracción se crean en el servidor del suscriptor y no en el servidor de distribución. 
- Un trabajo nuevo supervisa el estado (principal o secundario en un AG) de las bases de datos de distribución y habilita o deshabilita los trabajos de replicación en función del estado de las bases de datos de distribución.

Después de configurar una base de datos de distribución en el AG según los pasos que se describen a continuación, los trabajos de configuración de replicación y los trabajos de tiempo de ejecución se pueden ejecutar correctamente antes y después de una conmutación por error del AG de la base de datos de distribución.

## <a name="supported-scenarios"></a>Escenarios admitidos

- Configuración de la base de datos de distribución para incluirla en un AG.
- Configuración de la replicación (por ejemplo, publicaciones y suscripciones) antes y después de una conmutación por error del AG.
- Trabajos de replicación funcionales antes y después de una conmutación por error.
- Eliminación de la replicación en el distribuidor y el publicador cuando base de datos de distribución está en el AG.
- Agregación o eliminación de nodos de un AG de base de datos de distribución existente.
- Un distribuidor puede tener varias bases de datos de distribución. Cada base de datos de distribución puede estar en su propio AG y puede no estar en ninguno. Varias bases de datos de distribución pueden compartir un AG.
- El publicador y el distribuidor deben estar en instancias separadas de SQL Server.
- Si el agente de escucha del grupo de disponibilidad que hospeda la base de datos de distribución está configurado para usar un puerto no predeterminado, es necesario configurar un alias para el agente de escucha y el puerto no predeterminado.

## <a name="limitations-or-exclusions"></a>Limitaciones o exclusiones

- No se admiten los distribuidores locales. Por ejemplo, el publicador y el distribuidor deben ser instancias de SQL Server diferentes. Estas instancias se pueden hospedar en los mismos conjuntos de nodos.  Los publicadores que se usan a sí mismos como distribuidores (denominados "distribuidores locales") no admiten las bases de datos de distribución en un AG.
- No se admite el publicador de Oracle.
- No se admite la replicación de mezcla.
- No se admite la replicación transaccional con suscriptores de actualización inmediata o en cola.
- No se admite la replicación punto a punto.
- Todas las instancias de SQL Server 2017 que hospedan réplicas de bases de datos de distribución deben pertenecer a SQL Server 2017 CU 6 o una versión posterior. 
- Todas las instancias de SQL Server 2016 que hospedan réplicas de bases de datos de distribución deben pertenecer a SQL Server 2016 SP2 CU3 o una versión posterior.
- Todas las instancias de SQL Server que hospedan réplicas de base de datos de distribución deben tener la misma versión, excepto durante el período limitado en el que tiene lugar la actualización.
- La base de datos de distribución debe estar en modo de recuperación completa.
- Para efectuar la recuperación y para permitir el truncamiento del registro de transacciones, se deben configurar copias de seguridad de registro completas y de transacciones.
- El AG de la base de datos de distribución debe tener configurado un agente de escucha.
- Las réplicas secundarias de un AG de base de datos de distribución pueden ser sincrónicas o asincrónicas. Se recomienda y es preferible el modo sincrónico.
- No se admite la replicación transaccional bidireccional.
- SSMS no muestra la base de datos de distribución como en proceso de sincronización o sincronizada, cuando se agrega a un grupo de disponibilidad.


   >[!NOTE]
   >Antes de ejecutar cualquiera de los procedimientos almacenados de replicación (por ejemplo, `sp_dropdistpublisher`, `sp_dropdistributiondb`, `sp_dropdistributor`, `sp_adddistributiondb` o `sp_adddistpublisher`) en la réplica secundaria, asegúrese de que la réplica esté totalmente sincronizada.

- Todas las réplicas secundarias de un AG de base de datos de distribución deben ser legibles. Si una réplica secundaria no es legible, no se puede acceder a las propiedades del distribuidor en SQL Server Management Studio en dicha réplica secundaria, pero esta continuará funcionando correctamente. 
- Todos los nodos del AG de base de datos de distribución deben usar la misma cuenta de dominio para ejecutar el Agente SQL Server. Además, esta cuenta de dominio debe tener los mismos privilegios en cada nodo.
- Si cualquiera de los agentes de replicación se ejecutan en una cuenta de proxy, esta debe existir en todos los nodos del AG de base de datos de distribución y deben tener los mismos privilegios en cada nodo.
- Efectúe cambios en las propiedades de base de datos del distribuidor o de la distribución en todas las réplicas que participan en un AG de base de datos de distribución.
- Efectúe cambios en los trabajos de replicación mediante los procedimientos almacenado msdb o con SQL Server Management Studio en todas las réplicas que participan en un AG de base de datos de distribución.
- La configuración del distribuidor en el publicador se debe efectuar con scripts. No se puede usar el asistente de replicación. Se admiten los asistentes de replicación y las hojas de propiedades para otros fines.
- La configuración del AG para las bases de datos de distribución solo se puede efectuar con scripts.
- La configuración de bases de datos de distribución en un AG debe ser una nueva configuración de replicación. No se admite el cambio de una base de datos de distribución existente en un AG. Además, una vez que se elimina un AG a una base de datos de distribución, ya no puede funcionar como una base de datos de distribución válida, por lo que se debe quitar.

## <a name="configuration-architecture"></a>Arquitectura de la configuración

En los ejemplos de este artículo se usan las siguientes opciones y nombres de servidor.

- DIST1, DIST2 y DIST3 son servidores del distribuidor.
- PUB es un servidor del publicador.
- Una vez formado un AG de base de datos de distribución, el nombre del agente de escucha es DISTLISTENER.
- DIST1 está diseñado para ser la réplica principal inicial del AG de base de datos de distribución.

## <a name="configure-distributor-distribution-database-and-publisher"></a>Configurar el distribuidor, la base de datos de distribución y el publicador

En este ejemplo se configura un distribuidor y un publicador nuevos y se coloca la base de datos de distribución en un AG.

### <a name="distributors-workflow"></a>Flujo de trabajo de los distribuidores

1. Configure DIST1, DIST2 y DIST3 como distribuidores con `sp_adddistributor @@servername`. Especifique la contraseña de `distributor_admin` con `@password`. `@password` debería ser igual en DIST1, DIST2 y DIST3.
2. Cree la base de datos de distribución en DIST1 con `sp_adddistributiondb`. El nombre de la base de datos de distribución es `distribution`. Cambie el modo de recuperación de la base de datos `distribution` de simple a completo.
3. Cree un AG para la base de datos `distribution` con réplicas en DIST1, DIST2 y DIST3. Si es posible, todas las réplicas deberían ser sincrónicas. Configure las réplicas secundarias para que sean legibles o para que permitan la lectura. En este momento, las bases de datos de distribución son el AG, DIST1 es la réplica principal y DIST2 y DIST3 son réplicas secundarias.
4. Configure un agente de escucha llamado `DISTLISTENER` para el AG.
5. Para efectuar la recuperación y para permitir el truncamiento del registro de transacciones, se deben configurar copias de seguridad de registro completas y de transacciones.
6. En DIST2 y DIST3, ejecute lo siguiente:

   ```sql
   sp_adddistributiondb 'distribution'
   ```

1. Para agregar `PUB` como publicador en DIST1, ejecute lo siguiente:
   
   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   El valor de `@working_directory` debería ser una ruta de acceso de red independiente de DIST1, DIST2 y DIST3.

1. En DIST2 y DIST3, si la réplica es legible como secundaria, ejecute lo siguiente:  

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Si una réplica no es legible como secundaria, realice la conmutación por error de modo que se convierta en la principal y ejecute lo siguiente: 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   El valor de `@working_directory` debería ser el mismo que en el paso anterior.

### <a name="publisher-workflow"></a>Flujo de trabajo del publicador

Para agregar el agente de escucha del AG de base de datos `distribution` como distribuidor, ejecute lo siguiente en PUB: 

   ```sql
   sp_adddistributor @distributor = 'DISTLISTENER', @password = <distributor_admin password> 
   ```

   El valor de @password debería ser el que se especificó cuando se configuraron los distribuidores en el flujo de trabajo del distribuidor.

## <a name="remove-distributor-and-publisher"></a>Quitar el distribuidor y el publicador

En este ejemplo se quita el publicador y el distribuidor cuando la base de datos de distribución está en el AG.

### <a name="publisher-workflow"></a>Flujo de trabajo del publicador

En PUB, quite todas las suscripciones y publicaciones de este publicador y, después, llame a `sp_dropdistributor`.

### <a name="distributors-workflow"></a>Flujo de trabajo de los distribuidores

En este ejemplo, DIST1 es la réplica principal actual del AG de base de datos `distribution`. DIST2 y DIST3 son réplicas secundarias.

1. En DIST2 y DIST3, ejecute lo siguiente:

   ```sql
   sp_dropdistpublisher 'PUB',  @no_checks = 1
   ```

1. En DIST1, ejecute lo siguiente:

   ```sql
   sp_dropdistpublisher 'PUB'
   ```

1. Elimine el AG.
2. En DIST2 y DIST3, cambie la base de datos `distribution` al modo read_write restaurando la base de datos con una recuperación.
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION
   ```

1. Para quitar la base de datos `distribution` y conservar el directorio de instantáneas, ejecute lo siguiente: 

   ```sql
   sp_dropdistributiondb 'distribution' , @former_ag_secondary=1
   ```

  Con este procedimiento se quitan todos los trabajos pendientes en esta réplica.

1. Para quitar la base de datos `distribution` de DIST1, ejecute lo siguiente:

   ```sql
   sp_dropdistributiondb 'distribution'
   ``` 

1. Si no hay más bases de datos de distribución en el AG, ejecute `sp_dropdistributor` en DIST1, DIST2 y DIST3.

## <a name="add-a-replica-to-distribution-database-ag"></a>Agregar una réplica al AG de base de datos de distribución

En este ejemplo se agrega un distribuidor nuevo a una configuración de replicación existente con la base de datos de distribución del AG. En este ejemplo hay una base de datos de distribución existente que se encuentra en un AG. DIST1 y DIST2 son los distribuidores, `distribution` es la base de datos de distribución del AG y PUB es el publicador. Agregue DIST3 como réplica al AG.

### <a name="distributors-workflow"></a>Flujo de trabajo de los distribuidores

1. DIST3 debería estar configurado como distribuidor con `sp_adddistributor @@servername`. La contraseña de `distributor_admin` debe especificarse con el parámetro @password. La contraseña debe ser la misma que se especificó para DIST1 y DIST2.
2. Agregue DIST3 al AG de la base de datos de distribución actual.
3. En DIST3, ejecute lo siguiente:

   ```sql
   sp_adddistributiondb 'distribution'
   ```

4. En DIST3, si la réplica es legible como secundaria, ejecute lo siguiente: 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   Si la réplica no es legible como secundaria, realice la conmutación por error de modo que se convierta en la principal y ejecute lo siguiente:

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   El valor de `@working_directory` debe ser el mismo que se especificó para DIST1 y DIST2.

4. En DIST3, debe volver a crear servidores vinculados a los suscriptores.

## <a name="remove-a-replica-from-distribution-database-ag"></a>Quitar una réplica del AG de base de datos de distribución

En este ejemplo se quita un distribuidor de un AG de base de datos de distribución actual, mientras que el resto de las réplicas del AG de base de datos de distribución no se ven afectadas. En este ejemplo hay una base de datos de distribución que se encuentra en el AG. DIST1, DIST2 y DIST3 son los distribuidores, `distribution` es la base de datos de distribución del AG y PUB es el publicador. Quite DIST3 del AG.

### <a name="distributors-workflow"></a>Flujo de trabajo de los distribuidores

1. Asegúrese de que DIST3 sea una réplica secundaria del AG de base de datos `distribution`.
2. Quite DIST3 del AG de base de datos `distribution`.
3. En DIST3, cambie la base de datos `distribution` al modo read_write restaurando la base de datos con una recuperación. Por ejemplo, ejecute el siguiente comando:  
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION.
   ```
   
1. Para quitar todos los trabajos huérfanos de DIST3, ejecute lo siguiente: 

   ```sql
   sp_dropdistpublisher 'PUB', @no_checks = 1
   ```

1. En DIST3, ejecute lo siguiente:

   ```sql
   sp_dropdistributiondb 'distribution', @former_ag_secondary=1
   ```

1. En DIST3, ejecute lo siguiente: 

   ```sql
   sp_dropdistributor
   ```

## <a name="remove-a-publisher-from-distribution-database-ag"></a>Quitar un publicador del AG de base de datos de distribución

En este ejemplo se quita un publicador del AG de base de datos de distribución actual de un distribuidor, mientras que el resto de los publicadores atendidos por este AG de base de datos de distribución no se ven afectados. En este ejemplo hay una configuración existente que tiene una base de datos de distribución en un AG. DIST1, DIST2 y DIST3 son los distribuidores, `distribution` es la base de datos de distribución del AG y PUB1 y PUB2 son los publicadores atendidos por la base de datos `distribution`. En el ejemplo se quita PUB1 de estos distribuidores.

### <a name="publisher-workflow"></a>Flujo de trabajo del publicador

En PUB1, quite todas las suscripciones y publicaciones de este publicador y, después, llame a `sp_dropdistributor`.

### <a name="distributor-workflow"></a>Flujo de trabajo del distribuidor

DIST1 es la réplica principal actual del AG de base de datos `distribution`.

1. En DIST2 y DIST3, ejecute lo siguiente:

   ```sql
   sp_dropdistpublisher 'PUB1',  @no_checks = 1
   ```

1. En DIST1, ejecute lo siguiente:

   ```sql
   sp_dropdistpublisher 'PUB1'
   ```

1. En este momento puede que haya trabajos huérfanos relacionados con PUB1 en DIST2 o DIST3. Siempre que se produce una conmutación por error en DIST2 y DIST3, el trabajo `Monitor and sync replication agent jobs` quitará los trabajos huérfanos relacionados con todas las publicaciones de PUB1.

## <a name="add-subscription"></a>Agregar una suscripción

En este ejemplo se aborda la configuración correcta de la información del suscriptor entre varios distribuidores. En el ejemplo se agrega un suscriptor. DIST1 es la réplica principal actual de la base de datos de distribución del AG, mientras que DIST2 y DIST3 son réplicas secundarias de la base de datos de distribución del AG. El nombre del suscriptor es SUB.

### <a name="publisher-workflow"></a>Flujo de trabajo del publicador

En PUB, agregue de la manera habitual una suscripción al suscriptor `SUB`.

### <a name="distributor-workflow"></a>Flujo de trabajo del distribuidor

En DIST2 y DIST3, agregue un servidor vinculado para "SUB" en el caso de que no se haya registrado en DIST2 o DIST3. A continuación se muestra un TSQL de ejemplo para crear un servidor vinculado:

   ```sql 
   EXEC master.dbo.sp_addlinkedserver@server =N'SUB', @srvproduct=N'SQL Server'
   /* For security reasons the linked server remote logins password is changed with ######## */
   EXEC master.dbo.sp_addlinkedsrvlogin@rmtsrvname=N'SUB', @useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL
   ```

## <a name="add-a-pull-subscription"></a>Agregar una suscripción de extracción

### <a name="subscriber-workflow"></a>Flujo de trabajo del suscriptor

Para agregar una suscripción de extracción para una publicación con la base de datos de distribución de un AG, use el nombre del agente de escucha del AG en el parámetro `@distributor` de `sp_addpullsubscription_agent`.

## <a name="sample-t-sql-create-distribution-db-in-ag"></a>T-SQL de ejemplo para crear una base de datos de distribución en un AG

El siguiente script habilita una base de datos de distribución en un grupo de disponibilidad. 

```sql
--- WorkFlow to Enable Distribution Database In AG.

-- SECTION 1 ---- CONFIGURE THE DISTRIBUTOR SERVERS

-- Step1 - Configure the Distribution DB nodes (AG Replicas) to act as a distributor
:Connect SQLNode1
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go 
:Connect SQLNode2
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go

-- Step2 - Configure the Distribution Database
:Connect SQLNode1
USE master
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO
Alter Database [DistributionDB] Set Recovery Full
Go
Backup Database [DistributionDB] to Disk = 'Nul'
Go
-- Step 3 - Create AG for the Distribution DB.
:Connect SQLNode1
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode2
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode1
-- Create the Availability Group
CREATE AVAILABILITY GROUP [DistributionDB_AG]
FOR DATABASE [DistributionDB]
REPLICA ON 'SQLNode1'
WITH (ENDPOINT_URL = N'TCP://SQLNode1.contoso.com:5022', 
         FAILOVER_MODE = AUTOMATIC, 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
         BACKUP_PRIORITY = 50, 
         SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
         SEEDING_MODE = AUTOMATIC),
N'SQLNode2' WITH (ENDPOINT_URL = N'TCP://SQLNode2.contoso.com:5022', 
     FAILOVER_MODE = AUTOMATIC, 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
     BACKUP_PRIORITY = 50, 
     SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
     SEEDING_MODE = AUTOMATIC);
 GO


:Connect SQLNode2
ALTER AVAILABILITY GROUP [DistributionDB_AG] JOIN
GO  
ALTER AVAILABILITY GROUP [DistributionDB_AG] GRANT CREATE ANY DATABASE
Go

--STEP4 - Create the Listener for the Availability Group. This is very important.
:Connect SQLNode1

USE [master]
GO
ALTER AVAILABILITY GROUP [DistributionDB_AG]
ADD LISTENER N'DistributionDBList' (
WITH IP
((N'10.0.0.8', N'255.255.255.0')) , PORT=1500);
GO

-- STEP 5 - Enable SQLNode2 also as a Distributor
:CONNECT SQLNODE2
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO

--STEP 6 - On all Distributor Nodes Configure the Publisher Details 
:CONNECT SQLNODE1
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO
:CONNECT SQLNODE2
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO

-- SECTION 2 ---- CONFIGURE THE PUBLISHER SERVER
:CONNECT SQLNODE4
EXEC sp_addDistributor @distributor = 'DistributionDBList', -- Listener for the Distribution DB.    
    @password = 'Pass@word1'
Go

-- SECTION 3 ---- CONFIGURE THE SUBSCRIBERS 
-- On Publisher, create the publication as one would normally do.
-- On the Secondary replicas of the Distribution DB, add the Subscriber as a linked server.
:CONNECT SQLNODE2
EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE5', @srvproduct=N'SQL Server'
 /* For security reasons the linked server remote logins password is changed with ######## */
EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'SQLNODE5',@useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL 
```

## <a name="see-also"></a>Consulte también  
 [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Proteger el distribuidor](../../relational-databases/replication/security/secure-the-distributor.md)  
  
## <a name="next-steps"></a>Pasos siguientes
 [Ver y modificar las propiedades del distribuidor y del publicador](view-and-modify-distributor-and-publisher-properties.md)  
 [Deshabilitar la publicación y la distribución](disable-publishing-and-distribution.md)  
 [Habilitar una base de datos para replicación (SQL Server Management Studio)](enable-a-database-for-replication-sql-server-management-studio.md) 
