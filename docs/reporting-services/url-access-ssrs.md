---
title: Acceso URL | Microsoft Docs
description: Obtenga información sobre cómo el acceso URL del servidor de informes de SQL Server Reporting Services (SSRS) permite enviar comandos a un servidor de informes a través de una solicitud URL.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- URL access [Reporting Services]
- links [Reporting Services], URL access
- URL access [Reporting Services], about URL access
- parameters [Reporting Services], URL access
- report servers [Reporting Services], URL access
- hyperlinks [Reporting Services]
ms.assetid: 52c3f2a3-3d6d-4fee-9c46-83f366919398
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 54c41a9b4a735d5136cf86e805836305b2031bb7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078906"
---
# <a name="url-access-ssrs"></a>Acceso URL (SSRS)
  El acceso URL del servidor de informes de SQL Server Reporting Services (SSRS) permite enviar comandos a un servidor de informes a través de una solicitud URL. Por ejemplo, puede personalizar la representación de un informe en una biblioteca de SharePoint o un servidor de informes en modo nativo. Es posible que haya visto el informe usando un conjunto específico de valores de parámetro de informe o tal vez haya consultado una determina página de su interés. Puede encapsular esta información en la dirección URL usando los parámetros de acceso URL predefinidos. Puede personalizar más el modo en el que el servidor de informes procesa el informe incorporando parámetros sobre los formatos de representación o sobre la apariencia del visor de informes. Puede pegar esta dirección URL directamente en un mensaje de correo electrónico o una página web para permitir que otras personas tengan acceso al informe del mismo modo en el explorador.  
  
 Otras acciones que puede realizar con el acceso URL son:  
  
-   Enviar comandos al visor HTML para, por ejemplo, ajustar su apariencia.  
  
-   Mostrar la lista de elementos secundarios de una carpeta de catálogos.  
  
-   Recuperar la definición XML de un elemento de catálogo.  
  
-   Representar una instantánea específica del historial de informes.  
  
-   Administrar sesiones del informe.  
  
 Para consultar la lista completa de comandos y opciones de configuración disponibles con el acceso URL, vea [Referencia de parámetros de acceso URL](../reporting-services/url-access-parameter-reference.md).  
  
## <a name="url-access-concepts"></a>Conceptos del acceso URL  
 Las solicitudes URL que se dirigen a un servidor de informes contienen parámetros que se procesan en el servidor de informes. La manera en la que el servidor de informes administra las solicitudes URL depende de los parámetros, prefijos de parámetro y tipos de elementos que están incluidos en la dirección URL. Las direcciones URL del servidor de informes se rigen por las instrucciones de formato de dirección URL propuestas por el estándar de borrador W3C/IETF del World Wide Web Consortium. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] es compatible con la mayor parte de los exploradores de Internet o aplicaciones que admiten el direccionamiento con direcciones URL estándar.  
  
### <a name="url-access-syntax"></a>Sintaxis del acceso URL  
 Las solicitudes URL pueden contener varios parámetros que se muestran en cualquier orden. Los parámetros se separan mediante un símbolo de Y comercial (&), y los pares de nombre y valor se separan con un signo igual (=).  
  
```  
  
rswebserviceurl  
?  
reportpath  
      [&prefix:param=value]...n]  
  
```  
  
### <a name="syntax-description"></a>Descripción de la sintaxis  
 *rswebserviceurl*  
 Dirección URL del servicio web del servidor de informes. Para el modo nativo, es la dirección URL del servicio web de la instancia del servidor de informes configurada en el Administrador de configuración de Reporting Services (vea [Configurar las direcciones URL del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)). Por ejemplo:  
  
