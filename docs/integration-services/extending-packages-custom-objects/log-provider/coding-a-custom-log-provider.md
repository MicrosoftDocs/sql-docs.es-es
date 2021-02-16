---
description: Codificar un proveedor de registro personalizado
title: Programar un proveedor de registro personalizado | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom log providers [Integration Services], coding
ms.assetid: 979a29ca-956e-4fdd-ab47-f06e84cead7a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 93ddf518405b44b5abb6956ee5da063b98ece1e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348891"
---
# <a name="coding-a-custom-log-provider"></a>Codificar un proveedor de registro personalizado

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Una vez que haya creado una clase que herede de la clase base <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase> y haya aplicado el atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> a la clase, debe invalidar la implementación de las propiedades y los métodos de la clase base para proporcionar su funcionalidad personalizada.  
  
 Para obtener ejemplos funcionales de proveedores de registro personalizados, consulte [Desarrollar una interfaz de usuario para un proveedor de registro personalizado](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md).  
  
## <a name="configuring-the-log-provider"></a>Configurar el proveedor de registro  
  
### <a name="initializing-the-log-provider"></a>Inicializar el proveedor de registro  
 Puede invalidar el método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.InitializeLogProvider%2A> para almacenar en memoria caché las referencias a la colección de conexiones y a la interfaz de eventos. Puede utilizar estas referencias almacenadas en memoria caché más adelante en otros métodos del proveedor de registro.  
  
### <a name="using-the-configstring-property"></a>Utilizar la propiedad ConfigString  
 En tiempo de diseño, un proveedor de registro recibe información de configuración de la columna **Configuración**. Esta información de configuración corresponde a la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> del proveedor de registro. De forma predeterminada, esta columna contiene un cuadro de texto del que puede recuperar cualquier información de cadena. La mayoría de los proveedores de registro incluidos con [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] utilizan esta propiedad para almacenar el nombre del administrador de conexión que el proveedor utiliza para conectarse a un origen de datos externo. Si el proveedor de registro utiliza la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>, utilice el método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> para validarla y asegurarse de que se haya establecido correctamente.  
  
### <a name="validating-the-log-provider"></a>Validar el proveedor de registro  
 Puede invalidar el método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> para asegurarse de que el proveedor se ha configurado correctamente y está listo para la ejecución. Normalmente, un nivel mínimo de validación es asegurarse de que se establece <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> correctamente. La ejecución no puede continuar hasta que el proveedor de registro devuelva <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success> en el método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A>.  
  
 En el ejemplo de código siguiente se muestra una implementación de <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Validate%2A> que garantiza que se especifica el nombre de un administrador de conexión, que existe el administrador de conexión en el paquete y que el administrador de conexión devuelve un nombre de archivo en la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>.  
  
```csharp  
public override DTSExecResult Validate(IDTSInfoEvents infoEvents)  
{  
    if (this.ConfigString.Length == 0 || connections.Contains(ConfigString) == false)  
    {  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
        return DTSExecResult.Failure;  
    }  
    else  
    {  
        string fileName = connections[ConfigString].AcquireConnection(null) as string;  
  
        if (fileName == null || fileName.Length == 0)  
        {  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0);  
            return DTSExecResult.Failure;  
        }  
    }  
    return DTSExecResult.Success;  
}  
```  
  
```vb  
Public Overrides Function Validate(ByVal infoEvents As IDTSInfoEvents) As DTSExecResult  
    If Me.ConfigString.Length = 0 Or connections.Contains(ConfigString) = False Then  
        infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
        Return DTSExecResult.Failure  
    Else   
        Dim fileName As String =  connections(ConfigString).AcquireConnectionCType(as string, Nothing)  
  
        If fileName = Nothing Or fileName.Length = 0 Then  
            infoEvents.FireError(0, "MyTextLogProvider", "The ConnectionManager " + ConfigString + " specified in the ConfigString property cannot be found in the collection.", "", 0)  
            Return DTSExecResult.Failure  
        End If  
    End If  
    Return DTSExecResult.Success  
End Function  
```  
  
### <a name="persisting-the-log-provider"></a>Conservar el proveedor de registro  
 Normalmente, no tiene que implementar la persistencia personalizada para un administrador de conexión. Solo se requiere la persistencia personalizada cuando las propiedades de un objeto usan tipos de datos complejos. Para obtener más información, vea [Developing Custom Objects for Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md) (Desarrollar objetos personalizados para Integration Services).  
  
## <a name="logging-with-the-log-provider"></a>Registrar con el proveedor de registro  
 Hay tres métodos en tiempo de ejecución que todos los proveedores de registro deben invalidar: <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>.  
  
> [!IMPORTANT]  
>  Durante la validación y ejecución de un paquete único, se llama a los métodos <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> más de una vez. Asegúrese de que el código personalizado no hace que las entradas de registro anteriores se invaliden por la siguiente apertura y cierre del registro. Si ha seleccionado registrar los eventos de validación en el paquete de prueba, el primer evento registrado que debería ver es OnPreValidate; si en su lugar el primer evento registrado que ve es PackageStart, se han invalidado los eventos de validación iniciales.  
  
