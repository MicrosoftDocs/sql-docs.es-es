---
title: Navegar por las rutas de acceso de SQL Server PowerShell
description: Aprenda a usar cmdlets de PowerShell para navegar por las estructuras de ruta de acceso que representan la jerarquía de objetos admitidos por un proveedor de PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: ef6e6b71d43c73f92f5e73cc7f5faf84230c2c26
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838015"
---
# <a name="navigate-sql-server-powershell-paths"></a>Navegar por las rutas de acceso de SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

El proveedor de PowerShell de [!INCLUDE[ssDE](../includes/ssde-md.md)] expone el conjunto de objetos de una instancia de SQL Server en una estructura similar a una ruta de acceso del archivo. Puede usar los cmdlets de Windows PowerShell para navegar por la ruta de acceso del proveedor y crear las unidades personalizadas para acortar la ruta de acceso que tiene que escribir.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Windows PowerShell implementa cmdlets para navegar por la estructura de ruta de acceso que representa la jerarquía de objetos compatible con un proveedor de PowerShell. Cuando ha navegado a un nodo de la ruta de acceso, puede usar otros cmdlets para realizar operaciones básicas en el objeto actual. Dado que los cmdlets se usan con frecuencia, tienen alias canónicos cortos. También hay un conjunto de alias que asigna los cmdlets a comandos del símbolo del sistema similares, y otro conjunto para los comandos shell de UNIX.  

El proveedor de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] implementa un subconjunto de los cmdlets de proveedor, que se muestran en la tabla siguiente:  

|cmdlet|Alias canónico|Alias de cmd|Alias de shell de UNIX|Descripción|  
|------------|---------------------|---------------|----------------------|-----------------|  
|**Get-Location**|**gl**|**pwd**|**pwd**|Obtiene el nodo actual.|  
|**Set-Location**|**sl**|**cd, chdir**|**cd, chdir**|Cambia el nodo actual.|  
|**Get-ChildItem**|**gci**|**dir**|**ls**|Enumera los objetos almacenados en el nodo actual.|  
|**Get-Item**|**gi**|||Devuelve las propiedades del elemento actual.|  
|**Rename-Item**|**rni**|**rn**|**ren**|Cambia el nombre de un objeto.|  
|**Remove-Item**|**ri**|**del, rd**|**rm, rmdir**|Quita un objeto.|  

> [!IMPORTANT]
> Algunos identificadores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (nombres de objeto) contienen caracteres que Windows PowerShell no admite en los nombres de ruta de acceso. Para obtener más información sobre cómo usar nombres que contengan estos caracteres, vea [SQL Server Identifiers in PowerShell](sql-server-identifiers-in-powershell.md).  

## <a name="sql-server-information-returned-by-get-childitem"></a>Información de SQL Server devuelta por Get-ChildItem  

La información devuelta por **Get-ChildItem** (o sus alias **dir** e **ls** ) depende de la ubicación en una ruta de acceso de SQLSERVER.  

|Ubicación de la ruta de acceso|Resultados de Get-ChildItem|
|-------------------|----------------------------|
|SQLSERVER:\SQL|Devuelve el nombre del equipo local. Si ha usado SMO o WMI para conectarse a las instancias de [!INCLUDE[ssDE](../includes/ssde-md.md)] en otros equipos, esos equipos también se enumeran.|  
|SQLSERVER:\SQL\\*nombreDeEquipo*|Lista de instancias del [!INCLUDE[ssDE](../includes/ssde-md.md)] en el equipo.|  
|SQLSERVER:\SQL\\*nombreDeEquipo*\\*nombreDeInstancia*|Lista de tipos de objeto de nivel superior en la instancia, como Extremos, Certificados y Bases de datos.|  
|Nodo de clase de objeto, como Databases|Lista de objetos de ese tipo, como la lista de bases de datos: master, model, AdventureWorks2008R2.|  
|Nodo de nombre de objeto, como AdventureWorks2012|Lista de los tipos de objeto contenidos en el objeto. Por ejemplo, una base de datos mostraría tipos de objeto como tablas y vistas.|  

De forma predeterminada, **Get-ChildItem** no muestra ningún objeto del sistema. Use el parámetro *Force* para ver los objetos del sistema, como los objetos del esquema **sys** .  

