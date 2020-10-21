---
description: Crear un destino con el componente de script
title: Crear un destino con el componente de script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script component [Integration Services], destination components
- destinations [Integration Services], components
- input columns [Integration Services]
ms.assetid: 214e22e8-7e7d-4876-b690-c138e5721b81
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f02b74a6e5e28fc44a1bab9eb3f101b91610d481
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92193143"
---
# <a name="creating-a-destination-with-the-script-component"></a>Crear un destino con el componente de script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Los componentes de destino se utilizan en el flujo de datos de un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para guardar datos recibidos de orígenes y transformaciones de nivel superior en un origen de datos. Por lo general, el componente de destino se conecta al origen de datos a través de un administrador de conexiones existente.  
  
 Para obtener una introducción al componente de script, vea [Extending the Data Flow with the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md) (Extensión del Flujo de datos con el componente de script).  
  
 El componente de script y el código de infraestructura que genera simplifican considerablemente el proceso de desarrollo de un componente de flujo de datos personalizado. Sin embargo, para entender cómo funciona el componente de script, puede resultar útil leer los pasos que debe seguir para desarrollar componentes de flujo de datos personalizados en la sección [Developing a Custom Data Flow Component](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) (Desarrollo de un componente de flujo de datos personalizado) y, en especial, [Developing a Custom Destination Component](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md) (Desarrollo de un componente de destino personalizado).  
  
## <a name="getting-started-with-a-destination-component"></a>Introducción a un componente de destino  
 Al agregar un componente de script a la pestaña Flujo de datos del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)], se abre el cuadro de diálogo **Seleccionar el tipo de componente de script** y se solicita la selección de un script de **Origen**, **Destino** o **Transformación**. En este cuadro de diálogo, seleccione **Destino**.  
  
 A continuación, conecte la salida de una transformación al componente de destino en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Como prueba, puede conectar un origen directamente a un destino sin ninguna transformación.  
  
## <a name="configuring-a-destination-component-in-metadata-design-mode"></a>Configurar un componente de destino en modo de diseño de metadatos  
 Después de seleccionar la opción para crear un componente de destino, configure el componente mediante el **Editor de transformación Script**. Para obtener más información, consulte [Configuring the Script Component in the Script Component Editor](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md) (Configurar el componente de script en el editor de componentes de script).  
  
 Para seleccionar el lenguaje de script que utilizará el destino de script, establezca la propiedad **ScriptLanguage** en la página **Script** del cuadro de diálogo **Editor de transformación Script**.  
  
> [!NOTE]  
>  Para establecer el lenguaje de scripting predeterminado para el componente de script, utilice la opción **Lenguaje de scripting** en la página **General** del cuadro de diálogo **Opciones**. Para obtener más información, vea [General Page](../general-page-of-integration-services-designers-options.md).  
  
 Un componente de destino de flujo de datos tiene una entrada y ninguna salida. La configuración de la entrada del componente es uno de los pasos que debe completar en el modo de diseño de metadatos, mediante el **Editor de transformación Script**, antes de escribir un script personalizado.  
  
### <a name="adding-connection-managers"></a>Agregar administradores de conexión  
 Normalmente, un componente de destino utiliza un administrador de conexiones existente para conectarse al origen de datos donde guarda los datos del flujo de datos. En la página **Administradores de conexión** del **Editor de transformación Script**, haga clic en **Agregar** para agregar el administrador de conexiones adecuado.  
  
 Sin embargo, un administrador de conexiones es tan solo una unidad práctica que encapsula y almacena la información necesaria para conectarse a un origen de datos de un tipo determinado. Debe escribir su propio código personalizado para cargar o guardar los datos y posiblemente para abrir y cerrar la conexión al origen de datos.  
  
 Para obtener información general acerca de cómo se utilizan los administradores de conexión con el componente de script, vea [Connecting to Data Sources in the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/connecting-to-data-sources-in-the-script-component.md) (Conexión a orígenes de datos en el componente de script).  
  
 Para obtener más información acerca de la página **Administradores de conexión** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Connection Managers Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Administradores de conexión]).  
  
