---
title: Acceder a elementos del servidor de informes mediante el acceso URL | Microsoft Docs
description: Aprenda a acceder a elementos del catálogo de diferentes tipos en una base de datos del servidor de informes o en un sitio de SharePoint mediante rs:Command=Value.
ms.date: 05/08/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- referencing URL items for report server access
- URL access [Reporting Services], report servers
ms.assetid: a58b4ca6-129d-45e9-95c7-e9169fe5bba4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1dda91d82f56fc8e8ff46904eaa9fdd163ce56c2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474506"
---
# <a name="access-report-server-items-using-url-access"></a>Acceder a elementos del servidor de informes mediante el acceso URL
  En este tema se explica cómo acceder a los elementos del catálogo de diferentes tipos en una base de datos del servidor de informes o en un sitio de SharePoint mediante *rs:Command*=*Value*. En realidad, no es necesario agregar esta cadena de parámetro. Si la omite, el servidor de informes evalúa el tipo de elemento y selecciona el valor de parámetro apropiado automáticamente. Pero, si se usa la cadena *rs:Command*=*Value* en la dirección URL, mejora el rendimiento del servidor de informes.  
  
 Observe la sintaxis del proxy `_vti_bin` en los ejemplos siguientes. Para obtener más información acerca de cómo usar la sintaxis de proxy, vea [URL Access Parameter Reference](../reporting-services/url-access-parameter-reference.md).  

> [!NOTE]
> La integración de Reporting Services con SharePoint ya no está disponible a partir de SQL Server 2016.
  
## <a name="access-a-report"></a>Acceder a un informe  
 Para ver un informe en el explorador, use el parámetro *rs:Command*=*Render* . Por ejemplo:  
  
 - **Nativo** `https://myrshost/reportserver?/Sales/YearlySalesByCategory&rs:Command=Render`  

::: moniker range="=sql-server-2016"

 - **SharePoint** `https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/Sales/YearlySalesByCategory&rs:Command=Render`  
  
> [!TIP]  
>  Es importante que la dirección URL incluya la sintaxis de proxy de `_vti_bin` para enrutar la solicitud a través de SharePoint y el proxy HTTP de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . El proxy agrega algún contexto a la solicitud HTTP, contexto que es necesario para garantizar la correcta ejecución del informe para los servidores de informes de modo de SharePoint.  

::: moniker-end
  
## <a name="access-a-resource"></a>Acceder a un recurso  
 Para tener acceso a un recurso, use el parámetro *rs:Command*=*GetResourceContents* . Si el recurso es compatible con el explorador, como una imagen, se abre en el explorador. De lo contrario, le preguntarán si desea abrir o guardar el archivo o recurso en el disco.  
  
 **Nativo** `https://myrshost/reportserver?/Sales/StorePicture&rs:Command=GetResourceContents`  

::: moniker range="=sql-server-2016"
  
 **SharePoint** `https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/Sales/StorePicture.jpg&rs:Command=GetResourceContents`  

::: moniker-end
  
## <a name="access-a-data-source"></a>Acceder a un origen de datos  
 Para acceder al origen de datos, use el parámetro *rs:Command*=*GetDataSourceContents* . Si el explorador admite código XML, aparecerá la definición del origen de datos si es un usuario autenticado con el permiso **Read Contents** en el origen de datos. Por ejemplo:  
  
 **Nativo** `https://myrshost/reportserver?/Sales/AdventureWorks2012&rs:Command=GetDataSourceContents`  

::: moniker range="=sql-server-2016"
  
 **SharePoint** `https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/Sales/AdventureWorks2012&rs:Command=GetDataSourceContents`  
  
 La estructura XML se parecería al ejemplo siguiente:  

::: moniker-end
  
```  
<DataSourceDefinition>  
   <Extension>SQL</Extension>  
   <ConnectString>Provider=SQLOLEDB.1;Integrated Security=SSPI;Persist Security Info=False;Initial Catalog=AdventureWorks2012;Data Source=MYSERVER1;</ConnectString>  
   <CredentialRetrieval>Integrated</CredentialRetrieval>  
   <WindowsCredentials>False</WindowsCredentials>  
   <ImpersonateUser>False</ImpersonateUser>  
   <Prompt />  
   <Enabled>True</Enabled>  
</DataSourceDefinition>  
```  
  
 Se devuelve la cadena de conexión según el valor **SecureConnectionLevel** del servidor de informes. Para obtener más información acerca de la configuración **SecureConnectionLevel** , vea [Using Secure Web Service Methods](../reporting-services/report-server-web-service/net-framework/using-secure-web-service-methods.md).  
  
## <a name="access-the-contents-of-a-folder"></a>Acceder al contenido de una carpeta  
 Para acceder al contenido de una carpeta, use el parámetro *rs:Command*=*GetChildren* . Se devuelve una página de navegación por carpetas genérica que contiene vínculos a las subcarpetas, informes, orígenes de datos y recursos en la carpeta solicitada. Por ejemplo:  
  
 **Nativo** `https://myrshost/reportserver?/Sales&rs:Command=GetChildren`  

::: moniker range="=sql-server-2016"
  
 **SharePoint** `https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/Sales&rs:Command=GetChildren`  

::: moniker-end
  
 La interfaz de usuario que se ve es similar al modo de exploración de directorios que usa [!INCLUDE[msCoName](../includes/msconame-md.md)] Internet Information Server (IIS). El número de versión, incluido el número de compilación, del servidor de informes también se muestra debajo de la lista de carpetas.  
  
## <a name="see-also"></a>Consulte también  
 [Acceso URL &#40;SSRS&#41;](../reporting-services/url-access-ssrs.md)   
 [Referencia de parámetros de acceso URL](../reporting-services/url-access-parameter-reference.md) 
