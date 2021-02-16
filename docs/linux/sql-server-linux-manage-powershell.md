---
title: Administrar SQL Server en Linux con PowerShell
description: Obtenga detalles sobre SQL Server PowerShell y consulte un par de ejemplos sobre cómo usar Windows con SQL Server en Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: a3492ce1-5d55-4505-983c-d6da8d1a94ad
ms.openlocfilehash: d10b498569bac6a166ded3eaf9bd03f23ea6b718
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345634"
---
# <a name="use-powershell-on-windows-to-manage-sql-server-on-linux"></a>Uso de PowerShell en Windows para administrar SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se presenta [SQL Server PowerShell](../powershell/sql-server-powershell.md) y se describen un par de ejemplos sobre cómo usarlo con SQL Server en Linux. Actualmente, la compatibilidad de PowerShell con SQL Server está disponible en Windows, MacOS y Linux. Este artículo le guiará a través del uso de un equipo Windows para conectarse a una instancia de SQL Server remota en Linux.

## <a name="install-the-newest-version-of-sql-powershell-on-windows"></a>Instalación de la versión más reciente de SQL PowerShell en Windows

[SQL PowerShell](../powershell/download-sql-server-ps-module.md) en Windows se mantiene en la Galería de PowerShell. Cuando trabaje con SQL Server, siempre debe usar la versión más reciente del módulo de SqlServer PowerShell.

## <a name="before-you-begin"></a>Antes de empezar

Lea los [Problemas conocidos](sql-server-linux-release-notes.md) de SQL Server en Linux.

## <a name="launch-powershell-and-import-the-sqlserver-module"></a>Iniciar PowerShell e importar el módulo *sqlserver*

Comenzaremos por iniciar PowerShell en Windows. Use <kbd>Win</kbd>+<kbd>R</kbd>, en el equipo Windows y escriba **PowerShell** para iniciar una nueva sesión de Windows PowerShell.

```
PowerShell
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

Vamos a usar PowerShell en Windows para conectar con la instancia de SQL Server en Linux y mostrar un par de propiedades de servidor.

Copie y pegue los siguientes comandos en el símbolo del sistema de PowerShell. Al ejecutar estos comandos, PowerShell:
- Muestran un cuadro de diálogo que le pide el nombre de host o la dirección IP de la instancia.
- Mostrará el cuadro de diálogo *Solicitud de credenciales de Windows PowerShell*, que le solicita las credenciales. Puede usar el *nombre de usuario* y la  *contraseña de SQL* para conectarse a su instancia de SQL Server en Linux.
- Use el cmdlet **Get-SqlInstance** para conectarse al **servidor** y mostrar algunas propiedades

Opcionalmente, puede reemplazar la variable `$serverInstance` por la dirección IP o el nombre de host de la instancia de SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and get a few properties
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

Otra opción para conectarse a la instancia de SQL Server es usar el [proveedor de SQL Server PowerShell](../powershell/sql-server-powershell-provider.md).  Este proveedor permite navegar por la instancia de SQL Server de forma similar que si estuviera navegando por la estructura de árbol en el Explorador de objetos, pero en el cmdline.  De forma predeterminada, este proveedor se presenta como un PSDrive denominado `SQLSERVER:\` que puede usar para conectar y navegar instancias de SQL Server a las que tiene acceso su cuenta de dominio.  Consulte los [pasos de configuración](./sql-server-linux-active-directory-auth-overview.md#configuration-steps) para obtener información sobre cómo configurar la autenticación de Active Directory para SQL Server en Linux.

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

Si necesita ver todas las bases de datos en la instancia, una opción es usar el cmdlet [Get-SqlDatabase](/powershell/module/sqlserver/Get-SqlDatabase).

## <a name="examine-sql-server-error-logs"></a>Examinar registros de errores de SQL Server

En los pasos siguientes se usa PowerShell en Windows para examinar los registros de errores que se conectan a la instancia de SQL Server en Linux. También usaremos el cmdlet **Out-GridView** para mostrar información de los registros de errores en una pantalla de vista de cuadrícula.

Copie y pegue los siguientes comandos en el símbolo del sistema de PowerShell. La ejecución puede tardar unos minutos. Estos comandos hacen lo siguiente:
- Muestran un cuadro de diálogo que le pide el nombre de host o la dirección IP de la instancia.
- Mostrará el cuadro de diálogo *Solicitud de credenciales de Windows PowerShell*, que le solicita las credenciales. Puede usar el *nombre de usuario* y la  *contraseña de SQL* para conectarse a su instancia de SQL Server en Linux.
- Usan el cmdlet **Get-SqlErrorLog** para conectarse a la instancia de SQL Server en Linux y recuperar los registros de errores desde **ayer**.
- Canalizar el resultado al cmdlet **Out-GridView**

Opcionalmente, puede reemplazar la variable `$serverInstance` por la dirección IP o el nombre de host de la instancia de SQL Server.

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday | Out-GridView
# done
```
## <a name="see-also"></a>Consulte también
- [SQL Server PowerShell](../powershell/sql-server-powershell.md)
- [Cmdlets de SqlServer](/powershell/module/sqlserver)