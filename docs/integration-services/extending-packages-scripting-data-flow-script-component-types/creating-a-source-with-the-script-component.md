---
description: Crear un origen con el componente de script
title: Crear un origen con el componente de script | Microsoft Docs
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
- Script component [Integration Services], source components
- output columns [Integration Services]
- sources [Integration Services], components
ms.assetid: 547c4179-ea82-4265-8c6f-04a2aa77a3c0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1aa4ed0d717e8b2d6dfe7f2b6b7d331936e00100
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344872"
---
# <a name="creating-a-source-with-the-script-component"></a>Crear un origen con el componente de script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Los componentes de origen del flujo de datos de un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se usan para cargar datos de un origen de datos y pasarlos a transformaciones y destinos de nivel inferior. Normalmente la conexión al origen de datos se realiza a través de un administrador de conexiones existente.  
  
 Para obtener una introducción al componente de script, vea [Extending the Data Flow with the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md) (Extensión del Flujo de datos con el componente de script).  
  
 El componente de script y el código de infraestructura que genera simplifican considerablemente el proceso de desarrollo de un componente de flujo de datos personalizado. Sin embargo, para entender cómo funciona el componente de script, puede resultar útil leer los pasos necesarios para el desarrollo de un componente de flujo de datos personalizado. Vea la sección [Developing a Custom Data Flow Component](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) (Desarrollo de un componente de flujo de datos personalizado), especialmente el tema [Developing a Custom Source Component](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md) (Desarrollo de un componente de origen personalizado).  
  
## <a name="getting-started-with-a-source-component"></a>Introducción al componente de origen  
 Al agregar un componente de script al panel Flujo de datos del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)], se abre el cuadro de diálogo **Seleccionar el tipo de componente de script** y se solicita la selección de un script de origen, destino o transformación. En este cuadro de diálogo, seleccione **Origen**.  
  
## <a name="configuring-a-source-component-in-metadata-design-mode"></a>Configurar un componente de origen en modo de diseño de metadatos  
 Después de seleccionar la opción para crear un componente de origen, configure el componente mediante el **Editor de transformación Script**. Para obtener más información, consulte [Configuring the Script Component in the Script Component Editor](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md) (Configurar el componente de script en el editor de componentes de script).  
  
 Un componente de origen de flujo de datos no tiene ninguna entrada y admite una o más salidas. La configuración de las salidas del componente es uno de los pasos que debe completar en el modo de diseño de metadatos, mediante el **Editor de transformación Script**, antes de escribir un script personalizado.  
  
 También puede especificar el lenguaje de script mediante el establecimiento de la propiedad **ScriptLanguage** en la página **Script** del **Editor de transformación Script**.  
  
> [!NOTE]  
>  Para establecer el lenguaje de script predeterminado para los componentes Script y las tareas de script, utilice la opción **Lenguaje de scripting** en la página **General** del cuadro de diálogo **Opciones**. Para obtener más información, vea [General Page](~/integration-services/control-flow/script-task-editor-general-page.md).  
  
### <a name="adding-connection-managers"></a>Agregar administradores de conexión  
 Normalmente, un componente de origen utiliza un administrador de conexiones existente para conectarse al origen de datos del que carga los datos en el flujo de datos. En la página **Administradores de conexión** del **Editor de transformación Script**, haga clic en **Agregar** para agregar el administrador de conexiones adecuado.  
  
 Sin embargo, un administrador de conexiones solo es una unidad práctica que encapsula y almacena la información necesaria para conectarse a un origen de datos de un tipo determinado. Debe escribir su propio código personalizado para cargar o guardar los datos y posiblemente también para abrir y cerrar la conexión al origen de datos.  
  
 Para obtener información general acerca de cómo se utilizan los administradores de conexión con el componente de script, vea [Connecting to Data Sources in the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/connecting-to-data-sources-in-the-script-component.md) (Conexión a orígenes de datos en el componente de script).  
  
 Para obtener más información acerca de la página **Administradores de conexión** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Connection Managers Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Administradores de conexión]).  
  
