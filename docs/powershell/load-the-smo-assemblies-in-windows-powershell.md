---
title: Cargar ensamblados SMO en Windows PowerShell
description: Aprenda a cargar ensamblados de Objeto de administración de SQL Server (SMO) en scripts de Windows PowerShell que no usen el proveedor de SQL Server PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 8ca42b69-da5a-47f4-9085-34e443f0e389
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 7bfb0d68533e116c41a6ebb2277b3bae29199e13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340801"
---
# <a name="load-the-smo-assemblies-in-windows-powershell"></a>Cargar ensamblados SMO en Windows PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este artículo se describe cómo cargar los ensamblados del objeto de administración de SQL Server (SMO) en scripts de Windows PowerShell que no usan el proveedor de SQL Server PowerShell.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

El mecanismo preferido para cargar ensamblados SMO es cargar el módulo **SqlServer**. El proveedor de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] incluye en el módulo carga automáticamente los ensamblados SMO y también implementa características que amplían la utilidad de los objetos SMO en los scripts de PowerShell.

Hay dos casos en los que puede que tenga que cargar los ensamblados de SMO directamente:  

- Si un script hace referencia a un objeto de SMO antes del primer comando que hace referencia al proveedor o cmdlets desde los complementos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  

- Quiere pasar código de SMO desde otro lenguaje, como C# o Visual Basic, que no usa el proveedor ni cmdlets.  

## <a name="example-loading-the-sql-server-management-objects"></a>Ejemplo: Carga de los objetos de administración de SQL Server

El código siguiente carga los ensamblados de SMO:  

```powershell
# Loads the SQL Server Management Objects (SMO)  

$ErrorActionPreference = "Stop"
  
$sqlpsreg="HKLM:\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.SqlServer.Management.PowerShell.sqlps"  
  
if (Get-ChildItem $sqlpsreg -ErrorAction "SilentlyContinue")  
{  
    throw "SQL Server Provider for Windows PowerShell is not installed."  
}  
else  
{  
    $item = Get-ItemProperty $sqlpsreg  
    $sqlpsPath = [System.IO.Path]::GetDirectoryName($item.Path)  
}  
  
$assemblylist =
"Microsoft.SqlServer.Management.Common",  
"Microsoft.SqlServer.Smo",  
"Microsoft.SqlServer.Dmf ",  
"Microsoft.SqlServer.Instapi ",  
"Microsoft.SqlServer.SqlWmiManagement ",  
"Microsoft.SqlServer.ConnectionInfo ",  
"Microsoft.SqlServer.SmoExtended ",  
"Microsoft.SqlServer.SqlTDiagM ",  
"Microsoft.SqlServer.SString ",  
"Microsoft.SqlServer.Management.RegisteredServers ",  
"Microsoft.SqlServer.Management.Sdk.Sfc ",  
"Microsoft.SqlServer.SqlEnum ",  
"Microsoft.SqlServer.RegSvrEnum ",  
"Microsoft.SqlServer.WmiEnum ",  
"Microsoft.SqlServer.ServiceBrokerEnum ",  
"Microsoft.SqlServer.ConnectionInfoExtended ",  
"Microsoft.SqlServer.Management.Collector ",  
"Microsoft.SqlServer.Management.CollectorEnum",  
"Microsoft.SqlServer.Management.Dac",  
"Microsoft.SqlServer.Management.DacEnum",  
"Microsoft.SqlServer.Management.Utility"  
  
foreach ($asm in $assemblylist)  
{  
    $asm = [Reflection.Assembly]::LoadWithPartialName($asm)  
}  
  
Push-Location  
cd $sqlpsPath  
update-FormatData -prependpath SQLProvider.Format.ps1xml
Pop-Location  
```

## <a name="see-also"></a>Consulte también

- [SQL Server PowerShell](sql-server-powershell.md)