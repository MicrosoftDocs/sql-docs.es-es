---
title: Información general de las extensiones de representación | Microsoft Docs
description: Vea qué extensiones de representación de datos se incluyen con Reporting Services. Aprenda a agregar extensiones de representación personalizadas para generar informes en otros formatos.
ms.date: 12/7/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- formats [Reporting Services], rendering extensions
- rendering extensions [Reporting Services], about extensions
ms.assetid: 909356a0-4709-43e5-b597-33bd9bb22882
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b3d3dc5fac6915faafb48b045b70c5b57f6e6ad8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071148"
---
# <a name="rendering-extensions-overview"></a>Información general de las extensiones de representación
  Una extensión de representación es un componente o módulo de un servidor de informes que transforma los datos de informes y la información de diseño en un formato específico del dispositivo. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] incluye siete extensiones de representación: HTML, Excel, Word, CSV o Text, XML, Image y PDF. Puede crear extensiones de representación adicionales para generar informes en otros formatos.  
  
> [!NOTE]  
>  Para determinar qué extensiones de representación están disponibles, puede ver la lista de extensiones instaladas en el archivo RSReportServer.config.  
  
 En la tabla siguiente se describen las extensiones de representación que se incluyen con [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
|Nombre de la extensión|Descripción|  
|--------------------|-----------------|  
|**XML**|Representa un informe en XML. El informe se abre en un explorador. Las transformaciones adicionales aplicadas a esta salida XML pueden constituir una manera rentable de evitar desarrollar una extensión de representación propia.|  
|**CSV**|Representa un informe en formato delimitado por comas. El informe se abre en una herramienta de visualización asociada a los formatos de archivo CSV.|  
|**IMAGE**|Representa un informe en formato orientado a la página. El formato se muestra como **TIFF** en el cuadro desplegable Exportar de la barra de herramientas del informe.|  
|**PDF**|Representa un informe en Adobe Acrobat Reader. El formato se muestra como **Archivo de Acrobat (PDF)** en el cuadro desplegable Exportar de la barra de herramientas del informe.|  
|**EXCEL**|Representa un informe en [!INCLUDE[ofprexcel](../../../includes/ofprexcel-md.md)].|  
|**WORD**|Representar un informe en [!INCLUDE[ofprword](../../../includes/ofprword-md.md)].|  
|**HTML 4.0** (parte de la extensión de representación en HTML)|HTML es el formato que se usó inicialmente para representar el informe. Si el explorador admite HTML 4.0, ése es el formato que se utiliza. De lo contrario, se utiliza HTML 3.2.|  
|**MHTML** (parte de la extensión de representación en HTML)|Representa un informe en MHTML. El informe se abre en Internet Explorer. El formato se muestra como **Archivo web** en el cuadro desplegable Exportar de la barra de herramientas del informe.|  
|**NULL**|No representa un informe en un formato concreto. Esta extensión de representación es útil para colocar los informes en la memoria caché. La representación NULL se debería utilizar junto con una ejecución o entrega programada.|  
  
 Para más información sobre los formatos recomendados y sus usos, vea [Exportar informes &#40;Generador de informes y SSRS&#41;](../../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
  
 Cada una de las extensiones de representación que se implementan mediante [!INCLUDE[msCoName](../../../includes/msconame-md.md)] y se distribuyen con [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] usa un conjunto común de interfaces. De este modo se asegura de que cada extensión implementa una funcionalidad comparable y reduce la complejidad del código de la representación en el núcleo del servidor de informes.  
  
## <a name="rendering-object-model"></a>Modelo de objetos de representación  
 Cuando se procesa un informe, el resultado es un modelo de objetos expuesto públicamente conocido como Modelo de objetos de representación (ROM). El Modelo de objetos de representación es una colección de clases que definen el contenido, diseño y datos de un informe que se ha procesado. El ROM está disponible para los programadores que desean diseñar, desarrollar e implementar las extensiones de representación personalizadas para [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. ROM se genera cuando el servidor de informes procesa la definición XML de un informe junto con los datos del mismo definidos por el usuario. Cuando el procesamiento se completa, una extensión de representación utiliza el modelo de objetos público para definir la salida del informe. Las clases públicas disponibles del ROM se definen en el espacio de nombres **Microsoft.ReportingServices.OnDemandReportRendering**.  
  
## <a name="writing-custom-rendering-extensions"></a>Escribir extensiones de representación personalizadas  
 Antes de decidir crear una extensión de representación personalizada, debería evaluar alternativas más simples. Puede:  
  
-   Personalizar la salida representada especificando la configuración de la información de los dispositivos para las extensiones existentes.  
  
-   Agregar formato personalizado y características de presentación combinando Transformaciones XSL (XSLT) con la salida del formato de representación XML.  
  
 Escribir una extensión de representación personalizada es difícil. Normalmente, debe admitir todas las combinaciones posibles de elementos de informe y requiere que implemente centenares de clases, interfaces, métodos y propiedades. Si debe representar un informe en un formato que no está incluido con [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] y decide escribir su propia implementación de código administrado de una extensión de representación, el código de la extensión de representación debe implementar la interfaz **Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension**, que requiere el servidor de informes.  
  
## <a name="see-also"></a>Consulte también  
 [Implementar una extensión de representación](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
