---
description: Desarrollar objetos personalizados para Integration Services
title: Desarrollar objetos personalizados para Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom user interface [Integration Services]
- custom objects [Integration Services]
ms.assetid: ca1929a6-0ae6-47d7-b65f-08173b143720
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 15f7d17f7ea8f5a8736052bd43f9b09352597ae2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350320"
---
# <a name="developing-custom-objects-for-integration-services"></a>Desarrollar objetos personalizados para Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Cuando los objetos de flujo de datos y flujo de control que se incluyen con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] no cumplen completamente los requisitos, puede desarrollar muchos tipos de objetos personalizados, entre los que se incluyen:  
  
-   **Tareas personalizadas**.  
  
-   **Administradores de conexión personalizados.** Conexión a orígenes de datos externos que no son compatibles actualmente.  
  
-   **Proveedores de registro personalizados.** Eventos de paquete de registro en formatos que no son compatibles actualmente.  
  
-   **Enumeradores personalizados.** Admite iteración sobre un conjunto formatos de objetos o valores que no son compatibles actualmente.  
  
-   **Componentes de flujo de datos personalizados.** Se pueden configurar como orígenes, transformaciones o destinos.  
  
 El modelo de objetos [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] facilita este desarrollo personalizado con clases base que proporcionan un marco coherente y confiable para su implementación personalizada.  
  
 Si no tiene que reutilizar la funcionalidad personalizada por varios paquetes, la tarea Script y el componente de script le proporcionan toda la eficacia de un lenguaje de programación administrado con significativamente menos código de infraestructura para escribir. Para obtener más información, consulte [Comparing Scripting Solutions and Custom Objects](../../integration-services/extending-packages-scripting/comparing-scripting-solutions-and-custom-objects.md) (Comparar soluciones de scripting y objetos personalizados).  
  
## <a name="steps-in-developing-a-custom-object-for-integration-services"></a>Pasos para desarrollar un objeto personalizado para Integration Services  
 Cuando se desarrollar un objeto personalizado para su uso en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], se desarrolla una biblioteca de clases (una DLL) que se cargará en tiempo de diseño y tiempo de ejecución por el Diseñador SSIS y por el tiempo de ejecución de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Los métodos más importantes que debe implementar no son métodos a los que llama desde su propio código, sino métodos que el tiempo de ejecución llama en los momentos adecuados para inicializar y validar su componente e invocar su funcionalidad.  
  
 A continuación figuran los pasos que se deben seguir para desarrollar un objeto personalizado:  
  
1.  Cree un nuevo proyecto de tipo biblioteca de clases en su lenguaje de programación administrado preferido.  
  
2.  Herede de la clase base adecuada, como se muestra en la tabla siguiente.  
  
3.  Aplique el atributo adecuado a su nueva clase, como se muestra en la tabla siguiente.  
  
4.  Invalide los métodos de la clase base tal y como se requiere, y escriba el código para la funcionalidad personalizada de su objeto.  
  
5.  Opcionalmente, construya una interfaz de usuario personalizada para su componente. Para facilitar la implementación, puede que desee desarrollar la interfaz de usuario como un proyecto independiente dentro de la misma solución y generarlo como un ensamblado independiente.  
  
6.  Si lo desea, puede mostrar un vínculo en los ejemplos y en el contenido de ayuda del objeto personalizado, en el **Cuadro de herramientas de SSIS**.  
  
7.  Compile, implemente y depure el nuevo objeto personalizado tal y como se describe en [Building, Deploying, and Debugging Custom Objects (Compilar, implementar y depurar objetos personalizados)](../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md).  
  
