---
description: Ampliar el flujo de datos con el componente de script
title: Ampliar el flujo de datos con el componente de script | Microsoft Docs
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
- data flow task [Integration Services], components
- data flow [Integration Services], extending
- debugging [Integration Services], components
- code design mode [Integration Services]
- metadata [Integration Services]
- scripts [Integration Services], extending data flow
- data flow components [Integration Services], Script component
- components [Integration Services], data flow
- Script component [Integration Services], about Script component
- Script component [Integration Services]
- data flow [Integration Services], components
ms.assetid: 072bc4b8-363a-4131-87c3-240338e2fa5c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 82f34ab83935bee2972a4dbb2b007eb2632b9fba
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122935"
---
# <a name="extending-the-data-flow-with-the-script-component"></a>Ampliar el flujo de datos con el componente de script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  El componente de script amplía las funcionalidades de flujo de datos de los paquetes de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] con código personalizado escrito en [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# compilado y ejecutado en tiempo de ejecución del paquete. El componente de script simplifica el desarrollo de un origen, transformación o destino de flujo de datos personalizado cuando los orígenes, las transformaciones y los destinos incluidos en [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] no se adaptan totalmente a sus requisitos. Después de configurar el componente con las entradas y salidas esperadas, éste escribe todo el código de infraestructura necesario, lo que le permite centrarse exclusivamente en el código requerido para su procesamiento personalizado.  
  
 Un componente de script interactúa con el paquete contenedor y con el flujo de datos a través de las clases autogeneradas en los elementos de proyecto **ComponentWrapper** y **BufferWrapper**, que son instancias de las clases <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> y <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer>, respectivamente. Estas clases hacen que las conexiones, las variables y otros elementos del paquete estén disponibles como objetos con tipo y administran las entradas y salidas. El componente de script también puede utilizar el espacio de nombres de [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] y la biblioteca de clases de [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], además de ensamblados personalizados, para implementar la funcionalidad personalizada.  
  
 El componente de script y el código de infraestructura que genera simplifican considerablemente el proceso de desarrollo de un componente de flujo de datos personalizado. Sin embargo, para comprender cómo funciona el componente de script, puede resultar útil leer la sección [Desarrollar un componente de flujo de datos personalizado](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) a fin de entender los pasos necesarios para el desarrollo de un componente de flujo de datos personalizado.  
  
 Si va a crear un origen, una transformación o un destino que pretende reutilizar en varios paquetes, debe considerar la posibilidad de desarrollar un componente personalizado en lugar de utilizar el componente de script. Para obtener más información, vea [Desarrollar un componente de flujo de datos personalizado](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
## <a name="in-this-section"></a>En esta sección  
 Los temas siguientes proporcionan más información sobre el componente de script.  
  
 [Configurar el componente de script en el editor de componentes de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md)  
 Las propiedades que se configuran en el **Editor de transformación Script** afectan a las funcionalidades y al rendimiento del código del componente de script.  
  
 [Codificar y depurar el componente de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)  
 Se usa el entorno de desarrollo [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) para desarrollar los scripts incluidos en el componente de script.  
  
 [Descripción del modelo de objetos del componente de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/understanding-the-script-component-object-model.md)  
 Un proyecto de componente de script nuevo contiene tres elementos de proyecto con varias clases, así como propiedades y métodos autogenerados.  
  
 [Utilizar variables en el componente de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md)  
 El elemento de proyecto **ComponentWrapper** contiene propiedades de descriptor de acceso fuertemente tipado para variables de paquete.  
  
 [Conectarse a orígenes de datos del componente de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/connecting-to-data-sources-in-the-script-component.md)  
 El elemento de proyecto **ComponentWrapper** también contiene propiedades de descriptor de acceso fuertemente tipado para las conexiones definidas en el paquete.  
  
 [Provocar eventos en el componente de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/raising-events-in-the-script-component.md)  
 Puede provocar eventos para notificar sobre problemas y errores.  
  
 [Registrar en el componente de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/logging-in-the-script-component.md)  
 Puede registrar información en proveedores de registro habilitados en el paquete.  
  
 [Desarrollar tipos específicos de los componentes de script](../../../integration-services/extending-packages-scripting-data-flow-script-component-types/developing-specific-types-of-script-components.md)  
 Estos sencillos ejemplos explican y muestran cómo utilizar el componente de script para desarrollar orígenes, transformaciones y destinos de flujo de datos.  
  
 [Ejemplos de componente de script adicionales](../../../integration-services/extending-packages-scripting-data-flow-script-component-examples/additional-script-component-examples.md)  
 Estos sencillos ejemplos explican y muestran algunos usos posibles del componente de script.  
  
## <a name="see-also"></a>Consulte también  
 [Componente de script](../../../integration-services/data-flow/transformations/script-component.md)   
 [Comparar la tarea Script y el componente de script](../../../integration-services/extending-packages-scripting/comparing-the-script-task-and-the-script-component.md)  
  
  
