---
description: Volver a utilizar objetos de paquete
title: Volver a utilizar objetos de paquete | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- GUID regenerating [Integration Services]
- reusing packages
- templates [Integration Services]
- copying packages
- regenerating package GUID
ms.assetid: 08f723bf-15b5-44bd-9a46-04e8781bfbfb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f3ce79b8cc4d0a8fff0dda5bf833f918285eb4c7
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192445"
---
# <a name="reuse-of-package-objects"></a>Volver a utilizar objetos de paquete

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  Funcionalidad frecuente de paquetes que desea volver a usar. Por ejemplo, si creó un conjunto de tareas, es posible que desee volver a utilizar todos los elementos como grupo, o que desee volver a utilizar un único elemento, como por ejemplo, un administrador de conexiones que creó en un proyecto de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] diferente.  
  
## <a name="copy-and-paste"></a>Copiar y pegar  
 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] y el Diseñador [!INCLUDE[ssIS](../includes/ssis-md.md)] admiten la funcionalidad de copiar y pegar objetos de paquete, que puede incluir elementos de flujo de control, elementos de flujo de datos y administradores de conexión. Puede copiar y pegar entre proyectos y entre paquetes. Si la solución contiene varios proyectos, puede copiar entre proyectos, y los proyectos pueden ser de tipos diferentes.  
  
 Si una solución contiene varios paquetes, puede copiar y pegar entre éstos. Los paquetes pueden pertenecer a los mismos proyectos de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] o a proyectos diferentes. Sin embargo, los objetos de paquete pueden tener dependencias de otros objetos, sin las cuales no son válidos. Por ejemplo, una tarea Ejecutar SQL utiliza un administrador de conexiones que también debe copiar para que la tarea funcione. Asimismo, algunos objetos de paquete requieren que el paquete ya contenga un determinado objeto, ya que sin este objeto no podrá copiar ni pegar objetos correctamente en un paquete. Por ejemplo, no puede pegar un flujo de datos en un paquete que no tiene al menos una tarea Flujo de datos.  
  
 Puede encontrarse con que copia siempre los mismos paquetes. Para evitar el proceso de copia, puede designar estos paquetes como plantillas y utilizarlos en la creación de nuevos paquetes.  
  
 Al copiar un objeto de paquete, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] asigna automáticamente un nuevo GUID a la propiedad **ID** del nuevo objeto y actualiza la propiedad **Name** .  
  
 Las variables no se pueden copiar. Si un objeto, como una tarea, utiliza variables, debe volver a crear las variables en el paquete de destino. Por el contrario, si copia todo el paquete, también se copian las variables del paquete.  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Copiar objetos de paquete](../integration-services/copy-package-objects.md)  
  
-   [Copiar los elementos de proyectos](./integration-services-ssis-projects-and-solutions.md)  
  
-   [Guardar un paquete como una plantilla de paquete](./save-packages.md)  
  