```  
https://myrshost/reportserver  
https://machine.adventure-works.com/reportserver_MYNAMEDINSTANCE  
```  
  
 En el modo integrado de SharePoint, es la dirección URL del proxy de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] de un sitio de SharePoint integrado con [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. Por ejemplo:  
  
```  
https://myspsite/subsite/_vti_bin/reportserver  
```  
  
> [!TIP]  
>  Es importante que la dirección URL incluya la sintaxis de proxy de `_vti_bin` para enrutar la solicitud a través de SharePoint y el proxy HTTP de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . El proxy agrega algún contexto a la solicitud HTTP, contexto que es necesario para garantizar la correcta ejecución del informe para los servidores de informes de modo de SharePoint.  
  
 *pathinfo*  
 Nombre de la ruta de acceso relativa del elemento en la base de datos del servidor de informes en modo nativo o dirección URL completa del elemento en un catálogo de SharePoint.  
  
 Ruta de acceso del elemento del catálogo. En modo nativo, es la ruta de acceso relativa del elemento de la base de datos del servidor de informes, que empieza por una barra diagonal ( **/** ). Por ejemplo:  
  
```  
/AdventureWorks 2008R2/Employee_Sales_Summary_2008R2  
```  
  
 En el modo integrado de SharePoint, es la dirección URL completa del elemento de la biblioteca de SharePoint, incluida la extensión del elemento. Por ejemplo:  
  
```  
https://myspsite/subsite/AdventureWorks 2008R2/Employee_Sales_Summary_2008R2.rdl  
```  
  
 **&**  
 Se usa para separar los pares de nombre y valor de los parámetros de acceso URL.  
  
 **prefijo**  
 Opcional. Prefijo del parámetro de acceso URL (por ejemplo, `rs:` o `rc:`) que accede a un proceso concreto que se ejecuta en el servidor de informes.  
  
> [!NOTE]  
>  Si no se incluye un parámetro de acceso URL, el parámetro se procesa en el servidor de informes como un parámetro de informe. Los parámetros de informe no usan un prefijo de parámetro y distinguen entre mayúsculas y minúsculas.  
  
 **param**  
 El nombre del parámetro.  
  
 *value*  
 Texto de la dirección URL que corresponde al valor del parámetro que se va a usar.  
  
 **Nota:** Para obtener una lista de los parámetros de acceso URL disponibles, vea [Referencia de parámetros de acceso URL](../reporting-services/url-access-parameter-reference.md). Para obtener ejemplos de cómo pasar parámetros de informe en la dirección URL, vea [Pass a Report Parameter Within a URL](../reporting-services/pass-a-report-parameter-within-a-url.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripciones de las tareas|Vínculos|  
|-----------------------|-----------|  
|Acceder a los elementos del servidor de informes, como informes, orígenes de datos compartidos y recursos.|[Acceso a elementos del servidor de informes mediante el acceso URL](../reporting-services/access-report-server-items-using-url-access.md)|  
|Pasar parámetros de informe a un informe.|[Paso de un parámetro de informe en una dirección URL](../reporting-services/pass-a-report-parameter-within-a-url.md)|  
|Establecer la configuración regional de los parámetros de informe de la cadena de acceso URL, que define las interpretaciones de las fechas, divisas, etc. específicas de la configuración regional.|[Establecimiento del idioma para los parámetros de informe en una dirección URL](../reporting-services/set-the-language-for-report-parameters-in-a-url.md)|  
|Enviar valores específicos para la extensión de representación que personalicen cómo se va a representar el informe.|[Especificación de la configuración de la información del dispositivo en una dirección URL](../reporting-services/specify-device-information-settings-in-a-url.md)|  
|Exportar un informe directamente a un formato de archivo sin verlo en el explorador.|[Exportación de un informe con el acceso URL](../reporting-services/export-a-report-using-url-access.md)|  
|Abrir un informe y navegar directamente a la ubicación de una cadena.|[Búsqueda de un informe con el acceso URL](../reporting-services/search-a-report-using-url-access.md)|  
|Representar una instantánea específica del historial de informes.|[Representación de una instantánea del historial de informes con el acceso URL](../reporting-services/render-a-report-history-snapshot-using-url-access.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Pass a Report Parameter Within a URL](../reporting-services/pass-a-report-parameter-within-a-url.md)   
 [Referencia de parámetros de acceso URL](../reporting-services/url-access-parameter-reference.md)   
 [Integración de Reporting Services con el acceso URL](../reporting-services/application-integration/integrating-reporting-services-using-url-access.md)   
 [Buscar, ver y administrar informes &#40;Generador de informes y SSRS&#41;](../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)  
  
  