### <a name="configuring-inputs-and-input-columns"></a>Configurar entradas y columnas de entrada  
 Un componente de destino tiene una entrada y ninguna salida.  
  
 En la página **Columnas de entrada** del **Editor de transformación Script**, la lista de columnas muestra las columnas disponibles de la salida del componente de nivel superior en el flujo de datos. Seleccione las columnas que desea guardar.  
  
 Para obtener más información acerca de la página **Columnas de entrada** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Input Columns Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Columnas de entrada]).  
  
 La página **Entradas y salidas** del **Editor de transformación Script** muestra una entrada única, cuyo nombre puede cambiar. Hará referencia a la entrada por su nombre en el script mediante la propiedad de descriptor de acceso creada en el código generado automáticamente.  
  
 Para obtener más información acerca de la página **Entradas y salidas** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Inputs and Outputs Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Entradas y salidas]).  
  
### <a name="adding-variables"></a>Agregar variables  
 Si quiere utilizar variables existentes en el script, puede agregarlas en los campos de propiedades **ReadOnlyVariables** y **ReadWriteVariables** de la página **Script** del **Editor de transformación Script**.  
  
 Cuando agregue varias variables a los campos de propiedades, separe sus nombres con comas. Para seleccionar varias variables, también puede hacer clic en el botón de puntos suspensivos ( **…** ) junto a los campos de propiedades **ReadOnlyVariables** y **ReadWriteVariables** y, después, seleccionar las variables en el cuadro de diálogo **Seleccionar variables**.  
  
 Para obtener información general sobre la forma de usar variables con el componente de script, vea [Using Variables in the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md) (Uso de variables con el componente de script).  
  
 Para obtener más información acerca de la página **Script** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Script Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Script]).  
  