### <a name="configuring-outputs-and-output-columns"></a>Configurar salidas y columnas de salida  
 Un componente de origen no tiene ninguna entrada y admite una o más salidas. En la página **Entradas y salidas** del **Editor de transformación Script**, se ha creado una única salida de forma predeterminada, pero no se ha creado ninguna columna de salida. En esta página del editor, quizá necesite o desee configurar los elementos siguientes.  
  
-   Debe agregar y configurar manualmente las columnas de salida para cada salida. Seleccione la carpeta Columnas de salida para cada salida y, a continuación, utilice los botones **Agregar columna** y **Quitar columna** para administrar las columnas de salida de cada salida del componente de origen. Después, hará referencia a las columnas de salida en el script por los nombres que asigna aquí, mediante las propiedades de descriptor de acceso con tipo creadas en el código generado automáticamente.  
  
-   Puede crear una o varias salidas adicionales, como una salida de error simulada para las filas que contienen valores inesperados. Utilice los botones **Agregar salida** y **Quitar salida** para administrar las salidas del componente de origen. Todas las filas de entrada se dirigen a todas las salidas disponibles a menos que también especifique un valor distinto de cero idéntico para la propiedad **ExclusionGroup** de las salidas si desea dirigir cada fila a solo una de las salidas que comparten el mismo valor de **ExclusionGroup**. El valor entero determinado seleccionado para identificar el elemento **ExclusionGroup** no es significativo.  
  
    > [!NOTE]  
    >  También puede utilizar un valor de la propiedad **ExclusionGroup** distinto de cero con una salida única si no desea generar la salida de todas las filas. No obstante, en este caso, debe llamar explícitamente al método **DirectRowTo\<outputbuffer>** para cada fila que quiera enviar a la salida.  
  
-   Puede asignar un nombre descriptivo a las salidas. Después, hará referencia a las salidas en el script por sus nombres, mediante las propiedades de descriptor de acceso con tipo creadas en el código generado automáticamente.  
  
-   Normalmente, varias salidas del mismo grupo **ExclusionGroup** tienen las mismas columnas de resultados. Sin embargo, si crea una salida de error simulada, quizá desee agregar más columnas que almacenen información de error. Para obtener información acerca de cómo el motor de flujo de datos procesa las filas de errores, vea [Using Error Outputs in a Data Flow Component](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md) (Uso de las salidas de error en un componente de flujo de datos). En el componente de script, sin embargo, debe escribir su propio código para llenar las columnas adicionales con la información de error adecuada. Para obtener más información, vea [Simulating an Error Output for the Script Component](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md) (Simular una salida de error para el componente de script).  
  
 Para obtener más información acerca de la página **Entradas y salidas** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Inputs and Outputs Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Entradas y salidas]).  
  
### <a name="adding-variables"></a>Agregar variables  
 Si hay variables existentes cuyos valores quiere usar en el script, puede agregarlas en los campos de propiedades **ReadOnlyVariables** y **ReadWriteVariables** de la página **Script** del **Editor de transformación Script**.  
  
 Al introducir diversas variables en los campos de propiedades, separe los nombres de éstas por comas. Para escribir varias variables, también puede hacer clic en el botón de puntos suspensivos ( **…** ) junto a los campos de propiedades **ReadOnlyVariables** y **ReadWriteVariables** y, después, seleccionar variables en el cuadro de diálogo **Seleccionar variables**.  
  
 Para obtener información general sobre la forma de usar variables con el componente de script, vea [Using Variables in the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md) (Uso de variables con el componente de script).  
  
 Para obtener más información acerca de la página **Script** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Script Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Script]).  
  
