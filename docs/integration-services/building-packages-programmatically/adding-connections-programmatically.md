---
description: Agregar conexiones mediante programación
title: Agregar conexiones mediante programación | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- connection managers [Integration Services], packages
- Integration Services packages, connections
- connections [Integration Services], packages
- SSIS packages, connections
- packages [Integration Services], connections
- intrinsic properties [Integration Services]
- SQL Server Integration Services packages, connections
- ConnectionManager class
- SSIS connection managers
- adding package connections
ms.assetid: d90716d1-4c65-466c-b82c-4aabbee1e3e5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3d15ad56ceb2568f2071e4a4170e95afac24e886
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344922"
---
# <a name="adding-connections-programmatically"></a>Agregar conexiones mediante programación

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La clase <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> representa las conexiones físicas con los orígenes de datos externos. La clase <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> aísla los detalles de la implementación de la conexión del tiempo de ejecución. Esto habilita al tiempo de ejecución para que interactúe con cada administrador de conexiones de una manera coherente y de predicción. Los administradores de conexiones contienen un conjunto de propiedades estándar que todas las conexiones tienen en el común, como <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ID%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Description%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A>. Sin embargo, las propiedades <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A> son normalmente las únicas propiedades que se requieren para configurar un administrador de conexiones. A diferencia de otros paradigmas de programación, donde las clases de conexión exponen métodos como **Open** o **Connect** para establecer físicamente una conexión al origen de datos, el motor en tiempo de ejecución administra todas las conexiones para el paquete mientras se ejecuta.  
  
 La clase <xref:Microsoft.SqlServer.Dts.Runtime.Connections> es una colección de los administradores de conexiones agregados a ese paquete y que están disponibles para su uso en tiempo de ejecución. Puede agregar más administradores de conexiones a la colección mediante el método <xref:Microsoft.SqlServer.Dts.Runtime.Connections.Add%2A> de la colección y proporcionando una cadena que indica el tipo de administrador de conexiones. El método <xref:Microsoft.SqlServer.Dts.Runtime.Connections.Add%2A> devuelve la instancia <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> que se agregó al paquete.  
  
