---
description: Desarrollar un enumerador foreach personalizado
title: Desarrollar un enumerador Para cada uno personalizado | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom foreach enumerators [Integration Services]
- custom foreach enumerators [Integration Services], about custom foreach enumerators
- foreach enumerators [Integration Services]
ms.assetid: bffe26e0-1b9a-47ad-bae6-6b708cb4cf4f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f86fb73d17a36a0ace8accd4ca494cfc449f67af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350276"
---
# <a name="developing-a-custom-foreach-enumerator"></a>Desarrollar un enumerador foreach personalizado

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] usa enumeradores de foreach para iterar por los elementos de una colección y realizar las mismas tareas para cada elemento. [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] incluye varios enumeradores de foreach que admiten las colecciones de uso más frecuente, como todos los archivos de una carpeta, todas las tablas de una base de datos o todos los elementos de una lista almacenados en una variable de paquete. Si las colecciones y enumeradores foreach que se proporcionan no cumplen completamente sus requisitos, puede crear un enumerador foreach personalizado.  
  
 Para crear un enumerador foreach personalizado, debe crear una clase que herede de la clase base <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>, aplicar el atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> a la nueva clase e invalidar los métodos y propiedades importantes de la clase base, incluido el método <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A>.  
  
## <a name="in-this-section"></a>En esta sección  
 En esta sección se describe cómo crear, configurar y codificar un enumerador foreach personalizado y su interfaz de usuario personalizada.  
  
 [Crear un enumerador foreach personalizado](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/creating-a-custom-foreach-enumerator.md)  
 Describe cómo crear las clases para un proyecto de enumerador foreach personalizado.  
  
 [Codificar un enumerador foreach personalizado](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)  
 Describe cómo implementar un enumerador foreach personalizado invalidando los métodos y propiedades de la clase base.  
  
 [Desarrollar una interfaz de usuario para un enumerador foreach personalizado](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)  
 Describe cómo implementar la clase de interfaz de usuario y el formulario que se utilizan para configurar el enumerador foreach personalizado.  
  
## <a name="related-topics"></a>Temas relacionados  
  
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
  
 [Desarrollar un administrador de conexiones personalizado](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-custom-connection-manager.md)  
 Describe cómo programar los administradores de conexiones personalizados.  
  
 [Desarrollar un proveedor de registro personalizado](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 Describe cómo programar los proveedores de registro personalizados.  
  
 [Desarrollar un componente de flujo de datos personalizado](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Describe cómo programar orígenes, transformaciones y destinos personalizados del flujo de datos.  
 