### <a name="custom-drives"></a>Unidades personalizadas

Windows PowerShell deja que los usuarios definan las unidades virtuales, a las que se hace referencia como unidades de PowerShell. Estas unidades se asignan en los nodos de inicio de una instrucción de ruta de acceso. Se suelen usar para acortar las rutas de acceso que se escriben con frecuencia. Las rutas de acceso de SQLSERVER: pueden requerir mucho tiempo y espacio en la ventana de Windows PowerShell, así como mucho esfuerzo para escribirlas. Si va a trabajar mucho en un nodo de ruta de acceso determinado, puede definir una unidad de Windows PowerShell personalizada que se asigne a ese nodo.  
  
## <a name="use-powershell-cmdlet-aliases"></a>Use Alias de Cmdlet de PowerShell

**Use un alias de cmdlet**

- En lugar de escribir un nombre completo de cmdlet, escriba un alias más corto que se asigna a un comando de símbolo de sistema.  

### <a name="alias-example-powershell"></a>Ejemplo de Alias (PowerShell)  

Por ejemplo, puede usar uno de los conjuntos de cmdlets o alias siguientes para recuperar una lista de las instancias de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] disponibles para navegar a la carpeta SQLSERVER:\SQL y solicitar la lista de elementos secundarios de la misma:  
  
```powershell  
## Shows using the full cmdet name.  
Set-Location SQLSERVER:\SQL  
Get-ChildItem  
  
## Shows using canonical aliases.  
sl SQLSERVER:\SQL  
gci  
  
## Shows using command prompt aliases.  
cd SQLSERVER:\SQL  
dir  
  
## Shows using Unix shell aliases.  
cd SQLSERVER:\SQL  
ls  
```
  
## <a name="use-get-childitem"></a>Uso de Get-ChildItem  
 **Devuelve información mediante Get-Childitem**  
  
1.  Navega hasta el nodo del que quiere obtener una lista de elementos secundarios.  
  
2.  Ejecuta Get-Childitem para obtener la lista.  
  
### <a name="get-childitem-example-powershell"></a>Ejemplo de Get-ChildItem (PowerShell)  
 Estos ejemplos muestran la información devuelta por Get-Childitem para varios nodos en una ruta de acceso del proveedor de SQL Server.  
  
```powershell  
## Return the current computer and any computer  
## to which you have made a SQL or WMI connection.  
Set-Location SQLSERVER:\SQL  
Get-ChildItem  
  
## List the instances of the Database Engine on the local computer.  
  
Set-Location SQLSERVER:\SQL\localhost  
Get-ChildItem  
  
## Lists the categories of objects available in the  
## default instance on the local computer.  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT  
Get-ChildItem  
  
## Lists the databases from the local default instance.  
## The force parameter is used to include the system databases.  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
Get-ChildItem -force  
```
  
## <a name="create-a-custom-drive"></a>Crea una unidad personalizada  
 **Cree y use una unidad personalizada**  
  
1.  Use **New-PSDrive** para definir una unidad personalizada. Use el parámetro de **Root** para especificar la ruta de acceso representada por el nombre de la unidad personalizada.  
  
2.  Haga referencia al nombre de la unidad personalizada en cmdlets de navegación de la ruta de acceso como **Set-Location**.  
  
### <a name="custom-drive-example-powershell"></a>Ejemplo de unidad personalizada (PowerShell)  
 Este ejemplo crea una unidad virtual denominada AWDB que asigna al nodo de una copia implementada de la base de datos de ejemplo de AdventureWorks2012. La unidad virtual se usa para navegar a una tabla de la base de datos.  
  
```powershell  
## Create a new virtual drive.  
New-PSDrive -Name AWDB -Root SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
  
## Use AWDB: to navigate to a specific table.  
Set-Location AWDB:\Tables\Purchasing.Vendor  
```
  
## <a name="see-also"></a>Consulte también

- [Proveedor de SQL Server PowerShell Provider](sql-server-powershell-provider.md)
- [Trabajar con rutas acceso de SQL Server PowerShell](work-with-sql-server-powershell-paths.md)
- [SQL Server PowerShell](sql-server-powershell.md)
