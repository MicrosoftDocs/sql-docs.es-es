---
title: Especificar conexiones para extensiones de procesamiento de datos personalizadas | Microsoft Docs
description: Use esta información para aprender a especificar conexiones para extensiones de procesamiento de datos personalizadas de terceros.
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
helpviewer_keywords:
- custom data processing extensions [Reporting Services]
- IDbConnection interface, connection strings
- impersonation [Reporting Services]
- IDbConnectionExtension interface, connection strings
- data sources [Reporting Services], security
- connection strings [Reporting Services]
- credentials [Reporting Services]
- authentication [Reporting Services]
- external data sources [Reporting Services]
- data processing extensions [Reporting Services], connections
ms.assetid: 2cddc9ea-0e28-4350-80ae-332412908e47
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dbfb5f58cae73931acf1c856b7e2675d50c8dc47
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934655"
---
# <a name="specify-connections-for-custom-data-processing-extensions"></a>Especificar conexiones para extensiones de procesamiento de datos personalizadas
  Puede crear o usar extensiones de procesamiento de datos personalizadas de otros fabricantes en un servidor de informes con el fin de mejorar la capacidad de procesamiento de datos de orígenes de datos admitidos o proporcionar compatibilidad con orígenes de datos adicionales que no estén disponibles en una instalación predeterminada de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Las conexiones se tratan de forma diferente en función de la implementación. Las implementaciones siguientes están disponibles para extensiones de procesamiento de datos:  
  
-   Proveedores de datos personalizados de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] (si tiene acceso a datos de orígenes de datos DB2.NET, Oracle, ODP.NET o Teradata, puede que use un proveedor de datos .NET personalizado)  
  
-   Extensiones de procesamiento de datos personalizadas compatibles con <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection>  
  
-   Extensiones de procesamiento de datos personalizadas compatibles con <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>  
  
> [!NOTE]  
>  Consulte a su proveedor cómo se implementa la extensión de procesamiento de datos personalizada.  
  
## <a name="impersonation-and-custom-data-processing-extensions"></a>Suplantación y extensiones de procesamiento de datos personalizadas  
 Si una extensión de procesamiento de datos personalizada se conecta a orígenes de datos con suplantación, tendrá que usar el método Open en las interfaces <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> o <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension> para realizar la solicitud. Como alternativa, puede almacenar el objeto de identidad de usuario (System.Security.Principal.WindowsIdentity) y, después, reutilizarlo en el resto de las API de extensión de procesamiento de datos.  
  
 En versiones anteriores de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], se llamaba a todas las extensiones de procesamiento de datos personalizadas mediante la suplantación de usuarios. En esta versión, la suplantación de usuarios solo se usa para llamar al método Open. Si tiene una extensión de procesamiento de datos que necesite seguridad integrada, es necesario que modifique el código para que use el método Open o que almacene el objeto de identidad de usuario.  
  
