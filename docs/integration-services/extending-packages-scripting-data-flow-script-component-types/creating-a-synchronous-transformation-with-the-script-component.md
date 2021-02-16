---
description: Crear una transformación sincrónica con el componente de script
title: Crear una transformación sincrónica con el componente de script | Microsoft Docs
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
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- Script component [Integration Services], transformation components
ms.assetid: aa1bee1a-ab06-44d8-9944-4bff03d73016
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 08c7e20120ef5ff746b495535843bc93a46fea16
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344890"
---
# <a name="creating-a-synchronous-transformation-with-the-script-component"></a>Crear una transformación sincrónica con el componente de script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Un componente de transformación se utiliza en el flujo de datos de un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para modificar y analizar los datos cuando pasan del origen al destino. Una transformación con salidas sincrónicas procesa cada fila de entrada a medida que pasa por el componente. Una transformación con salidas asincrónicas espera hasta que haya recibido todas las filas de entrada para completar su procesamiento. En este tema se describe una transformación sincrónica. Para obtener información sobre transformaciones asincrónicas, vea [Crear una transformación asincrónica con el componente de script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md). Para obtener más información acerca de la diferencia entre los componentes sincrónicos y asincrónicos, vea [Descripción de las transformaciones sincrónicas y asincrónicas](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Para obtener una introducción al componente de script, vea [Extending the Data Flow with the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md) (Extensión del Flujo de datos con el componente de script).  
  
 El componente de script y el código de infraestructura que genera simplifican considerablemente el proceso de desarrollo de un componente de flujo de datos personalizado. Sin embargo, para entender cómo funciona el componente de script, puede resultar útil leer los pasos que debe seguir para desarrollar un componente de flujo de datos personalizado en la sección [Developing a Custom Data Flow Component](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) (Desarrollo de un componente Flujo de datos personalizado) y, en especial, [Developing a Custom Transformation Component with Synchronous Outputs](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md) (Desarrollo de un componente de transformación personalizado con salidas sincrónicas).  
  
## <a name="getting-started-with-a-synchronous-transformation-component"></a>Introducción a un componente de transformación sincrónica  
 Al agregar un componente de script al panel Flujo de datos del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)], se abre el cuadro de diálogo **Seleccionar el tipo de componente de script** y se solicita que seleccione un tipo de componente como Origen, Transformación o Destino. En este cuadro de diálogo, seleccione **Transformación**.  
  
## <a name="configuring-a-synchronous-transformation-component-in-metadata-design-mode"></a>Configurar un componente de transformación sincrónica en modo de diseño de metadatos  
 Después de seleccionar la opción para crear un componente de transformación, configure el componente mediante el **Editor de transformación Script**. Para obtener más información, consulte [Configuring the Script Component in the Script Component Editor](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md) (Configurar el componente de script en el editor de componentes de script).  
  
 Para establecer el lenguaje de script para el componente de script, puede establecer la propiedad **ScriptLanguage** en la página **Script** del **Editor de transformación Script**.  
  
> [!NOTE]  
>  Para establecer el lenguaje de scripting predeterminado para el componente de script, utilice la opción **Lenguaje de scripting** en la página **General** del cuadro de diálogo **Opciones**. Para obtener más información, vea [General Page](../general-page-of-integration-services-designers-options.md) (Página General).  
  
 Un componente de transformación de flujo de datos tiene una entrada y admite una o varias salidas. La configuración de la entrada y las salidas del componente es uno de los pasos que debe completar en el modo de diseño de metadatos, mediante el **Editor de transformación Script**, antes de escribir un script personalizado.  
  
### <a name="configuring-input-columns"></a>Configurar las columnas de entrada  
 Un componente de transformación tiene una entrada.  
  
 En la página **Columnas de entrada** del **Editor de transformación Script**, la lista de columnas muestra las columnas disponibles de la salida del componente de nivel superior en el flujo de datos. Seleccione las columnas que desee transformar o las columnas a través de las que desee pasar. Marque todas las columnas que desee transformar en contexto como de columnas de lectura/escritura.  
  
 Para obtener más información acerca de la página **Columnas de entrada** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Input Columns Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Columnas de entrada]).  
  
### <a name="configuring-inputs-outputs-and-output-columns"></a>Configurar entradas, salidas y columnas de salidas  
 Un componente de transformación admite una o más salidas.  
  
 En la página **Entradas y salidas** del **Editor de transformación Script** puede ver que se ha creado una salida única, pero la salida no tiene ninguna columna. En esta página del editor, quizá necesite o desee configurar los elementos siguientes.  
  
