---
title: Propiedad Detail | Microsoft Docs
description: Obtenga información sobre la propiedad Detail de la clase SoapException de Reporting Services, en concreto sobre los elementos XML que definen la propiedad.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-exception-handling
ms.topic: reference
helpviewer_keywords:
- Detail property
- SoapException class
ms.assetid: c1ddaeb6-c540-49fa-b06e-b6359d377ee8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 05b4c4f6715e35923c8517983886c5468f34d6ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100021707"
---
# <a name="detail-property"></a>Propiedad Detail
  La propiedad **Detail** de la clase **SoapException** de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] tiene la estructura XML siguiente:  
  
## <a name="elements"></a>Elementos  
 **Detail**  
 Elemento de nivel superior que contiene todos los demás elementos de detalle de error.  
  
 **ErrorCode**  
 Código de error específico de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 **HttpStatus**  
 El código de estado HTTP.  
  
 **Mensaje**  
 Mensaje de error y código de error asignados por el servidor de informes.  
  
 **HelpLink**  
 Dirección URL del vínculo de Ayuda a un sitio web en el que se puede encontrar más información sobre el error. Para más información, vea [Elemento HelpLink](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/helplink-element.md).  
  
 **LinkID**  
 Identificador asignado al vínculo.  
  
 **ProductName**  
 Nombre del producto. El valor predeterminado es **Microsoft SQL Server Reporting Services**.  
  
 **ProductVersion**  
 Versión de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. La longitud máxima es de 15 caracteres. El formato del número de versión debería ser como sigue: 8.00.0xxx.00.  
  
 **ProductLocaleId**  
 Identificador de configuración regional o de idioma de la DLL INTL de la aplicación (por ejemplo, 0x41A).  
  
 **OperatingSystem**  
 Sistema operativo en el que está instalado [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Los valores válidos incluyen **0** para un sistema operativo independiente, **1** para [!INCLUDE[win2kfamily](../../../includes/win2kfamily-md.md)] y **16** para Windows XP.  
  
 **CountryLocaleId**  
 Identificador de configuración regional o de idioma del sistema operativo. Por ejemplo, el valor correspondiente a la versión en francés de Windows es 0x040c.  
  
 **MoreInformation**  
 Cadena XML que contiene las excepciones anidadas que se produjeron mientras el método se ejecutaba.  
  
 **Origen**  
 Elemento secundario de **MoreInformation**. Origen del error.  
  
 **Mensaje**  
 Elemento secundario de **MoreInformation**. Mensaje de error de una excepción anidada. Este elemento incluye atributos XML para **ErrorCode** y **HelpLink**.  
  
 **Advertencias**  
 Cadena XML que contiene las advertencias que se devolvieron al procesar el informe.  
  
## <a name="see-also"></a>Consulte también  
 [Introducción a la administración de excepciones en Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/introducing-exception-handling-in-reporting-services.md)   
 [Clase SoapException de Reporting Services](../../../reporting-services/report-server-web-service-net-framework-exception-handling/soapexception-class/reporting-services-soapexception-class.md)   
 [Uso de la propiedad Detail para administrar errores concretos](../../../reporting-services/report-server-web-service-net-framework-exception-handling/best-practices/using-the-detail-property-to-handle-specific-errors.md)  
  
  