## <a name="intrinsic-properties"></a>Propiedades intrínsecas  
 La clase <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> expone un conjunto de propiedades comunes para todas las colecciones. Sin embargo, a veces se necesita tener acceso a las propiedades que son únicas para un tipo de conexión concreto. La colección <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Properties%2A> de la clase <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> proporciona el acceso a estas propiedades. Las propiedades se pueden recuperar de la colección mediante el indizador, o el nombre de propiedad y el método **GetValue**, y los valores se establecen mediante el método **SetValue**. Las propiedades de las propiedades de objeto de conexión subyacente también se pueden establecer adquiriendo una instancia real del objeto y estableciendo directamente sus propiedades. Para obtener la conexión subyacente, use la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.InnerObject%2A> del administrador de conexiones. La siguiente línea de código muestra una línea de C# que crea un administrador de conexiones de ADO.NET que tiene la clase subyacente, <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.ConnectionManagerAdoNetClass>.  
  
 `ConnectionManagerAdoNetClass cmado = cm.InnerObject as ConnectionManagerAdoNet;`  
  
 Esto convierte el objeto de administrador de conexiones en su objeto de conexión subyacente. Si usa C++, se llama al método **QueryInterface** del objeto <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> y se solicita la interfaz del objeto de conexión subyacente.  
  
 En la tabla siguiente se enumeran los administradores de conexión que se incluyen en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y la cadena que se utiliza en la instrucción `package.Connections.Add("xxx")`. Para obtener una lista de todos los administradores de conexión, vea [Conexiones de Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
|String|Administrador de conexiones|  
|------------|------------------------|  
|"OLEDB"|Administrador de conexiones para conexiones OLE DB.|  
|"ODBC"|Administrador de conexiones para conexiones ODBC.|  
|"ADO"|Administrador de conexiones para conexiones ADO.|  
|"ADO.NET:SQL"|Administrador de conexiones para conexiones ADO.NET (proveedor de datos SQL).|  
|"ADO.NET:OLEDB"|Administrador de conexiones para conexiones ADO.NET (proveedor de datos OLE DB).|  
|"FLATFILE"|Administrador de conexiones para conexiones de archivos planos.|  
|"FILE"|Administrador de conexiones para conexiones de archivos.|  
|"MULTIFLATFILE"|Administrador de conexiones para varias conexiones de archivos planos.|  
|"MULTIFILE"|Administrador de conexiones para varias conexiones de archivos.|  
|"SQLMOBILE"|Administrador de conexiones para conexiones [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.|  
|"MSOLAP100"|Administrador de conexiones para conexiones [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|"FTP"|Administrador de conexiones para conexiones FTP.|  
|"HTTP"|Administrador de conexiones para conexiones HTTP.|  
|"MSMQ"|Administrador de conexiones para conexiones Message Queuing (también denominado MSMQ).|  
|"SMTP"|Administrador de conexiones para conexiones SMTP.|  
|"WMI"|Administrador de conexiones para conexiones WMI (Instrumental de administración de Windows).|  
  
 En el siguiente ejemplo de código se muestra cómo agregar una conexión OLE DB y FILE a la colección <xref:Microsoft.SqlServer.Dts.Runtime.Package.Connections%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.Package>. Después, se establecen las propiedades <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Description%2A>.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      // Create a package, and retrieve its connections.  
      Package pkg = new Package();  
      Connections pkgConns = pkg.Connections;  
  
      // Add an OLE DB connection to the package, using the   
      // method defined in the AddConnection class.  
      CreateConnection myOLEDBConn = new CreateConnection();  
      myOLEDBConn.CreateOLEDBConnection(pkg);  
  
      // View the new connection in the package.  
      Console.WriteLine("Connection description: {0}",  
         pkg.Connections["SSIS Connection Manager for OLE DB"].Description);  
  
      // Add a second connection to the package.  
      CreateConnection myFileConn = new CreateConnection();  
      myFileConn.CreateFileConnection(pkg);  
  
      // View the second connection in the package.  
      Console.WriteLine("Connection description: {0}",  
        pkg.Connections["SSIS Connection Manager for Files"].Description);  
  
      Console.WriteLine();  
      Console.WriteLine("Number of connections in package: {0}", pkg.Connections.Count);  
  
      Console.Read();  
    }  
  }  
  // <summary>  
  // This class contains the definitions for multiple  
  // connection managers.  
  // </summary>  
  public class CreateConnection  
  {  
    // Private data.  
    private ConnectionManager ConMgr;  
  
    // Class definition for OLE DB Provider.  
    public void CreateOLEDBConnection(Package p)  
    {  
      ConMgr = p.Connections.Add("OLEDB");  
      ConMgr.ConnectionString = "Provider=SQLOLEDB.1;" +  
        "Integrated Security=SSPI;Initial Catalog=AdventureWorks;" +  
        "Data Source=(local);";  
      ConMgr.Name = "SSIS Connection Manager for OLE DB";  
      ConMgr.Description = "OLE DB connection to the AdventureWorks database.";  
    }  
    public void CreateFileConnection(Package p)  
    {  
      ConMgr = p.Connections.Add("File");  
      ConMgr.ConnectionString = @"\\<yourserver>\<yourfolder>\books.xml";  
      ConMgr.Name = "SSIS Connection Manager for Files";  
      ConMgr.Description = "Flat File connection";  
    }  
  }  
  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    ' Create a package, and retrieve its connections.  
    Dim pkg As New Package()  
    Dim pkgConns As Connections = pkg.Connections  
  
    ' Add an OLE DB connection to the package, using the   
    ' method defined in the AddConnection class.  
    Dim myOLEDBConn As New CreateConnection()  
    myOLEDBConn.CreateOLEDBConnection(pkg)  
  
    ' View the new connection in the package.  
    Console.WriteLine("Connection description: {0}", _  
      pkg.Connections("SSIS Connection Manager for OLE DB").Description)  
  
    ' Add a second connection to the package.  
    Dim myFileConn As New CreateConnection()  
    myFileConn.CreateFileConnection(pkg)  
  
    ' View the second connection in the package.  
    Console.WriteLine("Connection description: {0}", _  
      pkg.Connections("SSIS Connection Manager for Files").Description)  
  
    Console.WriteLine()  
    Console.WriteLine("Number of connections in package: {0}", pkg.Connections.Count)  
  
    Console.Read()  
  
  End Sub  
  
End Module  
  
' This class contains the definitions for multiple  
' connection managers.  
  
Public Class CreateConnection  
  ' Private data.  
  Private ConMgr As ConnectionManager  
  
  ' Class definition for OLE DB provider.  
  Public Sub CreateOLEDBConnection(ByVal p As Package)  
    ConMgr = p.Connections.Add("OLEDB")  
    ConMgr.ConnectionString = "Provider=SQLOLEDB.1;" & _  
      "Integrated Security=SSPI;Initial Catalog=AdventureWorks;" & _  
      "Data Source=(local);"  
    ConMgr.Name = "SSIS Connection Manager for OLE DB"  
    ConMgr.Description = "OLE DB connection to the AdventureWorks database."  
  End Sub  
  
  Public Sub CreateFileConnection(ByVal p As Package)  
    ConMgr = p.Connections.Add("File")  
    ConMgr.ConnectionString = "\\<yourserver>\<yourfolder>\books.xml"  
    ConMgr.Name = "SSIS Connection Manager for Files"  
    ConMgr.Description = "Flat File connection"  
  End Sub  
  
End Class  
```  
  
 **Salida del ejemplo:**  
  
 `Connection description: OLE DB connection to the AdventureWorks database.`  
  
 `Connection description: Flat File connection.`  
  
 `Number of connections in package: 2`  
  
## <a name="external-resources"></a>Recursos externos  
 Artículo técnico, [Connection Strings](https://go.microsoft.com/fwlink/?LinkId=220743) (Cadenas de conexión), en carlprothman.net.  
  
## <a name="see-also"></a>Vea también  
 [Conexiones de Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [Crear administradores de conexiones](../connection-manager/integration-services-ssis-connections.md)  
  