## <a name="connections-for-custom-net-framework-data-providers"></a>Conexiones para proveedores de datos personalizados de .NET Framework  
 Al configurar un informe para que utilice un origen de datos concreto, se establecen propiedades que determinan el tipo de origen de datos, la cadena de conexión y las credenciales que se utilizarán para tener acceso al origen de datos. La tabla siguiente describe los tipos de credenciales compatibles con proveedores de datos de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] . Para más información sobre cómo configurar propiedades de orígenes de datos de informe, vea [Especificar información de credenciales y conexión para los orígenes de datos de informes](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
|Credenciales|Conexiones|  
|-----------------|-----------------|  
|Seguridad integrada|Si su proveedor de datos lo admite, puede utilizar la seguridad integrada de Windows. La solicitud se envía utilizando las credenciales del usuario actual.<br /><br /> Al definir la cadena de conexión, asegúrese de usar argumentos que especifiquen la seguridad integrada (por ejemplo, en una conexión a un origen de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría incluirse **Integrated Security=SSPI** en la cadena de conexión).|  
|Autenticación de Windows|Si su proveedor de datos lo admite, puede utilizar una cuenta de usuario de dominio de Windows. El servidor de informes suplantará la cuenta de usuario antes de que se llame a la extensión de procesamiento de datos.<br /><br /> Al definir la cadena de conexión, asegúrese de usar argumentos que especifiquen la seguridad integrada (por ejemplo, en una conexión a un origen de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría incluirse **Integrated Security=SSPI** en la cadena de conexión).|  
|Credenciales de base de datos|La autenticación de base de datos no admite conexiones realizadas a través de un proveedor de datos personalizado de .NET. El servidor de informes generará un error de conexión en todos los casos.|  
|Sin credenciales|Puede utilizar la opción Sin credenciales con proveedores de datos personalizados de .NET. Si especifica una cuenta de ejecución desatendida, la cadena de conexión determinará las credenciales que se utilizarán. El servidor de informes suplantará la cuenta de ejecución desatendida para realizar la conexión.<br /><br /> Si no se ha definido la cuenta de ejecución desatendida, el servidor de informes generará un error de conexión. Para más información sobre cómo definir la cuenta, vea [Configurar la cuenta de ejecución desatendida &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).|  
  
## <a name="connections-for-idbconnection"></a>Conexiones para IDbConnection  
 Si usa una extensión de procesamiento de datos personalizada que solo admita <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection>, tiene que especificar la conexión del modo siguiente:  
  
1.  Configurar la cuenta de ejecución desatendida La configuración de esta cuenta es necesaria para las conexiones realizadas mediante **IDbConnection**. El servidor de informes suplantará la cuenta al realizar la conexión.  
  
2.  Configure las propiedades de orígenes de datos del informe para utilizar **Sin credenciales**.  
  
3.  Incluya las credenciales utilizadas para conectarse al origen de datos en la cadena de conexión.  
  
 Al usar **IDbConnection**, no se admiten los tipos de credencial siguientes: la seguridad integrada, las cuentas de usuario de Windows y las credenciales de la base de datos. Si alguna conexión del origen de datos utiliza estas opciones, generará error en el servidor de informes.  
  
## <a name="connections-for-idbconnectionextension"></a>Conexiones para IDbConnectionExtension  
 Si usa una extensión de procesamiento de datos personalizada compatible con <xref:Microsoft.ReportingServices.DataProcessing.IDbConnectionExtension>, puede especificar la conexión de las formas siguientes:  
  
|Credenciales|Conexiones|  
|-----------------|-----------------|  
|Seguridad integrada|Si su proveedor de datos lo admite, puede utilizar la seguridad integrada de Windows con extensiones de procesamiento de datos personalizadas que utilicen **IDbConnectionExtension**.<br /><br /> Al definir la cadena de conexión, asegúrese de usar argumentos que especifiquen la seguridad integrada (por ejemplo, en una conexión a un origen de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría incluirse **Integrated Security=SSPI** en la cadena de conexión).|  
|Autenticación de Windows|Si su proveedor de datos lo admite, puede utilizar una cuenta de usuario de dominio de Windows para extensiones de procesamiento de datos personalizadas que utilicen **IDbConnectionExtension**.<br /><br /> El servidor de informes suplantará la cuenta de usuario antes de que se llame a la extensión de procesamiento de datos. Al definir la cadena de conexión, asegúrese de usar argumentos que especifiquen la seguridad integrada (por ejemplo, en una conexión a un origen de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría incluirse **Integrated Security=SSPI** en la cadena de conexión).|  
|Credenciales de base de datos|Puede utilizar la autenticación de base de datos para configurar conexiones para extensiones de procesamiento de datos personalizadas que utilicen **IDbConnectionExtension**.|  
|Sin credenciales|Si especifica una cuenta de ejecución desatendida, la cadena de conexión determinará las credenciales que se utilizarán.<br /><br /> Si no se ha definido la cuenta de ejecución desatendida, el servidor de informes generará un error de conexión.|  
  
## <a name="see-also"></a>Consulte también  
 [Configurar la cuenta de ejecución desatendida &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)   
 [Especificar información de credenciales y conexión para los orígenes de datos de informes](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
 [Creación de cadenas de conexión de datos - Generador de informes y SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [Implementar una extensión de procesamiento de datos](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Configuración de propiedades de origen de datos para un informe](../../reporting-services/report-data/configure-data-source-properties-for-a-report-report-manager.md)  
  
  
