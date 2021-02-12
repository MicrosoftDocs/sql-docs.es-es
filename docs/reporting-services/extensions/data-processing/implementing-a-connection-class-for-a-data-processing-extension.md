---
title: Implementar una clase Connection para una extensión de procesamiento de datos | Microsoft Docs
description: Implemente un objeto Connection para una extensión de procesamiento de datos en Reporting Services. Vea qué interfaces implementar y qué exigir a los clientes.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- connections [Reporting Services], data processing extensions
- Connection class
- data processing extensions [Reporting Services], connections
ms.assetid: 7047d29e-a2c9-4e6f-ad02-635851a38ed7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d5b0c6cc93ea6b934be718e88873cf736f31d679
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061700"
---
# <a name="implementing-a-connection-class-for-a-data-processing-extension"></a>Implementar una clase Connection para una extensión de procesamiento de datos
  El objeto **Connection** representa una conexión a bases de datos o un recurso similar, y es el punto inicial para los usuarios de una extensión de procesamiento de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Representa las conexiones a los servidores de bases de datos, aunque cualquier entidad con un comportamiento similar se puede exponer como **Connection**.  
  
 Para implementar un objeto **Connection**, cree una clase que implemente <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> y que implemente opcionalmente <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>.  
  
 En la implementación debe asegurarse de que se crea y se abre una conexión para que se puedan ejecutar los comandos. Asegúrese de que la implementación requiere que los clientes abran y cierren las conexiones explícitamente, en lugar de hacer que abra y cierre las conexiones para el cliente de forma implícita. Realice comprobaciones de seguridad cuando se obtenga la conexión. Al requerir una conexión existente para las otras clases en la extensión de procesamiento de datos de [!INCLUDE[ssRS](../../../includes/ssrs.md)], se asegurará de que las comprobaciones de seguridad se realicen siempre al trabajar con el origen de datos.  
  
 Las propiedades de la conexión deseada se representan como una cadena de conexión. Se recomienda encarecidamente que las extensiones de procesamiento de datos de [!INCLUDE[ssRS](../../../includes/ssrs.md)] admitan la propiedad <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection.ConnectionString%2A> mediante el conocido sistema de par de nombre y valor definido por OLE DB.  
  
> [!NOTE]  
>  Para obtener objetos **Connection**, a menudo se necesitan muchos recursos, de modo que puede ser conveniente que considere agrupar las conexiones u otras técnicas para mitigar esto.  
  
 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> hereda de <xref:Microsoft.ReportingServices.Interfaces.IExtension>. Debe implementar la interfaz <xref:Microsoft.ReportingServices.Interfaces.IExtension> como parte de la implementación de la clase de conexión. La interfaz <xref:Microsoft.ReportingServices.Interfaces.IExtension> permite a una clase implementar un nombre de extensión localizado y procesar la información de configuración específica de la extensión que se almacena en el archivo de configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 El objeto **Connection** contiene la propiedad <xref:Microsoft.ReportingServices.Interfaces.IExtension.LocalizedName%2A> a través de su implementación de <xref:Microsoft.ReportingServices.Interfaces.IExtension>. Se recomienda encarecidamente que las extensiones de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] admitan la propiedad <xref:Microsoft.ReportingServices.Interfaces.IExtension.LocalizedName%2A>, de forma que los usuarios encuentren un nombre traducido y conocido para la extensión en una interfaz de usuario, como el Administrador de informes.  
  
 <xref:Microsoft.ReportingServices.Interfaces.IExtension> también permite que el objeto **Connection** recupere y procese los datos de configuración personalizados que se almacenan en el archivo RSReportServer.config. Para obtener más información acerca de cómo procesar los datos de configuración personalizados, vea el método <xref:Microsoft.ReportingServices.Interfaces.IExtension.SetConfiguration%2A>.  
  
 La clase que implementa <xref:Microsoft.ReportingServices.Interfaces.IExtension> no se descarga de la memoria cuando se descarga el resto de las clases de extensión de procesamiento de datos. Por ello, puede utilizar la clase **Extension** con el fin de almacenar información de estado a través de las conexiones o almacenar datos que pueden estar almacenados en la memoria caché. La clase **Extension** permanece en la memoria siempre que se ejecuta el servidor de informes.  
  
 Puede extender la clase **Connection** para incluir compatibilidad con las credenciales de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] implementando <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>. Al implementar las propiedades <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.IntegratedSecurity%2A>, <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.UserName%2A> y <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension.Password%2A> de la interfaz <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>, habilita la casilla **Seguridad integrada** y los cuadros de texto **NombreDeUsuario** y **Contraseña** del cuadro de diálogo **Origen de datos** del Diseñador de informes. Esto permite al Diseñador de informes almacenar y recuperar las credenciales de los orígenes de datos que admitan la autenticación. Las credenciales se almacenan seguras y se utilizan al representar los informes en el modo de vista previa.  
  
> [!NOTE]  
>  Implementar de forma implícita <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension> le exige que implemente los miembros de las interfaces <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> y <xref:Microsoft.ReportingServices.Interfaces.IExtension>.  
>   
>  Para obtener una muestra de la implementación de la clase **Connection**, vea [Muestras de productos de SQL Server Reporting Services](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
## <a name="see-also"></a>Consulte también  
 [Extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementar una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
