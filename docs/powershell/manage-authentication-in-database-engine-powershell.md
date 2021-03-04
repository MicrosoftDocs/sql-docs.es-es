---
title: Administración de la autenticación en SQL Server en PowerShell
description: Obtenga información sobre cómo usar la autenticación de SQL Server en lugar de la autenticación de Windows (el valor predeterminado) al conectarse a una instancia del motor de base de datos.
titleSuffix: SQL Server on Linux
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 10/14/2020
ms.openlocfilehash: 233c456a2e4c7f41efd6038efccc6b00b68eb72d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838069"
---
# <a name="manage-authentication-to-sql-server-in-powershell"></a>Administración de la autenticación en SQL Server en PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

De forma predeterminada, los componentes PowerShell de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usan la autenticación de Windows para conectarse a una instancia de [!INCLUDE[ssDE](../includes/ssde-md.md)]. Puede usar la autenticación de SQL Server definiendo una unidad virtual de PowerShell o especificando los parámetros **-Username** y **-Password** para **Invoke-Sqlcmd**.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="permissions"></a>Permisos

Todas las acciones que se pueden realizar en una instancia de [!INCLUDE[ssDE](../includes/ssde-md.md)] se controlan mediante los permisos concedidos a las credenciales de autenticación usadas para conectarse a la instancia. De forma predeterminada, el proveedor y los cmdlets de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] usa la cuenta de Windows de ejecución para establecer una conexión de autenticación de Windows con [!INCLUDE[ssDE](../includes/ssde-md.md)].  

Para establecer una conexión de autenticación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] debe proporcionar un identificador de inicio de sesión y contraseña de autenticación de SQL Server. Al usar el proveedor [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , debe asociar las credenciales de inicio de sesión de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a una unidad virtual y, después, usar el comando de cambio de directorio (**cd**) para conectarse a esa unidad de disco. En Windows PowerShell, las credenciales de seguridad solo se pueden asociar con unidades virtuales.  

## <a name="sql-server-authentication-using-a-virtual-drive"></a>Autenticación de SQL Server mediante una unidad virtual

### <a name="to-create-a-virtual-drive-associated-with-a-sql-server-authentication-login"></a>Para crear una unidad virtual asociada con el inicio de sesión de autenticación de SQL Server

1. Crear una función que:

    1. Tiene parámetros para que el nombre proporcione la unidad virtual, el identificador de inicio de sesión y la ruta de acceso del proveedor para asociarla a la unidad virtual.

    2. Usa **read-host** para pedir la contraseña al usuario.  

    3. Usa **new-object** para crear un objeto de credenciales.  

    4. Usa **new-psdrive** para crear una unidad virtual con las credenciales proporcionadas.  

2. Invocar la función para crear una unidad virtual con las credenciales proporcionadas.  

#### <a name="example-virtual-drive"></a>Ejemplo (unidad virtual)

En este ejemplo se crea una función denominada **sqldrive** que se puede usar para crear una unidad virtual asociada a la instancia e inicio de sesión de la autenticación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] especificados.  
  
 La función **sqldrive** pide que especifique la contraseña para su inicio de sesión, enmascarándola a medida que la escribe. Siempre que use el comando de cambio de directorio (**cd**) para conectar a una ruta de acceso usando el nombre de la unidad virtual, todas las operaciones se realizan usando las credenciales de inicio de sesión de la autenticación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que especificó al crear la unidad.  
  
```powershell
## Create a function that specifies the login and prompts for the password.  
  
function sqldrive  
{  
    param( [string]$name, [string]$login = "MyLogin", [string]$root = "SQLSERVER:\SQL\MyComputer\MyInstance" )  
    $pwd = read-host -AsSecureString -Prompt "Password"  
    $cred = new-object System.Management.Automation.PSCredential -argumentlist $login,$pwd  
    New-PSDrive $name -PSProvider SqlServer -Root $root -Credential $cred -Scope 1  
}  
  
## Use the sqldrive function to create a SQLAuth virtual drive.  
sqldrive SQLAuth
  
## Set-Location to the virtual drive, which invokes the supplied authentication credentials.  
sl SQLAuth:
```

## <a name="sql-server-authentication-using-invoke-sqlcmd"></a>Autenticación de SQL Server mediante Invoke-Sqlcmd

### <a name="to-use-invoke-sqlcmd-with-sql-server-authentication"></a>Para usar Invoke-Sqlcmd con la autenticación de SQL Server

1. Use el parámetro **-Username** para especificar un identificador de inicio de sesión y el parámetro **-Password** para especificar la contraseña asociada.  

#### <a name="example-invoke-sqlcmd"></a>Ejemplo (Invoke-Sqlcmd)

En este ejemplo se usa el cmdlet read-host para pedir al usuario una contraseña, y después se conecta con la autenticación de SQL Server.  

```powershell
## Prompt the user for their password.  
$pwd = read-host -AsSecureString -Prompt "Password"  
  
Invoke-Sqlcmd -Query "SELECT GETDATE() AS TimeOfQuery;" -ServerInstance "MyComputer\MyInstance" -Username "MyLogin" -Password $pwd  
```

## <a name="see-also"></a>Consulte también

- [SQL Server PowerShell](sql-server-powershell.md)
- [Proveedor de SQL Server PowerShell Provider](sql-server-powershell-provider.md)
- [Cmdlet Invoke-Sqlcmd](/powershell/module/sqlserver/invoke-sqlcmd)
- [Uso de PowerShell con Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)
