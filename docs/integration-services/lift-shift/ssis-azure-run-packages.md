---
title: Ejecutar paquetes SSIS en Azure | Microsoft Docs
description: Proporciona información general de los métodos disponibles para ejecutar paquetes SSIS que se implementan en Azure SQL Database.
ms.date: 05/29/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 2c7209d43628296169e973012ea459296acc3549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351977"
---
# <a name="run-sql-server-integration-services-ssis-packages-deployed-in-azure"></a>Ejecutar paquetes de SQL Server Integration Services (SSIS) implementados en Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Puede ejecutar los paquetes SSIS implementados en el catálogo de SSISDB en un servidor de Azure SQL Database si selecciona uno de los métodos que se describen en este artículo. Puede ejecutar un paquete directamente, o bien como parte de una canalización de Azure Data Factory. Para obtener información general sobre SSIS en Azure, vea [Implementar y ejecutar paquetes SSIS en Azure](ssis-azure-lift-shift-ssis-packages-overview.md).

- Ejecutar un paquete directamente

  - [Ejecutar con SSMS](#ssms)

  - [Ejecutar con procedimientos almacenados](#sproc)

  - [Ejecutar con un script o código](#script)

- Ejecutar un paquete como parte de una canalización de Azure Data Factory

  - [Ejecutar con la actividad Ejecutar paquete SSIS](#exec_activity)

  - [Ejecutar con la actividad de procedimiento almacenado](#sproc_activity)

> [!NOTE]
> La ejecución de un paquete con `dtexec.exe` no se ha probado con paquetes implementados en Azure.

## <a name="run-a-package-with-ssms"></a><a name="ssms"></a> Ejecutar un paquete con SSMS

En SQL Server Management Studio (SSMS), puede hacer clic con el botón derecho en un paquete implementado en la base de datos del catálogo de SSIS, SSISDB, y seleccionar **Ejecutar** para abrir el cuadro de diálogo **Ejecutar paquete**. Para obtener más información, consulte [Run an SSIS package with SQL Server Management Studio (SSMS) (Ejecutar un paquete de SSIS con SQL Server Management Studio [SSMS])](../ssis-quickstart-run-ssms.md).

## <a name="run-a-package-with-stored-procedures"></a><a name="sproc"></a> Ejecutar un paquete con procedimientos almacenados

En cualquier entorno desde el que se pueda conectar a Azure SQL Database y ejecutar código de Transact-SQL, puede ejecutar un paquete mediante la llamada a los procedimientos almacenados siguientes:

1. **[catalog].[create_execution]** . Para obtener más información, vea [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md).

2. **[catalog].[set_execution_parameter_value]** . Para obtener más información, vea [catalog.set_execution_parameter_value](../system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md).

3. **[catalog].[start_execution]** . Para obtener más información, vea [catalog.start_execution](../system-stored-procedures/catalog-start-execution-ssisdb-database.md).

Para obtener más información, vea los ejemplos siguientes:

- [Run an SSIS package from SSMS with Transact-SQL](../ssis-quickstart-run-tsql-ssms.md) (Ejecutar un paquete de SSIS con Transact-SQL desde SSMS)

- [Run an SSIS package from Visual Studio Code with Transact-SQL](../ssis-quickstart-run-tsql-vscode.md) (Ejecutar un paquete de SSIS desde Visual Studio Code con Transact-SQL)

## <a name="run-a-package-with-script-or-code"></a><a name="script"></a> Ejecutar un paquete con un script o código

En cualquier entorno de desarrollo desde el que se pueda llamar a una API administrada, se puede ejecutar un paquete mediante una llamada al método `Execute` del objeto `Package` en el espacio de nombres `Microsoft.SQLServer.Management.IntegrationServices`.

Para obtener más información, vea los ejemplos siguientes:

- [Run an SSIS package with PowerShell](../ssis-quickstart-run-powershell.md) (Ejecutar un paquete de SSIS con PowerShell)

- [Run an SSIS package with C# code in a .NET app](../ssis-quickstart-run-dotnet.md) (Ejecutar un paquete SSIS con código C# en una aplicación .NET)

## <a name="run-a-package-with-the-execute-ssis-package-activity"></a><a name="exec_activity"></a> Ejecutar un paquete con la actividad Ejecutar paquete de SSIS

Para obtener más información, vea [Ejecutar un paquete SSIS con la actividad Ejecutar paquete de SSIS en Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="run-a-package-with-the-stored-procedure-activity"></a><a name="sproc_activity"></a> Ejecutar un paquete con la actividad de procedimiento almacenado

Para obtener más información, vea [Ejecutar un paquete SSIS con la actividad de procedimiento almacenado en Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre las opciones para programar paquetes SSIS implementados en Azure. Para más información, vea [Programar paquetes SSIS en Azure](ssis-azure-schedule-packages.md).