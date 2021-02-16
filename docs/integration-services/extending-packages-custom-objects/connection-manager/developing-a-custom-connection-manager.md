---
description: Desarrollar un administrador de conexiones personalizado
title: Desarrollar un administrador de conexiones personalizado | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- packages [Integration Services], connections
- custom connection managers [Integration Services], about custom connection managers
- connection managers [Integration Services], custom
- Integration Services packages, connection managers
- SSIS packages, connection managers
- SQL Server Integration Services packages, connection managers
- custom connection managers [Integration Services]
ms.assetid: bda0b29e-57f5-4879-b04d-1396dc56daa8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d92ee03836944fd3a98f89c86369d2c442ee6ea5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348926"
---
# <a name="developing-a-custom-connection-manager"></a>Desarrollar un administrador de conexiones personalizado

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] usa administradores de conexión para encapsular la información necesaria para conectarse a un origen de datos externo. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] incluye diversos administradores de conexión que admiten conexiones a los orígenes de datos usados con más frecuencia, desde bases de datos empresariales hasta archivos de texto y hojas de cálculo de Excel. Si [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] admite administradores de conexiones y orígenes de datos externos que no cumplen completamente sus requisitos, puede crear un administrador de conexiones personalizado.  
  
 Para crear un administrador de conexiones personalizado, debe crear una clase que herede de la clase base <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>, aplicar el atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute> a la nueva clase e invalidar los métodos y propiedades importantes de la clase base, incluidos la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> y el método <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>.  
  
> [!IMPORTANT]  
>  Muchas de las tareas, orígenes y destinos que se han incluido en [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] se usan únicamente con tipos específicos de administradores de conexiones integrados. Antes de desarrollar un administrador de conexiones personalizado para utilizar con tareas y componentes integrados, compruebe si esos componentes restringen la lista de administradores de conexiones disponibles a aquéllos de un tipo específico. Si su solución requiere un administrador de conexiones personalizado, también podría tener que desarrollar una tarea personalizada, o un origen o destino personalizados, para su uso con el administrador de conexiones.  
  
## <a name="in-this-section"></a>En esta sección  
 En esta sección se describe cómo crear, configurar y codificar un administrador de conexiones y su interfaz de usuario personalizada opcional. Los fragmentos de código mostrados en esta sección se deducen del ejemplo de administrador de conexiones personalizado de Sql Server.  
  
 [Crear un administrador de conexiones personalizado](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)  
 Describe cómo crear las clases para un proyecto de administrador de conexiones personalizado.  
  
 [Codificar un administrador de conexiones personalizado](../../../integration-services/extending-packages-custom-objects/connection-manager/coding-a-custom-connection-manager.md)  
 Describe cómo implementar un administrador de conexiones personalizado invalidando los métodos y propiedades de la clase base.  
  
 [Desarrollar una interfaz de usuario para un administrador de conexiones personalizado](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
 Describe cómo implementar la clase de interfaz de usuario y el formulario que se utilizan para configurar el administrador de conexiones personalizado.  
  
## <a name="related-sections"></a>Secciones relacionadas  
  
### <a name="information-common-to-all-custom-objects"></a>Información común a todos los objetos personalizados  
 Para obtener información común a todos los tipos de objetos personalizados que puede crear en [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], vea los temas siguientes:  
  
 [Desarrollar objetos personalizados para Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)  
 Describe los pasos básicos para implementar todos los tipos de objetos personalizados para [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 [Conservar objetos personalizados](../../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)  
 Describe la persistencia personalizada y explica cuándo es necesaria.  
  
 [Generar, implementar y depurar objetos personalizados](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
 Describe las técnicas para generar, firmar, implementar y depurar objetos personalizados.  
  
### <a name="information-about-other-custom-objects"></a>Información sobre otros objetos personalizados  
 Para obtener información sobre los demás tipos de objetos personalizados que puede crear en [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], vea los temas siguientes:  
  
 [Desarrollar una tarea personalizada](../../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)  
 Describe cómo programar las tareas personalizadas.  
  
 [Desarrollar un proveedor de registro personalizado](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 Describe cómo programar los proveedores de registro personalizados.  
  
 [Desarrollar un enumerador ForEach personalizado](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 Describe cómo programar los enumeradores personalizados.  
  
 [Desarrollar un componente de flujo de datos personalizado](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Describe cómo programar orígenes, transformaciones y destinos personalizados del flujo de datos.  
  
