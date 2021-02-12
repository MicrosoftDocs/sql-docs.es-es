---
title: Usar ensamblados personalizados con informes | Microsoft Docs
description: Desarrolle un ensamblado de código personalizado mediante Microsoft .NET Framework para poder hacer referencia al ensamblado desde los archivos de definición de informe.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- custom assemblies [Reporting Services]
- assemblies [Reporting Services], custom
- custom assemblies [Reporting Services], about custom assemblies
ms.assetid: 53d141d0-2185-466a-84dc-7b90d284da3d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 19977f4a90ecfb8db81c53aebf7b7ee3e4be68ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064500"
---
# <a name="using-custom-assemblies-with-reports"></a>Usar ensamblados personalizados con informes
  En [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], puede escribir código personalizado para los valores de elementos de informe, estilos y formato. Por ejemplo, puede utilizar código personalizado para dar formato a las monedas según la configuración regional, marcar ciertos valores con formato especial o aplicar otras reglas de negocios en vigor en la compañía. Una manera de incluir este código en los informes es crear un ensamblado de código personalizado mediante [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] al que puede hacer referencia desde dentro de los archivos de definición de informe. El servidor llama a las funciones de los ensamblados personalizados cuando se ejecuta un informe. Los ensamblados personalizados se pueden utilizar para recuperar funciones especializadas que piensa utilizar en los informes.  
  
## <a name="in-this-section"></a>En esta sección  
 [Referencia a ensamblados en un archivo RDL](../../reporting-services/custom-assemblies/referencing-assemblies-in-an-rdl-file.md)  
 Describe cómo hacer referencia a ensamblados personalizados en un archivo de lenguaje RDL (Report Definition Language).  
  
 [Implementación de un ensamblado personalizado](../../reporting-services/custom-assemblies/deploying-a-custom-assembly.md)  
 Describe cómo implementar un ensamblado personalizado en el Diseñador de informes y el servidor de informes.  
  
 [Uso de ensamblados personalizados con nombre seguro](../../reporting-services/custom-assemblies/using-strong-named-custom-assemblies.md)  
 Describe cómo utilizar ensamblados personalizados con nombres seguros.  
  
 [Aserción de los permisos en ensamblados personalizados](../../reporting-services/custom-assemblies/asserting-permissions-in-custom-assemblies.md)  
 Describe cómo implementar los ensamblados personalizados con permisos limitados  concretos, y cómo validar esos permisos en el código.  
  
 [Acceso a los ensamblados personalizados a través de expresiones](../../reporting-services/custom-assemblies/accessing-custom-assemblies-through-expressions.md)  
 Describe cómo llamar a los métodos de ensamblado personalizados como expresiones de informe en las definiciones de informe.  
  
 [Inicialización de objetos de ensamblados personalizados](../../reporting-services/custom-assemblies/initializing-custom-assembly-objects.md)  
 Describe cómo inicializar los valores para los objetos de ensamblado personalizados llamados desde un informe.  
  
 [Cómo: Depurar ensamblados personalizados](../../reporting-services/custom-assemblies/how-to-debug-custom-assemblies.md)  
 Describe cómo depurar el código de ensamblado personalizado.  
  
## <a name="see-also"></a>Consulte también  
 [Report Definition Language &#40;SSRS&#41;](../../reporting-services/reports/report-definition-language-ssrs.md)  
  
  