-   Cree una o varias salidas adicionales, como una salida de error simulada para las filas que contienen valores inesperados. Utilice los botones **Agregar salida** y **Quitar salida** para administrar las salidas del componente de transformación sincrónica. Todas las filas de entrada se dirigen a todas las salidas disponibles a menos que indique que desea redirigir cada fila a una salida u otra. Para indicar que desea redirigir las filas, debe especificar un valor entero distinto de cero para la propiedad **ExclusionGroup** en las salidas. El valor entero concreto escrito en **ExclusionGroup** para identificar las salidas no es significativo, pero debe utilizar el mismo entero de forma coherente para el grupo de salidas especificado.  
  
    > [!NOTE]  
    >  También puede utilizar un valor de la propiedad **ExclusionGroup** distinto de cero con una salida única si no desea generar la salida de todas las filas. No obstante, en este caso, debe llamar explícitamente al método **DirectRowTo\<outputbuffer>** para cada fila que quiera enviar a la salida.  
  
-   Asigne un nombre más descriptivo a la entrada y las salidas. El componente de script utiliza estos nombres para generar las propiedades de descriptor de acceso con tipo que se usarán para hacer referencia a la entrada y las salidas en el script.  
  
-   Deje las columnas tal como están para las transformaciones sincrónicas. Normalmente una transformación sincrónica no agrega columnas al flujo de datos. Se modifican los datos in situ en el búfer y se pasa el contenido del búfer al componente siguiente del flujo de datos. Si éste es el caso, no tiene que agregar y configurar explícitamente las columnas de resultados en las salidas de la transformación. Las salidas aparecen en el editor sin las columnas definidas explícitamente.  
  
-   Agregue las nuevas columnas a las salidas de error simuladas para los errores de fila. Normalmente, varias salidas del mismo **ExclusionGroup** tienen el mismo conjunto de columnas de resultados. Sin embargo, si crea una salida de error simulada, quizá desee agregar más columnas que contengan información de error. Para obtener información acerca de cómo el motor de flujo de datos procesa las filas de errores, vea [Using Error Outputs in a Data Flow Component](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md) (Uso de las salidas de error en un componente de flujo de datos). Observe que en el componente de script debe escribir su propio código para llenar las columnas adicionales con la información de error adecuada. Para obtener más información, vea [Simulating an Error Output for the Script Component](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md) (Simular una salida de error para el componente de script).  
  
 Para obtener más información acerca de la página **Entradas y salidas** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Inputs and Outputs Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Entradas y salidas]).  
  
