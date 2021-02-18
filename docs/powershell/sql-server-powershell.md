---
title: SQL Server PowerShell
description: Obtenga información sobre los dos módulos de SQL Server PowerShell, SqlServer y SQLPS, que incluyen los proveedores y cmdlets de PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 89b70725-bbe7-4ffe-a27d-2a40005a97e7
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: ''
ms.date: 06/11/2020
ms.openlocfilehash: d8b03ae1e20acfefd673717858d57c894c619437
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350149"
---
# <a name="sql-server-powershell"></a>SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

**[Instalación de SQL Server PowerShell](download-sql-server-ps-module.md)**

Hay dos módulos de SQL Server PowerShell: **[SqlServer](/powershell/module/sqlserver)** y **[SQLPS](/powershell/module/sqlps)** .

El módulo **SqlServer** es el módulo de PowerShell actual que se va a usar.

El módulo **SQLPS** está incluido en la instalación de SQL Server (por motivos de compatibilidad con versiones anteriores), pero ya no se actualiza.

El módulo **SqlServer** contiene versiones actualizadas de los cmdlets en **SQLPS** e incluye cmdlets para admitir las características más recientes de SQL.

Las versiones anteriores del módulo **SqlServer** *estaban* incluidas en [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md), pero solo con las versiones 16.x de SSMS.

Para usar PowerShell con SSMS 17.0 y versiones posteriores, instale el módulo **SqlServer** desde la [Galería de PowerShell](https://www.powershellgallery.com/packages/SqlServer).

También puede usar [PowerShell con Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md).

**¿Por qué ha cambiado el módulo de SQLPS a SqlServer?**

Para incluir las actualizaciones de SQL PowerShell, fue necesario cambiar la identidad del módulo SQL PowerShell, así como el contenedor conocido como *SQLPS.exe*. Debido a este cambio, ahora hay dos módulos de SQL PowerShell, **SqlServer** y **SQLPS**.  

**Actualice los scripts de PowerShell si importa el módulo SQLPS.**

Si tiene algún script de PowerShell que ejecute `Import-Module -Name SQLPS` y quiere aprovechar la nueva funcionalidad de proveedor y los nuevos cmdlets, debe cambiarlos a `Import-Module -Name SqlServer`. El módulo nuevo se instala en la carpeta `%ProgramFiles%\WindowsPowerShell\Modules\SqlServer`. Por ese motivo, no tiene que actualizar la variable $env:PSModulePath. Si tiene scripts que usen una versión de comunidad o de terceros de un módulo denominado **SqlServer**, emplee el parámetro Prefix para evitar colisiones de nombres.

Se recomienda iniciar el script con *Import-Module SQLServer* para evitar problemas en paralelo si el módulo SQLPS está instalado en el mismo equipo.

Esta sección se aplica a los scripts que se ejecutan desde PowerShell y no desde el Agente SQL. El nuevo módulo se puede usar con pasos de trabajo del Agente SQL mediante [#NOSQLPS](#sql-server-agent).

## <a name="sql-server-powershell-components"></a>Componentes de SQL Server PowerShell

El módulo **SqlServer** incluye lo siguiente:

- [Proveedores de PowerShell](/powershell/module/microsoft.powershell.core/about/about_providers), que habilita un mecanismo de navegación sencillo similar a las rutas de acceso al sistema de archivos. Puede crear rutas de acceso similares a las del sistema de archivos, en las que la unidad se asocia a un modelo de objetos de administración de SQL Server y los nodos se basan en las clases del modelo de objetos. A continuación, puede usar comandos conocidos, como **cd** y **dir** , para navegar por las rutas de acceso de modo similar a como se navega por las carpetas en una ventana del símbolo del sistema. Puede usar otros comandos, como **ren** o **del**, para realizar acciones en los nodos de la ruta de acceso.

- Un conjunto de cmdlets que admiten acciones como la ejecución de un script **sqlcmd** que contenga instrucciones de Transact-SQL o XQuery.  

- El proveedor y los cmdlets de AS, que antes se instalaban por separado.

## <a name="sql-server-versions"></a>Versiones de SQL Server

Los cmdlets de SQL PowerShell se pueden usar para administrar instancias de Azure SQL Database, Azure Synapse Analytics y todos los [productos de SQL Server compatibles](https://support.microsoft.com/lifecycle/search/1044).

## <a name="sql-server-identifiers-that-contain-characters-not-supported-in-powershell-paths"></a>Identificadores de SQL Server que contienen caracteres no admitidos en las rutas de acceso de PowerShell

Los cmdlets **Encode-Sqlname** y **Decode-Sqlname** le ayudan a especificar los identificadores de SQL Server que contienen caracteres no admitidos en las rutas de acceso de PowerShell. Para más información, consulte [SQL Server Identifiers in PowerShell](sql-server-identifiers-in-powershell.md).

Use el cmdlet **Convert-UrnToPath** para convertir un nombre de recurso único de un objeto de Motor de base de datos en una ruta de acceso para el proveedor SQL Server PowerShell. Para más información, consulte [Convert URNs to SQL Server Provider Paths](/powershell/module/sqlserver/Convert-UrnToPath).
  
## <a name="query-expressions-and-unique-resource-names"></a>Expresiones de consulta y nombres de recursos únicos  

Las expresiones de consulta son cadenas que usan una sintaxis similar a XPath para especificar un conjunto de criterios en el que se enumeran uno o más objetos de una jerarquía del modelo de objetos. Un nombre de recurso único (URN) es un tipo específico de cadena de expresión de consulta que identifica exclusivamente un objeto único. Para más información, consulte [Query Expressions and Uniform Resource Names](query-expressions-and-uniform-resource-names.md).

## <a name="sql-server-agent"></a>Agente SQL Server

No hay ningún cambio en el módulo que usa el Agente SQL Server. Como tal, los trabajos del Agente SQL Server, que tienen pasos de trabajo de tipo de PowerShell, usan el módulo SQLPS. Para obtener más información, vea [Ejecución de PowerShell con el Agente SQL Server](run-windows-powershell-steps-in-sql-server-agent.md). Pero a partir de SQL Server 2019, puede deshabilitar SQLPS. Para ello, en la primera línea de un paso de trabajo del tipo PowerShell puede agregar `#NOSQLPS`, que impide que el Agente SQL cargue de forma automática el módulo SQLPS. Al hacerlo, el trabajo del Agente SQL ejecutará la versión de PowerShell instalada en el equipo y, después, podrá usar cualquier otro módulo de PowerShell que quiera.

Si quiere usar el módulo **SqlServer** en el paso de trabajo del Agente SQL, puede colocar este código en las dos primeras líneas del script.

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```

## <a name="cmdlet-reference"></a>Referencia de cmdlets

- [Cmdlets de SqlServer](/powershell/module/sqlserver)
- [Cmdlets de SQLPS](/powershell/module/sqlps)

## <a name="next-steps"></a>Pasos siguientes

- [Descarga del módulo de PowerShell de SQL Server](download-sql-server-ps-module.md)
- [Cmdlets de SQL Server PowerShell](/powershell/module/sqlserver)
- [Uso de PowerShell con Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)