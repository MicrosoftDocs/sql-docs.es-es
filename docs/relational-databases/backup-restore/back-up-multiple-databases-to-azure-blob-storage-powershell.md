---
title: 'Copia de seguridad de varias bases de datos: Azure Blob Storage'
description: En este tema se ofrecen scripts de ejemplo que se pueden utilizar para automatizar las copias de seguridad de SQL Server en el servicio Azure Blob Storage con los cmdlets de PowerShell.
titleSuffix: PowerShell
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: f7008339-e69d-4e20-9265-d649da670460
author: cawrites
ms.author: chadam
ms.openlocfilehash: e3fb357b16572f34c906942e79be2d8488b49a15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347719"
---
# <a name="back-up-multiple-databases-to-azure-blob-storage---powershell"></a>Realizar copias de seguridad de varias bases de datos en Azure Blob Storage - PowerShell

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Este tema proporciona ejemplos de scripts que se pueden utilizar para automatizar las copias de seguridad del servicio Azure Blob Storage con los cmdlets de PowerShell.  
  
## <a name="overview-of-powershell-cmdlets-for-backup-and-restore"></a>Información general de los cmdlets de PowerShell para las copias de seguridad y restauración

El **Backup-SqlDatabase** y **Restore-SqlDatabase** son dos cmdlets principales disponibles para realizar operaciones de copia de seguridad y restauración.

Además, hay otros cmdlets que pueden ser necesarios para automatizar las copias de seguridad en el almacenamiento de blobs de Windows Azure, como el conjunto de cmdlets de **SqlCredential**.

Para obtener una lista de todos los cmdlets disponibles, vea [Cmdlets de SqlServer](/powershell/module/sqlserver).
  
> [!TIP]  
> Los cmdlets de **SqlCredential** se usan en escenarios de copia de seguridad y restauración de Azure Blob Storage. Para más información sobre las credenciales y su uso, vea [Copia de seguridad y restauración de SQL Server con el servicio de Almacenamiento de blobs de Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).
  
### <a name="powershell-for-multi-database-multi-instance-backup-operations"></a>PowerShell para operaciones de copia de seguridad de varias instancias y varias bases de datos

En las secciones siguientes se incluyen scripts para varias operaciones como crear una credencial SQL en varias instancias de SQL Server, realizar copias de seguridad de todas las bases de datos de usuario en una instancia de SQL Server, etc. Puede utilizar estos scripts para automatizar o para programar las operaciones de copia de seguridad según los requisitos de su entorno. Los scripts que aquí se muestran son ejemplos y se pueden modificar o ampliar para su entorno.  
  
A continuación se presentan las consideraciones para los scripts de ejemplo:  
  
- SQL Server PowerShell implementa cmdlets para navegar por la estructura de ruta de acceso que representa la jerarquía de objetos compatible con un proveedor de PowerShell. Cuando ha navegado a un nodo de la ruta de acceso, puede usar otros cmdlets para realizar operaciones básicas en el objeto actual.

  Para obtener más información, consulte [Navigate SQL Server PowerShell Paths](../../powershell/navigate-sql-server-powershell-paths.md).

- Cmdlet **Get-ChildItem**: La información devuelta por **Get-ChildItem** depende de la ubicación de una ruta de acceso de SQL Server PowerShell. Por ejemplo, si la ubicación está en el equipo, este cmdlet devuelve todas las instancias del motor de base de datos de SQL Server instaladas en él. O bien, si la ubicación está en un nivel de objeto como las bases de datos, devuelve una lista de objetos de base de datos. De forma predeterminada, el cmdlet **Get-ChildItem** no devuelve los objetos del sistema. Use el parámetro `–Force` para ver los objetos del sistema.

- Una cuenta de almacenamiento de Azure y una credencial SQL son requisitos previos necesarios y para todas las operaciones de copia de seguridad y restauración en el servicio de almacenamiento de blobs de Azure.
  
### <a name="create-a-sql-credential-on-all-instances-of-sql-server"></a>Creación de una credencial SQL en todas las instancias de SQL Server

Se puede usar el script siguiente para crear una credencial SQL genérica en todas las instancias de SQL Server de un equipo. Si ya hay una credencial existente con el mismo nombre en una de las instancias del equipo, el script muestra un error y continúa.  
  
