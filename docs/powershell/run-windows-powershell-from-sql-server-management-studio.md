---
title: Ejecutar Windows PowerShell desde SQL Server Management Studio | Microsoft Docs
description: Obtenga información sobre cómo iniciar una sesión de Windows PowerShell desde el Explorador de objetos en SQL Server Management Studio, con la ruta de acceso preestablecida en la ubicación de los objetos que prefiera.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 1f841825-da1f-4062-9a81-3cdbab03845b
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 3e74949152adb8ecadc720ced14ff3b29b196fd6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338163"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Ejecutar Windows PowerShell desde SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Puede iniciar sesiones de Windows PowerShell desde el **Explorador de objetos** en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] inicia Windows PowerShell, carga el módulo **SqlServer** y establece el contexto de ruta de acceso al nodo asociado en el árbol del **Explorador de objetos**.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Al especificar la ejecución de PowerShell para un objeto en el **Explorador de objetos**, [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] inicia una sesión de Windows PowerShell en la que los complementos PowerShell de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se han cargado y registrado. La ruta de acceso para la sesión está preestablecida en la ubicación del objeto en el que hizo clic con el botón derecho en el Explorador de objetos. Por ejemplo, si hace clic con el botón derecho en el objeto de base de datos [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] en el Explorador de objetos y selecciona **Iniciar PowerShell**, la ruta de acceso de Windows PowerShell se establece en lo siguiente:  

```powershell
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```

## <a name="run-powershell"></a>Ejecutar PowerShell

### <a name="to-run-powershell-from-sql-server-management-studio"></a>Para ejecutar PowerShell desde SQL Server Management Studio

1. Abra el **Explorador de objetos**.

2. Navegue al nodo del objeto en el que trabajará.

3. Haga clic con el botón derecho en el objeto y seleccione **Iniciar PowerShell**.

## <a name="permissions"></a>Permisos

Cuando se abre desde [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)], PowerShell no se ejecuta con privilegios de Administrador, lo que puede impedir algunas actividades, como las llamadas a WMI.  
  
## <a name="see-also"></a>Consulte también

- [SQL Server PowerShell](sql-server-powershell.md)