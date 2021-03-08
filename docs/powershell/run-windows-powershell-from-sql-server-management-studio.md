---
title: Ejecutar Windows PowerShell desde SQL Server Management Studio
description: Obtenga información sobre cómo iniciar una sesión de Windows PowerShell desde el Explorador de objetos en SQL Server Management Studio, con la ruta de acceso preestablecida en la ubicación de los objetos que prefiera.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 9fd9b7039680b05515d1f70102408394b5443c9c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839480"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Ejecutar Windows PowerShell desde SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Puede iniciar sesiones de Windows PowerShell desde el **Explorador de objetos** en SQL Server Management Studio (SSMS). SSMS inicia Windows PowerShell, carga el módulo **SqlServer** y establece el contexto de ruta de acceso al nodo asociado en el árbol del **Explorador de objetos**.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Al especificar la ejecución de PowerShell para un objeto en el **Explorador de objetos**, SQL Server Management Studio inicia una sesión de Windows PowerShell en la que los complementos PowerShell de SQL Server se han cargado y registrado. La ruta de acceso para la sesión está preestablecida en la ubicación del objeto en el que hizo clic con el botón derecho en el Explorador de objetos.

Por ejemplo, si hace clic con el botón derecho en el objeto de base de datos AdventureWorks en el Explorador de objetos y selecciona **Iniciar PowerShell**, la ruta de acceso de Windows PowerShell se establece en lo siguiente:

```powershell
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```

## <a name="run-powershell"></a>Ejecutar PowerShell

### <a name="to-run-powershell-from-sql-server-management-studio"></a>Para ejecutar PowerShell desde SQL Server Management Studio

1. Abra el **Explorador de objetos**.

2. Navegue al nodo del objeto en el que trabajará.

3. Haga clic con el botón derecho en el objeto y seleccione **Iniciar PowerShell**.

## <a name="permissions"></a>Permisos

Cuando se abre desde SQL Server Management Studio, PowerShell no se ejecuta con privilegios de Administrador, lo que puede impedir algunas actividades, como las llamadas a WMI.

## <a name="see-also"></a>Consulte también

- [SQL Server PowerShell](sql-server-powershell.md)