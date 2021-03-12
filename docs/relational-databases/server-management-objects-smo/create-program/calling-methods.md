---
description: Llamar a métodos
title: Llamar a métodos | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- methods [SMO]
- calling methods
- SQL Server Management Objects, method calling
- SMO [SQL Server], method calling
ms.assetid: c88d5c5f-9ff0-4f84-b2b6-24c6b90fa15e
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ef9906590a509f05a6ef23153e273a98ff687d89
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770563"
---
# <a name="calling-methods"></a>Llamar a métodos
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Los métodos realizan tareas específicas relacionadas con el objeto, como la emisión de un **Checkpoint** en una base de datos o la solicitud de una lista enumerada de inicios de sesión para la instancia de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Los métodos realizan una operación en un objeto. Los métodos pueden tomar parámetros y a menudo tener un valor devuelto. El valor devuelto puede ser un tipo de datos simple, un objeto complejo o una estructura que contiene muchos miembros.  
  
 Use el control de excepciones para detectar si el método se realizó correctamente. Para más información, consulte [Handling SMO Exceptions](../../../relational-databases/server-management-objects-smo/create-program/handling-smo-exceptions.md).  
  
## <a name="examples"></a>Ejemplos  
Para utilizar cualquier ejemplo de código que se proporcione, deberá elegir el entorno de programación, la plantilla de programación y el lenguaje de programación con los que crear su aplicación. Para obtener más información, vea  [crear un proyecto de Visual C&#35; SMO en Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
 
  
## <a name="using-a-simple-smo-method-in-visual-basic"></a>Usar un método SMO simple en Visual Basic  
 En este ejemplo, el método <xref:Microsoft.SqlServer.Management.Smo.Database.Create%2A> no toma ningún parámetro y no devuelve ningún valor.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Define a Database object variable by supplying the parent server and the database name arguments in the constructor.
Dim db As Database
db = New Database(srv, "Test_SMO_Database")
'Call the Create method to create the database on the instance of SQL Server. 
db.Create()
```
  
## <a name="using-a-simple-smo-method-in-visual-c"></a>Usar un método SMO simple en Visual C#  
 En este ejemplo, el método <xref:Microsoft.SqlServer.Management.Smo.Database.Create%2A> no toma ningún parámetro y no devuelve ningún valor.  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Define a Database object variable by supplying the parent server and the database name arguments in the constructor.   
Database db;   
db = new Database(srv, "Test_SMO_Database");   
//Call the Create method to create the database on the instance of SQL Server.   
db.Create();   
}
```  
  
## <a name="using-an-smo-method-with-a-parameter-in-visual-basic"></a>Usar un método SMO con un parámetro en Visual Basic  
 El objeto <xref:Microsoft.SqlServer.Management.Smo.Table> tiene un método denominado <xref:Microsoft.SqlServer.Management.Smo.Table.RebuildIndexes%2A>. Este método requiere un parámetro numérico que especifica **FillFactor**.  
  
```VBNET
Dim srv As Server  
srv = New Server  
Dim tb As Table  
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources")  
tb.RebuildIndexes(70)  
```  
  
## <a name="using-an-smo-method-with-a-parameter-in-visual-c"></a>Usar un método SMO con un parámetro en Visual C#  
 El objeto <xref:Microsoft.SqlServer.Management.Smo.Table> tiene un método denominado <xref:Microsoft.SqlServer.Management.Smo.Table.RebuildIndexes%2A>. Este método requiere un parámetro numérico que especifica `FillFactor`.  
  
```csharp  
{   
Server srv = default(Server);   
srv = new Server();   
Table tb = default(Table);   
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources");   
tb.RebuildIndexes(70);   
}   
```  
  
## <a name="using-an-enumeration-method-that-returns-a-datatable-object-in-visual-basic"></a>Usar un método de enumeración que devuelve un objeto DataTable en Visual Basic  
 En esta sección se describe cómo llamar a un método de enumeración y cómo controlar los datos del objeto <xref:System.Data.DataTable> devuelto.  
  
 El método <xref:Microsoft.SqlServer.Management.Smo.Server.EnumCollations%2A> devuelve un objeto <xref:System.Data.DataTable>, que requiere más navegación para tener acceso a toda la información de intercalación disponible acerca de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```VBNET
'Connect to the local, default instance of SQL Server.  
Dim srv As Server  
srv = New Server  
'Call the EnumCollations method and return collation information to DataTable variable.  
Dim d As DataTable  
'Select the returned data into an array of DataRow.  
d = srv.EnumCollations  
'Iterate through the rows and display collation details for the instance of SQL Server.  
Dim r As DataRow  
Dim c As DataColumn  
For Each r In d.Rows  
    Console.WriteLine("==")  
    For Each c In r.Table.Columns  
        Console.WriteLine(c.ColumnName + " = " + r(c).ToString)  
    Next  
Next  
```  
  
## <a name="using-an-enumeration-method-that-returns-a-datatable-object-in-visual-c"></a>Usar un método de enumeración que devuelve un objeto DataTable en Visual C#  
 En esta sección se describe cómo llamar a un método de enumeración y cómo controlar los datos del objeto <xref:System.Data.DataTable> devuelto.  
  
 El método <xref:Microsoft.SqlServer.Management.Smo.Server.EnumCollations%2A> devuelve un objeto <xref:System.Data.DataTable> del sistema. El objeto <xref:System.Data.DataTable> requiere más navegación para obtener acceso a toda la información de intercalación disponible acerca de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
//Call the EnumCollations method and return collation information to DataTable variable.   
DataTable d = default(DataTable);   
//Select the returned data into an array of DataRow.   
d = srv.EnumCollations;   
//Iterate through the rows and display collation details for the instance of SQL Server.   
DataRow r = default(DataRow);   
DataColumn c = default(DataColumn);   
foreach ( r in d.Rows) {   
  Console.WriteLine("=========");   
  foreach ( c in r.Table.Columns) {   
    Console.WriteLine(c.ColumnName + " = " + r(c).ToString);   
  }   
}   
}   
```  
  
## <a name="constructing-an-object-in-visual-basic"></a>Construir un objeto en Visual Basic  
 Se puede llamar al constructor de cualquier objeto mediante el operador **New** . El constructor de objeto <xref:Microsoft.SqlServer.Management.Smo.Database> se sobrecarga y la versión del constructor de objeto <xref:Microsoft.SqlServer.Management.Smo.Database> que se usa en el ejemplo toma dos parámetros: el objeto primario <xref:Microsoft.SqlServer.Management.Smo.Server> al que pertenece la base de datos y una cadena que representa el nombre de la nueva base de datos.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Declare and define a Database object by supplying the parent server and the database name arguments in the constructor.
Dim d As Database
d = New Database(srv, "Test_SMO_Database")
'Create the database on the instance of SQL Server.
d.Create()
Console.WriteLine(d.Name)
```
  
