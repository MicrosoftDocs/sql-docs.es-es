---
title: Implementar una extensión de procesamiento de datos | Microsoft Docs
description: Obtenga información sobre cómo hacer que el servidor de informes y el Diseñador de informes puedan detectar la extensión de procesamiento de datos de Reporting Services.
ms.date: 03/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], deploying
- Extension element
- deploying [Reporting Services], extensions
ms.assetid: e5c0b5a9-1386-47cb-aade-96653ecfaa54
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: df78278f655d6a11bb560440f86d662c08889059
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061160"
---
# <a name="deploying-a-data-processing-extension"></a>Implementar una extensión de procesamiento de datos
  Después de escribir y compilar la extensión de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] en una biblioteca de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], es necesario hacer que el servidor de informes y el Diseñador de informes la puedan detectar. Esto es tan fácil como copiar la extensión en los directorios adecuados y agregar entradas a los archivos de configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] correspondientes.  
  
## <a name="configuration-file-extension-element"></a>Elemento de extension de archivos de configuración  
 Las extensiones de procesamiento de datos que implementa en el servidor de informes o el Diseñador de informes tienen que escribirse como elementos **Extension** en los archivos de configuración. Estos archivos son RSReportServer.config para el servidor de informes y RSReportDesigner.config para el Diseñador de informes.  
  
 En la tabla siguiente se describen los atributos del elemento **Extension** correspondientes a las extensiones de procesamiento de datos.  
  
|Atributo|Descripción|  
|---------------|-----------------|  
|**Nombre**|Un nombre único para la extensión, por ejemplo "SQL" para la extensión de procesamiento de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] u "OLEDB" para la extensión de procesamiento de datos de OLE DB. La longitud máxima para el atributo **Name** es de 255 caracteres. El nombre debe ser único entre todas las entradas en el elemento **Extension** del archivo de configuración.|  
|**Tipo**|Lista separada por comas que incluye el espacio de nombres completo junto con el nombre del ensamblado.|  
|**Visible**|El valor **false** indica que la extensión de procesamiento de datos no debería ser visible en las interfaces de usuario. Si el atributo no está incluido, el valor predeterminado es **true**.|  
  
 Para más información sobre los archivos RSReportServer.config o RSReportDesigner.config, vea [Archivos de configuración de Reporting Services](../../../reporting-services/report-server/reporting-services-configuration-files.md).  
  
## <a name="in-this-section"></a>En esta sección  
  
|Tema|Descripción|  
|-----------|-----------------|  
|[Cómo: Implementar una extensión de procesamiento de datos en un servidor de informes](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension-to-a-report-server.md)|Describe cómo implementar la extensión de procesamiento de datos para un servidor de informes.|  
|[Cómo: Implementar una extensión de procesamiento de datos en el Diseñador de informes](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension-to-report-designer.md)|Describe cómo implementar la extensión de procesamiento de datos para el Diseñador de informes.|  
  
## <a name="see-also"></a>Consulte también  
 [Extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementar una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
