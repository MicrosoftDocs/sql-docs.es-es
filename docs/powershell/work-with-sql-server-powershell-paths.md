---
title: Trabajar con rutas acceso de SQL Server PowerShell
description: Obtenga información sobre cómo manipular y recuperar información mediante cmdlets o los métodos y las propiedades del objeto identificado por la ruta de acceso del proveedor.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 0b6878c7e0e6435bbb763629547437e9c22b062e
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838857"
---
# <a name="work-with-sql-server-powershell-paths"></a>Trabajar con rutas acceso de SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Después de haber navegado a un nodo de una ruta de acceso del proveedor de [!INCLUDE[ssDE](../includes/ssde-md.md)] , puede realizar el trabajo o recuperar información mediante los métodos y propiedades de los objetos de administración de [!INCLUDE[ssDE](../includes/ssde-md.md)] asociados al nodo.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Después de haber navegado a un nodo de una ruta de acceso del proveedor de [!INCLUDE[ssDE](../includes/ssde-md.md)], puede realizar dos tipos de acciones:  

- Puede ejecutar los cmdlets de Windows PowerShell que operan en los nodos, como **Rename-Item**.  

- Puede llamar a los métodos desde el modelo de objetos de administración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] asociado, como SMO. Por ejemplo, si navega al nodo Bases de datos de una ruta de acceso, puede usar los métodos y las propiedades de la clase <xref:Microsoft.SqlServer.Management.Smo.Database> .  

El proveedor de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se utiliza para administrar los objetos en una instancia de [!INCLUDE[ssDE](../includes/ssde-md.md)]. No se utiliza para trabajar con los datos de las bases de datos. Si ha navegado a una tabla o vista, no puede utilizar el proveedor para seleccionar, insertar, actualizar o eliminar datos. Use el cmdlet **Invoke-Sqlcmd** para consultar o cambiar los datos de las tablas y vistas del entorno de Windows PowerShell. Para obtener más información, vea [cmdlet Invoke-Sqlcmd](/powershell/module/sqlserver/invoke-sqlcmd).  

##  <a name="listing-methods-and-properties"></a><a name="ListPropMeth"></a> Enumerar métodos y propiedades  

**Enumerar métodos y propiedades**  

Para ver los métodos y propiedades disponibles de determinados objetos o clases de objetos, use el cmdlet **Get-Member** .  

### <a name="examples-listing-methods-and-properties"></a>Ejemplos: Enumerar métodos y propiedades

En este ejemplo se establece una variable de Windows PowerShell en la clase <xref:Microsoft.SqlServer.Management.Smo.Database> de SMO y se enumeran los métodos y las propiedades:  

```powershell
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar | Get-Member -Type Methods  
$MyDBVar | Get-Member -Type Properties  
```  
  
 También puede usar **Get-Member** para mostrar los métodos y propiedades asociados con el nodo final de una ruta de acceso de Windows PowerShell.  
  
 En este ejemplo se navega al nodo Databases de una ruta de acceso de SQLSERVER: y se muestran las propiedades de la colección:  
  
```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
Get-Item . | Get-Member -Type Properties  
```  
  
 En este ejemplo se navega al nodo AdventureWorks2012 de una ruta de acceso de SQLSERVER: y se muestran las propiedades del objeto:  
  
```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
Get-Item . | Get-Member -Type Properties  
```  

##  <a name="using-methods-and-properties"></a><a name="UsePropMeth"></a> Usar métodos y propiedades  

**Usar métodos y propiedades de SMO**  

Para realizar el trabajo en objetos de una ruta de acceso del proveedor de [!INCLUDE[ssDE](../includes/ssde-md.md)] , puede usar los métodos y las propiedades de SMO.  

### <a name="examples-using-methods-and-properties"></a>Ejemplos: Usar métodos y propiedades

En este ejemplo se usa la propiedad **Schema** de SMO para obtener una lista de las tablas del esquema Sales en AdventureWorks2012:  

```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables  
Get-ChildItem | where {$_.Schema -eq "Sales"}  
```

En este ejemplo se usa el método **Script** de SMO para generar un script que contiene las instrucciones **CREATE VIEW** que se deben tener para volver a crear las vistas en AdventureWorks2012:  

```powershell
Remove-Item C:\PowerShell\CreateViews.sql  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Views  
foreach ($Item in Get-ChildItem) { $Item.Script() | Out-File -Filepath C:\PowerShell\CreateViews.sql -append }  
```

En este ejemplo se utiliza el método **Create** de SMO para crear una base de datos y, a continuación, se usa la propiedad **State** para mostrar si la base de datos existe:  

```powershell
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar.Parent = (Get-Item ..)  
$MyDBVar.Name = "NewDB"  
$MyDBVar.Create()  
$MyDBVar.State  
```

## <a name="see-also"></a>Consulte también

- [Proveedor de SQL Server PowerShell Provider](sql-server-powershell-provider.md)
- [Navegar por las rutas de acceso de SQL Server PowerShell](navigate-sql-server-powershell-paths.md)
- [Convertir URN en rutas de acceso del proveedor de SQL Server](/powershell/module/sqlserver/Convert-UrnToPath)
- [SQL Server PowerShell](sql-server-powershell.md)