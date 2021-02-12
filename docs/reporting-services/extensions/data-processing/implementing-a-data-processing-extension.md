---
title: Implementar una extensión de procesamiento de datos | Microsoft Docs
description: Obtenga información sobre cómo crear un puente entre un origen de datos y un conjunto de datos en Reporting Services mediante la implementación de una extensión de procesamiento de datos.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- custom data processing extensions [Reporting Services]
- data sources [Reporting Services], data processing extensions
- data processing extensions [Reporting Services]
- extensions [Reporting Services], data processing
ms.assetid: 8dc2b44e-5ad9-411d-a29f-7213e29321a9
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f4d4102c44cfedd25ed6a9870a303ebafb2515cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065413"
---
# <a name="implementing-a-data-processing-extension"></a>Implementar una extensión de procesamiento de datos
  Las extensiones de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] permiten conectarse a los orígenes de datos y recuperar los datos. También actúan como puente entre un origen de datos y un conjunto de datos. Las extensiones de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] se modelan según un subconjunto de las interfaces del proveedor de datos de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].  
  
## <a name="in-this-section"></a>En esta sección  
 [Introducción a las extensiones de procesamiento de datos](../../../reporting-services/extensions/data-processing/data-processing-extensions-overview.md)  
 Explica cómo escribir una extensión de procesamiento de datos personalizada para [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 [Preparación de la implementación de una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/preparing-to-implement-a-data-processing-extension.md)  
 Describe las interfaces disponibles al implementar una extensión de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], así como cuando tiene que implementar una interfaz determinada.  
  
 [Creación de una biblioteca de extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/creating-a-data-processing-extension-library.md)  
 Describe cómo asignar un espacio de nombres para la extensión de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] y compilar la extensión de procesamiento de datos en una DLL de biblioteca.  
  
 [Implementación de una clase Connection para una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/implementing-a-connection-class-for-a-data-processing-extension.md)  
 Describe los atributos de una conexión y cómo implementar una clase propia **Connection** para la extensión de procesamiento de datos.  
  
 [Implementación de una clase Command para una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/implementing-a-command-class-for-a-data-processing-extension.md)  
 Describe los atributos de un comando y cómo implementar una clase propia **Command** para la extensión de procesamiento de datos.  
  
 [Implementación de una clase DataReader para una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/implementing-a-datareader-class-for-a-data-processing-extension.md)  
 Describe los atributos de un lector de datos y cómo implementar una clase propia **DataReader** para la extensión de procesamiento de datos.  
  
 [Uso de un conjunto de datos externo con Reporting Services](../../../reporting-services/extensions/data-processing/using-an-external-dataset-with-reporting-services.md)  
 Describe cómo exponer los objetos **DataSet** personalizados en el servidor de informes para su uso.  
  
 [Implementación de una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)  
 Describe cómo implementar la extensión de procesamiento de datos.  
  
 [Depuración del código de extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/debugging-data-processing-extension-code.md)  
 Describe cómo depurar el código de las extensiones de procesamiento de datos.  
  
 Para obtener un ejemplo de una extensión de procesamiento de datos totalmente implementada, vea [Ejemplos del producto SQL Server Reporting Services](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
## <a name="see-also"></a>Consulte también  
 [Extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