## <a name="constructing-an-object-in-visual-c"></a>Construir un objeto en Visual C#  
 Se puede llamar al constructor de cualquier objeto mediante el operador **New** . El constructor de objeto <xref:Microsoft.SqlServer.Management.Smo.Database> se sobrecarga y la versión del constructor de objeto <xref:Microsoft.SqlServer.Management.Smo.Database> que se usa en el ejemplo toma dos parámetros: el objeto primario <xref:Microsoft.SqlServer.Management.Smo.Server> al que pertenece la base de datos y una cadena que representa el nombre de la nueva base de datos.  
  
```csharp  
{   
Server srv;   
srv = new Server();   
Table tb;   
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources");   
tb.RebuildIndexes(70);   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Declare and define a Database object by supplying the parent server and the database name arguments in the constructor.   
Database d;   
d = new Database(srv, "Test_SMO_Database");   
//Create the database on the instance of SQL Server.   
d.Create();   
Console.WriteLine(d.Name);   
}  
```  
  
## <a name="copying-an-smo-object-in-visual-basic"></a>Copiar un objeto SMO en Visual Basic  
 En este ejemplo de código se usa el método <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A> para crear una copia del objeto <xref:Microsoft.SqlServer.Management.Smo.Server>. El objeto <xref:Microsoft.SqlServer.Management.Smo.Server> representa una conexión a una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```VBNET  
'Connect to the local, default instance of SQL Server.
Dim srv1 As Server
srv1 = New Server()
'Modify the default database and the timeout period for the connection.
srv1.ConnectionContext.DatabaseName = "AdventureWorks2012"
srv1.ConnectionContext.ConnectTimeout = 30
'Make a second connection using a copy of the ConnectionContext property and verify settings.
Dim srv2 As Server
srv2 = New Server(srv1.ConnectionContext.Copy)
Console.WriteLine(srv2.ConnectionContext.ConnectTimeout.ToString)
```
  
