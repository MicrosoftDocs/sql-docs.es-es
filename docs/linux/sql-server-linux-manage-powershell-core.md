---
title: Administración de SQL Server en Linux con PowerShell Core
description: Obtenga información sobre SQL Server PowerShell a partir de un par de ejemplos sobre cómo usar SQL Server PowerShell con PowerShell Core (PS Core) en macOS y Linux.
ms.date: 04/22/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: vanto
ms.openlocfilehash: 8018a90b3483f9baf4b58a4b057fa93685c6d828
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016330"
---
# <a name="manage-sql-server-on-linux-with-powershell-core"></a>Administración de SQL Server en Linux con PowerShell Core

En este artículo se presenta [SQL Server PowerShell](../powershell/sql-server-powershell.md) y se describen un par de ejemplos sobre cómo usarlo con PowerShell Core (PS Core) en macOS y Linux. PowerShell Core es ahora un proyecto de código abierto en [GitHub](https://github.com/powershell/powershell).

## <a name="cross-platform-editor-options"></a>Opciones de editor multiplataforma

Todos los pasos de PowerShell Core que se indican a continuación funcionarán en un terminal normal, o bien se pueden ejecutar desde un terminal en VS Code o Azure Data Studio.  Tanto VS Code como Azure Data Studio están disponibles en macOS y Linux.  Para obtener más información sobre Azure Data Studio, consulte esta [guía de inicio rápido](../azure-data-studio/quickstart-sql-server.md).  También puede considerar la posibilidad de usar la [extensión de PowerShell](../azure-data-studio/extensions/powershell-extension.md).

## <a name="installing-powershell-core"></a>Instalación de PowerShell Core

Para obtener más información sobre la instalación de PowerShell Core en varias plataformas compatibles y experimentales, consulte los siguientes artículos:

- [Instalación de PowerShell Core en Windows](/powershell/scripting/install/installing-powershell-core-on-windows)
- [Instalación de PowerShell Core en Linux](/powershell/scripting/install/installing-powershell-core-on-linux)
- [Instalación de PowerShell Core en macOS](/powershell/scripting/install/installing-powershell-core-on-macos)
- [Instalación de PowerShell Core en ARM](/powershell/scripting/install/powershell-core-on-arm)

## <a name="install-the-sqlserver-module"></a>Instalación del módulo SqlServer

El módulo `SqlServer` se mantiene en la [Galería de PowerShell](https://www.powershellgallery.com/packages/SqlServer/). Cuando trabaje con SQL Server, siempre debe usar la versión más reciente del módulo de SqlServer PowerShell.

Para instalar el módulo SqlServer, abra una sesión de PowerShell Core y ejecute el código siguiente:

```powerhsell
Install-Module -Name SqlServer
```

Para obtener más información sobre cómo instalar el módulo SQLServer desde la Galería de PowerShell, consulte esta [página](../powershell/download-sql-server-ps-module.md).

## <a name="using-the-sqlserver-module"></a>Uso del módulo SqlServer

Comenzaremos por iniciar PowerShell Core.  Si está en macOS o Linux, abra una *sesión de terminal* en el equipo y escriba **pwsh** para iniciar una nueva sesión de PowerShell Core.  En Windows, use <kbd>Win</kbd>+<kbd>R</kbd> y escriba `pwsh` para iniciar una nueva sesión de PowerShell Core.

```
pwsh
```

SQL Server proporciona un módulo de PowerShell denominado **SqlServer**. Puede usar el **módulo SQLServer** para importar los componentes de SQL Server (proveedores y cmdlets de SQL Server) en un script o un entorno de PowerShell.

Copie y pegue el siguiente comando en el símbolo del sistema de PowerShell para importar el módulo **SqlServer** en la sesión actual de PowerShell:

```powershell
Import-Module SqlServer
```

Escriba el siguiente comando en el símbolo del sistema de PowerShell para comprobar que el módulo **SqlServer** se ha importado correctamente:

```powershell
Get-Module -Name SqlServer
```

PowerShell debe mostrar información similar al siguiente resultado:

```
ModuleType Version    Name          ExportedCommands
---------- -------    ----          ----------------
Script     21.1.18102 SqlServer     {Add-SqlAvailabilityDatabase, Add-SqlAvailabilityGroupList...
```

## <a name="connect-to-sql-server-and-get-server-information"></a>Conectarse a SQL Server y obtener información del servidor

En los pasos siguientes se usa PowerShell Core para conectar con la instancia de SQL Server en Linux y mostrar un par de propiedades de servidor.

Copie y pegue los siguientes comandos en el símbolo del sistema de PowerShell. Al ejecutar estos comandos, PowerShell:
- Muestran un cuadro de diálogo que le pide el nombre de host o la dirección IP de la instancia.
- Mostrará el cuadro de diálogo *Solicitud de credenciales para Windows PowerShell*, que le solicita las credenciales. Puede usar el *nombre de usuario* y la  *contraseña de SQL* para conectarse a su instancia de SQL Server en Linux.
- Use el cmdlet **Get-SqlInstance** para conectarse al **servidor** y mostrar algunas propiedades

Opcionalmente, puede reemplazar la variable `$serverInstance` por la dirección IP o el nombre de host de la instancia de SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and return a few properties
Get-SqlInstance -ServerInstance $serverInstance -Credential $credential
# done
```

PowerShell debe mostrar información similar al siguiente resultado:

```
Instance Name                   Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------                   -------    ------------ -----------  ------------ ----------------
your_server_instance            14.0.3048  RTM          CU13         Linux        Ubuntu
```

> [!NOTE]
> Si no se muestra nada para estos valores, lo más probable es que se produzca un error en la conexión con la instancia de SQL Server de destino. Asegúrese de que puede usar la misma información de conexión para conectarse desde SQL Server Management Studio. Luego revise las [recomendaciones para solucionar problemas de conexión](sql-server-linux-troubleshooting-guide.md#connection).

## <a name="using-the-sql-server-powershell-provider"></a>Usar el proveedor de SQL Server PowerShell

Otra opción para conectarse a la instancia de SQL Server es usar el [proveedor de SQL Server PowerShell](../powershell/sql-server-powershell-provider.md).  El uso de este proveedor permite navegar por la instancia de SQL Server como si estuviera navegando por la estructura de árbol en el Explorador de objetos, pero en el cmdline.  De forma predeterminada, este proveedor se presenta como un PSDrive denominado `SQLSERVER:\` que puede usar para conectar y navegar instancias de SQL Server a las que tiene acceso su cuenta de dominio.  Consulte los [pasos de configuración](./sql-server-linux-active-directory-auth-overview.md#configuration-steps) para obtener información sobre cómo configurar la autenticación de Active Directory para SQL Server en Linux.

También puede usar la autenticación de SQL con el proveedor de SQL Server PowerShell. Para ello, use el cmdlet `New-PSDrive` para crear un nuevo PSDrive y proporcione las credenciales adecuadas para conectarse.

En este ejemplo, verá un ejemplo de cómo crear un nuevo PSDrive mediante la autenticación de SQL.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

New-PSDrive -Name SQLonDocker -PSProvider SqlServer -Root 'SQLSERVER:\SQL\localhost,10002\Default\' -Credential $credential
```

Puede confirmar que la unidad se ha creado mediante la ejecución del cmdlet `Get-PSDrive`.

```powershell
Get-PSDrive
```

Una vez que haya creado el nuevo PSDrive, puede empezar a navegar por él.

```powershell
dir SQLonDocker:\Databases
```

Este es el aspecto que podría tener el resultado.  Es posible que observe que el resultado es similar al que mostrará SSMS en el nodo de bases de datos.  Muestra las bases de datos de usuario, pero no las bases de datos del sistema.

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.76 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.57 MB Simple       140 sa
```

Si necesita ver todas las bases de datos en la instancia, una opción es usar el cmdlet `Get-SqlDatabase`.

## <a name="get-databases"></a>Obtención de bases de datos

Un cmdlet importante que debe conocer es `Get-SqlDatabase`.  Puede usar el cmdlet `Get-SqlDatabase` para muchas operaciones que implican una base de datos u objetos contenidos en una.  Si proporciona valores para los parámetros `-ServerInstance` y `-Database`, solo se recuperará ese objeto de base de datos,  Pero, si solo especifica el parámetro `-ServerInstance`, se devolverá una lista completa de todas las bases de datos de esa instancia.

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

# Connect to the Instance and retrieve all databases
Get-SqlDatabase -ServerInstance ServerB -Credential $credential
```

Este es un ejemplo de lo que podría devolver el comando Get-SqlDatabase anterior:

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.88 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.63 MB Simple       140 sa
master               Normal        6.00 MB  600.00 KB Simple       140 sa
model                Normal       16.00 MB    5.70 MB Full         140 sa
msdb                 Normal       15.50 MB    1.14 MB Simple       140 sa
tempdb               Normal       16.00 MB    5.49 MB Simple       140 sa

```

## <a name="examine-sql-server-error-logs"></a>Examinar registros de errores de SQL Server

En los pasos siguientes se usa PowerShell Core para examinar los registros de errores que se conectan a la instancia de SQL Server en Linux.

Copie y pegue los siguientes comandos en el símbolo del sistema de PowerShell. La ejecución puede tardar unos minutos. Estos comandos realizan los pasos siguientes:
- Muestran un cuadro de diálogo que le pide el nombre de host o la dirección IP de la instancia.
- Muestran el cuadro de diálogo *Solicitud de credenciales para Windows PowerShell*, que le solicita las credenciales. Puede usar el *nombre de usuario* y la  *contraseña de SQL* para conectarse a su instancia de SQL Server en Linux.
- Usan el cmdlet **Get-SqlErrorLog** para conectarse a la instancia de SQL Server en Linux y recuperar los registros de errores desde **ayer**.

Opcionalmente, puede reemplazar la variable `$serverInstance` por la dirección IP o el nombre de host de la instancia de SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday
# done
```

## <a name="explore-cmdlets-currently-available-in-ps-core"></a>Exploración de los cmdlets disponibles actualmente en PS Core
Aunque el módulo SqlServer actualmente tiene 109 cmdlets disponibles en Windows PowerShell, solo 62 de 109 están disponibles en PSCore. A continuación se incluye una lista completa con los 62 cmdlets disponibles actualmente.  Para obtener documentación detallada de todos los cmdlets del módulo SqlServer, vea la [referencia de cmdlets](/powershell/module/sqlserver/) de SqlServer.

El siguiente comando le mostrará todos los cmdlets disponibles en la versión de PowerShell que está usando.

```powershell
Get-Command -Module SqlServer -CommandType Cmdlet |
Sort-Object -Property Noun |
SELECT Name
```

- ConvertFrom-EncodedSqlName
- ConvertTo-EncodedSqlName
- Get-SqlAgent
- Get-SqlAgentJob
- Get-SqlAgentJobHistory
- Get-SqlAgentJobSchedule
- Get-SqlAgentJobStep
- Get-SqlAgentSchedule
- Invoke-SqlAssessment
- Get-SqlAssessmentItem
- Remove-SqlAvailabilityDatabase
- Resume-SqlAvailabilityDatabase
- Add-SqlAvailabilityDatabase
- Suspend-SqlAvailabilityDatabase
- New-SqlAvailabilityGroup
- Set-SqlAvailabilityGroup
- Remove-SqlAvailabilityGroup
- Switch-SqlAvailabilityGroup
- Join-SqlAvailabilityGroup
- Revoke-SqlAvailabilityGroupCreateAnyDatabase
- Grant-SqlAvailabilityGroupCreateAnyDatabase
- New-SqlAvailabilityGroupListener
- Set-SqlAvailabilityGroupListener
- Add-SqlAvailabilityGroupListenerStaticIp
- Set-SqlAvailabilityReplica
- Remove-SqlAvailabilityReplica
- New-SqlAvailabilityReplica
- Set-SqlAvailabilityReplicaRoleToSecondary
- New-SqlBackupEncryptionOption
- Get-SqlBackupHistory
- Invoke-Sqlcmd
- New-SqlCngColumnMasterKeySettings
- Remove-SqlColumnEncryptionKey
- Get-SqlColumnEncryptionKey
- Remove-SqlColumnEncryptionKeyValue
- Add-SqlColumnEncryptionKeyValue
- Get-SqlColumnMasterKey
- Remove-SqlColumnMasterKey
- New-SqlColumnMasterKey
- Get-SqlCredential
- Set-SqlCredential
- New-SqlCredential
- Remove-SqlCredential
- New-SqlCspColumnMasterKeySettings
- Get-SqlDatabase
- Restore-SqlDatabase
- Backup-SqlDatabase
- Set-SqlErrorLog
- Get-SqlErrorLog
- New-SqlHADREndpoint
- Set-SqlHADREndpoint
- Get-SqlInstance
- Add-SqlLogin
- Remove-SqlLogin
- Get-SqlLogin
- Set-SqlSmartAdmin
- Get-SqlSmartAdmin
- Read-SqlTableData
- Write-SqlTableData
- Read-SqlViewData
- Read-SqlXEvent
- Convert-UrnToPath

## <a name="see-also"></a>Consulte también
- [SQL Server PowerShell](../powershell/sql-server-powershell.md)