### <a name="opening-the-log"></a>Abrir el registro  
 La mayoría de los proveedores de registro se conectan a un origen de datos externo, como un archivo o una base de datos, para almacenar la información de evento que se recopila durante la ejecución del paquete. Como con cualquier otro objeto en tiempo de ejecución, la conexión al origen de datos externo se lleva a cabo normalmente utilizando los objetos del administrador de conexión.  
  
 Se llama al método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A> al iniciar la ejecución del paquete. Invalide este método para establecer una conexión al origen de datos externo.  
  
 En el código de ejemplo siguiente se muestra un proveedor de registro que abre un archivo de texto para la escritura durante <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>. Abre el archivo llamando al método <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> del administrador de conexión especificado en la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A>.  
  
```csharp  
public override void OpenLog()  
{  
    if(!this.connections.Contains(this.ConfigString))  
        throw new Exception("The ConnectionManager " + this.ConfigString + " does not exist in the Connections collection.");  
  
    this.connectionManager = connections[ConfigString];  
    string filePath = this.connectionManager.AcquireConnection(null) as string;  
  
    if(filePath == null || filePath.Length == 0)  
        throw new Exception("The ConnectionManager " + this.ConfigString + " is not a valid FILE ConnectionManager");  
  
    //  Create a StreamWriter to append to.  
    sw = new StreamWriter(filePath,true);  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString());  
}  
```  
  
```vb  
Public Overrides  Sub OpenLog()  
    If Not Me.connections.Contains(Me.ConfigString) Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " does not exist in the Connections collection.")  
    End If  
  
    Me.connectionManager = connections(ConfigString)  
    Dim filePath As String =  Me.connectionManager.AcquireConnectionCType(as string, Nothing)  
  
    If filePath = Nothing Or filePath.Length = 0 Then  
        Throw New Exception("The ConnectionManager " + Me.ConfigString + " is not a valid FILE ConnectionManager")  
    End If  
  
    '  Create a StreamWriter to append to.  
    sw = New StreamWriter(filePath,True)  
  
    sw.WriteLine("Open log" + System.DateTime.Now.ToShortTimeString())  
End Sub  
```  
  
### <a name="writing-log-entries"></a>Escribir entradas del registro  
 Se llama al método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> cada vez que un objeto del paquete provoca un evento mediante la llamada a un método Fire\<event> en una de las interfaces de eventos. Cada evento se provoca con información sobre su contexto y normalmente un mensaje explicativo. Sin embargo, no todas las llamadas al método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> incluyen información de todos los parámetros de método. Por ejemplo, algunos eventos estándar cuyos nombres son autoexplicativos no proporcionan MessageText, y DataCode y DataBytes están pensados para la información complementaria opcional.  
  
 En el ejemplo de código siguiente se implementa el método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> y se escriben los eventos en el flujo que se abrió en la sección anterior.  
  
```csharp  
public override void Log(string logEntryName, string computerName, string operatorName, string sourceName, string sourceID, string executionID, string messageText, DateTime startTime, DateTime endTime, int dataCode, byte[] dataBytes)  
{  
    sw.Write(logEntryName + ",");  
    sw.Write(computerName + ",");  
    sw.Write(operatorName + ",");  
    sw.Write(sourceName + ",");  
    sw.Write(sourceID + ",");  
    sw.Write(messageText + ",");  
    sw.Write(dataBytes + ",");  
    sw.WriteLine("");  
}  
```  
  
```vb  
Public Overrides  Sub Log(ByVal logEnTryName As String, ByVal computerName As String, ByVal operatorName As String, ByVal sourceName As String, ByVal sourceID As String, ByVal executionID As String, ByVal messageText As String, ByVal startTime As DateTime, ByVal endTime As DateTime, ByVal dataCode As Integer, ByVal dataBytes() As Byte)  
    sw.Write(logEnTryName + ",")  
    sw.Write(computerName + ",")  
    sw.Write(operatorName + ",")  
    sw.Write(sourceName + ",")  
    sw.Write(sourceID + ",")  
    sw.Write(messageText + ",")  
    sw.Write(dataBytes + ",")  
    sw.WriteLine("")  
End Sub  
```  
  
### <a name="closing-the-log"></a>Cerrar el registro  
 Se llama al método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> al final de la ejecución del paquete, después de que todos los objetos del paquete han completado la ejecución, o bien, cuando el paquete se detiene debido a los errores.  
  
 En el ejemplo de código siguiente se muestra una implementación del método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> que cierra el flujo de archivos que se abrió durante el método <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>.  
  
```csharp  
public override void CloseLog()  
{  
    if (sw != null)  
    {  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString());  
        sw.Close();  
    }  
}  
```  
  
```vb  
Public Overrides  Sub CloseLog()  
    If Not sw Is Nothing Then  
        sw.WriteLine("Close log" + System.DateTime.Now.ToShortTimeString())  
        sw.Close()  
    End If  
End Sub  
```  
 
## <a name="see-also"></a>Consulte también  
 [Crear un proveedor de registro personalizado](../../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)   
 [Desarrollar una interfaz de usuario para un proveedor de registro personalizado](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
  
  
