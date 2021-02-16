---
description: Crear un proveedor de registro personalizado
title: Crear un proveedor de registro personalizado | Microsoft Docs
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
- custom log providers [Integration Services], creating
ms.assetid: fc20af96-9eb8-4195-8d3f-8a4d7c753f24
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1f6de779a67df0bdbd81086c1b34992d0fe896ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354369"
---
# <a name="creating-a-custom-log-provider"></a>Crear un proveedor de registro personalizado

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  El entorno en tiempo de ejecución de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] incluye amplias funciones de registro. Un registro permite capturar eventos que se generan durante la ejecución del paquete. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] incluye varios proveedores de registro que permiten crear registros y almacenarlos en diversos formatos como XML, texto, base de datos o en el registro de eventos de Windows. Si uno de estos proveedores o formatos de salida no se ajusta sus necesidades, puede crear un proveedor de registro personalizado.  
  
 Los pasos necesarios para crear un proveedor de registro personalizado son similares a los pasos para crear cualquier otro objeto personalizado para [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]:  
  
-   Cree una clase nueva que herede de la clase base. Para un proveedor de registro, la clase base es <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>.  
  
-   Aplique el atributo que identifica el tipo de objeto para la clase. Para un proveedor de registro, el atributo es <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute>.  
  
-   Invalide la implementación de los métodos y las propiedades de la clase base. Para un proveedor de registro, estos incluyen la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>, los métodos <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>.  
  
-   Las interfaces de usuario personalizadas para los proveedores de registro personalizados no están implementadas en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
## <a name="getting-started-with-a-custom-log-provider"></a>Introducción a un proveedor de registro personalizado  
  
### <a name="creating-projects-and-classes"></a>Crear proyectos y clases  
 Dado que todos los proveedores de registro administrados se derivan de la clase base <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>, el primer paso para crear un proveedor de registro personalizado consiste en crear un proyecto de bibliotecas de clases en el lenguaje de programación administrado que prefiera y, a continuación, crear una clase que herede de la clase base. En esta clase derivada se invalidarán los métodos y las propiedades de la clase base para implementar la funcionalidad personalizada.  
  
 Configure el proyecto para firmar el ensamblado que se generará con un archivo de claves del nombre seguro.  
  
> [!NOTE]  
>  Muchos proveedores de registro de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] tienen una interfaz de usuario personalizada que implementa <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsLogProviderUI> y reemplaza el cuadro de texto **Configuración** en el cuadro de diálogo **Configurar registros de SSIS** por una lista desplegable filtrada de administradores de conexiones disponibles. Sin embargo, las interfaces de usuario personalizadas para los proveedores de registro personalizados no se implementan en [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
### <a name="applying-the-dtslogprovider-attribute"></a>Aplicar el atributo DtsLogProvider  
 Aplique el atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> a la clase que ha creado para identificarlo como un proveedor de registro. Este atributo proporciona información en tiempo de diseño, como el nombre y la descripción del proveedor de registro. Las propiedades **DisplayName** y **Description** del atributo corresponden a las columnas **Nombre** y **Descripción** que se muestran en el editor **Configurar registros de SSIS**, que se muestra al configurar el registro para un paquete en [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
> [!IMPORTANT]  
>  La propiedad <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.LogProviderType%2A> del atributo no se usa. Sin embargo, debe especificar un valor para la propiedad o el proveedor de registro personalizado no aparecerá en la lista de proveedores de registro disponibles.  
  
> [!NOTE]  
>  Desde que las interfaces de usuario personalizadas para los proveedores de registro personalizados no se implementa en [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], especificar un valor para la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.UITypeName%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> no tiene ningún efecto.  
  
```vb  
<DtsLogProvider(DisplayName:="MyLogProvider", Description:="A simple log provider.", LogProviderType:="Custom")> _  
Public Class MyLogProvider  
     Inherits LogProviderBase  
    ' TODO: Override the base class methods.  
End Class  
```  
  
```csharp  
[DtsLogProvider(DisplayName="MyLogProvider", Description="A simple log provider.", LogProviderType="Custom")]  
public class MyLogProvider : LogProviderBase  
{  
    // TODO: Override the base class methods.  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-log-provider"></a>Generar, implementar y depurar un proveedor de registro personalizado  
 Los pasos para generar, implementar y depurar un proveedor de registro personalizado en [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] son muy similares a los pasos requeridos para otros tipos de objetos personalizados. Para obtener más información, consulte [Generar, implementar y depurar objetos personalizados](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md).  
  
## <a name="see-also"></a>Consulte también  
 [Programar un proveedor de registro personalizado](../../../integration-services/extending-packages-custom-objects/log-provider/coding-a-custom-log-provider.md)   
 [Desarrollar una interfaz de usuario para un proveedor de registro personalizado](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
  
  
