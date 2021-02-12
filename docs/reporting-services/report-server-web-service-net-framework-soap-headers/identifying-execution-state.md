---
title: Identificar el estado de ejecución | Microsoft Docs
description: Aprenda a usar Reporting Services para identificar el estado de ejecución de forma que pueda interactuar de varias maneras con el informe.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service-net-framework-soap-headers
ms.topic: reference
helpviewer_keywords:
- session states [Reporting Services]
- lifetimes [Reporting Services]
- sessions [Reporting Services]
- SessionHeader SOAP header
ms.assetid: d8143a4b-08a1-4c38-9d00-8e50818ee380
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: fbd189a85121d2ef18c69a1d7886f6b3c3294978
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045065"
---
# <a name="identifying-execution-state"></a>Identificar el estado de ejecución
  El Protocolo de transferencia de hipertexto (HTTP) es un protocolo sin estado y sin conexión, lo que significa que no indica de manera automática si las diferentes solicitudes provienen del mismo cliente o si una instancia del explorador todavía está viendo una página o un sitio. Las sesiones crean una conexión lógica para mantener el estado entre el servidor y el cliente sobre HTTP. La información específica del usuario pertinente para una sesión determinada se conoce como estado de sesión.  
  
 La administración de la sesión implica poner en correlación una solicitud HTTP con otras solicitudes anteriores generadas en la misma sesión. Sin la administración de sesiones, estas solicitudes parecen no estar relacionadas con el servicio web del servidor de informes debido a la naturaleza sin conexión y sin estado del protocolo HTTP.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no expone un concepto holístico del estado de sesión como el expuesto a través de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Sin embargo, al ejecutar los informes, el servidor de informes mantiene el estado entre las llamadas al método en forma de una **ejecución**. Una ejecución permite al usuario interactuar con el informe de varias maneras, por ejemplo al cargar el informe del servidor de informes, establecer las credenciales y los parámetros para el informe, y representar el informe.  
  
 Mientras se comunican con un servidor de informes, los clientes utilizan la ejecución para administrar la visualización de los informes y la navegación del usuario a otras páginas de un informe, y para mostrar u ocultar las secciones de un informe. Existe solo una ejecución para cada informe que la aplicación cliente ejecuta.  
  
 En general, la duración de una ejecución se inicia cuando un usuario navega a un explorador o aplicación cliente y selecciona un informe para verlo. La ejecución se descarta después de un breve período de tiempo de espera, una vez recibida la última solicitud de ejecución (el tiempo de espera predeterminado es de 20 minutos).  
  
 Desde la perspectiva del servicio web, la duración comienza cuando se llama a los métodos <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A>, <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> o <xref:ReportExecution2005.ReportExecutionService.Render%2A> del servicio web del servidor de informes. La aplicación puede utilizar otros métodos para manipular la ejecución activa (por ejemplo, configurar los parámetros y establecer los orígenes de datos). La ejecución se descarta después de un breve período de tiempo de espera, una vez recibida la última solicitud de ejecución (el tiempo de espera predeterminado es de 20 minutos).  
  
 Para mantener el seguimiento de varias ejecuciones activas entre las llamadas a los métodos del servicio web <xref:ReportExecution2005.ReportExecutionService.Render%2A> y <xref:ReportExecution2005.ReportExecutionService.RenderStream%2A>, una aplicación guarda el <xref:ReportExecution2005.ExecutionHeader.ExecutionID%2A>, que los métodos <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> y <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A> devuelven en el encabezado SOAP.  
  
 El diagrama siguiente muestra el procesamiento y la representación de la ruta de acceso para los informes.  
  
 ![Ruta de representación o procesamiento de informe](../../reporting-services/report-server-web-service-net-framework-soap-headers/media/rs-render-process-diagram.gif "Ruta de representación o procesamiento de informe")  
  
 Para admitir las funciones descritas antes, el método SOAP Render actual se ha dividido en varios métodos que abarcan las fases de inicialización de la ejecución, procesamiento y representación.  
  
 Para representar mediante programación un informe, debe:  
  
-   Cargar el informe o la definición de informe utilizando <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> o <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A>.  
  
-   Comprobar si el informe necesita credenciales o parámetros examinando los valores de las propiedades <xref:ReportExecution2005.ExecutionInfo.CredentialsRequired%2A> y <xref:ReportExecution2005.ExecutionInfo.ParametersRequired%2A> del objeto <xref:ReportExecution2005.ExecutionInfo> que devuelve <xref:ReportExecution2005.ReportExecutionService.LoadReport%2A> o <xref:ReportExecution2005.ReportExecutionService.LoadReportDefinition%2A>  
  
-   Si es necesario, establecer las credenciales y/o parámetros mediante los métodos <xref:ReportExecution2005.ReportExecutionService.SetExecutionCredentials%2A> y <xref:ReportExecution2005.ReportExecutionService.SetExecutionParameters%2A>.  
  
-   Llamar al método <xref:ReportExecution2005.ReportExecutionService.Render%2A> para representar el informe.  
  
 Aunque un informe esté en sesión, el informe subyacente almacenado en la base de datos del servidor de informes puede cambiar. Por ejemplo, la definición de informe puede cambiar, el informe se puede eliminar o mover, y los permisos de usuario pueden cambiar. Si el informe está en una sesión activa, los cambios realizados no afectan al informe subyacente (es decir, el informe almacenado en la base de datos del servidor de informes).  
  
 También puede administrar una sesión de informe mediante comandos de acceso URL.  
  
## <a name="see-also"></a>Consulte también  
 <xref:ReportExecution2005.ReportExecutionService.Render%2A>   
 [Referencia técnica &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)   
 [Uso de encabezados SOAP de Reporting Services](../../reporting-services/report-server-web-service-net-framework-soap-headers/using-reporting-services-soap-headers.md)  
  
  
