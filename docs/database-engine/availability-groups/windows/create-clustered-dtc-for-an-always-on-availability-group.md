---
title: Creación de un recurso DTC agrupado para un grupo de disponibilidad
description: Este tema le guiará a través de una configuración completa de un recurso DTC agrupado para un grupo de disponibilidad AlwaysOn de SQL Server.
ms.custom: seodec18
ms.date: 08/30/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 0e332aa4-2c48-4bc4-a404-b65735a02cea
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4974d6e0dcacfeae0c37092ea43bb529177c174b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350815"
---
# <a name="create-clustered-dtc-resource-for-an-always-on-availability-group"></a>Creación de un recurso DTC agrupado para un grupo de disponibilidad Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

Este tema le guiará a través de una configuración completa de un recurso DTC agrupado para un grupo de disponibilidad AlwaysOn de SQL Server. La configuración completa puede tardar una hora en completarse. 

En el tutorial se crea un recurso DTC agrupado y los grupos de disponibilidad de SQL Server para adaptarlos a los requisitos de [DTC agrupado para grupos de disponibilidad de SQL Server](../../../database-engine/availability-groups/windows/cluster-dtc-for-sql-server-2016-availability-groups.md).

El tutorial usa scripts de PowerShell y Transact-SQL (T-SQL).  Muchos de los scripts de T-SQL requieren que la opción **Modo SQLCMD** esté habilitada.  Para más información sobre la opción **Modo SQLCMD**, vea [Habilitar scripting SQLCMD en el Editor de consultas](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md).  El módulo de PowerShell **FailoverClusters** debe importarse.  Para obtener más información sobre la importación de un módulo de PowerShell, vea [Importación de un módulo de PowerShell](/powershell/scripting/developer/module/importing-a-powershell-module).  Este tutorial se basa en lo siguiente:
- Se han cumplido todos los requisitos de [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
- El dominio es `contoso.lab`.
- El usuario tiene el permiso para crear objetos de equipo en la UO donde se creará el recurso de nombre de red de DTC.
- El usuario es un usuario de dominio que tiene derechos de administrador en todos los nodos del clúster.
- Se ha creado un recurso compartido de archivos llamado `sqlbackups` para las copias de seguridad.
- Las instancias predeterminadas de SQL Server se denominan `SQLNODE1` y `SQLNODE2`.
- Se utiliza la misma cuenta de servicio en todas las instancias de SQL Server.
- El usuario es miembro del rol sysadmin fijo de SQL Server en todas las instancias de SQL Server.
- El resultado predeterminado de las transacciones que DTC no puede resolver se establecerá en `presume commit`.
- El punto de conexión de reflejo usará el puerto `5022`.
- No existen otros grupos de disponibilidad o recursos DTC agrupados.
- Detalles del clúster (existente):
  - Nombre: `Cluster`
  - Nombre de red: `Cluster Network 1`
  - Nodos: `SQLNODE1, SQLNODE2`
  - Almacenamiento compartido: `Cluster Disk 3` (propiedad de `SQLNODE1`)
- Detalles del clúster (para crear):
  - Recurso de nombre de red: `DTCnet1`
  - Recurso de nombre de red de DTC: `DTC1`
  - Recurso de disco físico de DTC: `DTCDisk1`
  - Recurso de subred y dirección IP de DTC: `192.168.2.54`, `255.255.255.0`
  - Recurso de dirección IP de DTC: `DTCIP1`

## <a name="1-check-operating-system"></a>1. Comprobar el sistema operativo
Para transacciones distribuidas compatibles, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] se debe ejecutar en Windows Server 2016 o Windows Server 2012 R2.  Para Windows Server 2012 R2, debe instalar la actualización de KB3090973 disponible en [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  Este script comprobará la versión del sistema operativo y si es necesario instalar la revisión 3090973.  Ejecute el siguiente script de PowerShell en `SQLNODE1`.

```powershell  
# A few OS checks

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {

    # At least 2012 R2
    $os = (Get-WmiObject -class Win32_OperatingSystem -ComputerName $node).caption;
    IF($os -like "*2012 R2*" -or $os -like "*2016*")
    {
        Write-Host "$os is supported on $node.";
    }
    ELSE
    {
        Write-Host "STOP!  $os is not supported on $node.";
    }
    
    # KB 3090973
    IF($os -like "*2012 R2*")
    {
        $kb = Get-Hotfix -ComputerName $node | Where {$_.HotFixID -eq 'KB3090973'};
        IF($kb)
        {
            Write-Host "KB3090973 is installed on $node."
        }
        ELSE
        {
            Write-Host "HotFixID KB3090973 must be applied on $node.  See https://support.microsoft.com/kb/3090973 for additional information and to download the hotfix.";
        }
    }
    ELSE
    {
        Write-Host "KB3090973 is not applicable to $os on $node."
    }
}
```  
## <a name="2---configure-firewall-rules"></a>2.   Configurar reglas de firewall
Este script configurará el firewall para permitir el tráfico DTC, en cada servidor SQL Server que hospede una réplica del grupo de disponibilidad, así como en cualquier otro servidor implicado en la transacción distribuida.  El script también configurará el firewall de modo que permita las conexiones del punto de conexión de reflejo de la base de datos.  Ejecute el siguiente script de PowerShell en `SQLNODE1`.

```powershell  
# Configure Firewall

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$nodes = (Get-ClusterNode).Name;
foreach ($node in $nodes) {
    Get-NetFirewallRule -CimSession $node -DisplayGroup 'Distributed Transaction Coordinator' -Enabled False -ErrorAction SilentlyContinue | Enable-NetFirewallRule;
    New-NetFirewallRule -CimSession $node -DisplayName 'SQL Server Mirroring' -Description 'Port 5022 for SQL Server Mirroring' -Action Allow -Direction Inbound -Protocol TCP -LocalPort 5022 -RemotePort Any -LocalAddress Any -RemoteAddress Any;
    };
```  
## <a name="3--configure-in-doubt-xact-resolution"></a>3.  Configurar **in-doubt xact resolution** 
Este script establecerá la opción de configuración de servidor **in-doubt xact resolution** para "suponer la confirmación" para las transacciones dudosas.  Ejecute el siguiente script T-SQL en SQL Server Management Studio (SSMS) contra `SQLNODE1` en el **modo SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Configure in-doubt xact resolution on all SQL Server instances to presume commit
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF (SELECT CAST(value_in_use as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'show advanced options') = 0
BEGIN   
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
END

-- Configure the server to presume commit for in-doubt transactions.
IF (SELECT CAST(value as bit) FROM sys.configurations WITH (NOLOCK) WHERE [name] = N'in-doubt xact resolution') <> 1
BEGIN   
    EXEC sp_configure 'in-doubt xact resolution', 1;
    RECONFIGURE;
END
GO
-----------------------------------------------------------------------------
```

## <a name="4-create-test-databases"></a>4. Crear bases de datos de prueba
El script creará una base de datos denominada `AG1` en `SQLNODE1` y una base de datos denominada `dtcDemoAG1` en `SQLNODE2`.  Ejecute el siguiente script T-SQL en SSMS contra `SQLNODE1` en el **modo SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- On SQLNODE1 
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'AG1')
BEGIN
    DROP DATABASE AG1;
END
GO

CREATE DATABASE AG1;
ALTER DATABASE AG1 SET RECOVERY FULL WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::AG1 to sa;
GO

USE AG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );

INSERT Names
VALUES ('AG1', GETDATE());
GO


-- Against SQNODE2
:connect SQLNODE2
USE master;
SET NOCOUNT ON;

IF  EXISTS (SELECT * FROM sys.databases WHERE name = N'dtcDemoAG1')
BEGIN
    DROP DATABASE dtcDemoAG1;
END
GO

CREATE DATABASE dtcDemoAG1;
ALTER DATABASE dtcDemoAG1 SET RECOVERY SIMPLE WITH NO_WAIT;
ALTER AUTHORIZATION ON DATABASE::dtcDemoAG1 to sa;
GO

USE dtcDemoAG1;
CREATE TABLE [dbo].[Names] (
        [Name] [varchar](64) NULL,
        [EditDate] datetime
        );
GO    
----------------------------------------------------------------
```
## <a name="5---create-endpoints"></a>5.   Crear los puntos de conexión
Este script creará un punto de conexión denominado `AG1_endpoint` que escucha en el puerto TCP `5022`.  Ejecute el siguiente script T-SQL en SSMS contra `SQLNODE1` en el **modo SQLCMD**.

```sql  
/**********************************************
Execute on SQLNODE1 in SQLCMD mode
**********************************************/

-- Create endpoint on server instance that hosts the primary replica:
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------

:connect SQLNODE2
IF NOT EXISTS (SELECT * FROM sys.database_mirroring_endpoints)
BEGIN
    CREATE ENDPOINT AG1_endpoint
    AUTHORIZATION [sa]
        STATE=STARTED 
        AS TCP (LISTENER_PORT=5022) 
        FOR DATABASE_MIRRORING (ROLE=ALL);
END
GO
-----------------------------------------------------------------------------
```

## <a name="6---prepare-databases-for-availability-group"></a>6.   Preparar las bases de datos para el grupo de disponibilidad
El script realizará una copia de seguridad de `AG1` en `SQLNODE1` y la restaurará en `SQLNODE2`.  Ejecute el siguiente script T-SQL en SSMS contra `SQLNODE1` en el **modo SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

-- Backup database
BACKUP DATABASE AG1 
TO DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH FORMAT, STATS = 10;

-- Backup transaction log
BACKUP LOG AG1
TO DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH FORMAT, STATS = 10;
GO


-- Restore database and logs on secondary WITH NORECOVERY
:connect SQLNODE2
USE [master]
RESTORE DATABASE AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1.bak' 
WITH NORECOVERY, STATS = 10;

RESTORE LOG AG1 
FROM DISK = N'\\sqlnode1\sqlbackups\AG1_Log.bak'
WITH NORECOVERY, STATS = 10;
GO
```

## <a name="7---create-availability-group"></a>7.   Crear grupo de disponibilidad
Los [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] deben crearse con el comando **CREATE AVAILABILITY GROUP** y la cláusula **WITH DTC_SUPPORT = PER_DB**.  Actualmente no se puede modificar un grupo de disponibilidad existente.  El asistente Nuevo grupo de disponibilidad no permite habilitar la compatibilidad con DTC para un nuevo grupo de disponibilidad.  El siguiente script creará el nuevo grupo de disponibilidad y unirá el secundario.  Ejecute el siguiente script T-SQL en SSMS contra `SQLNODE1` en el **modo SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
*******************************************************************/

--  Create Availability Group on SQLNODE1 
USE master;
CREATE AVAILABILITY GROUP DTCAG1
WITH (DTC_SUPPORT = PER_DB) 
FOR DATABASE AG1
REPLICA ON 
  'SQLNODE1' WITH
     (
     ENDPOINT_URL = 'TCP://SQLNODE1.contoso.lab:5022', 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ),
  'SQLNODE2' WITH 
     (
     ENDPOINT_URL = 'TCP://SQLNODE2.contoso.lab:5022',
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
     FAILOVER_MODE = MANUAL
     ); 
GO


-- SQLNODE2
-- Join secondary replica to the Availability Group 
:connect SQLNODE2
ALTER AVAILABILITY GROUP DTCag1 JOIN;

-- Join database to the Availability Group
ALTER DATABASE AG1 SET HADR AVAILABILITY GROUP = DTCAG1;
GO
```

> [!IMPORTANT]
> No puede habilitar DTC en un [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] existente.  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aceptará la siguiente sintaxis para un grupo de disponibilidad existente:  
> 
> USE master;    
> ALTER AVAILABILITY GROUP \<availability_group\>  
> SET (DTC_Support = Per_DB)  
> 
> Sin embargo, en realidad no se realizará ningún cambio de configuración.  Puede confirmar la configuración **dtc_support** con la siguiente consulta T-SQL:  
> 
> SELECT name, dtc_support FROM sys.availability_groups  
> 
> La única manera de habilitar la compatibilidad con DTC en un grupo de disponibilidad es creando un grupo de disponibilidad mediante Transact-SQL.
 
## <a name="8--prepare-cluster-resources"></a><a name="ClusterDTC"></a>8.  Preparar recursos de clúster

Este script preparará los recursos dependientes de DTC: disco y dirección IP.  El almacenamiento compartido se agregará a Windows Cluster.  Se crearán los recursos de red y, luego, DTC se creará y convertirá en un recurso en el grupo de disponibilidad.  Ejecute el siguiente script de PowerShell en `SQLNODE1`. Gracias a [Allan Hirt](https://sqlha.com/2013/03/12/how-to-properly-configure-dtc-for-clustered-instances-of-sql-server-with-windows-server-2008-r2/) por el script.

```powershell  
# Create a clustered Microsoft Distributed Transaction Coordinator properly in the resource group with SQL Server

\<#----------------------------------- Begin User Input -----------------------------------#>
$AGgrp = "DTCag1";                          # Name of the WSFC resource group that will contain the DTC resource

$WSFC = (Get-Cluster).Name;                 # Windows Failover Cluster name
$DTCnetwk = "Cluster Network 1"             # WSFC Network to use for the DTC IP address

$ClusterAvailableDisk = "Cluster Disk 3";   # Designated disk that can support failover clustering and is visible to all nodes, but not yet part of the set of clustered disks
$DTCdisk = "DTCDisk1";                      # Name of the disk to be used with DTC

$DTCipresnm = "DTCIP1";                     # WSFC Friendly Name of the DTC's IP resource 
$DTCipaddr = "192.168.2.54";                # IP address of the DTC resource 
$DTCsubnet = "255.255.255.0";               # Subnet for the DTC IP address 
$DTCnetnm = "DTCNet1";                      # WSFC Friendly Name of the Network Name resource
$DTCresnm = "DTC1";                         # Name of the WSFC DTC Network Name resource; Name must be unique in AD
\<#------------------------------------ End User Input ------------------------------------#>


# Make a new disk available for use in a failover cluster.
Get-ClusterAvailableDisk | Where {$_.Name -eq $ClusterAvailableDisk} | Add-ClusterDisk;

# Rename disk
$resource = Get-ClusterResource $ClusterAvailableDisk; $resource.Name = $DTCdisk;

# Create the IP resource
Add-ClusterResource -Name $DTCipresnm -ResourceType "IP Address" -Group $AGgrp;

# Set the network to use, IP address, and subnet
# All three have to be configured at the same time
$DTCIPres = Get-ClusterResource $DTCipresnm;
$ntwk = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Network,$DTCnetwk;
$ipaddr = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,Address,$DTCipaddr;
$subnet = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $DTCipres,SubnetMask,$dtcsubnet;

$setdtcipparams = $ntwk,$ipaddr,$subnet;
$setdtcipparams | Set-ClusterParameter;

# Create the Network Name resource
Add-ClusterResource $DTCnetnm -ResourceType "Network Name" -Group $AGgrp;

# Set the value for the Network Name resource
Get-ClusterResource $DTCnetnm | Set-ClusterParameter DnsName $DTCresnm;

# Add the IP address as a depenency of the Network Name resource
Add-ClusterResourceDependency $DTCnetnm $DTCipresnm;

# Create the Distributed Transaction Coordinator resource
Add-ClusterResource $DTCresnm -ResourceType "Distributed Transaction Coordinator" -Group $AGgrp;

# Add the Network Name as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCnetnm;

# Move the disk into the resource group with SQL Server
Move-ClusterResource -Name $DTCdisk -Group $AGgrp;

# Add the disk as a depenency of the DTC resource
Add-ClusterResourceDependency $DTCresnm $DTCdisk;

# Bring the IP resource online
Start-ClusterResource $DTCipresnm;

# Bring the Network Name resource online
Start-ClusterResource $DTCnetnm;

# Bring the DTC resource online
Start-ClusterResource $DTCresnm;
```  

## <a name="9---enable-network-dtc"></a>9.   Habilitar DTC de red 

El siguiente script permitirá el acceso DTC de red para el servicio DTC agrupado para permitir que los equipos remotos participen en transacciones distribuidas a través de la red.  Ejecute el siguiente script de PowerShell en `SQLNODE1`.

```powershell  
# Enable Network DTC access for the clustered DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

# Enter Name of DTC resource

$DtcName = "DTC1";
\<# ------- End of User Input ------- #>

[bool]$restart = 0;
$node = (Get-ClusterResource -Name $DtcName).OwnerNode.Name;
$DtcSettings = Get-DtcNetworkSetting -DtcName $DtcName;

IF ($DtcSettings.InboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -InboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($DtcSettings.OutboundTransactionsEnabled -eq $false)   
{
    Set-DtcNetworkSetting -CimSession $node -DtcName $DtcName -AuthenticationLevel "Mutual" -OutboundTransactionsEnabled $true -Confirm:$false;
    $restart = 1;
}

IF ($restart -eq 1)
{
    Stop-Dtc -CimSession $node -DtcName $DTCname -Confirm:$false;
    Start-Dtc -CimSession $node -DtcName $DTCname;
}
```  

## <a name="10--disable-and-stop-the-local-dtc-service-on-each-node"></a>10.  Deshabilitar y detener el servicio DTC local en cada nodo

Para garantizar que las transacciones distribuidas usen el recurso DTC agrupado, deshabilite el DTC local en ambos nodos.  El siguiente script deshabilitará y detendrá el servicio DTC local en cada nodo.  Ejecute el siguiente script de PowerShell en `SQLNODE1`.
```powershell  
# Disable local DTC service

\<#
Script: 
    1) is re-runnable 
    2) will work on any node in the cluster, and 
    3) will execute against every node in the cluster
#>

$DTCname = 'Local';
$nodes = (Get-ClusterNode).Name;

 foreach ($node in $nodes) {

    $service = Get-WmiObject -class Win32_Service -computername $node -Filter "Name='MSDTC'";
    IF ($service.StartMode -ne 'Disabled')
    {
        $service.ChangeStartMode('Disabled');
    }
    
    IF ($service.State -ne 'Stopped')
    {
        $service.StopService();
    }
}
```  

## <a name="11--cycle-the-ssnoversion-service-for-each-instance"></a>11.  Recorrer el servicio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para cada instancia

Con el servicio DTC agrupado completamente configurado, debe detener y reiniciar cada instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] del grupo de disponibilidad para asegurarse de que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esté inscrito para usar este servicio DTC.

La primera vez que el servicio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] necesita una transacción distribuida, se inscribe con un servicio DTC. El servicio SQL Server seguirá usando ese servicio DTC hasta que se reinicie. Si un servicio DTC agrupado está disponible, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se inscribirá con él. Si hay un servicio DTC agrupado no está disponible, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se inscribirá con él. Para comprobar que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se inscribe con el servicio DTC agrupado, detenga y reinicie cada instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 

Siga los pasos contenidos en el siguiente script T-SQL:
```sql  
/*
Gracefully cycle the SQL Server service and failover the Availability Group
    a.  On SQLNODE2, cycle the SQL Server service from SQL Server Configuration Manger

    b.  On SQLNODE2 failover the Availability Group to SQLNODE2
        Execute T-SQL script, below, on SQLNODE2 (Use Results to Text)

    c.  On SQLNODE1, cycle the SQL Server service from SQL Server Configuration Manger

    d.  On SQLNODE1 failover the Availability Group to SQLNODE1 once the databases are back in sync.
        Execute T-SQL script, below, on SQLNODE1 (Use Results to Text)
*/

SET NOCOUNT ON;

-- Ensure replica is secondary
IF (
SELECT rs.is_primary_replica 
    FROM sys.availability_groups ag
    JOIN sys.dm_hadr_database_replica_states rs
    ON  ag.group_id = rs.group_id
    WHERE ag.name = N'DTCag1'
    AND rs.is_local = 1) = 0
BEGIN
    -- Wait for SYNCHRONIZED state
    DECLARE @ctr tinyint = 0;
    declare @msg varchar(128);
    WHILE (SELECT synchronization_state 
        FROM sys.availability_groups ag
        JOIN sys.dm_hadr_database_replica_states rs
        ON  ag.group_id = rs.group_id
        WHERE ag.name = N'DTCag1'
        AND rs.is_primary_replica = 0
        AND rs.is_local = 1) <> 2
    BEGIN
        WAITFOR DELAY '00:00:01'
        SET @ctr += 1
        SET @msg = 'Waiting for databases to become synchronized. Duration in seconds: ' + cast(@ctr AS varchar(3))
        RAISERROR (@msg, 0, 1) WITH NOWAIT
    END

    ALTER AVAILABILITY GROUP DTCAG1 FAILOVER;
    SELECT 'Failover complete' AS [Sucess]
END
ELSE BEGIN
    SELECT 'This instance is the primary replica.  Connect to the secondary replica and try again.' AS [Error]
END

```

## <a name="12--test-configuration"></a>12.  Configuración de prueba

Esta prueba usa un servidor vinculado desde `SQLNODE1` a `SQLNODE2` para crear una transacción distribuida.  Asegúrese de que la réplica principal del grupo de disponibilidad está en `SQLNODE1`. Para probar la configuración tendrá que:

- Crear servidores vinculados
- Ejecutar una transacción distribuida

### <a name="create-linked-servers"></a>Crear servidores vinculados  
El siguiente script creará dos servidores vinculados en `SQLNODE1`.  Ejecute el siguiente script T-SQL en SSMS contra `SQLNODE1`.

```sql  
-- SQLNODE1
IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE1')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE1';   
END

IF NOT EXISTS (SELECT * FROM sys.servers where name = N'SQLNODE2')
BEGIN
    EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE2';   
END
 ```

### <a name="execute-a-distributed-transaction"></a>Ejecutar una transacción distribuida
Este script devolverá primero las estadísticas de transacción DTC actuales.  Después, el script ejecutará una transacción distribuida utilizando bases de datos en `SQLNODE1` y `SQLNODE2`.  Luego, el script devolverá las estadísticas de transacción DTC que ahora deberían mostrar un recuento mayor.  Conéctese físicamente a `SQLNODE1` y ejecute el siguiente script T-SQL en SSMS contra `SQLNODE1` en el **modo SQLCMD**.

```sql  
/*******************************************************************
    Execute script in its entirety on SQLNODE1 in SQLCMD mode
    Must be physically connected to SQLNODE1
*******************************************************************/

USE AG1;
SET NOCOUNT ON;

-- Get Baseline
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;

SET XACT_ABORT ON
BEGIN DISTRIBUTED TRANSACTION
    INSERT INTO SQLNODE1.[AG1].[dbo].[Names] VALUES ('TestValue1', GETDATE());
    INSERT INTO SQLNODE2.[dtcDemoAG1].[dbo].[Names] VALUES ('TestValue2', GETDATE());
COMMIT TRAN
GO

-- Review DTC Transaction Statistics
!! Powershell; $DtcNameC = Get-DtcClusterDefault; Get-DtcTransactionsStatistics -DtcName $DtcNameC;
```

> [!IMPORTANT]
> El instrucción `USE AG1` se debe ejecutar para garantizar que el contexto de base de datos se establece en `AG1`.  De lo contrario, es probable que reciba el mensaje de error siguiente: "Contexto de transacción en uso por otra sesión".