## <a name="scripting-a-destination-component-in-code-design-mode"></a>Generar script en un componente de destino en modo de diseño de código  
 Después de haber configurado los metadatos para el componente, puede escribir un script personalizado. En la página **Script** del **Editor de transformación Script**, haga clic en **Editar script** para abrir el IDE de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA), donde puede agregar el script personalizado. El lenguaje de scripting que utilice dependerá de si ha seleccionado [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic o [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# como lenguaje de script para la propiedad **ScriptLanguage** en la página **Script**.  
  
 Para obtener información importante aplicable a todos los tipos de componentes creados mediante el componente de script, vea [Coding and Debugging the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md) (Programación y depuración del componente de script).  
  
### <a name="understanding-the-auto-generated-code"></a>Descripción del código generado automáticamente  
 Al abrir el IDE de VSTA después de crear y configurar un componente de destino, la clase **ScriptMain** modificable aparece en el editor de código con un código auxiliar para el método **ProcessInputRow**. En la clase **ScriptMain** escribirá el código personalizado. **ProcessInputRow** es el método más importante de un componente de destino.  
  
 Si abre la ventana **Explorador de proyectos** de VSTA, puede ver que el componente de script también ha generado elementos de proyecto **BufferWrapper** y **ComponentWrapper** de solo lectura. La clase **ScriptMain** hereda de la clase **UserComponent** en el elemento de proyecto **ComponentWrapper**.  
  
 En tiempo de ejecución, el motor de flujo de datos invoca el método **ProcessInput** de la clase **UserComponent**, lo que invalida el método <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ProcessInput%2A> de la clase primaria <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. A su vez, el método **ProcessInput** recorre las filas del búfer de entrada y llama al método **ProcessInputRow** una vez por cada fila.  
  
### <a name="writing-your-custom-code"></a>Escribir código personalizado  
 Para terminar de crear un componente de destino personalizado, puede escribir script en los siguientes métodos disponibles en la clase **ScriptMain**.  
  
1.  Invalide el método **AcquireConnections** para conectarse al origen de datos externo. Extraiga el objeto de conexión, o la información de conexión necesaria, del administrador de conexiones.  
  
2.  Invalide el método **PreExecute** para preparar el almacenamiento de los datos. Por ejemplo, en este método puede crear y configurar una clase **SqlCommand** y sus parámetros.  
  
3.  Utilice el método **ProcessInputRow** invalidado para copiar cada fila de entrada en el origen de datos externo. Por ejemplo, para un destino [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede copiar los valores de columna en los parámetros de **SqlCommand** y ejecutar el comando una vez para cada fila. Para un destino de archivo plano, puede escribir los valores de cada columna en **StreamWriter** y separarlos mediante el delimitador de columna.  
  
4.  Invalide el método **PostExecute** para desconectarse del origen de datos externo, si se requiere, y para realizar cualquier otra limpieza necesaria.  
  
## <a name="examples"></a>Ejemplos  
 Los ejemplos siguientes muestran código que se requiere en la clase **ScriptMain** para crear un componente de destino.  
  
> [!NOTE]
>  En estos ejemplos se usa la tabla **Person.Address** de la base de datos de ejemplo **AdventureWorks** y se pasan las columnas primera y cuarta, las columnas **int*AddressID*** y **nvarchar(30)City**, a través del flujo de datos. Estos mismos datos se usan en los ejemplos de origen, transformación y destino de esta sección. Se documentan requisitos previos y suposiciones adicionales para cada ejemplo.  
  
### <a name="adonet-destination-example"></a>Ejemplo de destino ADO.NET  
 En este ejemplo se muestra un componente de destino que utiliza un administrador de conexiones de [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existente para guardar datos del flujo de datos en una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si desea ejecutar este código de ejemplo, debe configurar el paquete y el componente de la siguiente forma:  
  
1.  Cree un administrador de conexiones de [!INCLUDE[vstecado](../../includes/vstecado-md.md)] que utilice el proveedor **SqlClient** para conectarse a la base de datos **AdventureWorks**.  
  
2.  Puede crear una tabla de destino ejecutando el siguiente comando [!INCLUDE[tsql](../../includes/tsql-md.md)] en la base de datos **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
3.  Agregue un nuevo componente de script a la superficie del diseñador de flujo de datos y configúrelo como destino.  
  
4.  Conecte la salida de un origen o transformación de nivel superior al componente de destino en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)]. (Puede conectar directamente un origen a un destino sin ninguna transformación). Esta salida debe proporcionar datos de la tabla **Person.Address** de la base de datos de ejemplo **AdventureWorks** que contiene al menos las columnas **AddressID** y **City**.  
  
5.  Abra el **Editor de transformación Script**. En la página **Columnas de entrada**, seleccione las columnas de entrada **AddressID** y **City**.  
  
6.  En la página **Entradas y salidas**, cambie el nombre de la entrada por un nombre más descriptivo, como **MyAddressInput**.  
  
7.  En la página **Administradores de conexión**, agregue o cree el administrador de conexiones de [!INCLUDE[vstecado](../../includes/vstecado-md.md)] con un nombre descriptivo como **MyADONETConnectionManager**.  
  
8.  En la página **Script**, haga clic en **Editar script** y escriba el script que se indica a continuación. A continuación, cierre el entorno de desarrollo de script.  
  
9. Cierre el **Editor de transformación Script** y ejecute el ejemplo.  
  
```vb  
Imports System.Data.SqlClient  
...  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Dim connMgr As IDTSConnectionManager100  
    Dim sqlConn As SqlConnection  
    Dim sqlCmd As SqlCommand  
    Dim sqlParam As SqlParameter  
  
    Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
        connMgr = Me.Connections.MyADONETConnectionManager  
        sqlConn = CType(connMgr.AcquireConnection(Nothing), SqlConnection)  
  
    End Sub  
  
    Public Overrides Sub PreExecute()  
  
        sqlCmd = New SqlCommand("INSERT INTO Person.Address2(AddressID, City) " & _  
            "VALUES(@addressid, @city)", sqlConn)  
        sqlParam = New SqlParameter("@addressid", SqlDbType.Int)  
        sqlCmd.Parameters.Add(sqlParam)  
        sqlParam = New SqlParameter("@city", SqlDbType.NVarChar, 30)  
        sqlCmd.Parameters.Add(sqlParam)  
  
    End Sub  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
        With sqlCmd  
            .Parameters("@addressid").Value = Row.AddressID  
            .Parameters("@city").Value = Row.City  
            .ExecuteNonQuery()  
        End With  
    End Sub  
  
    Public Overrides Sub ReleaseConnections()  
  
        connMgr.ReleaseConnection(sqlConn)  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System.Data.SqlClient;  
public class ScriptMain:  
    UserComponent  
  
{  
    IDTSConnectionManager100 connMgr;  
    SqlConnection sqlConn;  
    SqlCommand sqlCmd;  
    SqlParameter sqlParam;  
  
    public override void AcquireConnections(object Transaction)  
    {  
  
        connMgr = this.Connections.MyADONETConnectionManager;  
        sqlConn = (SqlConnection)connMgr.AcquireConnection(null);  
  
    }  
  
    public override void PreExecute()  
    {  
  
        sqlCmd = new SqlCommand("INSERT INTO Person.Address2(AddressID, City) " +  
            "VALUES(@addressid, @city)", sqlConn);  
        sqlParam = new SqlParameter("@addressid", SqlDbType.Int);  
        sqlCmd.Parameters.Add(sqlParam);  
        sqlParam = new SqlParameter("@city", SqlDbType.NVarChar, 30);  
        sqlCmd.Parameters.Add(sqlParam);  
  
    }  
  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
        {  
            sqlCmd.Parameters["@addressid"].Value = Row.AddressID;  
            sqlCmd.Parameters["@city"].Value = Row.City;  
            sqlCmd.ExecuteNonQuery();  
        }  
    }  
  
    public override void ReleaseConnections()  
    {  
  
        connMgr.ReleaseConnection(sqlConn);  
  
    }  
  
}  
```  
  
### <a name="flat-file-destination-example"></a>Ejemplo de destino de archivo plano  
 En este ejemplo se muestra un componente de destino que utiliza un administrador de conexiones de archivos planos existente para guardar datos del flujo de datos en un archivo plano.  
  
 Si desea ejecutar este código de ejemplo, debe configurar el paquete y el componente de la siguiente forma:  
  
1.  Cree un administrador de conexiones de archivos planos que conecte a un archivo de destino. El archivo no tiene que existir; el componente de destino lo creará. Configure el archivo de destino como un archivo delimitado por comas que contenga las columnas **AddressID** y **City**.  
  
2.  Agregue un nuevo componente de script a la superficie del diseñador de flujo de datos y configúrelo como destino.  
  
3.  Conecte la salida de un origen o transformación de nivel superior al componente de destino en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)]. (Puede conectar directamente un origen a un destino sin ninguna transformación). Esta salida debe proporcionar datos de la tabla **Person.Address** de la base de datos de ejemplo **AdventureWorks** y debe contener al menos las columnas **AddressID** y **City**.  
  