## <a name="base-classes-attributes-and-important-methods"></a>Clases base, atributos y métodos importantes  
 En esta tabla se proporciona una referencia sencilla de los elementos más importantes en el modelo de objetos [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para cada tipo de objeto personalizado que puede desarrollar.  
  
|Objeto personalizado|Clase base|Atributo|Métodos importantes|  
|-------------------|----------------|---------------|-----------------------|  
|Tarea|<xref:Microsoft.SqlServer.Dts.Runtime.Task>|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>|<xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>|  
|Administrador de conexiones|<xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>|<xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute>|<xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A>|  
|Proveedor de registro|<xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>|<xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute>|<xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>|  
|Enumerador|<xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>|<xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute>|<xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A>|  
|Componente de flujo de datos|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>|<xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>|  
  
## <a name="providing-links-to-samples-and-help-content"></a>Proporcionar vínculos a ejemplos y al contenido de la Ayuda  
 Para mostrar un vínculo del **Cuadro de herramientas de SSIS** en ejemplos y en el contenido de ayuda de un objeto personalizado escrito en código administrado, use las siguientes propiedades.  
  
-   [Microsoft.SqlServer.Dts.Pipeline.DTSPipelineComponentAttribute.SamplesTag*](/dotnet/api/microsoft.sqlserver.dts.pipeline.dtspipelinecomponentattribute.samplestag)  
  
-   [Microsoft.SqlServer.Dts.Pipeline.DTSPipelineComponentAttribute.HelpCollection*](/dotnet/api/microsoft.sqlserver.dts.pipeline.dtspipelinecomponentattribute.helpcollection)  
  
-   [Microsoft.SqlServer.Dts.Pipeline.DTSPipelineComponentAttribute.HelpKeyword*](/dotnet/api/microsoft.sqlserver.dts.pipeline.dtspipelinecomponentattribute.helpkeyword)  
  
-   [Microsoft.SqlServer.Dts.Runtime.DTSTaskAttribute.SamplesTag*](/dotnet/api/microsoft.sqlserver.dts.runtime.dtstaskattribute.samplestag)  
  
-   [Microsoft.SqlServer.Dts.Runtime.DTSTaskAttribute.HelpCollection*](/dotnet/api/microsoft.sqlserver.dts.runtime.dtstaskattribute.helpcollection)  
  
-   [Microsoft.SqlServer.Dts.Runtime.DTSTaskAttribute.HelpKeyword*](/dotnet/api/microsoft.sqlserver.dts.runtime.dtstaskattribute.helpkeyword)  
  
 Para mostrar un vínculo a ejemplos y al contenido de la Ayuda para un objeto personalizado escrito en código nativo, agregue las entradas del archivo de script de Registro (.rgs) de SamplesTag, HelpKeyword y HelpCollection. A continuación se muestra un ejemplo.  
  
 `val HelpKeyword = s 'sql11.dts.designer.executepackagetask.F1'`  
  
 `val SamplesTag = s 'ExecutePackageTask'`  
  
## <a name="providing-a-custom-user-interface"></a>Proporcionar una interfaz de usuario personalizada  
 Para permitir a los usuarios de su objeto personalizado configurar sus propiedades, puede que tenga que desarrollar también una interfaz de usuario personalizada. En aquellos casos donde no se requiere una interfaz de usuario personalizada de forma estricta, puede que decida crear una para proporcionar una interfaz más fácil de comprender que el editor predeterminado.  
  
 Un proyecto o ensamblado personalizado de la interfaz de usuario suele tener dos clases: una clase que implementa una interfaz [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para las interfaces de usuario para el tipo específico de objeto personalizado y el formulario de Windows Forms que se muestra para recopilar información del usuario. Las interfaces que implementa tienen solo unos cuantos métodos y una interfaz de usuario personalizada que no es difícil de desarrollar.  
  
> [!NOTE]  
>  Muchos proveedores de registro de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] tienen una interfaz de usuario personalizada que implementa <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsLogProviderUI> y reemplaza el cuadro de texto **Configuración** con una lista desplegable que filtra los administradores de conexión disponibles. Sin embargo, las interfaces de usuario personalizadas para los proveedores de registro personalizados no se implementan en esta versión de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Especificar un valor para la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.UITypeName%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> no tiene ningún efecto.  
  
 La tabla siguiente proporciona una referencia sencilla para las interfaces que debe implementar al desarrollar una interfaz de usuario personalizada para cada tipo de objeto personalizado. Asimismo, también se explica en esta tabla lo que el usuario ve si decide no desarrollar ninguna interfaz de usuario personalizada del objeto, o si no vincula ese objeto a la interfaz de usuario mediante la propiedad **UITypeName** en el atributo del objeto. Aunque el potente editor avanzado puede resultar satisfactorio para un componente de flujo de datos, la ventana Propiedades es una solución menos fácil de usar en tareas y administradores de conexión, y un enumerador ForEach personalizado no se puede configurar en absoluto sin un formulario personalizado.  
  
|Objeto personalizado|Clase base para la interfaz de usuario|Comportamiento de edición de valor predeterminado si no se proporciona ninguna interfaz de usuario personalizada|  
|-------------------|-----------------------------------|----------------------------------------------------------------------|  
|Tarea|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>|Solo la ventana Propiedades|  
|Administrador de conexiones|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI>|Solo la ventana Propiedades|  
|Proveedor de registro|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsLogProviderUI><br /><br /> (No se implementa en [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)])|Cuadro de texto en la columna **Configuración**|  
|Enumerador|<xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI>|Solo la ventana Propiedades. El área del editor Configuración de enumeradores está vacía.|  
|Componente de flujo de datos|<xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>|Editor avanzado|  
  
## <a name="external-resources"></a>Recursos externos  
  
-   Entrada de Blog, [Visual Studio solution build process give a warning about indirect dependency on the .NET Framework assembly due to SSIS references (El proceso de compilación de la solución de Visual Studio muestra una advertencia acerca de la dependencia indirecta del ensamblado de .NET Framework debido a referencias SSIS)](/archive/blogs/jason_howell/visual-studio-2010-solution-build-process-give-a-warning-about-indirect-dependency-on-the-net-framework-assembly-due-to-ssis-references) en blogs.msdn.com.  
  
## <a name="see-also"></a>Consulte también  
 [Conservar objetos personalizados](../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)   
 [Generar, implementar y depurar objetos personalizados](../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
  
