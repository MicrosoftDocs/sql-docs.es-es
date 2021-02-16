---
description: Simular una salida de error para el componente de script
title: Simular una salida de error para el componente de script | Microsoft Docs
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
- Script component [Integration Services], error output
- error outputs [Integration Services], Script component
ms.assetid: f8b6ecff-ac99-4231-a0e7-7ce4ad76bad0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 88af189347fe799f58939f167e6713c90b3a8275
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344912"
---
# <a name="simulating-an-error-output-for-the-script-component"></a>Simular una salida de error para el componente de script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Aunque no puede configurar directamente una salida como una salida de error en el componente de script para el control automático de filas del error, puede reproducir la funcionalidad de una salida de error integrada mediante la creación de una salida adicional y la utilización de una lógica condicional en su script para dirigir las filas a esta salida cuando corresponda. Puede que desee imitar el comportamiento de una salida de error integrada agregando dos columnas de salida adicionales para recibir el número de error y el identificador de la columna en la que se produjo un error.  
  
 Si desea agregar la descripción del error que corresponde a un determinado código de error de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] predefinido, puede usar el método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.GetErrorDescription%2A> de la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>, disponible a través de la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> del componente de script.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo se utiliza un componente de script configurado como una transformación con dos salidas sincrónicas. El propósito del componente de script es filtrar las filas que generan un error de los datos de dirección en la base de datos de ejemplo AdventureWorks. En este ejemplo ficticio se supone que se está preparando una promoción para clientes norteamericanos y es necesario filtrar las direcciones que no se encuentran en Norteamérica.  
  
#### <a name="to-configure-the-example"></a>Para configurar el ejemplo  
  
1.  Antes de crear el nuevo componente de script, cree un administrador de conexiones y configure un origen de flujo de datos que seleccione datos de la dirección de la base de datos de ejemplo AdventureWorks. En este ejemplo, donde solo se examina la columna CountryRegionName, simplemente puede usar la vista Person.vStateCountryProvinceRegion o puede seleccionar los datos combinando las tablas Person.Address, Person.StateProvincey Person.CountryRegion.  
  
2.  Agregue un nuevo componente de script a la superficie del diseñador de flujo de datos y configúrelo como una transformación. Abra el **Editor de transformación Script**.  
  
3.  En la página **Script**, establezca la propiedad **ScriptLanguage** en el lenguaje de script que quiere usar para programar el script.  
  
4.  Haga clic en **Editar script** para abrir [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA).  
  
5.  En el método **Input0_ProcessInputRow**, escriba o pegue el código de ejemplo que se muestra a continuación.  
  
6.  Cierre VSTA.  
  
7.  En la página **Columnas de entrada**, seleccione las columnas que quiere procesar en la transformación Script. En este ejemplo se utiliza únicamente la columna CountryRegionName. Las columnas de entrada disponibles que no se seleccionen pasarán simplemente al flujo de datos sin cambios.  
  
8.  En la página **Entradas y salidas**, agregue una segunda salida nueva y establezca su valor **SynchronousInputID** en el identificador de la entrada, que también es el valor de la propiedad **SynchronousInputID** de la salida predeterminada. Establezca la propiedad **ExclusionGroup** de ambas salidas en el mismo valor distinto de cero (por ejemplo, 1) para indicar que cada fila se dirigirá únicamente a una de las dos salidas. Asigne un nombre distintivo a la nueva salida de error, como "MyErrorOutput".  
  
9. Agregue las columnas de salida adicionales a la nueva salida de error para capturar la información de error deseada que puede incluir el código de error, el identificador de la columna en la que se produjo el error y, posiblemente, la descripción del error. En este ejemplo se crean las nuevas columnas, ErrorColumn y ErrorMessage. Si está detectando los errores de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] predefinidos en su propia implementación, asegúrese de agregar una columna ErrorCode para el número de error.  
  
10. Observe el valor del identificador de la columna o columnas de entrada que el componente de script comprobará para las condiciones de error. En este ejemplo se usa este identificador de columna para rellenar el valor ErrorColumn.  
  
11. Cierre el **Editor de transformación Script**.  
  
12. Adjunte las salidas del componente de script a destinos convenientes. Para pruebas ad hoc, la manera más fácil de configurar es mediante destinos de archivo plano.  
  
13. Ejecute el paquete.  
  
```vb  
Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
  If Row.CountryRegionName <> "Canada" _  
      And Row.CountryRegionName <> "United States" Then  
  
    Row.ErrorColumn = 68 ' ID of CountryRegionName column  
    Row.ErrorMessage = "Address is not in North America."  
    Row.DirectRowToMyErrorOutput()  
  
  Else  
  
    Row.DirectRowToOutput0()  
  
  End If  
  
End Sub  
```  
  
```csharp  
public override void Input0_ProcessInputRow(Input0Buffer Row)  
{  
  
  if (Row.CountryRegionName!="Canada"&&Row.CountryRegionName!="United States")  
  
  {  
    Row.ErrorColumn = 68; // ID of CountryRegionName column  
    Row.ErrorMessage = "Address is not in North America.";  
    Row.DirectRowToMyErrorOutput();  
  
  }  
  else  
  {  
  
    Row.DirectRowToOutput0();  
  
  }  
  
}  
```  
  
## <a name="see-also"></a>Consulte también  
 [Control de errores en los datos](../../integration-services/data-flow/error-handling-in-data.md)   
 [Usar las salidas de error en un componente de flujo de datos](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md)   
 [Crear una transformación sincrónica con el componente de script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)  
  
  
