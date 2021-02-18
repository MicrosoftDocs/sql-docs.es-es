---
title: Identificadores de SQL Server en PowerShell | Microsoft Docs
description: Obtenga información sobre las rutas de acceso que los proveedores de Windows PowerShell usan para exponer jerarquías de datos y sobre la necesidad de codificar determinados caracteres no admitidos por PowerShell en estas rutas de acceso.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- Cmdlets [SQL Server], Encode-Sqlname
- PowerShell [SQL Server], identifiers
- Encode-Sqlname cmdlet
- PowerShell [SQL Server], Encode-Sqlname
- Decode-Sqlname cmdlet
- PowerShell [SQL Server], Decode-Sqlname
- identifiers [SQL Server], PowerShell
- Cmdlets [SQL Server], Decode-Sqlname
ms.assetid: 651099b0-33b4-453a-a864-b067f21eb8b9
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.openlocfilehash: b448e5455b985baffbeb8eb611c3f357e083cff2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354921"
---
# <a name="sql-server-identifiers-in-powershell"></a>Identificadores de SQL Server en PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

El proveedor de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para Windows PowerShell usa identificadores [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en las rutas de acceso de Windows PowerShell. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pueden contener caracteres que Windows PowerShell no admite en las rutas de acceso. Debe definir estos caracteres como caracteres de escape o usar una codificación especial para ellos al usar los identificadores en las rutas de acceso de Windows PowerShell.  
  
[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="sql-server-identifiers-in-windows-powershell-paths"></a>Identificadores de SQL Server en rutas de Windows PowerShell  

Los proveedores de Windows PowerShell exponen las jerarquías de datos mediante una estructura de ruta similar al sistema de archivos de Windows. El proveedor de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] implementa rutas a los objetos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . En el [!INCLUDE[ssDE](../includes/ssde-md.md)], la unidad se establece en SQLSERVER:, la primera carpeta se establece en \SQL y se hace referencia a los objetos de la base de datos como contenedores y elementos. Esta es la ruta de acceso a la tabla Vendor en el esquema Purchasing de la base de datos [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] en una instancia predeterminada de [!INCLUDE[ssDE](../includes/ssde-md.md)]:  
  
```  
SQLSERVER:\SQL\MyComputer\DEFAULT\Databases\AdventureWorks2012\Tables\Purchasing.Vendor  
```  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] son los nombres de los objetos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , tales como nombres de tabla o de columna. Hay dos tipos de identificadores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :  
  
-   Los identificadores normales se limitan a un juego de caracteres que también se admite en las rutas de Windows PowerShell. Estos nombres se pueden utilizar en las rutas de Windows PowerShell sin cambiarse.  
  
-   Los identificadores delimitados pueden utilizar caracteres no admitidos en los nombres de ruta de Windows PowerShell. Los identificadores delimitados se denominan identificadores entre corchetes si se escriben entre corchetes ([nombreDeIdentificador]) e identificadores entrecomillados si se escriben entre comillas dobles ("nombreDeIdentificador"). Si un identificador delimitado utiliza caracteres no admitidos en las rutas de Windows PowerShell, estos se deben codificar o hacer que se eludan antes de utilizar el identificador como contenedor o nombre de elemento. La codificación funciona con todos los caracteres. En el caso de ciertos caracteres, como el carácter de dos puntos (:), no se puede hacer que se eludan.  
  
## <a name="sql-server-identifiers-in-cmdlets"></a>Identificadores de SQL Server en cmdlets  
 Algunos cmdlets de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tienen un parámetro que toma un identificador como entrada. Los valores de los parámetros se suelen proporcionar como constantes de cadena entrecomilladas o en variables de cadena. Cuando los identificadores se proporcionan como constantes de cadena o en variables, no hay ningún conflicto con el juego de caracteres admitidos por Windows PowerShell.  
  
## <a name="sql-server-identifier-tasks"></a>Tareas de identificador de SQL Server  
  
|Descripción de la tarea|Artículo|  
|----------------------|-----------|  
|Describe cómo especificar un nombre de instancia, incluido el nombre del equipo en el que se ejecuta la instancia.|[Especificar instancias del proveedor de SQL Server PowerShell](specify-instances-in-the-sql-server-powershell-provider.md)|  
|Describe cómo especificar la codificación hexadecimal para los caracteres en identificadores delimitados que no se admiten en las rutas de acceso de Windows PowerShell. También describe cómo descodificar los caracteres hexadecimales.|[Codificar y descodificar identificadores de SQL Server](encode-and-decode-sql-server-identifiers.md)|  
|Describe cómo usar el carácter de escape de Windows PowerShell para los caracteres no admitidos en las rutas de acceso de PowerShell.|[Identificadores de SQL Server de escape](escape-sql-server-identifiers.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Proveedor de PowerShell de SQL Server](sql-server-powershell-provider.md)   
 [SQL Server PowerShell](sql-server-powershell.md)   
 [Identificadores de base de datos](../relational-databases/databases/database-identifiers.md)  
  
  
