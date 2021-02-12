---
title: Usar los archivos de directivas de seguridad de Reporting Services | Microsoft Docs
description: Obtenga información sobre los archivos de configuración que usa Reporting Services para almacenar información de la directiva de seguridad para los ensamblados de código. Descubra cómo modificarlos.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- code groups [Reporting Services]
- CodeGroup elements
- configuration files [Reporting Services]
- code access security [Reporting Services], security policies
- security policies [Reporting Services]
- security configuration files [Reporting Services]
- named permission sets [Reporting Services]
ms.assetid: 2280fff6-3de7-44b1-87da-5db0ec975928
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e5f82b3db5f52656e88c54062f203d273d8ab24c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065270"
---
# <a name="using-reporting-services-security-policy-files"></a>Usar los archivos de directivas de seguridad de Reporting Services
  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] almacena la información de los componentes de directiva de seguridad en tres archivos de configuración que se copian en el sistema de archivos durante la instalación. Estos archivos de configuración pueden contener una combinación de uso interno y las directivas de seguridad definidas por el usuario para los ensamblados de código en [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Los tres archivos de configuración corresponden a tres componentes protegibles de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]: el servidor de informes y el servicio de Windows, la aplicación web del Administrador de informes y la ventana de vista previa del Diseñador de Informes.  
  
> [!NOTE]  
>  Hay dos modos de vista previa para el Diseñador de informes: la pestaña de vista previa y la ventana de vista previa emergente que se inician cuando se inicia su Proyecto de informe en modo **DebugLocal**. La pestaña **Vista previa** no es un componente protegible y no aplica la configuración de directiva de seguridad. La ventana de vista previa está pensada para simular la funcionalidad del servidor de informes y por consiguiente, tiene un archivo de configuración de directiva que usted o un administrador debe modificar para utilizar ensamblados y extensiones personalizadas en el Diseñador de informes.  
  
 Los archivos de configuración de directivas de seguridad contienen información de clase de seguridad, algunos conjuntos de permisos con nombre predeterminados y los grupos de código para los ensamblados en [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Los archivos de configuración de directivas de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] son similares al archivo Security.config que determina los conjuntos de permisos y la jerarquía de grupos de código asociados a las directivas de nivel de equipos y de empresas de [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. La ubicación de este archivo es C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\CONFIG\security.config.  
  
## <a name="policy-files-in-reporting-services"></a>Archivos de directivas en Reporting Services  
 La tabla siguiente contiene una lista de los archivos de configuración de directivas en [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], sus ubicaciones (para una instalación predeterminada) y sus funciones respectivas.  
  
|Nombre de archivo|Ubicación (instalación predeterminada)|Descripción|  
|---------------|---------------------------------------|-----------------|  
|rssrvpolicy.config|C:\Archivos de programa\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer|El archivo de configuración de directivas del servidor de informes. Estas directivas de seguridad afectan principalmente a las expresiones de informe y a los ensamblados personalizados una vez que se implementa un informe en un servidor de informes. Este archivo de directivas también afecta a los datos personalizados, entregas, representaciones y extensiones de seguridad que se implementan en el servidor de informes.|  
|rsmgrpolicy.config|C:\Archivos de programa\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportManager|Archivo de configuración de directivas del Administrador de informes. Estas directivas de seguridad afectan a todos los ensamblados que abarca el Administrador de informes; por ejemplo, extensiones de interfaz de usuario de suscripciones para entregas personalizadas.|  
|rspreviewpolicy.config|C:\Archivos de programa\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies|El archivo de configuración de directivas de vista previa independiente del Diseñador de informes. Estas directivas de seguridad afectan a las expresiones de informe y a los ensamblados personalizados que se utilizan en los informes durante la vista previa y el desarrollo. Estas directivas afectan también a las extensiones personalizadas, como las extensiones de procesamiento de datos, que se implementan en el Diseñador de informes.|  
  
## <a name="modifying-configuration-files"></a>Modificación de los archivos de configuración  
 Los parámetros de configuración se especifican como atributos o elementos XML. Si comprende XML y los archivos de configuración, puede utilizar un editor de texto o de código para modificar las opciones de configuración definibles por el usuario. Los archivos de configuración de seguridad contienen información sobre los conjuntos de permisos y la jerarquía de grupos de código asociados a un nivel de directiva en [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Se recomienda que utilice la utilidad de configuración de .NET Framework (Mscorcfg.msc) o la utilidad de directiva de seguridad de acceso del código (Caspol.exe) para modificar primero las directivas de seguridad en el archivo Security.config, de forma que los cambios de directiva correspondan a los elementos de configuración de XML válidos para los archivos de directivas. Una vez finalizada esa tarea, puede cortar y pegar los nuevos grupos de código y conjuntos de permisos de Security.config en el archivo de directivas para el componente al que va a agregar los permisos de código.  
  
> [!IMPORTANT]  
>  Debería hacer una copia de seguridad de los archivos de configuración de directivas antes de realizar cualquier modificación.  
  
 De esta manera se logran dos cosas. Primero, permite utilizar una herramienta visual para construir grupos de código y conjuntos de permisos para [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Esto es mucho más fácil que escribir elementos de configuración de XML desde el principio. En segundo lugar, garantiza que no se dañan los archivos de configuración de directivas de seguridad con elementos y atributos XML corruptos. Para obtener más información acerca de la utilidad de la directiva de seguridad de acceso del código, vea la sección sobre los archivos de directivas de seguridad de Reporting Services en MSDN.  
  
 Antes de modificar los archivos de configuración de directivas, debería leer toda la información disponible en esta sección y los temas relacionados. Modificar la configuración de directivas de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] puede afectar significativamente a la seguridad acerca de cómo ejecutan los componentes de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] los módulos de código externo.  
  
## <a name="placement-of-codegroup-elements-for-extensions"></a>Colocación de los elementos CodeGroup para las extensiones  
 La colocación de los elementos CodeGroup en un archivo de directiva de seguridad es importante. Para las extensiones y los ensamblados personalizados que desarrolla, se recomienda que coloque sus grupos de código personalizados directamente debajo de la entrada existente para la pertenencia de dirección URL "$CodeGen$/*", como se indica a continuación:  
  
```  
<CodeGroup  
    class="UnionCodeGroup"  
    version="1"  
    PermissionSetName="FullTrust">  
    <IMembershipCondition   
        class="UrlMembershipCondition"  
        version="1"  
        Url="$CodeGen$/*"  
    />  
</CodeGroup>  
<CodeGroup   
    class="UnionCodeGroup"  
    version="1"  
    PermissionSetName="FullTrust"  
    Name="MyCustomCodeGroup"  
    Description="Code group for my custom extension">  
        <IMembershipCondition class="UrlMembershipCondition"  
        version="1"  
        Url="C:\Program Files\Microsoft SQL Server\MSSQL\Reporting Services\ReportServer\bin\MyAssembly.dll"  
        />  
</CodeGroup>  
```  
  
 Los grupos de código adicionales se pueden agregar uno después del otro.  
  
## <a name="see-also"></a>Consulte también  
 [Descripción de las directivas de seguridad](../../../reporting-services/extensions/secure-development/understanding-security-policies.md)  
  
  
