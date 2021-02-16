---
description: Codificar un administrador de conexiones personalizado
title: Programar un administrador de conexiones personalizado | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom connection managers [Integration Services], coding
ms.assetid: b12b6778-1f01-4a7d-984d-73f2f7630aa5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3ad9aa72bb972204bc762ff69692c82c4dda5963
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348937"
---
# <a name="coding-a-custom-connection-manager"></a>Codificar un administrador de conexiones personalizado

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Una vez que haya creado una clase que herede de la clase base <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase> y haya aplicado el atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute> a la clase, debe invalidar la implementación de las propiedades y los métodos de la clase base para proporcionar su funcionalidad personalizada.  
  
 Para obtener ejemplos de administradores de conexiones personalizados, consulte [Desarrollar una interfaz de usuario para un administrador de conexiones personalizado](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md). Los ejemplos de código mostrados en este tema se deducen del ejemplo de administrador de conexiones personalizado de SQL Server.  
  
> [!NOTE]  
>  Muchas de las tareas, orígenes y destinos que se han incluido en [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] se usan únicamente con tipos específicos de administradores de conexiones integrados. Por consiguiente, estos ejemplos no se pueden probar con las tareas y componentes integrados.  
  
## <a name="configuring-the-connection-manager"></a>Configurar el administrador de conexiones  
  
### <a name="setting-the-connectionstring-property"></a>Establecer la propiedad ConnectionString  
 La propiedad <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> es una propiedad importante y la única propiedad para un administrador de conexiones personalizado. El administrador de conexiones usa el valor de esta propiedad para conectar al origen de datos externo. Si combina varias propiedades, como nombre de servidor y nombre de base de datos, para crear la cadena de conexión, puede usar una función del asistente para ensamblar la cadena reemplazando ciertos valores en una plantilla de cadena de conexión con el nuevo valor que proporciona el usuario. En el ejemplo de código siguiente se muestra una implementación de la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> que confía en una función del asistente para ensamblar la cadena.  
  
```vb  
' Default values.  
Private _serverName As String = "(local)"  
Private _databaseName As String = "AdventureWorks"  
Private _connectionString As String = String.Empty  
  
Private Const CONNECTIONSTRING_TEMPLATE As String = _  
  "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=SSPI"  
  
Public Property ServerName() As String  
  Get  
    Return _serverName  
  End Get  
  Set(ByVal value As String)  
    _serverName = value  
  End Set  
End Property  
  
Public Property DatabaseName() As String  
  Get  
    Return _databaseName  
  End Get  
  Set(ByVal value As String)  
    _databaseName = value  
  End Set  
End Property  
  
Public Overrides Property ConnectionString() As String  
  Get  
    UpdateConnectionString()  
    Return _connectionString  
  End Get  
  Set(ByVal value As String)  
    _connectionString = value  
  End Set  
End Property  
  
Private Sub UpdateConnectionString()  
  
  Dim temporaryString As String = CONNECTIONSTRING_TEMPLATE  
  
  If Not String.IsNullOrEmpty(_serverName) Then  
    temporaryString = temporaryString.Replace("<servername>", _serverName)  
  End If  
  If Not String.IsNullOrEmpty(_databaseName) Then  
    temporaryString = temporaryString.Replace("<databasename>", _databaseName)  
  End If  
  
  _connectionString = temporaryString  
  
End Sub  
```  
  
```csharp  
// Default values.  
private string _serverName = "(local)";  
private string _databaseName = "AdventureWorks";  
private string _connectionString = String.Empty;  
  
private const string CONNECTIONSTRING_TEMPLATE = "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=SSPI";  
  
public string ServerName  
{  
  get  
  {  
    return _serverName;  
  }  
  set  
  {  
    _serverName = value;  
  }  
}  
  
public string DatabaseName  
{  
  get  
  {  
    return _databaseName;  
  }  
  set  
  {  
    _databaseName = value;  
  }  
}  
  
public override string ConnectionString  
{  
  get  
  {  
    UpdateConnectionString();  
    return _connectionString;  
  }  
  set  
  {  
    _connectionString = value;  
  }  
}  
  
private void UpdateConnectionString()  
{  
  
  string temporaryString = CONNECTIONSTRING_TEMPLATE;  
  
  if (!String.IsNullOrEmpty(_serverName))  
  {  
    temporaryString = temporaryString.Replace("<servername>", _serverName);  
  }  
  
  if (!String.IsNullOrEmpty(_databaseName))  
  {  
    temporaryString = temporaryString.Replace("<databasename>", _databaseName);  
  }  
  
  _connectionString = temporaryString;  
  
}  
```  
  
