---
description: Utilizar variables en el componente de script
title: Utilizar variables en el componente de script | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], using variables
ms.assetid: 92d1881a-1ef1-43ae-b1ca-48d0536bdbc2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d27181eac591f6c66166810e9662c04b6b97fc40
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196427"
---
# <a name="using-variables-in-the-script-component"></a>Utilizar variables en el componente de script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Las variables almacenan valores que un paquete y sus contenedores, tareas y controladores de eventos pueden usar en tiempo de ejecución. Para más información, vea [Variables de Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-variables.md).  
  
 Puede hacer que las variables existentes estén disponibles para el acceso de solo lectura o de lectura/escritura por parte del script personalizado escribiendo listas de variables delimitadas por comas en los campos **ReadOnlyVariables** y **ReadWriteVariables** de la página **Script** del **Editor de transformación Script**. Tenga presente que los nombres de variable distinguen entre mayúsculas y minúsculas. Utilice la propiedad **Value** para leer y escribir en las variables individuales. El componente de script administra en segundo plano cualquier bloqueo necesario cuando el script manipula las variables en tiempo de ejecución.  
  
> [!IMPORTANT]  
>  La colección de **ReadWriteVariables** solo está disponible en el método **PostExecute** para maximizar el rendimiento y minimizar el riesgo de conflictos de bloqueo. Por consiguiente no puede incrementar directamente el valor de una variable de paquete cuando procesa cada fila de datos. Incremente el valor de una variable local en su lugar y establezca el valor de la variable de paquete en el de la variable local en el método **PostExecute** una vez procesados todos los datos. También puede utilizar la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> para evitar esta limitación, tal y como se describe más adelante en este tema. Sin embargo, si se escribe directamente en una variable de paquete cuando se procesa cada fila afectará negativamente al rendimiento y aumentará el riesgo de conflictos de bloqueo.  
  
 Para obtener más información acerca de la página **Script** del **Editor de transformación Script**, vea [Configurar el componente de script en el editor de componentes de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md) y [Editor de transformación Script &#40;página Script&#41;](../../data-flow/transformations/script-component.md).  
  
 El componente de script crea una clase de colección **Variables** en el elemento de proyecto **ComponentWrapper** con una propiedad de descriptor de acceso fuertemente tipado para el valor de cada variable preconfigurada donde la propiedad tiene el mismo nombre que la propia variable. Esta colección se expone mediante la propiedad **Variables** de la clase **ScriptMain**. La propiedad de descriptor de acceso proporciona permiso de solo lectura o de lectura/escritura al valor de la variable según corresponda. Por ejemplo, si ha agregado una variable entera denominada `MyIntegerVariable` a la lista **ReadOnlyVariables**, puede recuperar el valor en el script utilizando el código siguiente:  
  
 `Dim myIntegerVariableValue As Integer = Me.Variables.MyIntegerVariable`  
  
 También puede utilizar la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A>, a la que se tiene acceso llamando a `Me.VariableDispenser`, para trabajar con variables del componente de script. En este caso no utiliza las propiedades de descriptor de acceso indicadas y escritas para las variables, sino que obtiene acceso directamente a las variables. Al utilizar <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A>, debe administrar la semántica de bloqueo y la conversión de tipos de datos para los valores de variables en su propio código. Tiene que utilizar la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> en lugar de las propiedades de descriptor de acceso con nombre y tipo si desea trabajar con una variable que no esté disponible en tiempo de diseño sino que se crea mediante programación en tiempo de ejecución.  
  
## <a name="see-also"></a>Consulte también  
 [Variables de Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-variables.md)   
 [Usar variables en paquetes](../../integration-services-ssis-variables.md)  
  