4.  Abra el **Editor de transformación Script**. En la página **Columnas de entrada**, seleccione las columnas **AddressID** y **City**.  
  
5.  En la página **Entradas y salidas**, cambie el nombre de la entrada por un nombre más descriptivo, como **MyAddressInput**.  
  
6.  En la página **Administradores de conexión**, agregue o cree el administrador de conexiones de archivos planos con un nombre descriptivo como **MyFlatFileDestConnectionManager**.  
  
7.  En la página **Script**, haga clic en **Editar script** y escriba el script que se indica a continuación. A continuación, cierre el entorno de desarrollo de script.  
  
8.  Cierre el **Editor de transformación Script** y ejecute el ejemplo.  
  
```vb  
Imports System.IO  
...  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Dim copiedAddressFile As String  
    Private textWriter As StreamWriter  
    Private columnDelimiter As String = ","  
  
    Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
        Dim connMgr As IDTSConnectionManager100 = _  
            Me.Connections.MyFlatFileDestConnectionManager  
        copiedAddressFile = CType(connMgr.AcquireConnection(Nothing), String)  
  
    End Sub  
  
    Public Overrides Sub PreExecute()  
  
        textWriter = New StreamWriter(copiedAddressFile, False)  
  
    End Sub  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        With textWriter  
            If Not Row.AddressID_IsNull Then  
                .Write(Row.AddressID)  
            End If  
            .Write(columnDelimiter)  
            If Not Row.City_IsNull Then  
                .Write(Row.City)  
            End If  
            .WriteLine()  
        End With  
  
    End Sub  
  
    Public Overrides Sub PostExecute()  
  
        textWriter.Close()  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System.IO;  
public class ScriptMain:  
    UserComponent  
  
{  
    string copiedAddressFile;  
    private StreamWriter textWriter;  
    private string columnDelimiter = ",";  
  
    public override void AcquireConnections(object Transaction)  
    {  
  
        IDTSConnectionManager100 connMgr = this.Connections.MyFlatFileDestConnectionManager;  
        copiedAddressFile = (string) connMgr.AcquireConnection(null);  
  
    }  
  
    public override void PreExecute()  
    {  
  
        textWriter = new StreamWriter(copiedAddressFile, false);  
  
    }  
  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        {  
            if (!Row.AddressID_IsNull)  
            {  
                textWriter.Write(Row.AddressID);  
            }  
            textWriter.Write(columnDelimiter);  
            if (!Row.City_IsNull)  
            {  
                textWriter.Write(Row.City);  
            }  
            textWriter.WriteLine();  
        }  
  
    }  
  
    public override void PostExecute()  
    {  
  
        textWriter.Close();  
  
    }  
  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 [Creating a Source with the Script Component](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-source-with-the-script-component.md)  (Crear un origen con el componente de script)  
 [Desarrollar un componente de destino personalizado](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md)  
  
