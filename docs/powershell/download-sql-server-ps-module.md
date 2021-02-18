---
title: Descarga del módulo de PowerShell de SQL Server
description: Obtenga información sobre cómo instalar el módulo SqlServer de PowerShell, que proporciona cmdlets que admiten las características de SQL más recientes, y también contiene versiones actualizadas de los cmdlets en el módulo SQLPS.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 892c92ff1415aa2c7e76aa8b49ba1c57286f0716
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346249"
---
# <a name="install-the-sql-server-powershell-module"></a>Instalación del módulo SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este artículo se proporcionan instrucciones para instalar el módulo de PowerShell **SqlServer**.

## <a name="powershell-modules-for-sql-server"></a>Módulos de PowerShell para SQL Server

Hay dos módulos de SQL Server PowerShell:

- **SqlServer**: El módulo SqlServer incluye cmdlets nuevos para la compatibilidad con las características más recientes de SQL. El módulo además contiene versiones actualizadas de los cmdlets de **SQLPS**. Para descargar el módulo SqlServer, vaya al [módulo SqlServer en la Galería de PowerShell](https://www.powershellgallery.com/packages/Sqlserver).

- **SQLPS**: SQLPS es el módulo que usa el [Agente SQL](sql-server-powershell.md#sql-server-agent) para ejecutar trabajos del agente en pasos de trabajo del agente mediante el subsistema de PowerShell.

> [!NOTE]
> Las versiones del módulo **SqlServer** de la Galería de PowerShell admiten el control de versiones y requieren la versión 5.0 de PowerShell o versiones posteriores.

Para ver los temas de ayuda, vaya a:

- Cmdlets de [SqlServer](/powershell/module/sqlserver).
- Cmdlets de [SQLPS](/powershell/module/sqlps).

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

[SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) no instala ninguno de los módulos de PowerShell. Para usar PowerShell con SSMS, instale el módulo **SqlServer** desde la [Galería de PowerShell](https://www.powershellgallery.com/packages/Sqlserver).

> [!NOTE]
> Con SSMS 16.x, se incluye una versión anterior del módulo **SqlServer** con SQL Server Management Studio (SSMS).

## <a name="azure-data-studio"></a>Azure Data Studio

[Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) no instala ninguno de los módulos de PowerShell. Para usar PowerShell con Azure Data Studio, instale el módulo **SqlServer** desde la [Galería de PowerShell](https://www.powershellgallery.com/packages/Sqlserver).

Puede usar la [extensión de PowerShell](../azure-data-studio/extensions/powershell-extension.md) que proporciona compatibilidad con el editor de PowerShell enriquecida en Azure Data Studio.

## <a name="installing-or-updating-the-sqlserver-module"></a>Instalación o actualización del módulo SqlServer

Para instalar el módulo **SqlServer** desde la Galería de PowerShell, inicie una sesión de [PowerShell](/powershell/scripting/overview) como administrador. También puede iniciar Azure Data Studio como administrador y ejecutar estos comandos en una sesión de PowerShell en el terminal integrado.

También puede usar *Install-Module SQLServer -Scope CurrentUser* para ejecutar permisos elevados. Este cmdlet es útil para los usuarios que no son administradores de su entorno. Pero como el ámbito se limita al usuario actual, otros usuarios del mismo equipo no pueden usar el módulo.

### <a name="install-the-sqlserver-module"></a>Instalación del módulo SqlServer

Ejecute el comando siguiente en la sesión de PowerShell para instalar el módulo SqlServer para todos los usuarios:

```powershell
Install-Module -Name SqlServer
```

### <a name="to-view-the-versions-of-the-sqlserver-module-installed"></a>Para ver las versiones del módulo SqlServer instaladas

Ejecute el comando siguiente para ver las versiones del módulo SqlServer que se han instalado.

```powershell
Get-Module SqlServer -ListAvailable
```

### <a name="install-for-the-current-user-rather-than-as-an-administrator"></a>Instalación para el usuario actual en lugar de como administrador

Si no puede ejecutar la sesión de PowerShell como administrador, haga la instalación para el usuario actual con el comando siguiente:

```powershell
Install-Module -Name SqlServer -Scope CurrentUser
```

### <a name="to-overwrite-a-previous-version-of-the-sqlserver-module"></a>Para sobrescribir una versión anterior del módulo SqlServer

También puede usar el comando `Install-Module` para sobrescribir una versión anterior.

```powershell
Install-Module -Name SqlServer -AllowClobber
```

> [!Note]
> PowerShell siempre usa el módulo más reciente instalado.

### <a name="update-the-installed-version-of-the-sqlserver-module"></a>Actualización de la versión instalada del módulo SqlServer

Cuando haya disponibles versiones actualizadas del módulo **SqlServer**, puede instalar la versión más reciente con el comando siguiente:

```powershell
Install-Module -Name SqlServer -AllowClobber
```

Puede usar el comando `Update-Module` para instalar la versión más reciente del módulo SQLServer de PowerShell, pero eso no quita las versiones anteriores. Instala la versión más reciente en paralelo para que pueda experimentar con la versión más reciente, pero con los módulos más antiguos todavía instalados.

Sin embargo, si no quiere conservar las versiones anteriores del módulo, puede usar el comando `Uninstall-Module` para quitarlas.

Puede usar el comando siguiente para enumerar si hay más de una versión instalada:

```powershell
Get-Module SqlServer -ListAvailable
```

Puede usar el comando siguiente para quitar versiones anteriores:

```powershell
Uninstall-module -Name SQLServer -RequiredVersion "<version number>" -AllowClobber
```

### <a name="troubleshooting"></a>Solución de problemas

Si le surgen problemas durante la instalación, consulte la [documentación de instalación de módulos](https://www.powershellgallery.com/packages/PowerShellGet/2.2.1) y la [referencia de instalación de módulos](/powershell/module/powershellget/Install-Module).

## <a name="using-a-specific-version-of-the-sqlserver-module"></a>Uso de una versión específica del módulo SqlServer

Para usar una versión específica del módulo, impórtela con un número de versión específico de forma similar al comando siguiente:

```powershell
Import-Module SqlServer -Version 21.1.18218
```

## <a name="pre-release-versions-of-the-sqlserver-module"></a>Versiones preliminares del módulo SqlServer

Las versiones preliminares del módulo SqlServer pueden estar disponibles en la Galería de PowerShell.

> [!IMPORTANT]
> Estas versiones se pueden detectar e instalar mediante los cmdlets actualizados *Find-Module* e *Install-Module* que forman parte del módulo [PowerShellGet](https://www.powershellgallery.com/packages/PowerShellGet) si se pasa el modificador *-AllowPrerelease*. Para usar estos cmdlets, instale el módulo PowerShellGet y, a continuación, abra una sesión nueva.

### <a name="to-discover-pre-release-versions-of-the-sqlserver-module"></a>Para detectar las versiones preliminares del módulo SqlServer

Para detectar las versiones preliminares del módulo SqlServer, ejecute el comando siguiente:

```powershell
Find-Module SqlServer -AllowPrerelease
```

### <a name="to-install-a-specific-pre-release-version-of-the-sqlserver-module"></a>Para instalar una versión preliminar específica del módulo SqlServer

Para instalar una versión de lanzamiento preliminar específica del módulo, instálela con un número de versión específico.

Puede intentar usar el comando siguiente:

```powershell
Install-Module SqlServer -RequiredVersion 21.1.18218-preview -AllowPrerelease
```

## <a name="sql-server-powershell-on-linux"></a>SQL Server PowerShell en Linux

Visite [Administración de SQL Server en Linux con PowerShell Core](../linux/sql-server-linux-manage-powershell-core.md) para ver cómo instalar SQL Server PowerShell en Linux.

## <a name="other-modules"></a>Otros módulos

- [Az.Sql](https://www.powershellgallery.com/packages/Az.Sql/): cmdlets del servicio SQL para Azure Resource Manager en Windows PowerShell y PowerShell Core.

- [SqlServerDsc](https://www.powershellgallery.com/packages/SqlServerDsc/): módulo con recursos de DSC para la implementación y configuración de Microsoft SQL Server.

## <a name="cmdlet-reference"></a>Referencia de cmdlets

- [Cmdlets de SqlServer](/powershell/module/sqlserver)
- [Cmdlets de SQLPS](/powershell/module/sqlps)

## <a name="next-steps"></a>Pasos siguientes

- [SQL Server PowerShell](sql-server-powershell.md)
- [Cmdlets de SQL Server PowerShell](/powershell/module/sqlserver)
- [Uso de PowerShell con Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)