```powershell
# load the sqlps module
import-module sqlps  
  
# set parameters
$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$storageKey = "<myStorageAccessKey>"  
$secureString = ConvertTo-SecureString $storageKey -AsPlainText -Force  
$credentialName = "myCredential-$(Get-Random)"

Write-Host "Generate credential: " $credentialName
  
#cd to sql server and get instances  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and create a SQL credential, output any errors
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials"
        New-SqlCredential -Name $credentialName -Identity $storageAccount -Secret $secureString -Path $path -ea Stop | Out-Null
        Write-Host "...generated credential $($path)\$($credentialName)."  }
    catch { Write-Host $_.Exception.Message } }
```

> [!NOTE]
> La instrucción `-ea Stop | Out-Null` se usa para la salida de excepciones definidas por el usuario. Si prefiere los mensajes de error de PowerShell predeterminados, esta instrucción se puede quitar. 

### <a name="remove-a-sql-credential-from-all-instances-of-sql-server"></a>Eliminación de una credencial SQL de todas las instancias de SQL Server

El script siguiente se puede usar para quitar una credencial específica de todas las instancias de SQL Server instaladas en el equipo. Si la credencial no existe en una instancia concreta, se muestra un mensaje de error y el script continúa hasta que se comprueban todas las instancias.  
  
```powershell
import-module sqlps

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$credentialName = "<myCredential>"

Write-Host "Delete credential: " $credentialName

cd $sqlPath
$instances = Get-ChildItem

#loop through instances and delete a SQL credential
foreach ($instance in $instances)  {
    try {
        $path = "$($sqlPath)\$($instance.DisplayName)\credentials\$($credentialName)"
        Remove-SqlCredential -Path $path -ea Stop | Out-Null
        Write-Host "...deleted credential $($path)."  }
    catch { Write-Host $_.Exception.Message } }
```  
  
### <a name="full-backup-for-all-databases"></a>Copia de seguridad completa de todas las bases de datos

El siguiente script crea copias de seguridad de todas las bases de datos del equipo. Esto incluye tanto a las bases de datos de usuario como a la base de datos del sistema **msdb** . El script filtra las bases de datos del sistema **tempdb** y **model** .  
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $backupUrlContainer
  
cd $sqlPath
$instances = Get-ChildItem

#loop through instances and backup all databases (excluding tempdb and model)
foreach ($instance in $instances)  {
    $path = "$($sqlPath)\$($instance.DisplayName)\databases"
    $databases = Get-ChildItem -Force -Path $path | Where-object {$_.name -ne "tempdb" -and $_.name -ne "model"}

    foreach ($database in $databases) {
        try {
            $databasePath = "$($path)\$($database.Name)"
            Write-Host "...starting backup: " $databasePath
            Backup-SqlDatabase -Database $database.Name -Path $path -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
            Write-Host "...backup complete."  }
        catch { Write-Host $_.Exception.Message } } }
```  
  
### <a name="full-backup-for-system-databases-only-on-a-specific-instance-of-sql-server"></a>Copia de seguridad completa de las bases de datos del sistema solo en una instancia específica de SQL Server

El script completo se puede usar para crear una copia de seguridad de las bases de datos **master** y **msdb** en una instancia con nombre de SQL Server. Se puede usar el mismo script para cualquier instancia si se cambia el valor del parámetro de instancia. La instancia predeterminada de SQL Server se denomina `DEFAULT`.
  
```powershell
import-module sqlps  

$sqlPath = "sqlserver:\sql\$($env:COMPUTERNAME)"
$instanceName = "DEFAULT"
$storageAccount = "<myStorageAccount>"  
$blobContainer = "<myBlobContainer>"  
$backupUrlContainer = "https://$storageAccount.blob.core.windows.net/$blobContainer/"  
$credentialName = "<myCredential>"

Write-Host "Backup database: " $instanceName " to " $backupUrlContainer
  
cd "$($sqlPath)\$($instanceName)"

#loop through instance and backup specific databases
$databases = "master", "msdb"  
foreach ($database in $databases) {
    try {
        Write-Host "...starting backup: " $database
        Backup-SqlDatabase -Database $database -BackupContainer $backupUrlContainer -SqlCredential $credentialName -Compression On
        Write-Host "...backup complete." }
    catch { Write-Host $_.Exception.Message } }
```  
  
## <a name="see-also"></a>Consulte también

[Copia de seguridad y restauración de SQL Server con el servicio Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)

[Procedimientos recomendados y solución de problemas de Copia de seguridad en URL de SQL Server](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)