## <a name="scripting-a-source-component-in-code-design-mode"></a>Generar script en un componente de origen en modo de diseño de código  
 Después de configurar los metadatos para el componente, abra el IDE de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) para programar el script personalizado. Para abrir VSTA, haga clic en **Editar script** en la página **Script** del **Editor de transformación Script**. Puede escribir el script mediante [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic o [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#, dependiendo del lenguaje de script seleccionado para la propiedad **ScriptLanguage**.  
  
 Para obtener información importante aplicable a todos los tipos de componentes creados mediante el componente de script, vea [Coding and Debugging the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md) (Programación y depuración del componente de script).  
  
### <a name="understanding-the-auto-generated-code"></a>Descripción del código generado automáticamente  
 Al abrir el IDE de VSTA después de crear y configurar un componente de origen, la clase **ScriptMain** modificable aparece en el editor de código. El código personalizado se escribe en la clase **ScriptMain**.  
  
 La clase **ScriptMain** incluye un código auxiliar para el método **CreateNewOutputRows**. **CreateNewOutputRows** es el método más importante en un componente de origen.  
  
 Si abre la ventana **Explorador de proyectos** de VSTA, puede ver que el componente de script también ha generado elementos de proyecto **BufferWrapper** y **ComponentWrapper** de solo lectura. La clase **ScriptMain** hereda de la clase **UserComponent** en el elemento de proyecto **ComponentWrapper**.  
  
 En tiempo de ejecución, el motor de flujo de datos invoca al método **PrimeOutput** de la clase **UserComponent**, lo que invalida el método <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.PrimeOutput%2A> de la clase primaria <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. El método **PrimeOutput** a su vez llama a los métodos siguientes:  
  
1.  El método **CreateNewOutputRows**, que se invalida en **ScriptMain** para agregar filas del origen de datos a los búferes de salida, que al principio están vacíos.  
  
2.  El método **FinishOutputs**, que está vacío de forma predeterminada. Invalide este método en **ScriptMain** para realizar cualquier procesamiento que se requiera para completar la salida.  
  
3.  El método privado **MarkOutputsAsFinished**, que llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer.SetEndOfRowset%2A> de la clase primaria <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer> para indicar al motor de flujo de datos que la salida ha finalizado. No tiene que llamar explícitamente a **SetEndOfRowset** en su propio código.  
  
### <a name="writing-your-custom-code"></a>Escribir código personalizado  
 Para terminar de crear un componente de origen personalizado, puede escribir el script en los siguientes métodos disponibles en la clase **ScriptMain**.  
  
1.  Invalide el método **AcquireConnections** para conectarse al origen de datos externo. Extraiga el objeto de conexión, o la información de conexión necesaria, del administrador de conexiones.  
  
2.  Invalide el método **PreExecute** para cargar datos, si puede cargar al mismo tiempo todos los datos de origen. Por ejemplo, puede ejecutar **SqlCommand** en una conexión [!INCLUDE[vstecado](../../includes/vstecado-md.md)] a una base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y cargar al mismo tiempo todos los datos de origen en **SqlDataReader**. Si debe cargar los datos de origen por filas individuales (por ejemplo, al leer un archivo de texto), puede cargar los datos mientras recorre las filas en bucle en **CreateNewOutputRows**.  
  
3.  Utilice el método **CreateNewOutputRows** invalidado para agregar nuevas filas a los búferes de salida vacíos y rellenar los valores de cada columna en las nuevas filas de salida. Utilice el método **AddRow** de cada búfer de salida para agregar una nueva fila vacía y, a continuación, establezca los valores de cada columna. Normalmente copia los valores de las columnas cargadas del origen externo.  
  
4.  Invalide el método **PostExecute** para finalizar el procesamiento de los datos. Por ejemplo, puede cerrar la clase **SqlDataReader** que utilizó para cargar datos.  
  
5.  Invalide el método **ReleaseConnections** para desconectarse del origen de datos externo, si se requiere.  
  
## <a name="examples"></a>Ejemplos  
 En los ejemplos siguientes se muestra el código personalizado que se necesita en la clase **ScriptMain** para crear un componente de origen.  
  
> [!NOTE]  
>  En estos ejemplos se usa la tabla **Person.Address** de la base de datos de ejemplo **AdventureWorks** y se pasan la primera y la cuarta columna, las columnas **intAddressID** y **nvarchar(30)City**, a través del flujo de datos. Estos mismos datos se usan en los ejemplos de origen, transformación y destino de esta sección. Se documentan requisitos previos y suposiciones adicionales para cada ejemplo.  
  
### <a name="adonet-source-example"></a>Ejemplo de origen de ADO.NET  
 En este ejemplo se muestra un componente de origen que utiliza un administrador de conexiones de [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existente para cargar datos de una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el flujo de datos.  
  
 Si desea ejecutar este código de ejemplo, debe configurar el paquete y el componente de la siguiente forma:  
  
1.  Cree un administrador de conexiones de [!INCLUDE[vstecado](../../includes/vstecado-md.md)] que utilice el proveedor **SqlClient** para conectarse a la base de datos **AdventureWorks**.  
  
2.  Agregue un nuevo componente de script a la superficie del diseñador de flujo de datos y configúrelo como origen.  
  
3.  Abra el **Editor de transformación Script**. En la página **Entradas y salidas**, cambie el nombre de la salida predeterminada por un nombre más descriptivo como **MyAddressOutput**, y agregue y configure las dos columnas de salida, **AddressID** y **City**.  
  
    > [!NOTE]  
    >  Asegúrese de cambiar el tipo de datos de la columna de salida **City** a DT_WSTR.  
  
4.  En la página **Administradores de conexión**, agregue o cree el administrador de conexiones de [!INCLUDE[vstecado](../../includes/vstecado-md.md)] y asígnele un nombre como **MyADONETConnection**.  
  
5.  En la página **Script**, haga clic en **Editar script** y escriba el script que se indica a continuación. A continuación, cierre el entorno de desarrollo de script y el **Editor de transformación Script**.  
  
6.  Cree y configure un componente de destino, como un destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el componente de destino de ejemplo que se muestra en [Creating a Destination with the Script Component](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) (Crear un destino con el componente de script), que espere las columnas **AddressID** y **City**. A continuación, conecte el componente de origen al destino. (Puede conectar directamente un origen a un destino sin ninguna transformación). Puede crear una tabla de destino ejecutando el siguiente comando [!INCLUDE[tsql](../../includes/tsql-md.md)] en la base de datos **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
7.  Ejecute el ejemplo.  
  
    ```vb  
    Imports System.Data.SqlClient  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Dim connMgr As IDTSConnectionManager100  
        Dim sqlConn As SqlConnection  
        Dim sqlReader As SqlDataReader  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            connMgr = Me.Connections.MyADONETConnection  
            sqlConn = CType(connMgr.AcquireConnection(Nothing), SqlConnection)  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            Dim cmd As New SqlCommand("SELECT AddressID, City, StateProvinceID FROM Person.Address", sqlConn)  
            sqlReader = cmd.ExecuteReader  
  
        End Sub  
  
        Public Overrides Sub CreateNewOutputRows()  
  
            Do While sqlReader.Read  
                With MyAddressOutputBuffer  
                    .AddRow()  
                    .AddressID = sqlReader.GetInt32(0)  
                    .City = sqlReader.GetString(1)  
                End With  
            Loop  
  
        End Sub  
  
        Public Overrides Sub PostExecute()  
  
            sqlReader.Close()  
  
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
        SqlDataReader sqlReader;  
  
        public override void AcquireConnections(object Transaction)  
        {  
            connMgr = this.Connections.MyADONETConnection;  
            sqlConn = (SqlConnection)connMgr.AcquireConnection(null);  
  
        }  
  
        public override void PreExecute()  
        {  
  
            SqlCommand cmd = new SqlCommand("SELECT AddressID, City, StateProvinceID FROM Person.Address", sqlConn);  
            sqlReader = cmd.ExecuteReader();  
  
        }  
  
        public override void CreateNewOutputRows()  
        {  
  
            while (sqlReader.Read())  
            {  
                {  
                    MyAddressOutputBuffer.AddRow();  
                    MyAddressOutputBuffer.AddressID = sqlReader.GetInt32(0);  
                    MyAddressOutputBuffer.City = sqlReader.GetString(1);  
                }  
            }  
  
        }  
  
        public override void PostExecute()  
        {  
  
            sqlReader.Close();  
  
        }  
  
        public override void ReleaseConnections()  
        {  
  
            connMgr.ReleaseConnection(sqlConn);  
  
        }  
  
    }  
    ```  
  
### <a name="flat-file-source-example"></a>Ejemplo de origen de archivo plano  
 En este ejemplo se muestra un componente de origen que utiliza un administrador de conexiones de archivos planos existente para cargar datos de un archivo plano en el flujo de datos. Los datos de origen del archivo plano se crean mediante su exportación desde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si desea ejecutar este código de ejemplo, debe configurar el paquete y el componente de la siguiente forma:  
  
1.  Utilice el Asistente para importación y exportación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para exportar la tabla **Person.Address** de la base de datos de ejemplo **AdventureWorks** a un archivo plano separado por comas. En este ejemplo se utiliza el nombre de archivo ExportedAddresses.txt.  
  
2.  Cree un administrador de conexiones de archivos planos que se conecte al archivo de datos exportado.  
  
3.  Agregue un nuevo componente de script a la superficie del diseñador de flujo de datos y configúrelo como origen.  
  
4.  Abra el **Editor de transformación Script**. En la página **Entradas y salidas**, cambie el nombre de la salida predeterminada por un nombre más descriptivo, como **MyAddressOutput**. Agregue y configure las dos columnas de salida, **AddressID** y **City**.  
  
5.  En la página **Administradores de conexión**, agregue o cree el administrador de conexiones de archivos planos utilizando un nombre descriptivo, como **MyFlatFileSrcConnectionManager**.  
  
6.  En la página **Script**, haga clic en **Editar script** y escriba el script que se indica a continuación. A continuación, cierre el entorno de desarrollo de script y el **Editor de transformación Script**.  
  
7.  Cree y configure un componente de destino, como un destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el componente de destino de ejemplo que se muestra en [Creating a Destination with the Script Component](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) (Crear un destino con el componente de script). A continuación, conecte el componente de origen al destino. (Puede conectar directamente un origen a un destino sin ninguna transformación). Puede crear una tabla de destino ejecutando el siguiente comando [!INCLUDE[tsql](../../includes/tsql-md.md)] en la base de datos **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
8.  Ejecute el ejemplo.  
  
    ```vb  
    Imports System.IO  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Private textReader As StreamReader  
        Private exportedAddressFile As String  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            Dim connMgr As IDTSConnectionManager100 = _  
                Me.Connections.MyFlatFileSrcConnectionManager  
            exportedAddressFile = _  
                CType(connMgr.AcquireConnection(Nothing), String)  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
            MyBase.PreExecute()  
            textReader = New StreamReader(exportedAddressFile)  
        End Sub  
  
        Public Overrides Sub CreateNewOutputRows()  
  
            Dim nextLine As String  
            Dim columns As String()  
  
            Dim delimiters As Char()  
            delimiters = ",".ToCharArray  
  
            nextLine = textReader.ReadLine  
            Do While nextLine IsNot Nothing  
                columns = nextLine.Split(delimiters)  
                With MyAddressOutputBuffer  
                    .AddRow()  
                    .AddressID = columns(0)  
                    .City = columns(3)  
                End With  
                nextLine = textReader.ReadLine  
            Loop  
  
        End Sub  
  
        Public Overrides Sub PostExecute()  
            MyBase.PostExecute()  
            textReader.Close()  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.IO;  
    public class ScriptMain:  
        UserComponent  
  
    {  
        private StreamReader textReader;  
        private string exportedAddressFile;  
  
        public override void AcquireConnections(object Transaction)  
        {  
  
            IDTSConnectionManager100 connMgr = this.Connections.MyFlatFileSrcConnectionManager;  
            exportedAddressFile = (string)connMgr.AcquireConnection(null);  
  
        }  
  
        public override void PreExecute()  
        {  
            base.PreExecute();  
            textReader = new StreamReader(exportedAddressFile);  
        }  
  
        public override void CreateNewOutputRows()  
        {  
  
            string nextLine;  
            string[] columns;  
  
            char[] delimiters;  
            delimiters = ",".ToCharArray();  
  
            nextLine = textReader.ReadLine();  
            while (nextLine != null)  
            {  
                columns = nextLine.Split(delimiters);  
                {  
                    MyAddressOutputBuffer.AddRow();  
                    MyAddressOutputBuffer.AddressID = columns[0];  
                    MyAddressOutputBuffer.City = columns[3];  
                }  
                nextLine = textReader.ReadLine();  
            }  
  
        }  
  
        public override void PostExecute()  
        {  
  
            base.PostExecute();  
            textReader.Close();  
  
        }  
  
    }  
    ```  
  
## <a name="see-also"></a>Consulte también  
 [Creating a Destination with the Script Component (Crear un destino con el componente de script)](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)   
 [Desarrollar un componente de origen personalizado](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md)  
  