### <a name="validating-the-connection-manager"></a>Validar el administrador de conexiones  
 Invalide el método <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A> para asegurarse de que el administrador de conexiones se ha configurado correctamente. Como mínimo, debería validar el formato de la cadena de conexión y asegurarse de que se han proporcionado valores para todos los argumentos. La ejecución no puede continuar hasta que el administrador de conexiones no devuelva <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success> en el método <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A>.  
  
 En el siguiente ejemplo de código se muestra una implementación de <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A> que asegura que el usuario ha especificado un nombre de servidor para la conexión.  
  
```vb  
Public Overrides Function Validate(ByVal infoEvents As Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents) As Microsoft.SqlServer.Dts.Runtime.DTSExecResult  
  
  If String.IsNullOrEmpty(_serverName) Then  
    infoEvents.FireError(0, "SqlConnectionManager", "No server name specified", String.Empty, 0)  
    Return DTSExecResult.Failure  
  Else  
    Return DTSExecResult.Success  
  End If  
  
End Function  
```  
  
```csharp  
public override Microsoft.SqlServer.Dts.Runtime.DTSExecResult Validate(Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents infoEvents)  
{  
  
  if (String.IsNullOrEmpty(_serverName))  
  {  
    infoEvents.FireError(0, "SqlConnectionManager", "No server name specified", String.Empty, 0);  
    return DTSExecResult.Failure;  
  }  
  else  
  {  
    return DTSExecResult.Success;  
  }  
  
}  
```  
  
### <a name="persisting-the-connection-manager"></a>Conservar el administrador de conexiones  
 Normalmente, no tiene que implementar la persistencia personalizada para un administrador de conexiones. Solo se requiere la persistencia personalizada cuando las propiedades de un objeto usan tipos de datos complejos. Para obtener más información, vea [Developing Custom Objects for Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md) (Desarrollar objetos personalizados para Integration Services).  
  
## <a name="working-with-the-external-data-source"></a>Trabajar con el origen de datos externos  
 Los métodos que permiten conectar a un origen de datos externo son los métodos más importantes de un administrador de conexiones personalizado. Se llama varias veces a los métodos <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A> durante tiempo de diseño y tiempo de ejecución.  
  
### <a name="acquiring-the-connection"></a>Adquirir la conexión  
 Debe decidir el tipo de objeto que es el adecuado para devolver el método <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> de su administrador de conexiones personalizado. Por ejemplo, un administrador de conexiones de archivos devuelve únicamente una cadena que contiene una ruta y nombre de archivo, mientras que una conexión ADO.NET devuelve un objeto de conexión administrado que ya está abierto. Un administrador de conexiones OLE DB devuelve un objeto de conexión OLE DB nativo que el código administrado no puede utilizar. El administrador de conexiones SQL Server personalizado, del que se toman los fragmentos de código en este tema, devuelve un objeto **SqlConnection** abierto.  
  
 Los usuarios de su administrador de conexiones necesitan conocer de antemano el tipo de objeto que se espera, para que puedan convertir el objeto devuelto al tipo adecuado y obtener acceso sus métodos y propiedades.  
  
```vb  
Public Overrides Function AcquireConnection(ByVal txn As Object) As Object  
  
  Dim sqlConnection As New SqlConnection  
  
  UpdateConnectionString()  
  
  With sqlConnection  
    .ConnectionString = _connectionString  
    .Open()  
  End With  
  
  Return sqlConnection  
  
End Function  
```  
  
```csharp  
public override object AcquireConnection(object txn)  
{  
  
  SqlConnection sqlConnection = new SqlConnection();  
  
  UpdateConnectionString();  
  
  {  
    sqlConnection.ConnectionString = _connectionString;  
    sqlConnection.Open();  
  }  
  
  return sqlConnection;  
  
}  
```  
  
### <a name="releasing-the-connection"></a>Liberar la conexión  
 Las medidas que toma en el método <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A> dependen del tipo de objeto devuelto del método <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>. Si hay un objeto de conexión abierto, debería cerrarlo y liberar cualquier recurso que se esté utilizando. Si <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> devuelve únicamente un valor de cadena, ya no se necesita realizar ninguna acción.  
  
```vb  
Public Overrides Sub ReleaseConnection(ByVal connection As Object)  
  
  Dim sqlConnection As SqlConnection  
  
  sqlConnection = DirectCast(connection, SqlConnection)  
  
  If sqlConnection.State <> ConnectionState.Closed Then  
    sqlConnection.Close()  
  End If  
  
End Sub  
```  
  
```csharp  
public override void ReleaseConnection(object connection)  
{  
  SqlConnection sqlConnection;  
  sqlConnection = (SqlConnection)connection;  
  if (sqlConnection.State != ConnectionState.Closed)  
    sqlConnection.Close();  
}  
```  
 
## <a name="see-also"></a>Consulte también  
 [Crear un administrador de conexiones personalizado](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)   
 [Desarrollar una interfaz de usuario para un administrador de conexiones personalizado](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
  
  