## <a name="copying-an-smo-object-in-visual-c"></a>Copiar un objeto SMO en Visual C#  
 En este ejemplo de código se usa el método <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A> para crear una copia del objeto <xref:Microsoft.SqlServer.Management.Smo.Server>. El objeto <xref:Microsoft.SqlServer.Management.Smo.Server> representa una conexión a una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv1;   
srv1 = new Server();   
//Modify the default database and the timeout period for the connection.   
srv1.ConnectionContext.DatabaseName = "AdventureWorks2012";   
srv1.ConnectionContext.ConnectTimeout = 30;   
//Make a second connection using a copy of the ConnectionContext property and verify settings.   
Server srv2;   
srv2 = new Server(srv1.ConnectionContext.Copy);   
Console.WriteLine(srv2.ConnectionContext.ConnectTimeout.ToString);   
}  
```  
  
## <a name="monitoring-server-processes-in-visual-basic"></a>Supervisar los procesos de servidor en Visual Basic  
 Puede obtener la información de tipo de estado actual de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a través de los métodos de enumeración. El ejemplo de código usa el método <xref:Microsoft.SqlServer.Management.Smo.Server.EnumProcesses%2A> para detectar información sobre los procesos actuales. También muestra cómo trabajar con las columnas y filas del objeto <xref:System.Data.DataTable> devuelto.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Call the EnumCollations method and return collation information to DataTable variable.
Dim d As DataTable
'Select the returned data into an array of DataRow.
d = srv.EnumProcesses
'Iterate through the rows and display collation details for the instance of SQL Server.
Dim r As DataRow
Dim c As DataColumn
For Each r In d.Rows
    Console.WriteLine("============================================")
    For Each c In r.Table.Columns
        Console.WriteLine(c.ColumnName + " = " + r(c).ToString)
    Next
Next
```
  
## <a name="monitoring-server-processes-in-visual-c"></a>Supervisar los procesos de servidor en Visual C#  
 Puede obtener la información de tipo de estado actual de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a través de los métodos de enumeración. El ejemplo de código usa el método <xref:Microsoft.SqlServer.Management.Smo.Server.EnumProcesses%2A> para detectar información sobre los procesos actuales. También muestra cómo trabajar con las columnas y filas del objeto <xref:System.Data.DataTable> devuelto.  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
//Call the EnumCollations method and return collation information to DataTable variable.   
DataTable d = default(DataTable);   
//Select the returned data into an array of DataRow.   
d = srv.EnumProcesses;   
//Iterate through the rows and display collation details for the instance of SQL Server.   
DataRow r = default(DataRow);   
DataColumn c = default(DataColumn);   
foreach ( r in d.Rows) {   
  Console.WriteLine("=====");   
  foreach ( c in r.Table.Columns) {   
    Console.WriteLine(c.ColumnName + " = " + r(c).ToString);   
  }   
}   
}   
```  
  
## <a name="see-also"></a>Consulte también  
 <xref:Microsoft.SqlServer.Management.Smo.Server>   
 <xref:Microsoft.SqlServer.Management.Common.ServerConnection>  
  
  