### <a name="adding-variables"></a>Agregar variables  
 Si quiere utilizar variables existentes en el script, puede agregarlas en los campos de propiedades **ReadOnlyVariables** y **ReadWriteVariables** de la página **Script** del **Editor de transformación Script**.  
  
 Cuando agregue varias variables a los campos de propiedades, separe sus nombres con comas. Para seleccionar varias variables, también puede hacer clic en el botón de puntos suspensivos ( **…** ) junto a los campos de propiedades **ReadOnlyVariables** y **ReadWriteVariables** y, después, seleccionar las variables en el cuadro de diálogo **Seleccionar variables**.  
  
 Para obtener información general sobre la forma de usar variables con el componente de script, vea [Using Variables in the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md) (Uso de variables con el componente de script).  
  
 Para obtener más información acerca de la página **Script** del **Editor de transformación Script**, vea [Script Transformation Editor &#40;Script Page&#41;](../data-flow/transformations/script-component.md) (Editor de transformación Script [página Script]).  
  
## <a name="scripting-a-synchronous-transformation-component-in-code-design-mode"></a>Scripting de un componente de transformación sincrónica en el modo de diseño de código  
 Después de haber configurado los metadatos para el componente, puede escribir un script personalizado. En la página **Script** del **Editor de transformación Script**, haga clic en **Editar script** para abrir el IDE de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA), donde puede agregar el script personalizado. El lenguaje de scripting que utilice dependerá de si ha seleccionado [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic o [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# como lenguaje de script para la propiedad **ScriptLanguage** en la página **Script**.  
  
 Para obtener información importante aplicable a todos los tipos de componentes creados mediante el componente de script, vea [Coding and Debugging the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md) (Programación y depuración del componente de script).  
  
### <a name="understanding-the-auto-generated-code"></a>Descripción del código generado automáticamente  
 Al abrir el IDE de VSTA después de crear y configurar un componente de transformación, la clase **ScriptMain** modificable aparece en el editor de código con un código auxiliar para el método **ProcessInputRow**. En la clase **ScriptMain** escribirá el código personalizado; **ProcessInputRow** es el método más importante de un componente de transformación.  
  
 Si abre la ventana **Explorador de proyectos** de VSTA, puede ver que el componente de script también ha generado elementos de proyecto **BufferWrapper** y **ComponentWrapper** de solo lectura. La clase **ScriptMain** se hereda de la clase **UserComponent** en el elemento de proyecto **ComponentWrapper**.  
  
 En tiempo de ejecución, el motor de flujo de datos invoca el método **ProcessInput** de la clase **UserComponent**, lo que invalida el método <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ProcessInput%2A> de la clase primaria <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. A su vez, el método **ProcessInput** recorre las filas del búfer de entrada y llama al método **ProcessInputRow** una vez por cada fila.  
  
### <a name="writing-your-custom-code"></a>Escribir código personalizado  
 Un componente de transformación con salidas sincrónicas es el más sencillo de escribir de todos los componentes de flujo de datos. Por ejemplo, el ejemplo de salida única mostrado más adelante en este tema consta del código personalizado siguiente:  
  
```vb  
Row.City = UCase(Row.City)  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
```  
  
 Para terminar de crear un componente de transformación sincrónica personalizado, debe utilizar el método **ProcessInputRow** invalidado para transformar los datos de cada fila del búfer de entrada. El motor de flujo de datos pasa este búfer, cuando está lleno, al componente siguiente del flujo de datos.  
  
 En función de sus requisitos, puede que también quiera escribir script en los métodos **PreExecute** y **PostExecute**, disponibles en la clase **ScriptMain**, para realizar un procesamiento preliminar o final.  
  
### <a name="working-with-multiple-outputs"></a>Trabajar con varias salidas  
 Dirigir las filas de entrada a una de dos o más posibles salidas no requiere mucho más código personalizado que el caso de ejemplo de salida única descrito anteriormente. Por ejemplo, el ejemplo de dos salidas mostrado más adelante en este tema consta del código personalizado siguiente:  
  
```vb  
Row.City = UCase(Row.City)  
If Row.City = "REDMOND" Then  
    Row.DirectRowToMyRedmondAddresses()  
Else  
    Row.DirectRowToMyOtherAddresses()  
End If  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
if (Row.City=="REDMOND")  
{  
    Row.DirectRowToMyRedmondAddresses();  
}  
else  
{  
    Row.DirectRowToMyOtherAddresses();  
}  
```  
  
 En este ejemplo, el componente de script genera los métodos **DirectRowTo\<OutputBufferX>** automáticamente, basándose en los nombres de las salidas configuradas. Puede utilizar un código similar para dirigir las filas de errores a una salida de error simulada.  
  
## <a name="examples"></a>Ejemplos  
 En los ejemplos que figuran aquí se muestra el código personalizado requerido en la clase **ScriptMain** para crear un componente de transformación sincrónica.  
  
> [!NOTE]  
>  En estos ejemplos se usa la tabla **Person.Address** de la base de datos de ejemplo **AdventureWorks** y se pasan la primera y la cuarta columna, las columnas **intAddressID** y **nvarchar(30)City**, a través del flujo de datos. Estos mismos datos se usan en los ejemplos de origen, transformación y destino de esta sección. Se documentan requisitos previos y suposiciones adicionales para cada ejemplo.  
  
### <a name="single-output-synchronous-transformation-example"></a>Ejemplo de transformación sincrónica de salida única  
 En este ejemplo se muestra un componente de transformación sincrónica con una salida única. Esta transformación pasa por la columna **AddressID** y convierte la columna **City** a mayúsculas.  
  
 Si desea ejecutar este código de ejemplo, debe configurar el paquete y el componente de la siguiente forma:  
  
1.  Agregue un nuevo componente de script a la superficie del diseñador de flujo de datos y configúrelo como una transformación.  
  
2.  Conecte la salida de un origen o de otra transformación al nuevo componente de transformación en el diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Esta salida debe proporcionar datos de la tabla **Person.Address** de la base de datos de ejemplo **AdventureWorks** que contiene las columnas **AddressID** y **City**.  
  
3.  Abra el **Editor de transformación Script**. En la página **Columnas de entrada**, seleccione las columnas **AddressID** y **City**. Marque la columna **City** como de lectura y escritura.  
  
4.  En la página **Entradas y salidas**, cambie el nombre de la entrada y la salida por nombres más descriptivos, como **MyAddressInput** y **MyAddressOutput**. Observe que el valor de **SynchronousInputID** de la salida se corresponde con el valor de **ID** de la entrada. Por consiguiente no tiene que agregar y configurar las columnas de resultados.  
  
5.  En la página **Script**, haga clic en **Editar script** y escriba el script que se indica a continuación. A continuación, cierre el entorno de desarrollo de script y el **Editor de transformación Script**.  
  
6.  Cree y configure un componente de destino que espere las columnas **AddressID** y **City**, como un destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], o el componente de destino de ejemplo que se muestra en [Creating a Destination with the Script Component](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) (Crear un destino con el componente de script). A continuación, conecte la salida de transformación al componente de destino. Puede crear una tabla de destino ejecutando el siguiente comando [!INCLUDE[tsql](../../includes/tsql-md.md)] en la base de datos **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
7.  Ejecute el ejemplo.  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
{  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
    }  
  
}  
```  
  
### <a name="two-output-synchronous-transformation-example"></a>Ejemplo de transformación sincrónica de dos salidas  
 En este ejemplo se muestra un componente de transformación sincrónica con dos salidas. Esta transformación pasa por la columna **AddressID** y convierte la columna **City** a mayúsculas. Si el nombre de la ciudad es Redmond, dirige la fila a una salida; dirige todas las demás filas a otra salida.  
  
 Si desea ejecutar este código de ejemplo, debe configurar el paquete y el componente de la siguiente forma:  
  
1.  Agregue un nuevo componente de script a la superficie del diseñador de flujo de datos y configúrelo como una transformación.  
  
2.  Conecte la salida de un origen o de otra transformación al nuevo componente de transformación en el diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Esta salida debe proporcionar datos de la tabla **Person.Address** de la base de datos de ejemplo **AdventureWorks** que contiene al menos las columnas **AddressID** y **City**.  
  
3.  Abra el **Editor de transformación Script**. En la página **Columnas de entrada**, seleccione las columnas **AddressID** y **City**. Marque la columna **City** como de lectura y escritura.  
  
4.  En la página **Entradas y salidas**, cree una segunda salida. Después de agregar la nueva salida, asegúrese de que establece **SynchronousInputID** en el **ID** de la entrada. Esta propiedad ya está establecida en la primera salida, que se crea de forma predeterminada. Para cada salida, establezca la propiedad **ExclusionGroup** en el mismo valor distinto de cero para indicar que dividirá las filas de entrada entre dos salidas mutuamente excluyentes. No tiene que agregar ninguna columna de resultados a las salidas.  
  
5.  Cambie el nombre de la entrada y las salidas con nombres más descriptivos, como **MyAddressInput**, **MyRedmondAddresses** y **MyOtherAddresses**.  
  
6.  En la página **Script**, haga clic en **Editar script** y escriba el script que se indica a continuación. A continuación, cierre el entorno de desarrollo de script y el **Editor de transformación Script**.  
  
7.  Cree y configure dos componentes de destino que esperen las columnas **AddressID** y **City**, como un destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un destino de archivo plano o el componente de destino de ejemplo que se muestra en [Creating a Destination with the Script Component](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) (Crear un destino con el componente de script). A continuación, conecte cada una de las salidas de la transformación a uno de los componentes de destino. Puede crear las tablas de destino ejecutando un comando [!INCLUDE[tsql](../../includes/tsql-md.md)] similar al siguiente (con nombres de tabla únicos) en la base de datos **AdventureWorks**:  
  
    ```sql
    CREATE TABLE [Person].[Address2](  
        [AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL  
    ```  
  
8.  Ejecute el ejemplo.  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
        If Row.City = "REDMOND" Then  
            Row.DirectRowToMyRedmondAddresses()  
        Else  
            Row.DirectRowToMyOtherAddresses()  
        End If  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
        if (Row.City == "REDMOND")  
        {  
            Row.DirectRowToMyRedmondAddresses();  
        }  
        else  
        {  
            Row.DirectRowToMyOtherAddresses();  
        }  
  
    }  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 [Descripción de las transformaciones sincrónicas y asincrónicas](~/integration-services/understanding-synchronous-and-asynchronous-transformations.md)  
 [Crear una transformación asincrónica con el componente de script](~/integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
 [Desarrollar un componente de transformación personalizado con salidas sincrónicas](~/integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)
